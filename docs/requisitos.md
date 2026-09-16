# Especificación de requisitos

Sistema de Gestión Integral — Vitalis Centro de Entrenamiento
Equipo: Grupo 02
Versión: 1.3

---

## Índice

- [1. Contexto](#1-contexto)
- [2. Problema](#2-problema)
- [3. Objetivos](#3-objetivos)
- [4. Alcance](#4-alcance)
- [5. Fuera de alcance](#5-fuera-de-alcance)
- [6. Requisitos funcionales](#6-requisitos-funcionales)
- [7. Requisitos no funcionales](#7-requisitos-no-funcionales)
- [8. Reglas de negocio](#8-reglas-de-negocio)
- [9. Restricciones](#9-restricciones)
- [10. Roles y permisos](#10-roles-y-permisos)
- [11. Dependencias](#11-dependencias)
- [12. Supuestos](#12-supuestos)
- [13. Puntos abiertos](#13-puntos-abiertos)
- [14. Matriz de trazabilidad](#14-matriz-de-trazabilidad)

---

## 1. Contexto

Vitalis es un centro de entrenamiento físico ubicado en Pueblo Esther, provincia de Santa Fe. Ofrece disciplinas para adultos y niños en dos franjas horarias y complementa su propuesta con el servicio de una nutricionista.

### 1.1 — Dimensión de la operación

| Indicador | Valor |
|---|---|
| Alumnos registrados (activos e históricos) | Más de 200 |
| Instructores y profesionales | Alrededor de 12 |
| Turno mañana | 07:00 a 12:00 |
| Turno tarde-noche | 16:00 a 22:00 |
| Consultorio nutricional | Martes de 15:00 a 18:00 |

### 1.2 — Oferta de disciplinas

| Disciplina | Perfil | Turnos |
|---|---|---|
| Full Body | Adultos | Mañana y tarde-noche |
| Funcional | Adultos | Mañana y tarde-noche |
| Rutina Personalizada | Adultos | Ambos turnos |
| GAP | Adultos | Tarde-noche |
| Yoga | Adultos | Mañana |
| Zumba | Adultos | Tarde-noche |
| Pilates | Adultos | Mañana y tarde-noche |
| Aeróbica Infantil | Niños | Tarde |
| Kids Fit & Fun | Niños | Tarde |

### 1.3 — Situación actual

Toda la gestión administrativa se realiza mediante planillas de cálculo:

| Planilla | Contenido |
|---|---|
| Registro de alumnos y cuotas | Padrón de alumnos activos e históricos, modalidades de pago y cuotas abonadas. |
| Planificación de actividades | Grilla de clases por turno, disciplinas e instructores asignados. Se relevaron dos versiones conviviendo. |

### 1.4 — Origen del pedido

La dirección del centro solicitó analizar y documentar el diseño de un sistema de información que centralice y digitalice los procesos administrativos, elimine la dependencia de las planillas manuales y brinde a cada actor una herramienta confiable, accesible y escalable.

---

## 2. Problema

Durante el relevamiento se identificaron seis condiciones que impactan directamente en el diseño del sistema.

| ID | Problema | Descripción | Impacto |
|---|---|---|---|
| **P1** | Modalidades de cobro variables | Cada alumno puede abonar una cuota mensual fija, un valor por clase asistida o una tarifa combinada según las actividades que realice. | Dificulta el control automático de morosidad y la proyección de ingresos. |
| **P2** | Alta rotación de alumnos | Un porcentaje significativo de los registros corresponde a alumnos dados de baja. | Si se eliminan los registros se pierde el historial; si no se eliminan, ensucian el padrón activo. |
| **P3** | Planificación por temporada | Los horarios de clases e instructores se reorganizan periódicamente. Se identificaron al menos dos versiones de grilla en los archivos relevados. | Las versiones anteriores se pisan o se duplican en archivos sueltos, sin trazabilidad. |
| **P4** | Ausencia de acceso por rol | Solo quien tiene acceso a las planillas puede consultar información. | Instructores y alumnos dependen de terceros para cualquier consulta. El control de asistencia es informal o inexistente. |
| **P5** | Datos sensibles sin resguardo | El seguimiento nutricional se lleva en registros físicos. | Riesgo de pérdida y de exposición de información de salud. |
| **P6** | Errores de carga y duplicación | Las planillas no tienen validaciones. | Padrón inconsistente. Un alta duplicada contamina pagos, asistencias e historial. |

---

## 3. Objetivos

### 3.1 — Objetivo general

Diseñar un sistema de información web que centralice y digitalice los procesos administrativos de Vitalis, eliminando la dependencia de las planillas manuales y brindando a cada actor una herramienta confiable, accesible y escalable, con vistas y permisos diferenciados según su rol.

### 3.2 — Objetivos específicos

| ID | Objetivo | Problemas que ataca |
|---|---|---|
| **OE1** | Unificar el padrón de alumnos en un único repositorio digital, con validación de duplicados y baja lógica. | P2, P6 |
| **OE2** | Registrar los pagos de cuotas contemplando las tres modalidades de cobro vigentes y detectar la morosidad de forma automática. | P1 |
| **OE3** | Gestionar la planificación de disciplinas, horarios e instructores admitiendo múltiples versiones de grilla. | P3 |
| **OE4** | Digitalizar el control de asistencia para que cada instructor lo registre desde el salón. | P4 |
| **OE5** | Aislar el seguimiento nutricional en un módulo de acceso restringido. | P5 |
| **OE6** | Proveer reportes de gestión a la dirección. | P1, P4 |
| **OE7** | Implementar control de acceso por roles sobre todas las funcionalidades del sistema. | P4, P5 |

---

## 4. Alcance

El sistema cubre los siguientes procesos de negocio:

| Módulo | Procesos incluidos |
|---|---|
| **M1 — Gestión de alumnos** | Altas, modificaciones, bajas lógicas, reactivaciones, búsqueda y consulta del padrón. |
| **M2 — Gestión de cuotas y pagos** | Registro de pagos con modalidades variables, seguimiento de morosidad, historial de pagos y reportes. |
| **M3 — Planificación de actividades** | Gestión de disciplinas, clases, turnos, versiones de grilla, asignación de instructores e inscripción de alumnos. |
| **M4 — Control de asistencia** | Listado de inscriptos por clase, registro de asistencia, asistentes ocasionales e historial. |
| **M5 — Seguimiento nutricional** | Registro de consultas, evolución de parámetros y acceso restringido. |
| **M6 — Gestión de instructores** | Altas, bajas, asignaciones y visualización de horarios propios. |
| **M7 — Seguridad, usuarios y roles** | Autenticación, gestión de usuarios, permisos y auditoría. |
| **M8 — Reportes de gestión** | Morosidad, ingresos y ocupación de clases. |

> Los módulos M6, M7 y M8 estaban contemplados en el alcance del relevamiento inicial pero no contaban con requisitos funcionales propios. Los requisitos que los cubren se incorporan en este documento como requisitos adicionales (RF22 a RF31) y están identificados como tales.

---

## 5. Fuera de alcance

| Elemento excluido | Motivo |
|---|---|
| Gestión contable general del negocio (libro diario, balances, liquidación de sueldos). | Excede el propósito del sistema. Vitalis lo tiene tercerizado en un estudio contable. |
| Integración con medios de pago electrónicos externos (pasarelas, débito automático, QR). | Requiere contratación de servicios de terceros. Queda para una versión posterior. |
| Comunicación masiva con alumnos (campañas, newsletters, notificaciones push). | Excluido explícitamente en el relevamiento. Queda identificada como extensión futura, viable mediante la herramienta de automatización prevista en RE11. |
| Facturación electrónica e integración con organismos fiscales. | Requiere homologación externa y excede el alcance académico. |
| Control de acceso físico al establecimiento (molinetes, tarjetas, biometría). | Requiere hardware específico. |
| Aplicación móvil nativa. | La solución se plantea como web responsiva (RNF07), lo que cubre el uso desde tablet y teléfono. |
| Gestión de stock, ventas de indumentaria o suplementos. | No forma parte de los procesos relevados. |

Estos puntos quedan fuera de la primera versión. El sistema debe diseñarse de forma modular (RNF12) para poder incorporarlos más adelante sin refactorizar lo existente.

---

## 6. Requisitos funcionales

### Convenciones

| Elemento | Significado |
|---|---|
| Prioridad Alta | Sin este requisito el módulo no cumple su propósito. |
| Prioridad Media | Necesario para la operación completa, pero puede diferirse a una iteración posterior. |
| Prioridad Baja | Mejora la operación pero no la condiciona. |
| Origen: Relevamiento | Requisito surgido del relevamiento inicial en Vitalis. |
| Origen: Adicional | Requisito incorporado por el equipo para completar el diseño. Ver nota al pie de cada tabla. |

---

### 6.1 — Módulo 1: Gestión de alumnos

| ID | Requisito | Prioridad | Origen |
|---|---|---|---|
| **RF01** | El sistema debe permitir registrar un nuevo alumno con sus datos personales (nombre, apellido, DNI, teléfono, email) y la actividad o actividades en las que participa. | Alta | Relevamiento |
| **RF02** | El sistema debe validar que el DNI ingresado no esté duplicado antes de confirmar el alta. | Alta | Relevamiento |
| **RF03** | El sistema debe permitir modificar los datos de un alumno existente y registrar el historial de cambios. | Media | Relevamiento |
| **RF04** | El sistema debe permitir dar de baja a un alumno de forma lógica (no elimina el registro), registrando la fecha y el motivo de la baja. | Alta | Relevamiento |
| **RF05** | El sistema debe permitir reactivar un alumno dado de baja, recuperando su historial previo. | Media | Relevamiento |
| **RF24** | El sistema debe permitir buscar y listar alumnos por apellido, DNI o número de legajo, con filtros por estado, actividad y turno. | Alta | Adicional |

> **RF24 (adicional).** Incorporado por el equipo. El relevamiento define cómo se cargan los alumnos pero no cómo se los encuentra después. Con un padrón de más de 200 registros, el alta sin búsqueda no es operable. Sostiene la historia HU-09.

---

### 6.2 — Módulo 2: Gestión de cuotas y pagos

| ID | Requisito | Prioridad | Origen |
|---|---|---|---|
| **RF06** | El sistema debe registrar el pago de la cuota de un alumno indicando monto, fecha, medio de pago y período abonado. | Alta | Relevamiento |
| **RF07** | El sistema debe soportar modalidades de cobro variables por alumno: mensual fijo, por clase y tarifa combinada (por ejemplo, Pilates más Entrenamiento). | Alta | Relevamiento |
| **RF08** | El sistema debe identificar automáticamente a los alumnos con cuota vencida (más de 30 días sin pago registrado) y clasificarlos como morosos. | Alta | Relevamiento |
| **RF09** | El sistema debe permitir generar un reporte de alumnos morosos, filtrable por actividad y turno, exportable en PDF. | Alta | Relevamiento |
| **RF10** | El sistema debe mostrar el historial de pagos de cada alumno. | Media | Relevamiento |
| **RF30** | El sistema debe permitir al rol Administrador configurar el umbral de días para clasificar a un alumno como moroso, con 30 días como valor por defecto. | Media | Adicional |

> **RF30 (adicional).** Incorporado por el equipo. RF08 fija el umbral en 30 días, pero la evaluación INVEST de HU-03 identificó que ese valor es una decisión de negocio ajustable por la dirección. Parametrizarlo evita tener que modificar el código ante un cambio de criterio comercial. Corresponde a la decisión D6.

---

### 6.3 — Módulo 3: Planificación de actividades

| ID | Requisito | Prioridad | Origen |
|---|---|---|---|
| **RF11** | El sistema debe permitir crear y gestionar disciplinas (nombre, descripción, turno, días y horario). | Alta | Relevamiento |
| **RF12** | El sistema debe permitir asignar un instructor a cada clase programada. | Alta | Relevamiento |
| **RF13** | El sistema debe soportar múltiples versiones de grilla (por ejemplo, grilla regular y grilla de agosto), permitiendo activar una versión sin eliminar las anteriores. | Media | Relevamiento |
| **RF14** | El sistema debe inscribir alumnos a clases específicas, validando que el alumno esté activo. | Alta | Relevamiento |

---

### 6.4 — Módulo 4: Control de asistencia

| ID | Requisito | Prioridad | Origen |
|---|---|---|---|
| **RF15** | El sistema debe mostrar al instructor el listado de alumnos inscriptos en su clase del día. | Alta | Relevamiento |
| **RF16** | El instructor debe poder registrar la asistencia de cada alumno con los estados: Presente, Ausente o Justificado. | Alta | Relevamiento |
| **RF17** | El sistema debe registrar fecha y hora de cada registro de asistencia. | Media | Relevamiento |
| **RF18** | El sistema debe permitir consultar el historial de asistencia de un alumno. | Media | Relevamiento |
| **RF27** | El sistema debe permitir al instructor registrar la asistencia de un alumno activo que no esté inscripto en esa clase, marcándolo como asistente ocasional. | Media | Adicional |

> **RF27 (adicional).** Incorporado por el equipo. El criterio de aceptación de HU-04 contempla que el instructor pueda agregar a un alumno no inscripto, pero ningún requisito lo respaldaba. Sin este requisito, el modelo de datos obligaría a que toda asistencia derive de una inscripción previa. Corresponde a la decisión D7.

---

### 6.5 — Módulo 5: Seguimiento nutricional

| ID | Requisito | Prioridad | Origen |
|---|---|---|---|
| **RF19** | El sistema debe permitir a la nutricionista registrar consultas por alumno-paciente, incluyendo fecha, peso, medidas y observaciones. | Media | Relevamiento |
| **RF20** | La nutricionista debe poder consultar el historial de evolución de cada paciente. | Media | Relevamiento |
| **RF21** | El acceso al módulo nutricional debe estar restringido exclusivamente al rol nutricionista. | Alta | Relevamiento |

> **Precisión sobre RF19.** El enunciado original menciona "medidas" sin especificar cuáles, imprecisión que motivó el rechazo de la historia HU-05 por la Definition of Ready. El enunciado del requisito se conserva sin modificar; los parámetros concretos quedan definidos en la regla de negocio [RN26](#85--seguimiento-nutricional), conforme a la decisión D11.

---

### 6.6 — Módulo 6: Gestión de instructores

| ID | Requisito | Prioridad | Origen |
|---|---|---|---|
| **RF25** | El sistema debe permitir registrar, modificar y dar de baja lógica a instructores, indicando apellido, nombre, DNI, especialidad y estado. | Alta | Adicional |
| **RF26** | El sistema debe permitir a cada instructor consultar la agenda de las clases que tiene asignadas en la grilla activa. | Media | Adicional |

> **RF25 y RF26 (adicionales).** Incorporados por el equipo. La gestión de instructores figura en el alcance del relevamiento y la entidad `Instructor` existe en el modelo de datos, pero no había requisitos que definieran cómo se administra. RF25 es prerrequisito de RF12: no se puede asignar un instructor que no está cargado. RF26 responde a la necesidad relevada de que los instructores puedan consultar su propia información sin intermediarios (problema P4).

---

### 6.7 — Módulo 7: Seguridad, usuarios y roles

| ID | Requisito | Prioridad | Origen |
|---|---|---|---|
| **RF22** | El sistema debe permitir a los usuarios autenticarse mediante nombre de usuario y contraseña, y cerrar sesión de forma explícita. | Alta | Adicional |
| **RF23** | El sistema debe permitir al rol Administrador crear, modificar y desactivar usuarios, asignando a cada uno un rol del conjunto definido. | Alta | Adicional |
| **RF31** | El sistema debe permitir al rol Administrador consultar el log de auditoría, filtrable por usuario, tipo de operación y rango de fechas. | Baja | Adicional |

> **RF22, RF23 y RF31 (adicionales).** Incorporados por el equipo. El diagrama de casos de uso del relevamiento incluye "Autenticar usuario" y los requisitos RNF03, RNF04 y RNF05 exigen control de acceso, contraseñas cifradas y log de auditoría. Sin embargo, ningún requisito funcional definía cómo se inicia sesión, cómo se crean los usuarios ni cómo se consulta el log. RF22 y RF23 hacen operable lo que RNF03 y RNF04 exigen; RF31 hace consultable el registro que RNF05 obliga a generar.

---

### 6.8 — Módulo 8: Reportes de gestión

| ID | Requisito | Prioridad | Origen |
|---|---|---|---|
| **RF28** | El sistema debe permitir generar un reporte de ingresos por período, discriminado por modalidad de cobro y por disciplina. | Baja | Adicional |
| **RF29** | El sistema debe permitir consultar la ocupación de cada clase de la grilla activa, expresada como cantidad de inscriptos y porcentaje de asistencia promedio. | Baja | Adicional |

> **RF28 y RF29 (adicionales).** Incorporados por el equipo. El objetivo OE6 menciona reportes de gestión y el relevamiento indica que la dirección necesita conocer la proyección de ingresos, pero el único reporte especificado era el de morosidad (RF09). RF28 responde a la necesidad de proyección de ingresos afectada por el problema P1. RF29 permite decidir qué horarios conviene sostener, abrir o cerrar.

---

### 6.9 — Resumen de requisitos funcionales

| Módulo | Requisitos del relevamiento | Requisitos adicionales | Total |
|---|---|---|---|
| M1 — Gestión de alumnos | RF01 a RF05 | RF24 | 6 |
| M2 — Gestión de cuotas y pagos | RF06 a RF10 | RF30 | 6 |
| M3 — Planificación de actividades | RF11 a RF14 | — | 4 |
| M4 — Control de asistencia | RF15 a RF18 | RF27 | 5 |
| M5 — Seguimiento nutricional | RF19 a RF21 | — | 3 |
| M6 — Gestión de instructores | — | RF25, RF26 | 2 |
| M7 — Seguridad, usuarios y roles | — | RF22, RF23, RF31 | 3 |
| M8 — Reportes de gestión | — | RF28, RF29 | 2 |
| **Total** | **21** | **10** | **31** |

Ningún requisito del relevamiento original fue eliminado ni modificado en su enunciado.

---

## 7. Requisitos no funcionales

### 7.1 — Rendimiento

| ID | Requisito | Cómo se verifica |
|---|---|---|
| **RNF01** | El sistema debe responder a cualquier acción del usuario en un tiempo máximo de 3 segundos bajo condiciones normales de red y carga habitual. | Medición del tiempo de respuesta en las operaciones más frecuentes. |
| **RNF02** | El sistema debe soportar al menos 50 usuarios concurrentes sin degradación perceptible del tiempo de respuesta. | Prueba de carga con 50 sesiones simultáneas. |

### 7.2 — Seguridad

| ID | Requisito | Cómo se verifica |
|---|---|---|
| **RNF03** | El sistema debe implementar control de acceso por roles: Administrador, Recepcionista, Instructor y Nutricionista. Cada rol accede únicamente a las funciones que le corresponden. | Prueba de acceso a cada función con cada rol, incluyendo acceso por URL directa. |
| **RNF04** | Las contraseñas deben almacenarse cifradas. Ningún actor puede ver la contraseña de otro usuario. | Inspección de la base de datos: ninguna contraseña legible en texto plano. |
| **RNF05** | El sistema debe registrar en un log de auditoría cada operación sensible: alta y baja de alumno, registro de pago y modificación de datos. | Verificación de que cada operación sensible genera un registro con usuario, fecha, hora y operación. |
| **RNF06** | Los datos del módulo nutricional deben estar aislados del resto del sistema y ser accesibles exclusivamente por la nutricionista y el administrador. | Prueba de acceso con los roles Recepcionista e Instructor: acceso denegado. |

> **Compatibilidad entre RF21 y RNF06.** Ambos requisitos provienen del relevamiento y su lectura literal es contradictoria. Se resuelven sin modificar sus enunciados separando el acceso en dos planos, según la regla [RN29](#86--seguridad) y la decisión D10: RF21 rige el contenido clínico, RNF06 rige la administración del módulo.

### 7.3 — Usabilidad

| ID | Requisito | Cómo se verifica |
|---|---|---|
| **RNF07** | La interfaz debe ser responsiva y funcionar correctamente en tablets y computadoras de escritorio. | Prueba en resoluciones de escritorio y de tablet. |
| **RNF08** | El flujo de registro de un pago no debe requerir más de 4 pasos desde la búsqueda del alumno hasta la confirmación. | Conteo de pasos sobre el prototipo. |
| **RNF09** | Todos los mensajes de error deben estar redactados en lenguaje claro, sin tecnicismos, e indicar una acción concreta a seguir. | Revisión del listado completo de mensajes de error. |

### 7.4 — Disponibilidad

| ID | Requisito | Cómo se verifica |
|---|---|---|
| **RNF10** | El sistema debe estar disponible en el horario de atención del gimnasio (07:00 a 22:00), con una disponibilidad mínima garantizada del 99 por ciento mensual en esa franja. | Registro de indisponibilidad mensual. |
| **RNF11** | Las tareas de mantenimiento programado deben realizarse fuera del horario de atención y notificarse a los usuarios con al menos 24 horas de anticipación. | Registro de ventanas de mantenimiento y sus avisos. |

### 7.5 — Mantenibilidad

| ID | Requisito | Cómo se verifica |
|---|---|---|
| **RNF12** | El sistema debe estar desarrollado de forma modular, permitiendo incorporar nuevas funcionalidades (por ejemplo, un módulo de comunicaciones) sin refactorizar las existentes. | Revisión de la estructura del código. |
| **RNF13** | El código debe estar documentado y contar con pruebas unitarias sobre los módulos de cobro y de acceso por roles. | Existencia y ejecución de las pruebas unitarias. |

---
