# 02 — Procesos: flujos as-is y to-be

Este documento describe en texto los seis flujos del caso (as-is y to-be de los
tres procesos). Es la fuente desde la cual se dibujan los diagramas BPMN 2.0, que
se exportan como imagen a `assets/`.

---

## 1. Entes

Los entes son los mismos para los seis flujos.

### Pools externos

- **Cliente / Prospecto**
- **Proveedor**

### Pool Unter, con tres lanes

| Lane | Alcance |
|---|---|
| **Jani** | Tesorería, finanzas, precios, condiciones de pago, plazos, y todo el trato y negociación con proveedores. |
| **Ariel** | Todo lo demás. Cualquier ente o rol adicional que se identifique recae en Ariel, incluido el rol de gestor de derivaciones. |
| **Sistema Unter** | Solo existe en los flujos to-be. En los as-is este lane va **dibujado y vacío**: esa ausencia es el diagnóstico. |

### Regla única de derivación

Válida para los tres procesos: **si la consulta es de dinero, condiciones o
proveedor, va a Jani. Cualquier otro caso va a Ariel.**

Unter son dos personas. No existe un tercer nivel de escalamiento: si uno no toma
algo, el único que queda es el otro.

---

## 2. Flujo 1 — Atención de consultas de clientes

### 2.1 As-is

1. **Inicio:** el cliente escribe o llama, por WhatsApp, correo o teléfono, al
   contacto que tenga guardado de antes.
2. El mensaje llega a Jani o a Ariel según a quién le haya escrito. No hay punto
   único de entrada ni criterio de quién atiende qué.
3. Quien recibió el mensaje lo lee y evalúa si puede responderlo.
4. **Decisión — ¿sabe la respuesta de memoria?** Si sí, responde y termina, sin
   dejar registro. Si no, continúa.
5. Busca el dato donde puede: su propio correo, una planilla, la carpeta con PDF
   de proveedores, el catálogo impreso.
6. **Decisión — ¿encontró el dato?** Si sí, responde y termina, tampoco con
   registro. Si no, continúa.
7. **Decisión — ¿es un dato técnico o comercial?** Técnico lo termina resolviendo
   Ariel; precio, plazo o condiciones con el proveedor, Jani. Si llegó a la
   persona equivocada, se reenvía, normalmente por WhatsApp.
8. Esa persona le escribe al proveedor pidiendo el dato.
9. **Espera.** Es el tiempo muerto principal del proceso: el cliente ya preguntó,
   Unter no puede responder, y nadie controla cuánto lleva esperando.
10. **Decisión — ¿el proveedor contestó?** Si sí, se responde al cliente con lo
    que llegó. Si no, se insiste cuando alguien se acuerda; si pasa mucho, el
    cliente se va y no queda registrado que se perdió.
11. La conversación termina sin quedar registrada. Si el mismo cliente vuelve a
    preguntar lo mismo, el proceso se repite completo desde el paso 1.
12. **Fin.** No se sabe cuántas consultas llegaron, cuánto demoraron, ni cuántas
    se perdieron.

### 2.2 To-be

1. **Inicio:** el cliente abre el chat integrado en el sitio de Unter y escribe.
   Punto único de entrada.
2. El asistente saluda e identifica al interlocutor: nombre, empresa, correo y
   teléfono.
3. **Decisión — ¿el cliente ya está en la base?** Si sí, confirma los datos y no
   los vuelve a pedir. Si no, los captura y crea el registro.
4. El asistente interpreta la pregunta y la clasifica: técnica de producto,
   precio o disponibilidad, estado de cotización o pedido, u otra.
5. El asistente consulta la base: catálogo, fichas técnicas aprobadas, códigos de
   producto, historial del cliente.
6. **Decisión — ¿la base tiene la respuesta completa?** Si sí, responde, guarda la
   conversación y ofrece ayuda adicional; si el cliente cierra, va al paso 13. Si
   es parcial o no la tiene, continúa.
7. El asistente entrega la parte disponible, declara qué le falta y avisa que
   deriva a una persona con un plazo comprometido. **Regla dura: el asistente
   nunca inventa una especificación ni un precio.**
8. El asistente arma la ficha de derivación con todo el contexto y la deja en la
   bandeja compartida.
9. **Decisión — ¿de qué es la consulta?** Precio, condiciones, plazos o proveedor
   van a Jani. Especificación técnica, producto, catálogo o cualquier otro caso
   van a Ariel.
10. La persona asignada resuelve. Si necesita al proveedor, la solicitud queda
    registrada con fecha.
11. **Evento de tiempo:** si la derivación no se toma dentro del plazo, el sistema
    avisa a la persona asignada; al segundo vencimiento avisa a la otra. No hay
    tercer nivel.
12. La persona responde al cliente y **registra la respuesta en el sistema**, no
    solo en el chat. Esto es lo que permite que la próxima consulta igual la
    resuelva el asistente en el paso 6.
13. **Decisión — ¿esta consulta se habría podido responder sola si el dato
    hubiera existido?** Si sí, el producto se marca como "documentación
    incompleta", lo que **dispara el flujo 3**.
14. **Fin.** Queda registrado qué se preguntó, cuánto demoró, quién lo resolvió y
    qué faltaba.

---

## 3. Flujo 2 — Búsqueda y captación de clientes

### 3.1 As-is

1. **Inicio:** Ariel decide buscar clientes nuevos. La decisión es reactiva,
   normalmente porque bajaron las ventas.
2. Busca a mano: internet, redes profesionales, referencias, guías del rubro.
3. Por cada empresa candidata entra al sitio o a la red social a buscar contacto.
4. **Decisión — ¿encontró un contacto con nombre y correo?** Si sí, lo anota. Si
   no, anota el teléfono general o descarta.
5. Anota donde le acomoda ese día: una planilla, el teléfono, un cuaderno.
6. **Decisión — ¿ya habían contactado a esta empresa?** Se resuelve de memoria,
   porque no hay dónde consultarlo. Acá se producen los contactos duplicados.
7. Ariel escribe el mensaje, redactándolo desde cero cada vez.
8. Lo envía y sigue con el siguiente.
9. **Decisión — ¿el prospecto contestó?** Si sí, Ariel sigue la conversación; si
   llega a precio o condiciones, entra Jani. Si no, no hay criterio de cuándo
   insistir y muchos no se retoman nunca.
10. **Fin.** No hay registro consolidado de a quién se contactó, cuándo, ni qué
    pasó. El trabajo se pierde cuando Ariel está ocupado en otra cosa.

### 3.2 To-be

1. **Inicio:** Ariel crea una campaña de búsqueda con criterios explícitos: rubro,
   zona, tamaño aproximado y qué producto de Unter le calzaría.
2. El sistema recorre las fuentes abiertas configuradas y trae empresas
   candidatas con razón social, dirección, teléfono, sitio web y rubro.
3. El sistema busca los contactos de cada empresa y captura nombre, cargo, correo
   y teléfono.
4. Por cada dato capturado, el sistema guarda de dónde salió y en qué fecha.
5. El sistema deduplica contra la base. **Decisión — ¿la empresa ya existe?** Si
   ya está como cliente o prospecto trabajado, se marca como existente con la
   fecha del último contacto y sale de la campaña.
6. El sistema califica al prospecto y le asigna un puntaje.
7. **Decisión — ¿supera el umbral?** Si no, queda archivado con el motivo, sin
   borrarse. Si sí, entra a la lista propuesta.
8. El sistema entrega la lista calificada a Ariel, con datos completos y un
   mensaje sugerido por prospecto.
9. Ariel revisa: aprueba, corrige o saca prospectos.
10. **Ariel envía el primer contacto a mano**, prospecto por prospecto. El sistema
    no envía nada por su cuenta.
11. Ariel marca en el sistema qué prospectos contactó y en qué fecha. Paso manual
    y punto frágil del flujo: si no se marca, el seguimiento del paso 12 no
    ocurre. Por eso el registro debe ser de un solo clic (RNF-06).
12. **Evento de tiempo:** vencido el plazo sin respuesta, el sistema le recuerda a
    Ariel el segundo intento, hasta el máximo definido. Agotados los intentos, el
    prospecto queda en frío con su historial.
13. **Decisión — ¿respondió?** Si sí, se convierte en oportunidad.
14. **Decisión — ¿de qué es la respuesta?** Precio, cotización o condiciones la
    trabaja Jani. Producto, ficha o especificación, y cualquier otro caso, Ariel.
15. Quien la trabajó registra el resultado: cerrada, perdida, o en seguimiento con
    fecha.
16. El sistema usa ese resultado para ajustar el puntaje de la próxima campaña.
17. **Subproceso en paralelo:** si un contacto pide no ser contactado, Ariel lo
    marca y el sistema bloquea a esa empresa para toda campaña futura.
18. **Fin.**

---

## 4. Flujo 3 — Documentación técnica del catálogo

### 4.1 As-is

1. **Inicio:** entra un producto nuevo al catálogo, o el proveedor actualiza uno
   existente.
2. El proveedor manda su ficha, su catálogo o un correo con especificaciones.
   Normalmente le llega a Jani, que tiene el trato con proveedores.
3. Jani guarda el documento donde le llegó: el correo se queda en el correo, el
   PDF en una carpeta o como adjunto.
4. **Decisión — ¿ese documento se sube a algún lugar compartido?** Hoy ese paso no
   existe. El documento queda donde cayó.
5. El producto entra al catálogo identificado con el nombre que le puso el
   proveedor. No hay ficha propia ni código propio.
6. **El proceso queda detenido indefinidamente** hasta que alguien lo necesite.
7. **Disparador tardío:** un cliente pide la especificación de ese producto.
8. Ariel o Jani busca el documento en el correo o en la carpeta.
9. **Decisión — ¿lo encuentra?** Si sí, se lo reenvía al cliente **tal cual**, con
   el logo, la marca y a menudo los datos de contacto del proveedor impresos
   encima: Unter le entrega al cliente el camino directo a su proveedor. Si no, se
   le pide al proveedor y se espera, con el mismo tiempo muerto del flujo 1.
10. Se responde al cliente.
11. Nada se convierte en activo de Unter. El producto sigue sin ficha propia y a
    la siguiente consulta se repite desde el paso 8.
12. **Fin.**

### 4.2 To-be

1. **Dos formas de iniciar.** Por calendario: el sistema recorre periódicamente el
   inventario buscando productos sin ficha propia aprobada. Por evento: cuando el
   flujo 1 marca un producto como "documentación incompleta", o cuando se carga un
   producto nuevo.
2. El sistema arma la cola y la ordena por prioridad: primero los productos más
   cotizados y los que ya provocaron una consulta no resuelta.
3. Por cada producto, el sistema revisa qué material existe: ficha del proveedor,
   página de catálogo, correo con especificaciones, fotos.
4. **Decisión — ¿hay material suficiente?** Si no, se genera una solicitud al
   proveedor, que toma Jani por ser trato con proveedor; **evento de tiempo:** si
   el proveedor no responde en el plazo, el producto queda marcado como "sin
   respaldo", no se le publica ficha y se avisa a Ariel para que decida si lo
   mantiene en catálogo. Si sí hay material, continúa.
5. El sistema extrae los atributos técnicos y los normaliza al formato de Unter:
   unidades, nomenclatura, campos obligatorios.
6. El sistema asigna o valida el código interno según la regla de codificación de
   Unter.
7. **Decisión — ¿el código choca con uno existente?** Si sí, se levanta un
   conflicto que resuelve Ariel antes de seguir. Esto evita el mismo producto
   cargado dos veces con nombres distintos.
8. El sistema genera el borrador de ficha con la identidad visual de Unter,
   marcando el origen de cada dato.
9. Ariel revisa y valida. **Ninguna ficha se publica sin aprobación humana:** con
   la marca de Unter, el error deja de ser del proveedor.
10. **Decisión — ¿Ariel aprueba?** Si rechaza, vuelve al paso 5 con la corrección
    anotada, y el sistema la aplica a los productos siguientes del mismo proveedor.
11. Ficha aprobada: se publica y se vincula al producto en el inventario. Queda
    disponible para cotizaciones, para el catálogo y **como fuente de respuesta
    del asistente del flujo 1**.
12. El sistema guarda la ficha con número de versión y fecha.
13. **Evento posterior:** si el proveedor cambia la especificación, la ficha se
    marca como desactualizada y vuelve a la cola del paso 2.
14. **Fin.** Unter deja de reenviar documentos del proveedor y entrega documentos
    propios.

---

## 5. Mejoras que introduce el SIG

**Qué se automatiza.** La recepción y clasificación de consultas, la búsqueda de
la respuesta en la base, la recopilación y deduplicación de prospectos, y la
extracción y normalización de atributos técnicos. Son las tareas que hoy consumen
tiempo de dos personas sin agregar criterio.

**Qué se controla.** Los plazos de las derivaciones, el seguimiento de prospectos,
la ausencia de documentación por producto, y las colisiones de código. En el as-is
ninguno de estos tiene control: dependen de que alguien se acuerde.

**Qué se mide.** Consultas recibidas y resueltas, tiempo de respuesta promedio y
peor caso, consultas no resueltas por falta de dato, cobertura de fichas propias
sobre el catálogo, prospectos contactados y tasa de conversión. Ninguno de estos
indicadores existe hoy, y son la línea base para los KPI de la Entrega 2.

**Qué no cambia.** Las decisiones comerciales y técnicas siguen siendo humanas: el
sistema prepara, prioriza y registra, pero Ariel aprueba las fichas y envía los
contactos, y Jani mantiene la relación con los proveedores.

---

## 6. Diagramas

Los diagramas BPMN 2.0 correspondientes a estos seis flujos se exportan a
`assets/`. Nomenclatura:

```
assets/flujo1-asis.png    assets/flujo1-tobe.png
assets/flujo2-asis.png    assets/flujo2-tobe.png
assets/flujo3-asis.png    assets/flujo3-tobe.png
```
