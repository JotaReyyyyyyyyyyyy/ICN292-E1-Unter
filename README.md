# ICN292 — Entrega 1: Propuesta de SIG para Unter

Repositorio de la Entrega 1 del proyecto de **ICN-292, Sistemas de Información
para la Gestión** (Universidad Técnica Federico Santa María, Campus Vitacura,
segundo semestre 2026).

---

## 1. Qué PYME y qué problema

**Unter** es una empresa pequeña de **intermediación y distribución**: no fabrica,
revende productos de terceros. La operación completa recae en **dos personas**.

El problema de gestión de información se manifiesta en tres frentes que están
conectados entre sí:

1. **Las consultas de clientes no tienen entrada única ni registro.** Llegan por
   WhatsApp, correo o teléfono a quien el cliente tenga guardado. Nadie sabe
   cuántas llegan, cuánto se demoran en responderse ni cuántas se pierden.
2. **La búsqueda de clientes nuevos es manual y sin memoria.** No hay registro
   de a quién se contactó, lo que produce trabajo repetido y contactos duplicados.
3. **Unter no tiene documentación técnica propia.** Cuando un cliente pide una
   especificación, se le reenvía el PDF del proveedor tal cual, con la marca y los
   datos de contacto del proveedor impresos. Esto es un riesgo de
   **desintermediación**: Unter le entrega al cliente el camino directo a su
   proveedor.

## 2. Objetivo del SIG propuesto

Centralizar la información que hoy vive dispersa en correos, WhatsApp y la memoria
de dos personas, y convertirla en un activo consultable. Se proponen tres
componentes de un mismo sistema:

| # | Componente | Qué resuelve |
|---|---|---|
| 1 | Asistente de IA con chat integrado | Entrada única de consultas; responde desde la base y deriva con contexto cuando no puede |
| 2 | Sistema de búsqueda de clientes | Recopila y califica prospectos con trazabilidad de origen; entrega la lista a revisión humana |
| 3 | Automatización de fichas técnicas | Genera documentación con marca propia de Unter a partir del material del proveedor |

Los tres se cierran entre sí: el componente 1 detecta qué información falta, el
componente 3 la produce, el componente 1 la usa para responder, y el componente 2
trae los clientes que entran por el componente 1.

## 3. Qué hay en cada carpeta

```
README.md                  este archivo
docs/
  00-caso-pyme.md          identificacion de Unter, evidencia, problema y objetivo
  01-requerimientos.md     actores, alcance, requisitos funcionales y no funcionales
  02-bpmn.md               (pendiente) flujos as-is y to-be de los tres procesos
  03-er-preliminar.md      (pendiente) modelo entidad-relacion preliminar
assets/                    diagramas BPMN y ER exportados como imagen
informe/                   informe final en PDF y en LaTeX (.tex)
```

La carpeta `informe/` contiene la versión entregable del informe. Es la misma que
se sube a Aula.

## 4. Relación con la Entrega 2

La Entrega 2 implementa en localhost el sistema diseñado acá. Este repositorio
continúa en uso: el código de la E2 se suma a estas mismas carpetas, de modo que
el diagnóstico y la implementación queden en un solo historial. El stack tentativo
está documentado en el informe (sección de arquitectura lógica).

## 5. Equipo

**Grupo 2 · Paralelo 100**

| Nombre completo | Rol USM | RUT | Rol en el equipo | Usuario GitHub |
|---|---|---|---|---|
| Juan Pablo Godoy Fajardo | 202360577-8 | 21.633.620-8 | Evidencia del caso y gobernanza de datos | _pendiente_ |
| Vicente Soto Martínez | 202204651-1 | 21.270.145-9 | Interfaz y tablero de KPI | _pendiente_ |
| Joaquín Tapia Mandiola | 202260674-6 | 21.407.361-7 | Modelo de datos y automatización | JotaReyyyyyyyyyyyy |
| Claudio Vera Avila | 202104575-9 | 22.231.192-6 | Base de datos y reproducibilidad | _pendiente_ |

Los roles en el equipo corresponden al reparto de hitos del plan de Entrega 2
(sección 7 del informe) y se confirman en la primera reunión posterior a la E1.

## 6. Asignatura

- **Ramo:** ICN-292, Sistemas de Información para la Gestión
- **Profesores:** José Miguel González Paul · José Luis Sáez Tamayo
- **Campus:** Vitacura, USM
- **Período:** Segundo semestre 2026
- **Cierre Entrega 1:** sábado 12 de septiembre de 2026, 12:30
