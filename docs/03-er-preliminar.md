# 03 — Modelo entidad–relación preliminar

Nueve entidades que sostienen los tres procesos to-be. Notación simplificada de
la asignatura: clave primaria marcada con `#` y `(ip)`, foráneas con `(fk)`, y la
relación N:M resuelta con una entidad puente.

El diagrama está en `informe/er_unter.png` y a página completa en el informe.

## Entidades

| Entidad | PK candidata | Para qué existe |
|---|---|---|
| `CLIENTE` | `ID_CLIENTE` | Empresa que compra o podría comprar. Lleva `NO_CONTACTAR` para bloquearla en campañas futuras. |
| `CONTACTO` | `ID_CONTACTO` | Persona dentro de un cliente. Guarda `FUENTE_DATO` y `FECHA_OBTENCION`, que es lo que exige la Ley 21.719. |
| `CONSULTA` | `ID_CONSULTA` | Cada pregunta que entra por el chat, con canal, categoría y fechas de ingreso y respuesta. |
| `DERIVACION` | `ID_DERIVACION` | Consulta que el asistente no pudo resolver: a quién se asignó, con qué plazo y en qué estado. |
| `PRODUCTO` | `SKU` | Artículo del catálogo. `ESTADO_DOC` es lo que permite detectar los productos sin ficha propia. |
| `FICHA_TECNICA` | `ID_FICHA` | Documentación con marca Unter. **Entidad propia y versionada**, no un campo de `PRODUCTO`, porque la aprobación y el versionado son parte del modelo. |
| `PROVEEDOR` | `ID_PROV` | Quien abastece el producto y origina la documentación técnica. |
| `CAMPANA` | `ID_CAMPANA` | Búsqueda de prospectos con criterios de rubro, zona y umbral de puntaje. |
| `PROSPECCION` | `ID_CAMPANA` + `ID_CLIENTE` | Entidad puente de la relación N:M entre campaña y cliente. Lleva puntaje, estado y fecha de contacto. |

## Relaciones

| Relación | Cardinalidad | Qué significa |
|---|---|---|
| `CLIENTE` — `CONTACTO` | 1:N | Una empresa tiene varias personas de contacto. |
| `CONTACTO` — `CONSULTA` | 1:N | Una persona realiza varias consultas. |
| `CONSULTA` — `DERIVACION` | 1:N | Una consulta genera una derivación cuando no se puede responder. |
| `PRODUCTO` — `CONSULTA` | 1:N | Una consulta se refiere a un producto del catálogo. |
| `PRODUCTO` — `FICHA_TECNICA` | 1:N | Un producto acumula versiones de ficha, con una sola vigente. |
| `PROVEEDOR` — `PRODUCTO` | 1:N | Un proveedor abastece varios productos. |
| `CAMPANA` — `PROSPECCION` — `CLIENTE` | N:M vía puente | Una campaña incluye muchos clientes y un cliente puede aparecer en varias campañas. |

## Trazabilidad proceso ↔ datos

| Lo que el proceso to-be necesita hacer | Dónde queda en el modelo |
|---|---|
| Responder desde la base sin intervención humana | `FICHA_TECNICA` vigente asociada al `PRODUCTO` |
| Derivar con contexto y plazo controlado | `DERIVACION`: `ASIGNADO_A`, `PLAZO`, `ESTADO` |
| Detectar productos sin ficha propia | `PRODUCTO.ESTADO_DOC` |
| Publicar una ficha solo tras aprobación | `FICHA_TECNICA`: `ESTADO` y `VIGENTE` |
| No volver a contactar a quien pidió no serlo | `CLIENTE.NO_CONTACTAR` |
| Conservar origen y fecha de cada dato de contacto | `CONTACTO`: `FUENTE_DATO`, `FECHA_OBTENCION` |

El punto de cierre entre los tres procesos está en `PRODUCTO`: el proceso de
fichas recorre el inventario y llena `FICHA_TECNICA`; el de atención lee la ficha
vigente para responder sin intervención humana; y el de prospección alimenta
`CLIENTE` y `CONTACTO`, que son quienes después hacen las consultas. Esa
dependencia es la razón por la que los tres módulos son **un solo sistema** y no
tres proyectos separados.
