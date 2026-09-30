# 09 — Control de asistencia

Explicación del wireframe [`09-control-asistencia.puml`](../09-control-asistencia.puml).

| | |
|---|---|
| **Pantalla** | P13 — Control de asistencia |
| **Rol** | Instructor (Administrador sobre cualquier clase) |
| **Historias** | HU-04, HU-17 |
| **Caso de uso** | CU-12 |
| **Requisitos** | RF15, RF16, RF17, RF27, RNF07 |
| **Detalle funcional** | [`docs/diseño-ui.md`](../../../docs/diseño-ui.md), pantalla P13 |

![Wireframe P13 — Control de asistencia](../09-control-asistencia.png)

---

## Qué representa

Es la pantalla más condicionada por su contexto de uso. La restricción RE05 establece que se opera desde una tablet, de pie, en el salón y con poco tiempo, sobre el wifi del establecimiento. Cada decisión del boceto responde a eso. Es además la única que registra un dato que hoy no existe en la organización: la asistencia.

## Recorrido del boceto

| Zona del boceto | Qué es | Por qué está |
|---|---|---|
| Encabezado reducido | Barra simplificada | Solo el sistema, el instructor y la salida. No hay menú lateral: en la tablet se contrae. |
| Franja de la clase | Contexto | Disciplina, día, horario y fecha. Evita cargar la asistencia en la clase equivocada. |
| Contadores Presentes / Ausentes / Justificados | Resumen en vivo | Permiten controlar el avance sin recorrer la lista. |
| Botón Marcar todos presentes | Acción masiva | Responde al caso más frecuente: en una clase de doce suele faltar uno o dos. |
| Campo Agregar alumno | Búsqueda | Permite registrar a un alumno activo que se presenta sin estar inscripto. |
| Tres botones por alumno | Selección de estado | Presente, Ausente y Justificado, en botones grandes. No hay casillas chicas ni desplegables: se opera con el dedo. |
| Marca (OCA) | Indicador | Señala al asistente ocasional, el alumno no inscripto en esa clase. Corresponde al requisito RF27 y a la decisión D7. |
| Botón CONFIRMAR ASISTENCIA | Acción única | Una sola confirmación para toda la clase, destacada y al pie. |

## Decisiones que el boceto hace visibles

- **Botones grandes en lugar de casillas.** Viene del requisito RNF07 y de la restricción RE05: la pantalla se usa con el dedo, de pie y sin precisión.
- **Marcar todos y corregir las excepciones son dos toques en lugar de doce.** El diseño optimiza el caso habitual, no el teórico.
- **Una sola confirmación para toda la clase.** Con conectividad inestable, doce guardados individuales multiplican las probabilidades de que alguno falle sin que el instructor se entere.
- **El asistente ocasional está previsto desde el boceto.** El modelo preliminar no podía representarlo: toda asistencia derivaba de una inscripción. La decisión D7 cambió eso, y acá se ve la consecuencia.

## Lo que este boceto no define

Un wireframe define qué elementos tiene la pantalla y cómo se organizan. No define colores, tipografías, iconos ni espaciados exactos: eso corresponde a la etapa de implementación. Tampoco muestra los estados alternativos de la pantalla —errores, listas vacías, cargas en curso—, que están descriptos en `docs/diseño-ui.md`. No están dibujados el estado posterior a la confirmación ni el comportamiento ante una pérdida de conexión, que en esta pantalla es el escenario de falla más probable.

---

<sub>Escuela Superior de Comercio N° 49 "Justo José de Urquiza" — Desarrollo Web / Analista Funcional de Sistemas — Grupo 02 — 2026.</sub>
