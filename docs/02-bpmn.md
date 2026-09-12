# 02 — Procesos BPMN (as-is y to-be)

Se modelaron los **tres procesos críticos** del caso, cada uno en su versión
as-is y to-be: seis diagramas en total, en notación BPMN 2.0.

## Dónde están

La fuente son tres archivos nativos de **Bizagi Modeler**. Cada uno trae dos
pestañas, `AS IS` y `TO BE`.

| Archivo fuente (`assets/bpm/`) | Diagramas exportados (`informe/`) |
|---|---|
| `Atención a clientes.bpm` | `atencion-ASIS.png`, `atencion-TOBE.png` |
| `Búsqueda de clientes.bpm` | `busqueda-ASIS.png`, `busqueda-TOBE.png` |
| `Creación de Fichas Técnicas.bpm` | `fichas-ASIS.png`, `fichas-TOBE.png` |

Los seis van a página completa en el anexo del informe. Si se edita un `.bpm`,
hay que volver a exportar los PNG y recompilar el informe.

## Entes y regla de asignación

Los entes son dos personas de Unter y el sistema propuesto. La regla es única:
**lo que involucra dinero, condiciones comerciales o negociación con proveedores
queda en finanzas y proveedores; todo lo demás queda en operaciones.** Al ser una
empresa de dos personas, no existe un tercer nivel de escalamiento.

En los diagramas as-is **el carril del sistema no existe, y esa ausencia es el
diagnóstico**.

## 1. Atención de consultas de clientes

**As-is.** Las consultas entran por WhatsApp, correo o teléfono, a quien el
cliente tenga guardado. Quien recibe busca en su propio correo o le pregunta al
otro; si nadie tiene la respuesta, se le escribe al proveedor y se espera. Nada
queda registrado, así que la misma consulta se vuelve a resolver desde cero.

**To-be.** Un chat integrado es la entrada única. El asistente identifica al
interlocutor, clasifica la consulta y responde desde la base de fichas
aprobadas. Cuando no puede, entrega lo que sí tiene, declara qué falta y genera
una derivación con contexto y plazo, asignada a Jani o a Ariel según el tema.

## 2. Búsqueda de clientes (prospección)

**As-is.** Ariel arma a mano un listado por zona y lo anota en una planilla. Si
no puede contactar por internet, visita en persona; si no lo atienden, se pierde
el viaje. No hay registro reutilizable de a quién se contactó ni cuándo.

**To-be.** El sistema recorre fuentes abiertas, arma la lista de prospectos con
fuente y fecha de cada dato, descarta duplicados y califica según los criterios
de la campaña. **El envío del primer contacto lo sigue haciendo Ariel a mano**:
es una decisión de diseño, no una limitación técnica.

## 3. Creación de fichas técnicas

**As-is.** Es un proceso interno: no lo dispara el cliente. El documento del
proveedor se guarda y ahí termina. Cuando se necesita la ficha, alguien busca
ese documento, se lo pide al proveedor si no lo encuentra, y la arma a mano. Esa
ficha se usa para la consulta puntual que la motivó y después se desecha, así
que el trabajo se repite entero la próxima vez.

**To-be.** Un creador de fichas semi-automático que trabaja sobre el inventario.
Dos disparadores: automático al agregar un producto nuevo, y a pedido cuando
operaciones lanza una revisión periódica. El sistema detecta los productos sin
ficha Unter, busca la documentación, extrae y normaliza los atributos y genera
la ficha. **La parte "semi" es la aprobación**: ninguna ficha se publica sin que
operaciones la revise. Si falta documentación, operaciones se la solicita al
proveedor y el producto queda marcado como sin respaldo.

## Mejoras que introduce el SIG

**Qué se automatiza.** La respuesta a consultas resolubles desde la base, la
construcción de la lista de prospectos y la generación del borrador de ficha.

**Qué se controla.** El plazo de las derivaciones, el seguimiento de los
prospectos sin respuesta y qué productos del catálogo no tienen ficha propia. En
el as-is nada de esto tiene control: depende de que alguien se acuerde.

**Qué se mide.** Consultas recibidas y resueltas por el asistente, tiempo de
respuesta promedio y peor caso, prospectos contactados y convertidos, y
cobertura de fichas propias sobre el catálogo. Ninguno de estos indicadores
existe hoy, y son la línea base de los KPI de la Entrega 2.

**Qué no cambia.** Las decisiones siguen siendo humanas: el sistema prepara,
prioriza y registra, pero la aprobación de una ficha y el envío de un primer
contacto los hace una persona.
