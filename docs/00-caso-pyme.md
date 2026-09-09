# 00 — Caso PYME: Unter

> **Estado del documento.** El texto cualitativo está redactado. Los campos
> marcados como `[[DATO: ...]]` se completan con lo que salga de la entrevista
> formal. **Ninguno de esos campos debe inventarse**: el enunciado prohíbe
> fabricar evidencia y la rúbrica lo sanciona de inmediato.

---

## 1. Identificación de la PYME

| Campo | Valor |
|---|---|
| Nombre legal | `[[DATO: razón social exacta]]` |
| Nombre comercial | Unter |
| RUT | `[[DATO: RUT]]` |
| Rubro | Intermediación y distribución de productos de terceros |
| Tamaño | **2 personas** |
| Ubicación | La Concepción 81, `[[DATO: comuna y ciudad]]` |
| Años de operación | `[[DATO: desde cuándo operan]]` |
| Presencia pública | `[[DATO: sitio web / Instagram / ficha SII]]` |

**Personas de la organización.** Unter opera con dos personas, y esa es una
característica del caso, no un detalle: no existe un tercer nivel al cual escalar
nada.

- **Jani** — tesorería, finanzas, precios, condiciones de pago y plazos, y todo el
  trato y la negociación con proveedores.
- **Ariel** — todo lo demás: comercial, catálogo, especificaciones técnicas,
  prospección y atención.

## 2. Evidencia de existencia

El enunciado exige acreditar la PYME por al menos una vía verificable. Vías
disponibles para este caso:

- [ ] Presencia pública verificable — `[[DATO: URL con fecha de acceso]]`
- [x] Entrevista a encargado, con acta firmada — ver anexo del informe
- [ ] Unidad operativa de una organización mayor — no aplica

**Entrevista.** Se realizó una primera conversación de levantamiento en modalidad
informal. La instancia formal, con acta firmada y fecha, queda
`[[DATO: fecha de la entrevista formal]]`. El acta va escaneada en el anexo del
informe y la pauta usada está en este mismo repositorio.

## 3. Problema de negocio

**En una frase:** Unter no tiene dónde guardar ni cómo consultar la información
que su propia operación genera, de modo que cada consulta de un cliente se
resuelve desde cero, y la documentación técnica que entrega es la del proveedor,
lo que expone a la empresa a la desintermediación.

El problema se descompone en tres procesos. Cada uno está modelado as-is y to-be
en los archivos de Bizagi de `assets/bpm/`, y los seis diagramas van en el anexo
del informe:

### 3.1 Atención de consultas sin entrada única ni registro

Las consultas entran por WhatsApp, correo o teléfono, a quien el cliente tenga
guardado de antes. Quien recibe busca la respuesta en su propio correo o le
pregunta al otro; si nadie la tiene, se le escribe al proveedor y se espera. Nada
queda registrado, así que la misma consulta se vuelve a resolver desde cero cuando
reaparece.

**Impacto medible:**

- Consultas recibidas al mes: `[[DATO]]`
- Tiempo promedio de respuesta: `[[DATO]]`
- Peor tiempo de respuesta: `[[DATO]]`
- Consultas al mes que se atrasan o se pierden por falta de datos: `[[DATO]]`

### 3.2 Prospección manual y sin memoria

La búsqueda de clientes nuevos es reactiva y la ejecuta Ariel a mano. No hay
registro de a quién se contactó ni cuándo, lo que produce trabajo repetido y, en
algunos casos, contactos duplicados a la misma empresa.

**Impacto medible:**

- Horas al mes dedicadas a prospección: `[[DATO]]`
- Prospectos contactados al mes: `[[DATO]]`
- Registro existente de contactos previos: `[[DATO: sí / no / parcial]]`

### 3.3 Ausencia de documentación técnica propia

Unter no genera fichas técnicas. Cuando un cliente pide una especificación, se le
reenvía el documento del proveedor con su marca y sus datos de contacto. Además de
perder la oportunidad de posicionar la marca propia, el proveedor queda a un clic
del cliente.

**Impacto medible:**

- Productos distintos en catálogo: `[[DATO]]`
- Productos con ficha propia de Unter: `[[DATO]]`
- Productos que dependen del PDF del proveedor: `[[DATO]]`
- Tiempo en conseguir una especificación que no se tiene: `[[DATO]]`
- Casos conocidos de proveedor vendiendo directo a un cliente de Unter: `[[DATO]]`

## 4. Objetivo del SIG propuesto

Que la información que hoy vive dispersa en correos, conversaciones de WhatsApp y
la memoria de dos personas pase a estar centralizada, consultable y trazable, de
modo que:

1. Una consulta de cliente se responda desde la base sin depender de quién la
   reciba, y cuando no se pueda, se derive con contexto y con un plazo controlado.
2. La prospección deje registro reutilizable, con el origen y la fecha de cada dato.
3. Cada producto del catálogo tenga documentación técnica con marca de Unter,
   validada por una persona antes de publicarse.

**Decisión u operación que mejora:** la respuesta a un cliente deja de depender de
la disponibilidad y la memoria de una persona específica, y pasa a depender de un
dato registrado.

## 5. Alcance de la Entrega 1

**Dentro:** diagnóstico de los tres procesos, requerimientos, modelado as-is y
to-be, modelo de datos preliminar y arquitectura lógica.

**Fuera:** implementación, un ERP completo para toda la operación de Unter, y la
integración con sistemas contables o tributarios. Se documentan como alcance
posterior porque el problema que resuelven todavía no está definido con el nivel
de detalle que exige un diseño.
