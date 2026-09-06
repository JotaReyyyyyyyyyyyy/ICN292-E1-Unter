# 01 — Requerimientos

Los requisitos de este documento se derivan directamente de los flujos to-be
descritos en `02-bpmn.md`. Cada uno indica de qué problema de `00-caso-pyme.md`
proviene, de modo que la trazabilidad problema → proceso → requisito quede
explícita.

Priorización **MoSCoW**: `M` imprescindible, `S` importante, `C` deseable,
`W` fuera de esta entrega.

---

## 1. Actores y roles

### Actores humanos

| Actor | Tipo | Descripción |
|---|---|---|
| **Cliente** | Externo | Empresa o persona que consulta por productos, precios o especificaciones. |
| **Prospecto** | Externo | Empresa candidata a cliente, todavía sin relación comercial. |
| **Jani** | Interno | Tesorería, finanzas, precios, condiciones de pago y plazos, y todo el trato y negociación con proveedores. |
| **Ariel** | Interno | Comercial, catálogo, especificaciones técnicas, prospección y atención. Por definición del caso, **cualquier rol adicional que se identifique recae en Ariel**. |
| **Proveedor** | Externo | Empresa que abastece los productos y es la fuente de la documentación técnica original. |

### Actores de sistema

| Actor | Descripción |
|---|---|
| **Asistente de IA** | Atiende el chat, interpreta la consulta, responde desde la base y deriva cuando no puede. |
| **Motor de búsqueda de prospectos** | Recorre fuentes abiertas, recopila y califica empresas candidatas. |
| **Generador de fichas técnicas** | Extrae, normaliza y arma borradores de ficha con la identidad de Unter. |

### Fuentes externas

Fuentes abiertas de datos de empresas y redes profesionales, consultadas por el
motor de búsqueda de prospectos.

---

## 2. Alcance

### Dentro del alcance (in)

- Entrada única de consultas de clientes mediante chat, con derivación interna.
- Base consultable de productos, fichas técnicas y códigos propios.
- Registro de consultas, derivaciones y tiempos de respuesta.
- Recopilación y calificación de prospectos, con trazabilidad de origen.
- Generación asistida de fichas técnicas con marca Unter y validación humana.
- Versionado de fichas y detección de documentación desactualizada.

### Fuera del alcance (out)

| Elemento | Por qué queda fuera |
|---|---|
| ERP completo de la operación | El problema que resolvería no está definido al nivel de detalle que exige un diseño. Se documenta como trabajo posterior. |
| Integración contable o tributaria | Depende de decisiones de la empresa que exceden el proyecto. |
| Envío automático de mensajes a prospectos | **Decisión de diseño explícita**: el sistema prepara, Ariel envía. Ver RNF-04. |
| Comercio electrónico o carro de compra | Unter opera por cotización, no por venta directa en línea. |
| Gestión de stock y logística | No es el problema levantado. |

---

## 3. Requisitos funcionales

### Componente 1 — Asistente de IA

| ID | Requisito | Prio. | Problema |
|---|---|---|---|
| RF-01 | El sistema debe ofrecer un chat como punto único de entrada de consultas de clientes. | M | 3.1 |
| RF-02 | El asistente debe identificar al interlocutor capturando nombre, empresa, correo y teléfono, y reconocer a un cliente ya registrado sin volver a pedirle los datos. | M | 3.1 |
| RF-03 | El asistente debe clasificar cada consulta en una de cuatro categorías: técnica de producto, precio o disponibilidad, estado de cotización o pedido, u otra. | M | 3.1 |
| RF-04 | El asistente debe responder consultas cuya información esté disponible en la base de productos y fichas técnicas aprobadas. | M | 3.1 / 3.3 |
| RF-05 | Cuando la base no contenga la respuesta completa, el asistente debe entregar la parte disponible, declarar explícitamente qué falta y generar una derivación. | M | 3.1 |
| RF-06 | La derivación debe registrar quién pregunta, de qué empresa, cómo contactarlo, qué pidió, qué ya se le respondió y qué falta. | M | 3.1 |
| RF-07 | El sistema debe asignar la derivación según su naturaleza: precio, condiciones, plazos o proveedor a Jani; especificación técnica, producto, catálogo y cualquier otro caso a Ariel. | M | 3.1 |
| RF-08 | El sistema debe notificar cuando una derivación supere el plazo comprometido, y volver a notificar a la otra persona si se vence por segunda vez. | S | 3.1 |
| RF-09 | El sistema debe permitir registrar la respuesta entregada, de modo que quede disponible para consultas posteriores. | M | 3.1 |
| RF-10 | El sistema debe marcar el producto como "documentación incompleta" cuando una consulta no se pudo responder por falta de un dato que debería existir. | S | 3.1 / 3.3 |
| RF-11 | El sistema debe registrar todas las consultas con su fecha, tiempo de respuesta y estado de resolución. | M | 3.1 |

### Componente 2 — Búsqueda de clientes

| ID | Requisito | Prio. | Problema |
|---|---|---|---|
| RF-12 | El sistema debe permitir definir campañas de búsqueda con criterios de rubro, zona y tamaño. | M | 3.2 |
| RF-13 | El sistema debe recopilar, por cada empresa candidata, razón social, dirección, teléfono, sitio web y rubro. | M | 3.2 |
| RF-14 | El sistema debe recopilar, por cada empresa, sus contactos con nombre, cargo, correo y teléfono. | M | 3.2 |
| RF-15 | El sistema debe registrar la fuente y la fecha de obtención de cada dato recopilado. | M | 3.2 |
| RF-16 | El sistema debe detectar y descartar empresas que ya existan en la base como cliente o prospecto trabajado, indicando la fecha del último contacto. | M | 3.2 |
| RF-17 | El sistema debe calificar cada prospecto con un puntaje según los criterios de la campaña y descartar los que no superen el umbral, conservando el motivo. | S | 3.2 |
| RF-18 | El sistema debe entregar la lista calificada a Ariel para revisión, con un mensaje de primer contacto sugerido por prospecto. | M | 3.2 |
| RF-19 | El sistema debe permitir registrar en un paso qué prospectos fueron contactados y en qué fecha. | M | 3.2 |
| RF-20 | El sistema debe recordar el seguimiento de prospectos sin respuesta hasta el máximo de intentos definido. | C | 3.2 |
| RF-21 | El sistema debe permitir marcar un contacto como "no contactar" y bloquear a esa empresa en toda campaña futura. | M | 3.2 |

### Componente 3 — Fichas técnicas

| ID | Requisito | Prio. | Problema |
|---|---|---|---|
| RF-22 | El sistema debe detectar los productos del catálogo que no tienen ficha propia aprobada. | M | 3.3 |
| RF-23 | El sistema debe priorizar la cola de generación según frecuencia de cotización y consultas no resueltas previas. | C | 3.3 |
| RF-24 | El sistema debe generar una solicitud de documentación al proveedor, asignada a Jani, cuando el material disponible sea insuficiente. | S | 3.3 |
| RF-25 | El sistema debe extraer los atributos técnicos del material del proveedor y normalizarlos al formato de Unter. | M | 3.3 |
| RF-26 | El sistema debe asignar un código interno propio a cada producto y detectar colisiones con códigos existentes. | M | 3.3 |
| RF-27 | El sistema debe generar un borrador de ficha con la identidad visual de Unter, indicando el origen de cada dato. | M | 3.3 |
| RF-28 | El sistema debe exigir la aprobación de Ariel antes de publicar cualquier ficha. | M | 3.3 |
| RF-29 | El sistema debe versionar cada ficha con número y fecha, y marcarla como desactualizada cuando el proveedor cambie la especificación. | S | 3.3 |
| RF-30 | Las fichas aprobadas deben quedar disponibles como fuente de respuesta del asistente (RF-04). | M | 3.1 / 3.3 |

---

## 4. Requisitos no funcionales

| ID | Requisito | Prio. | Justificación |
|---|---|---|---|
| RNF-01 | **El asistente no debe entregar nunca una especificación técnica o un precio que no esté en la base.** Ante la ausencia del dato, debe declararlo y derivar. | M | Un dato técnico inventado, entregado con la marca de Unter, es responsabilidad de Unter frente al cliente. |
| RNF-02 | **Ninguna ficha técnica puede publicarse sin validación humana.** | M | Desde que lleva la marca de Unter, el error deja de ser del proveedor. |
| RNF-03 | Todo dato de contacto recopilado debe conservar su fuente y su fecha de obtención, y debe existir un mecanismo de baja a solicitud del titular. | M | Ley 21.719 sobre protección de datos personales. |
| RNF-04 | **Ningún mensaje a un prospecto puede salir sin aprobación y envío manual de una persona.** | M | Decisión de diseño del caso; refuerza RNF-03 y evita el contacto masivo no supervisado. |
| RNF-05 | El chat debe estar disponible fuera del horario laboral y responder en menos de 5 segundos las consultas resolubles desde la base. | S | El valor del componente 1 está en atender cuando las dos personas no pueden. |
| RNF-06 | El registro de una respuesta o de un contacto enviado debe requerir un solo paso. | S | Con dos personas, todo registro que cueste más de un clic no se hace, y sin registro el sistema no funciona. |
| RNF-07 | El sistema debe ser operable por una persona sin formación técnica. | M | Unter no tiene personal de TI. |
| RNF-08 | La Entrega 2 debe poder levantarse en localhost de forma reproducible a partir del repositorio. | M | Exigido por el enunciado. |

---

## 5. Trazabilidad resumida

| Problema | Proceso | Requisitos |
|---|---|---|
| 3.1 Consultas sin entrada única ni registro | Flujo 1 | RF-01 a RF-11, RF-30, RNF-01, RNF-05, RNF-06 |
| 3.2 Prospección manual y sin memoria | Flujo 2 | RF-12 a RF-21, RNF-03, RNF-04, RNF-06 |
| 3.3 Sin documentación técnica propia | Flujo 3 | RF-22 a RF-30, RF-04, RF-10, RNF-02 |
