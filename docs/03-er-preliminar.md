# 03 — Modelo entidad-relación preliminar

Modelo de datos que sostiene los flujos to-be de `02-bpmn.md`. Cada entidad indica
de qué paso del proceso proviene, de modo que la trazabilidad proceso ↔ datos sea
verificable y no una justificación posterior.

Convención: `PK` clave primaria, `FK` clave foránea, `*` obligatorio.

---

## 1. Entidades

### EMPRESA
Toda organización externa con la que Unter se relaciona: cliente, prospecto o
ambos a lo largo del tiempo. Se modela una sola entidad y no dos, porque un
prospecto que compra se convierte en cliente sin dejar de ser la misma empresa, y
duplicarla es justamente el error del as-is.

| Atributo | Tipo | Nota |
|---|---|---|
| `id_empresa` | PK | |
| `razon_social` * | texto | RF-13 |
| `rut` | texto | puede no estar disponible al prospectar |
| `direccion` | texto | RF-13 |
| `telefono` | texto | RF-13 |
| `sitio_web` | texto | RF-13 |
| `rubro` | texto | RF-13 |
| `estado` * | enum | `prospecto`, `cliente`, `archivado`, `bloqueado` |
| `fecha_ultimo_contacto` | fecha | sostiene la deduplicación (RF-16) |
| `no_contactar` * | booleano | RF-21, RNF-03 |

*Origen:* flujo 2, pasos 2 y 5.

### CONTACTO
Persona dentro de una empresa. Separada de EMPRESA porque una empresa tiene varios
contactos y porque el derecho de baja de la Ley 21.719 es de la persona, no de la
organización.

| Atributo | Tipo | Nota |
|---|---|---|
| `id_contacto` | PK | |
| `id_empresa` * | FK → EMPRESA | |
| `nombre` * | texto | RF-14 |
| `cargo` | texto | RF-14 |
| `correo` | texto | RF-14 |
| `telefono` | texto | RF-14 |
| `fuente_dato` * | texto | RF-15, RNF-03 |
| `fecha_obtencion` * | fecha | RF-15, RNF-03 |

*Origen:* flujo 2, pasos 3 y 4.

### PROVEEDOR
Quien abastece los productos y es la fuente de la documentación técnica original.
Entidad aparte de EMPRESA porque la relación es de otra naturaleza: la gestiona
Jani y su ciclo es el del flujo 3, no el del flujo 2.

| Atributo | Tipo | Nota |
|---|---|---|
| `id_proveedor` | PK | |
| `nombre` * | texto | |
| `contacto` | texto | |
| `correo` | texto | |
| `plazo_respuesta_dias` | entero | sostiene el evento de tiempo del flujo 3 paso 4 |

*Origen:* flujo 3, paso 2.

### PRODUCTO
Ítem del catálogo de Unter.

| Atributo | Tipo | Nota |
|---|---|---|
| `id_producto` | PK | |
| `codigo_unter` * | texto, único | RF-26; la unicidad es lo que detecta la colisión |
| `nombre` * | texto | |
| `id_proveedor` * | FK → PROVEEDOR | |
| `nombre_proveedor_original` | texto | cómo lo llama el proveedor |
| `estado_documentacion` * | enum | `sin_ficha`, `en_cola`, `sin_respaldo`, `con_ficha`, `desactualizada` |
| `prioridad_cola` | entero | RF-23 |

*Origen:* flujo 3, pasos 1, 2 y 6.

### FICHA_TECNICA
Documentación con marca Unter. Entidad propia y versionada, no un campo de
PRODUCTO, porque el versionado (RF-29) y la aprobación (RF-28) son parte del
modelo, no metadatos.

| Atributo | Tipo | Nota |
|---|---|---|
| `id_ficha` | PK | |
| `id_producto` * | FK → PRODUCTO | |
| `version` * | entero | RF-29 |
| `vigente` * | booleano | solo una vigente por producto |
| `estado` * | enum | `borrador`, `aprobada`, `rechazada`, `desactualizada` |
| `contenido` | texto/JSON | atributos normalizados (RF-25) |
| `origen_datos` * | texto/JSON | origen por atributo (RF-27) |
| `aprobada_por` | texto | RF-28, RNF-02 |
| `fecha_aprobacion` | fecha | |

*Origen:* flujo 3, pasos 8 a 12.

### CONSULTA
Cada interacción de un cliente con el asistente.

| Atributo | Tipo | Nota |
|---|---|---|
| `id_consulta` | PK | |
| `id_contacto` | FK → CONTACTO | nulo si el interlocutor aún no se identificó |
| `id_producto` | FK → PRODUCTO | nulo si la consulta no es sobre un producto |
| `categoria` * | enum | `tecnica`, `precio`, `estado_pedido`, `otra` (RF-03) |
| `texto` * | texto | |
| `fecha_ingreso` * | fecha-hora | RF-11 |
| `resuelta_por_asistente` * | booleano | RF-04 |
| `fecha_resolucion` | fecha-hora | permite el tiempo de respuesta (RF-11) |
| `dato_faltante` | texto | dispara el marcado del flujo 1 paso 13 (RF-10) |

*Origen:* flujo 1, pasos 4 a 6 y 13.

### DERIVACION
Traspaso de una consulta a una persona. Entidad separada porque tiene su propio
ciclo de vida, su asignado y su plazo, y porque una consulta puede derivarse más
de una vez.

| Atributo | Tipo | Nota |
|---|---|---|
| `id_derivacion` | PK | |
| `id_consulta` * | FK → CONSULTA | |
| `asignado_a` * | enum | `Jani`, `Ariel` (RF-07) |
| `contexto` * | texto | lo respondido y lo que falta (RF-06) |
| `plazo_comprometido` * | fecha-hora | RF-08 |
| `estado` * | enum | `pendiente`, `tomada`, `respondida`, `vencida` |
| `respuesta` | texto | RF-09 |
| `fecha_respuesta` | fecha-hora | |

*Origen:* flujo 1, pasos 8 a 12.

### CAMPANA
Búsqueda de prospectos con criterios definidos.

| Atributo | Tipo | Nota |
|---|---|---|
| `id_campana` | PK | |
| `nombre` * | texto | |
| `criterio_rubro` | texto | RF-12 |
| `criterio_zona` | texto | RF-12 |
| `criterio_tamano` | texto | RF-12 |
| `umbral_puntaje` * | entero | RF-17 |
| `fecha_creacion` * | fecha | |

*Origen:* flujo 2, paso 1.

### PROSPECCION
Resultado de evaluar una empresa dentro de una campaña. Es la tabla que resuelve
la relación muchos-a-muchos entre CAMPANA y EMPRESA, y guarda lo que pasó en esa
campaña específica.

| Atributo | Tipo | Nota |
|---|---|---|
| `id_prospeccion` | PK | |
| `id_campana` * | FK → CAMPANA | |
| `id_empresa` * | FK → EMPRESA | |
| `puntaje` * | entero | RF-17 |
| `estado` * | enum | `propuesto`, `descartado`, `aprobado`, `contactado`, `respondido`, `frio` |
| `motivo_descarte` | texto | RF-17 |
| `fecha_contacto` | fecha | RF-19 |
| `intentos` * | entero | RF-20 |
| `resultado` | enum | `cerrado`, `perdido`, `en_seguimiento` (RF-15 del flujo, paso 15) |

*Origen:* flujo 2, pasos 5 a 16.

---

## 2. Relaciones y cardinalidades

| Relación | Cardinalidad | Lectura |
|---|---|---|
| EMPRESA — CONTACTO | 1 : N | Una empresa tiene varios contactos; un contacto pertenece a una empresa. |
| PROVEEDOR — PRODUCTO | 1 : N | Un proveedor abastece varios productos; cada producto tiene un proveedor principal. |
| PRODUCTO — FICHA_TECNICA | 1 : N | Un producto acumula varias versiones de ficha, con **una sola vigente**. |
| CONTACTO — CONSULTA | 1 : N | Un contacto puede hacer muchas consultas. |
| PRODUCTO — CONSULTA | 1 : N | Un producto puede ser objeto de muchas consultas; una consulta apunta a lo más a un producto. |
| CONSULTA — DERIVACION | 1 : N | Una consulta puede derivarse más de una vez; toda derivación pertenece a una consulta. |
| CAMPANA — PROSPECCION | 1 : N | Una campaña evalúa muchas empresas. |
| EMPRESA — PROSPECCION | 1 : N | Una empresa puede aparecer en varias campañas a lo largo del tiempo. |
| CAMPANA — EMPRESA | N : M | Resuelta mediante PROSPECCION. |

---

## 3. Cómo el ER sostiene los procesos

| Paso del proceso | Qué entidad lo hace posible |
|---|---|
| Reconocer a un cliente ya registrado (flujo 1, paso 3) | EMPRESA + CONTACTO |
| Responder desde la base (flujo 1, paso 6) | FICHA_TECNICA vigente vinculada a PRODUCTO |
| Derivar con contexto y plazo (flujo 1, pasos 8-11) | DERIVACION |
| Medir tiempo de respuesta (flujo 1, paso 14) | CONSULTA: `fecha_ingreso` y `fecha_resolucion` |
| Detectar documentación faltante (flujo 1, paso 13) | CONSULTA.`dato_faltante` → PRODUCTO.`estado_documentacion` |
| Deduplicar prospectos (flujo 2, paso 5) | EMPRESA.`fecha_ultimo_contacto` + PROSPECCION histórica |
| Trazabilidad de origen de datos (flujo 2, paso 4) | CONTACTO: `fuente_dato` y `fecha_obtencion` |
| Bloquear a quien pide no ser contactado (flujo 2, paso 17) | EMPRESA.`no_contactar` |
| Priorizar la cola de fichas (flujo 3, paso 2) | PRODUCTO.`prioridad_cola` y `estado_documentacion` |
| Detectar colisión de código (flujo 3, paso 7) | PRODUCTO.`codigo_unter` con restricción de unicidad |
| Exigir aprobación humana (flujo 3, paso 9) | FICHA_TECNICA: `estado` y `aprobada_por` |
| Marcar ficha desactualizada (flujo 3, paso 13) | FICHA_TECNICA.`vigente` + `version` |

El punto de cierre entre los tres procesos está en PRODUCTO: el flujo 1 escribe
`estado_documentacion`, el flujo 3 lo lee para armar su cola, y el flujo 1 vuelve a
leer la FICHA_TECNICA resultante para responder. Esa es la razón por la que los
tres componentes son un solo sistema y no tres proyectos separados.

---

## 4. Pendientes de esta versión

- Confirmar si un producto puede tener más de un proveedor. Se modeló uno solo;
  si en la entrevista formal aparece abastecimiento alternativo, PRODUCTO —
  PROVEEDOR pasa a N:M con una tabla intermedia.
- Definir la regla de composición de `codigo_unter`. La entidad ya soporta
  cualquier regla, pero la lógica de generación se especifica en la Entrega 2.
- Evaluar si CONSULTA necesita distinguir el canal de entrada, en caso de que se
  mantengan WhatsApp o correo en paralelo al chat.
