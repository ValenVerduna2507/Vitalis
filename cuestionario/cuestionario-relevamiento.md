# Cuestionario de relevamiento

Sistema de Gestión Integral — Vitalis Centro de Entrenamiento
Equipo: Grupo 02
Versión: 1.0

---

## Contenido de esta carpeta

| Archivo | Contenido |
|---|---|
| `cuestionario-relevamiento.md` | Este documento. El instrumento: qué se preguntó, a quién y por qué. |
| `respuestas-relevamiento.md` | Las respuestas obtenidas en cada entrevista. |
| `hallazgos.md` | Qué se concluyó del relevamiento y en qué requisito se convirtió cada hallazgo. |

---

## Índice

- [1. Objetivo del relevamiento](#1-objetivo-del-relevamiento)
- [2. Técnicas utilizadas](#2-técnicas-utilizadas)
- [3. Planificación de las entrevistas](#3-planificación-de-las-entrevistas)
- [4. Criterios de redacción de las preguntas](#4-criterios-de-redacción-de-las-preguntas)
- [5. Bloque general](#5-bloque-general)
- [6. Entrevista a la propietaria y directora](#6-entrevista-a-la-propietaria-y-directora)
- [7. Entrevista a la recepcionista](#7-entrevista-a-la-recepcionista)
- [8. Entrevista a los instructores](#8-entrevista-a-los-instructores)
- [9. Entrevista a la nutricionista](#9-entrevista-a-la-nutricionista)
- [10. Encuesta breve a alumnos](#10-encuesta-breve-a-alumnos)
- [11. Guía de análisis documental](#11-guía-de-análisis-documental)
- [12. Guía de observación directa](#12-guía-de-observación-directa)

---

## 1. Objetivo del relevamiento

Comprender cómo funciona hoy la gestión administrativa de Vitalis, con qué herramientas se lleva adelante y qué problemas concretos genera, para poder especificar un sistema que resuelva esos problemas y no otros.

### Preguntas que el relevamiento debía responder

| # | Pregunta de investigación |
|---|---|
| Q1 | ¿Qué procesos administrativos existen y quién los ejecuta? |
| Q2 | ¿Qué herramientas se usan hoy y qué limitaciones tienen? |
| Q3 | ¿Cuáles son las reglas de negocio que el sistema debe respetar? |
| Q4 | ¿Qué información necesita cada actor y hoy no consigue? |
| Q5 | ¿Qué errores ocurren con la operatoria actual y con qué frecuencia? |
| Q6 | ¿Qué restricciones condicionan la solución? |

---

## 2. Técnicas utilizadas

| Técnica | Cuándo se aplicó | Por qué |
|---|---|---|
| **Entrevista semiestructurada** | Con cada stakeholder interno. | Permite seguir una guía de preguntas pero desviarse cuando aparece información relevante no prevista. |
| **Análisis documental** | Sobre las dos planillas de cálculo provistas por el centro. | Los datos reales muestran cosas que nadie menciona en una entrevista, como la cantidad de bajas o la existencia de dos versiones de grilla. |
| **Observación directa** | Durante una jornada en el mostrador y en una clase. | Permite ver el flujo real de trabajo, que no siempre coincide con el que la persona describe. |
| **Encuesta breve** | A una muestra de alumnos. | Los alumnos son muchos y no se los puede entrevistar uno por uno. |

### Por qué se combinaron las cuatro

Cada técnica tiene un punto ciego. En la entrevista, la persona cuenta lo que recuerda y lo que considera importante. En la planilla están los datos pero no el motivo. En la observación se ve lo que pasa pero no lo que la persona piensa. La encuesta da volumen pero poca profundidad.

Un ejemplo concreto de este proyecto: en la entrevista la recepcionista dijo que las bajas de alumnos "no son tantas". Al revisar la planilla aparecieron 78 registros dados de baja sobre 214 activos. Ninguna de las dos fuentes por separado hubiera dado la dimensión real del problema P2.

---

## 3. Planificación de las entrevistas

| # | Entrevistado | Rol | Fecha | Duración | Modalidad |
|---|---|---|---|---|---|
| E1 | Andrea Sosa | Propietaria y directora | 16/03/2026 | 75 min | Presencial |
| E2 | Julieta Ferreyra | Recepcionista, turno tarde | 19/03/2026 | 60 min | Presencial |
| E3 | Martín López | Instructor de Funcional y Full Body | 23/03/2026 | 30 min | Presencial |
| E4 | Sofía Martínez | Instructora de Yoga y GAP | 23/03/2026 | 25 min | Presencial |
| E5 | Paula Ibarra | Nutricionista | 31/03/2026 | 45 min | Presencial |
| E6 | Andrea Sosa | Segunda entrevista, cierre de dudas | 10/04/2026 | 40 min | Videollamada |

**Observación directa:** 26/03/2026, jornada completa en el mostrador y presencia en una clase de Funcional.

**Encuesta a alumnos:** entre el 30/03/2026 y el 03/04/2026. 34 respuestas sobre 214 alumnos activos.

### Organización del equipo

En cada entrevista participaron dos integrantes: uno conducía y el otro tomaba nota. El rol rotó para que los cuatro estuvieran presentes en al menos dos entrevistas. Las notas se pasaron en limpio dentro de las 24 horas y se validaron entre ambos participantes antes de incorporarlas al documento de respuestas.

---

## 4. Criterios de redacción de las preguntas

| Criterio | Qué significa |
|---|---|
| **Abiertas primero, cerradas después** | Se empieza pidiendo que la persona describa con sus palabras y recién después se pregunta por datos puntuales. |
| **Sin lenguaje técnico** | Ninguna pregunta menciona base de datos, entidad, requisito ni módulo. |
| **Sin inducir la respuesta** | Se pregunta "¿cómo hacen hoy para saber quién debe?" y no "¿les gustaría un reporte de morosos?". |
| **Una pregunta por vez** | Nada de "¿cómo cobran y cómo controlan la asistencia?". |
| **Pedir el caso concreto** | Ante una respuesta general se repregunta por el último caso real que ocurrió. |
| **Buscar la excepción** | Después de entender el caso normal se pregunta qué pasa cuando algo sale distinto. |

### Preguntas de repregunta habituales

Estas se usaron a lo largo de todas las entrevistas cuando una respuesta quedaba vaga:

- ¿Me podés contar la última vez que pasó eso?
- ¿Cada cuánto sucede?
- ¿Qué hacen cuando ocurre?
- ¿Quién se entera cuando pasa?
- ¿Y si eso falla, qué pasa?

---

## 5. Bloque general

Preguntas formuladas a todos los entrevistados internos, al inicio de cada entrevista.

| # | Pregunta |
|---|---|
| G1 | Contame con tus palabras qué hacés en el centro en un día normal. |
| G2 | ¿Qué herramientas usás para trabajar? |
| G3 | ¿Qué información necesitás para hacer tu tarea y hoy te cuesta conseguir? |
| G4 | ¿Qué es lo que más tiempo te lleva y sentís que podría ser más rápido? |
| G5 | ¿Qué cosas se anotan en papel? |
| G6 | ¿Cuál fue el último problema que tuviste con la información del centro? |
| G7 | Si pudieras cambiar una sola cosa de cómo se trabaja hoy, ¿cuál sería? |

---

## 6. Entrevista a la propietaria y directora

### 6.1 — Sobre el negocio

| # | Pregunta |
|---|---|
| D1 | ¿Cuántos alumnos tiene el centro actualmente y cuántos pasaron alguna vez? |
| D2 | ¿Qué disciplinas se dictan y en qué turnos? |
| D3 | ¿Cuántos instructores trabajan y cómo se les asignan las clases? |
| D4 | ¿Cómo se organiza el horario de clases? ¿Cambia durante el año? |
| D5 | ¿Qué servicios complementarios ofrece el centro además de las clases? |
| D6 | ¿El centro tiene una sola sede o hay planes de abrir otra? |

### 6.2 — Sobre el cobro

| # | Pregunta |
|---|---|
| D7 | ¿Cómo se le cobra a un alumno? ¿Todos pagan igual? |
| D8 | Si un alumno hace dos actividades, ¿cómo se calcula lo que paga? |
| D9 | ¿Qué pasa con el alumno que viene salteado y no todos los meses? |
| D10 | ¿Cada cuánto se actualizan los precios? ¿Quién lo decide? |
| D11 | Cuando sube un precio, ¿qué pasa con lo que ya se cobró? |
| D12 | ¿Qué medios de pago se aceptan? |
| D13 | ¿A partir de cuándo considerás que un alumno está atrasado con la cuota? |
| D14 | ¿Ese plazo es fijo o depende del alumno? |
| D15 | ¿Cómo hacés hoy para saber quién debe? |
| D16 | ¿Qué hacés cuando detectás a alguien atrasado? |

### 6.3 — Sobre la información de gestión

| # | Pregunta |
|---|---|
| D17 | ¿Qué información necesitás para decidir si abrir o cerrar un horario? |
| D18 | ¿Sabés hoy cuánto factura el centro por disciplina? |
| D19 | ¿Cómo te enterás de que un alumno dejó de venir? |
| D20 | ¿Te interesaría enterarte antes de que pida la baja? |
| D21 | ¿Qué información te gustaría tener y hoy no tenés de ninguna manera? |

### 6.4 — Sobre el personal y los accesos

| # | Pregunta |
|---|---|
| D22 | ¿Quién puede ver la planilla de alumnos hoy? |
| D23 | ¿Hay información que preferirías que no vea todo el mundo? |
| D24 | ¿Los instructores necesitan ver algo del sistema? |
| D25 | ¿Qué pasa cuando entra o se va un empleado? |
| D26 | ¿Alguna vez tuviste un problema por un dato modificado sin que supieras quién lo hizo? |

### 6.5 — Sobre el alcance

| # | Pregunta |
|---|---|
| D27 | ¿El sistema tendría que ocuparse de la facturación? |
| D28 | ¿Y de la contabilidad del negocio? |
| D29 | ¿Necesitás que los alumnos puedan pagar online? |
| D30 | ¿Querrías mandar avisos masivos a los alumnos? |
| D31 | ¿Hay algo que definitivamente NO querés que el sistema haga? |

---

## 7. Entrevista a la recepcionista

### 7.1 — Sobre el alta de alumnos

| # | Pregunta |
|---|---|
| R1 | Cuando llega alguien nuevo, ¿qué hacés paso a paso? |
| R2 | ¿Qué datos le pedís? |
| R3 | ¿Cuáles son imprescindibles y cuáles podés completar después? |
| R4 | ¿Cuánto te lleva anotar a un alumno nuevo? |
| R5 | ¿Te pasó de anotar dos veces a la misma persona? ¿Cómo te diste cuenta? |
| R6 | ¿Qué hacés si viene un menor de edad? |
| R7 | ¿Qué pasa cuando alguien se anota y después no vuelve nunca? |

### 7.2 — Sobre el cobro

| # | Pregunta |
|---|---|
| R8 | Contame cómo cobrás una cuota, desde que la persona llega hasta que se va. |
| R9 | ¿Cómo sabés cuánto le corresponde pagar? |
| R10 | ¿Dónde anotás el pago? |
| R11 | ¿Qué hacés si el alumno dice que ya pagó y en la planilla no figura? |
| R12 | ¿Te pasó de cargar mal un pago? ¿Cómo lo corregiste? |
| R13 | ¿Cuántos cobros hacés en un día de mucho movimiento? |
| R14 | ¿En qué momento del mes se junta más gente a pagar? |

### 7.3 — Sobre las consultas de los alumnos

| # | Pregunta |
|---|---|
| R15 | ¿Qué te preguntan más seguido los alumnos? |
| R16 | ¿Cuánto tardás en responder si alguien está al día? |
| R17 | ¿Cómo buscás a un alumno en la planilla? |
| R18 | ¿Qué pasa si no recordás el apellido exacto? |
| R19 | ¿Te preguntan por los horarios de las clases? ¿De dónde sacás esa información? |

### 7.4 — Sobre las bajas y reincorporaciones

| # | Pregunta |
|---|---|
| R20 | ¿Qué hacés cuando un alumno avisa que deja de venir? |
| R21 | ¿Borrás el registro o lo dejás? |
| R22 | ¿Anotás el motivo por el que se va? |
| R23 | ¿Qué pasa si esa persona vuelve a los seis meses? |
| R24 | ¿Le volvés a pedir todos los datos? |

### 7.5 — Sobre la herramienta actual

| # | Pregunta |
|---|---|
| R25 | ¿Qué es lo que más te molesta de trabajar con la planilla? |
| R26 | ¿Y qué es lo que te resulta cómodo y no querrías perder? |
| R27 | ¿Alguna vez se te borró o se te desconfiguró algo? |
| R28 | Si el sistema nuevo te pidiera más pasos que la planilla para cobrar, ¿qué harías? |

---

## 8. Entrevista a los instructores

| # | Pregunta |
|---|---|
| I1 | ¿Cómo sabés qué clases tenés asignadas esta semana? |
| I2 | ¿Cómo te enterás si cambia tu horario? |
| I3 | ¿Sabés quiénes están anotados en tu clase antes de empezar? |
| I4 | ¿Tomás lista? ¿Cómo? |
| I5 | ¿Qué hacés con esa lista después? |
| I6 | ¿Alguien te la pide alguna vez? |
| I7 | ¿Qué pasa si viene un alumno que no está en tu lista? |
| I8 | ¿Cada cuánto pasa eso? |
| I9 | ¿Te llevaría mucho tiempo marcar la asistencia en una tablet? |
| I10 | ¿Hay wifi en el salón? ¿Anda bien? |
| I11 | ¿Qué información te gustaría tener de tus alumnos? |
| I12 | ¿Hay algo que preferirías no tener que hacer vos? |

---

## 9. Entrevista a la nutricionista

### 9.1 — Sobre la consulta

| # | Pregunta |
|---|---|
| N1 | ¿Cómo llega un alumno a tu consultorio? |
| N2 | ¿Cuántos pacientes atendés por semana? |
| N3 | Contame qué hacés en una consulta, paso a paso. |
| N4 | ¿Qué datos registrás de cada paciente? |
| N5 | ¿Cuáles registrás siempre y cuáles solo a veces? |
| N6 | ¿En qué unidad anotás cada medida? |
| N7 | ¿Hay algún valor que calcules a mano? |
| N8 | ¿Dónde anotás todo eso? |
| N9 | ¿Qué pasa si perdés una ficha? |

### 9.2 — Sobre el seguimiento

| # | Pregunta |
|---|---|
| N10 | ¿Cómo comparás la evolución de un paciente entre consultas? |
| N11 | ¿Qué mirás primero cuando vuelve un paciente? |
| N12 | ¿Le mostrás los números al paciente? |
| N13 | ¿Le entregás algo por escrito? |

### 9.3 — Sobre la confidencialidad

| # | Pregunta |
|---|---|
| N14 | ¿Quién más puede ver las fichas de tus pacientes hoy? |
| N15 | ¿Te parecería bien que la recepción pudiera verlas? |
| N16 | ¿Y la directora del centro? |
| N17 | ¿Atendés menores de edad? |
| N18 | ¿Necesitás alguna autorización de los padres? |
| N19 | ¿Cómo la registrás hoy? |

---

## 10. Encuesta breve a alumnos

Cuestionario autoadministrado, entregado en recepción. Anónimo, seis preguntas.

| # | Pregunta | Tipo |
|---|---|---|
| A1 | ¿Hace cuánto entrenás en Vitalis? | Menos de 3 meses / 3 a 12 meses / Más de 1 año |
| A2 | ¿Alguna vez dejaste de venir y después volviste? | Sí / No |
| A3 | ¿Sabés siempre si tenés la cuota al día? | Sí / No / A veces |
| A4 | Cuando querés saber algo de tu cuenta, ¿cómo lo averiguás? | Abierta |
| A5 | ¿Alguna vez te confundieron un pago o te reclamaron algo ya abonado? | Sí / No |
| A6 | Si pudieras consultar desde el celular tu estado de cuenta y tus horarios, ¿lo usarías? | Sí / No / Tal vez |

---

## 11. Guía de análisis documental

Preguntas que el equipo se hizo al revisar las dos planillas provistas por el centro.

### 11.1 — Planilla de alumnos y cuotas

| # | Qué se buscó verificar |
|---|---|
| P1 | ¿Cuántos registros hay en total y cuántos corresponden a alumnos activos? |
| P2 | ¿Qué campos se registran de cada alumno? |
| P3 | ¿Hay campos vacíos de forma sistemática? ¿Cuáles? |
| P4 | ¿Existen registros duplicados? ¿Con qué criterio se pueden detectar? |
| P5 | ¿Cómo se representa que un alumno se dio de baja? |
| P6 | ¿Qué modalidades de cobro aparecen efectivamente en los datos? |
| P7 | ¿Cómo se registra un pago? ¿Una columna por mes o una fila por pago? |
| P8 | ¿Se puede saber la fecha exacta de cada pago o solo el mes? |
| P9 | ¿Hay fórmulas que calculen algo automáticamente? |
| P10 | ¿Hay datos que estén escritos de más de una forma? |

### 11.2 — Planilla de planificación de actividades

| # | Qué se buscó verificar |
|---|---|
| P11 | ¿Cuántas hojas tiene el archivo y qué contiene cada una? |
| P12 | ¿Hay más de una versión de la grilla? ¿Cómo se distinguen? |
| P13 | ¿Qué datos se registran de cada clase? |
| P14 | ¿Cómo se indica el instructor a cargo? |
| P15 | ¿Hay clases con el mismo instructor en el mismo horario? |
| P16 | ¿Se registra quién está inscripto en cada clase? |
| P17 | ¿Hay algún registro de asistencia? |

---

## 12. Guía de observación directa

Puntos a observar durante la jornada en el centro, sin intervenir.

### 12.1 — En el mostrador

| # | Qué observar |
|---|---|
| O1 | Cuántas veces se abre la planilla en una hora. |
| O2 | Cuánto tarda un cobro desde que llega el alumno hasta que se va. |
| O3 | Qué preguntas hacen los alumnos y cuánto tarda la respuesta. |
| O4 | Si la recepcionista usa algún atajo o anotación paralela no mencionada en la entrevista. |
| O5 | Qué pasa cuando hay más de una persona esperando. |
| O6 | Si se producen errores de tipeo y cómo se corrigen. |

### 12.2 — En el salón

| # | Qué observar |
|---|---|
| O7 | Cómo empieza la clase y en qué momento el instructor podría registrar asistencia. |
| O8 | Si el instructor tiene las manos libres o está ocupado. |
| O9 | Si hay lugar donde apoyar un dispositivo. |
| O10 | Cómo llegan los alumnos: todos juntos o escalonados. |
| O11 | Si aparece algún alumno que no estaba previsto. |
| O12 | Calidad de la señal wifi dentro del salón. |

---

## Historial de versiones

| Versión | Fecha | Cambio |
|---|---|---|
| 1.0 | 04/2026 | Versión inicial. Guía de entrevistas para cinco stakeholders, encuesta a alumnos, guía de análisis documental y guía de observación directa. |

---

<sub>Escuela Superior de Comercio N° 49 "Justo José de Urquiza" — Desarrollo Web / Analista Funcional de Sistemas — 2026.</sub>
