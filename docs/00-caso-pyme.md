# 00 — Caso PYME: Unter

> **Estado del documento.** Completo. Los indicadores provienen de la entrevista
> formal al encargado, que está grabada y transcrita en este mismo repositorio
> (ver sección 2). Ningún dato de este documento es inventado: el enunciado
> prohíbe fabricar evidencia y la rúbrica lo sanciona de inmediato.

---

## 1. Identificación de la PYME

| Campo | Valor |
|---|---|
| Nombre legal | Unter SpA |
| Nombre comercial | Unter |
| Rubro | Comercializadora y distribuidora de equipos de protección personal (EPP), herramientas de mano y equipos eléctricos |
| Tamaño | **2 personas** (microempresa) |
| Ubicación | La Concepción 81, Providencia, Santiago |

> El RUT y los años de operación no se piden en la estructura obligatoria del
> informe (§3.1.B del enunciado: nombre, rubro, tamaño y ubicación). Quedan
> fuera a propósito, no por falta de dato inventable.

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

- [x] **Entrevista al encargado, grabada y transcrita** — la vía elegida
- [ ] Presencia pública verificable — no se usó como ancla
- [ ] Unidad operativa de una organización mayor — no aplica

**Entrevista.** Tras una primera conversación informal de levantamiento, se
realizó la entrevista formal al encargado, **grabada en audio y transcrita
íntegramente**. Ambos archivos están en el repositorio:

| Archivo | Qué es |
|---|---|
| `informe/ICN292_P100_AudioEntrevista.mp4` | grabación completa de la entrevista |
| `informe/ICN292_P100_TranscripcionEntrevista.txt` | transcripción íntegra del audio |

La pauta que se usó para conducirla está en `04-pauta-entrevista.md`. De esa
entrevista salen los indicadores de la sección 3 y la tabla de línea base del
informe.

## 3. Problema de negocio

**En una frase:** Unter no tiene dónde guardar ni cómo consultar la información
que su propia operación genera, de modo que cada consulta de un cliente se
resuelve desde cero, y la documentación técnica que entrega es la del proveedor,
lo que expone a la empresa a la desintermediación.

El problema se descompone en tres procesos, modelados as-is y to-be en
`02-bpmn.md`:

### 3.1 Atención de consultas sin entrada única ni registro

Las consultas entran por WhatsApp, correo o teléfono, a quien el cliente tenga
guardado de antes. Quien recibe busca la respuesta en su propio correo o le
pregunta al otro; si nadie la tiene, se le escribe al proveedor y se espera. Nada
queda registrado, así que la misma consulta se vuelve a resolver desde cero cuando
reaparece.

**Impacto medible:**

- Consultas recibidas al mes: **12**
- Tiempo promedio de respuesta: **30 min**
- Peor tiempo de respuesta: **no responder**
- Consultas al mes que se atrasan o se pierden por falta de datos: **0**

### 3.2 Prospección manual y sin memoria

La búsqueda de clientes nuevos es reactiva y la ejecuta Ariel a mano. No hay
registro de a quién se contactó ni cuándo, lo que produce trabajo repetido y, en
algunos casos, contactos duplicados a la misma empresa.

**Impacto medible:**

- Tiempo en armar el listado de prospectos: **1,5 h** por cada 5 clientes
- Visita en terreno a esos 5 clientes: **5 a 6 h** del día siguiente
- Frecuencia con que ocurre: **todos los días**
- Gasto en campañas publicitarias: **$400.000 a $600.000 al mes**, sin buen retorno
- Contactos útiles que llegan por ese canal: **2 de cada 10**
- Registro existente de contactos previos: **no**, se trabaja con Google y planillas sueltas

### 3.3 Ausencia de documentación técnica propia

Unter no genera fichas técnicas. Cuando un cliente pide una especificación, se le
reenvía el documento del proveedor con su marca y sus datos de contacto. Además de
perder la oportunidad de posicionar la marca propia, el proveedor queda a un clic
del cliente.

**Impacto medible:**

- Productos distintos en catálogo: **3000** en la línea general (**70** activos hoy)
- Productos con ficha propia de Unter: **0**
- Tiempo en elaborar una ficha a mano: **15 a 20 min** por producto
- Herramientas que usan para armarla: Canva, Word, PDF o PowerPoint, indistintamente

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
