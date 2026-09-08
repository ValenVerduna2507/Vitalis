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

## 8. Reglas de negocio

Las reglas de negocio describen restricciones del dominio que el sistema debe respetar, independientemente de cómo se implemente.

### 8.1 — Alumnos

| ID | Regla | Requisitos relacionados |
|---|---|---|
| **RN01** | Un alumno se identifica unívocamente por su DNI. No pueden existir dos alumnos activos o históricos con el mismo DNI. | RF02 |
| **RN02** | Ningún alumno se elimina físicamente del sistema. La baja es siempre lógica y conserva todo el historial de pagos, inscripciones y asistencias. | RF04 |
| **RN03** | Un alumno con estado Baja no puede registrar pagos ni inscribirse a clases. Para operar debe reactivarse previamente. | RF04, RF05, RF14 |
| **RN04** | El número de legajo se asigna de forma automática y correlativa al confirmar el alta. No se reutiliza ni se reasigna, aun cuando el alumno se dé de baja. | RF01 |
| **RN05** | Los alumnos menores de 18 años deben tener registrado un familiar o tutor responsable, que es quien autoriza la participación y gestiona los trámites administrativos. | RF01 |

### 8.2 — Cuotas y pagos

| ID | Regla | Requisitos relacionados |
|---|---|---|
| **RN06** | Existen tres modalidades de cobro: mensual fija, por clase asistida y combinada. Cada alumno tiene una única modalidad vigente. | RF07 |
| **RN07** | Los montos de cada modalidad son definidos por la dirección. Los valores vigentes a mayo de 2026 son: mensual fija 28.000 pesos, por clase 4.500 pesos, combinada 38.000 pesos. | RF07 |
| **RN08** | Un cambio en el monto de una modalidad no altera las cuotas ya registradas. Cada cuota conserva el monto y la modalidad con la que fue cobrada. | RF06, RF07 |
| **RN09** | Un alumno con modalidad mensual fija se considera moroso cuando transcurrieron más de 30 días desde su último pago registrado. El umbral es configurable por el Administrador. | RF08, RF30 |
| **RN10** | El monto de un pago debe ser mayor a cero. | RF06 |
| **RN11** | Los medios de pago admitidos son efectivo y transferencia. | RF06 |
| **RN25** | La modalidad de cobro por clase no genera morosidad, ya que el pago se realiza de forma anticipada o en el momento de la clase. Un alumno con esta modalidad que no registre pagos ni asistencias durante más de 60 días se clasifica como inactivo, no como moroso, y no figura en el reporte de morosos. | RF07, RF08, RF09 |

### 8.3 — Planificación y clases

| ID | Regla | Requisitos relacionados |
|---|---|---|
| **RN12** | Solo puede existir una grilla activa por vez. Las grillas anteriores se conservan en modo de solo lectura. | RF13 |
| **RN13** | Un alumno solo puede inscribirse a clases pertenecientes a la grilla activa. | RF13, RF14 |
| **RN14** | Cada clase tiene un único instructor asignado. Un instructor puede dictar varias clases, siempre que no se superpongan en día y horario. | RF12 |
| **RN15** | Las disciplinas infantiles (Aeróbica Infantil y Kids Fit & Fun) se dictan únicamente en el turno tarde. | RF11 |

### 8.4 — Asistencia

| ID | Regla | Requisitos relacionados |
|---|---|---|
| **RN16** | Los estados de asistencia admitidos son: Presente, Ausente y Justificado. | RF16 |
| **RN17** | Un instructor solo puede registrar asistencia de las clases que tiene asignadas y correspondientes al día en curso. | RF15, RF16 |
| **RN18** | Un alumno activo puede ser registrado como asistente ocasional en una clase donde no está inscripto. Esa asistencia se marca como ocasional y no genera una inscripción permanente. | RF27 |
| **RN19** | Solo puede existir un registro de asistencia por alumno, clase y fecha. | RF16, RF17 |

### 8.5 — Seguimiento nutricional

| ID | Regla | Requisitos relacionados |
|---|---|---|
| **RN20** | Los datos del módulo nutricional son información de salud y se consideran confidenciales. No son visibles para los roles Recepcionista ni Instructor. | RF21, RNF06 |
| **RN21** | Solo los alumnos activos pueden ser registrados como pacientes del módulo nutricional. | RF19 |
| **RN26** | Cada consulta nutricional registra: fecha, peso en kilogramos, altura en centímetros, perímetro de cintura en centímetros, perímetro de cadera en centímetros, porcentaje de masa grasa, objetivo del paciente y observaciones. Son obligatorios la fecha, el peso y el objetivo; el resto es opcional. La altura es obligatoria en la primera consulta y se arrastra a las siguientes. El índice de masa corporal se calcula automáticamente a partir del peso y la altura, y no se ingresa de forma manual. | RF19, RF20 |
| **RN27** | Para registrar como paciente nutricional a un alumno menor de 18 años, el sistema exige que estén cargados el familiar o tutor responsable y la fecha de autorización. Sin esos datos no se puede crear la primera consulta. | RF19, RN05 |
| **RN28** | El rol Alumno no accede a su historial nutricional a través del sistema. La devolución de resultados se realiza en el consultorio, a criterio de la nutricionista. | RF21, RNF06 |

### 8.6 — Seguridad

| ID | Regla | Requisitos relacionados |
|---|---|---|
| **RN22** | Cada usuario del sistema tiene un único rol asignado. | RF23, RNF03 |
| **RN23** | Toda operación sensible queda registrada en el log de auditoría con usuario, fecha, hora y tipo de operación. El log no puede modificarse ni eliminarse. | RF31, RNF05 |
| **RN24** | Un usuario desactivado no puede iniciar sesión, pero sus registros previos en el log de auditoría se conservan. | RF23, RNF05 |
| **RN29** | El acceso al módulo nutricional se separa en dos planos. El **contenido clínico** de cada consulta (peso, medidas, porcentaje de masa grasa, objetivo y observaciones) es accesible únicamente por el rol Nutricionista, conforme a RF21. La **administración del módulo** (creación del usuario nutricionista, asignación de pacientes, copias de seguridad y consulta del log de auditoría del módulo) corresponde al rol Administrador, sin acceso al contenido de las consultas, conforme a RNF06. | RF21, RF23, RNF05, RNF06 |

---

## 9. Restricciones

### 9.1 — Restricciones de negocio

| ID | Restricción | Implicancia |
|---|---|---|
| **RE01** | El centro no puede interrumpir su operación durante la implementación del sistema. | La migración debe ser gradual. Cada entrega parcial debe ser usable por sí sola. Ver `slicing.md`. |
| **RE02** | El personal administrativo actual no tiene formación técnica. | La interfaz debe ser aprendible sin capacitación formal extensa. Refuerza RNF08 y RNF09. |
| **RE03** | El sistema debe convivir con las planillas existentes durante la transición. | Debe contemplarse la carga inicial del padrón desde los datos actuales. |
| **RE04** | La dirección define precios y modalidades y los modifica sin previo aviso al equipo técnico. | Los montos y el umbral de mora deben ser parametrizables (RF30, RN07). |

### 9.2 — Restricciones técnicas

| ID | Restricción | Implicancia |
|---|---|---|
| **RE05** | Los instructores usarán el sistema desde una tablet en el salón, con conectividad wifi del establecimiento. | La interfaz de toma de asistencia debe funcionar en pantalla táctil y tolerar conexiones inestables. |
| **RE06** | La solución debe ser web y accesible desde navegador, sin instalación en cada equipo. | Descarta aplicaciones de escritorio. |
| **RE07** | El presupuesto del centro es acotado. | Se privilegian tecnologías de código abierto y hosting de bajo costo. |
| **RE11** | La arquitectura prevista es Web App, back-end y base de datos PostgreSQL, complementada con **n8n** como motor de automatización e integración. | n8n ejecuta los procesos periódicos que no requieren intervención de un usuario. No contiene lógica de negocio ni reemplaza al back-end: consulta al sistema y distribuye el resultado. Ver la sección 9.4. |

### 9.3 — Restricciones del trabajo académico

| ID | Restricción | Implicancia |
|---|---|---|
| **RE08** | El alcance de la materia comprende el análisis funcional y el diseño de la solución, no su implementación completa. | Los entregables son documentales: requisitos, historias, casos de uso, modelo de datos, diagramas y diseño de interfaz. |
| **RE09** | El equipo está integrado por cuatro personas con disponibilidad parcial. | El backlog se ordena por prioridad de negocio, sin comprometer la entrega de todas las funcionalidades. |
| **RE10** | La documentación se versiona en un repositorio Git público. | Formato Markdown y diagramas en PlantUML, versionables como texto. |

### 9.4 — Arquitectura tecnológica prevista

La restricción RE11 define la arquitectura sobre la que se implementará el sistema. Se documenta acá porque condiciona cómo se satisfacen algunos requisitos, no porque agregue requisitos nuevos.

```text
   Web App  ──►  Back-end  ──►  PostgreSQL
                     ▲
                     │  consulta y dispara
                     │
                    n8n
```

| Componente | Responsabilidad |
|---|---|
| Web App | Interfaz de usuario. Las diecinueve pantallas documentadas en `docs/diseño-ui.md`. |
| Back-end | Toda la lógica de negocio: validaciones, reglas RN01 a RN29, cálculo de datos derivados y control de acceso por roles. |
| PostgreSQL | Persistencia del modelo de datos definido en `docs/er-modelo.md`. |
| n8n | Ejecución programada de procesos que no dependen de una acción del usuario, y distribución de sus resultados. |

**Qué hace n8n en este sistema**

| Automatización | Requisito que apoya | Estado |
|---|---|---|
| Ejecución diaria de la clasificación de alumnos morosos e inactivos. | RF08, RN09, RN25 | Prevista para la implementación |
| Generación y envío periódico del reporte de morosidad y de ingresos a la dirección. | RF09, RF28 | Prevista para la implementación |
| Notificaciones automáticas por email a alumnos. | Ninguno de la versión 1.0 | Extensión futura. La comunicación masiva está fuera de alcance. |

**Qué no hace n8n**

Ningún requisito funcional de este documento depende de n8n para cumplirse. RF08 exige que el sistema identifique automáticamente a los alumnos morosos, y esa clasificación se resuelve en el back-end: n8n solo determina cuándo se ejecuta y a quién se le entrega el resultado. Si n8n no estuviera disponible, el sistema seguiría siendo funcional y la dirección obtendría la misma información consultando el reporte desde la pantalla P08.

Esta separación es deliberada. Poner reglas de negocio dentro de un flujo de automatización las saca del alcance del control de versiones del código y de las pruebas unitarias que exige RNF13.

---

## 10. Roles y permisos

### 10.1 — Definición de roles

| Rol | Quién lo ocupa | Alcance |
|---|---|---|
| **Administrador** | Propietaria y directora del centro. | Acceso completo al sistema, incluida la configuración y los reportes de gestión. |
| **Recepcionista** | Personal administrativo. | Operación diaria: padrón, cobros e inscripciones. |
| **Instructor** | Profesores de cada disciplina. | Solo sus propias clases: listado de inscriptos y registro de asistencia. |
| **Nutricionista** | Profesional de nutrición. | Solo el módulo nutricional y únicamente sus pacientes. |
| **Alumno** | Alumnos del centro. | Consulta de su propia información. Sin permisos de escritura. |

### 10.2 — Matriz de permisos

Referencias: **T** total, **P** parcial (limitado a los registros propios), **C** solo consulta, **A** administrativo (gestiona el módulo sin ver su contenido clínico), **N** sin acceso.

| Funcionalidad | Requisitos | Administrador | Recepcionista | Instructor | Nutricionista | Alumno |
|---|---|:---:|:---:|:---:|:---:|:---:|
| Iniciar sesión | RF22 | T | T | T | T | T |
| Gestionar usuarios y roles | RF23 | T | N | N | N | N |
| Consultar log de auditoría | RF31 | T | N | N | N | N |
| Registrar alumno | RF01, RF02 | T | T | N | N | N |
| Modificar alumno | RF03 | T | T | N | N | N |
| Dar de baja alumno | RF04 | T | T | N | N | N |
| Reactivar alumno | RF05 | T | T | N | N | N |
| Buscar y listar alumnos | RF24 | T | T | C | N | N |
| Registrar pago | RF06, RF07 | T | T | N | N | N |
| Consultar historial de pagos | RF10 | T | T | N | N | P |
| Consultar alumnos morosos | RF08, RF09 | T | C | N | N | N |
| Configurar umbral de mora | RF30 | T | N | N | N | N |
| Gestionar disciplinas | RF11 | T | N | N | N | N |
| Crear clases y asignar instructor | RF12 | T | N | N | N | N |
| Gestionar versiones de grilla | RF13 | T | N | N | N | N |
| Consultar la grilla | RF11, RF13 | T | C | C | N | C |
| Inscribir alumno a clase | RF14 | T | T | N | N | N |
| Ver inscriptos de la clase del día | RF15 | T | C | P | N | N |
| Registrar asistencia | RF16, RF17 | T | N | P | N | N |
| Registrar asistente ocasional | RF27 | T | N | P | N | N |
| Consultar historial de asistencia | RF18 | T | C | P | N | P |
| Registrar consulta nutricional | RF19 | A | N | N | P | N |
| Consultar evolución del paciente | RF20, RF21 | A | N | N | P | N |
| Gestionar instructores | RF25 | T | N | N | N | N |
| Consultar agenda propia | RF26 | T | N | P | N | N |
| Reporte de ingresos | RF28 | T | N | N | N | N |
| Reporte de ocupación de clases | RF29 | T | N | N | N | N |

> Las dos filas del módulo nutricional muestran **A** para el Administrador según la regla RN29 y la decisión D10: puede crear el usuario de la nutricionista, asignarle pacientes, realizar copias de seguridad y consultar el log de auditoría del módulo, pero no puede ver el peso, las medidas ni las observaciones de ninguna consulta. El contenido clínico queda reservado al rol Nutricionista, tal como exige RF21.

### 10.3 — Alcance del rol Alumno

El rol Alumno tiene permisos de consulta limitados a su propia información: estado de cuenta, historial de pagos, horarios de las clases en las que está inscripto e historial de asistencia propio. No puede modificar ningún dato ni consultar información de otros alumnos. Corresponde a la decisión D5.

El historial nutricional queda **fuera** del alcance de este rol en la versión 1.0. La devolución de resultados se realiza en el consultorio, a criterio de la nutricionista, según la regla RN28 y la decisión D13. La incorporación de una vista de consulta para el paciente queda registrada como posible evolución del sistema, sujeta a un acuerdo previo sobre el tratamiento de datos de salud.

---

## 11. Dependencias

### 11.1 — Dependencias funcionales internas

| Funcionalidad | Depende de | Motivo |
|---|---|---|
| Todo el sistema | RF22 (autenticación) | Sin identificación del usuario no se puede aplicar el control por roles ni el log de auditoría. |
| RF06 (registrar pago) | RF01 (registrar alumno), RF07 (modalidades) | No se puede cobrar a un alumno inexistente ni sin una modalidad asignada. |
| RF08 (detectar morosos) | RF06 (registrar pago) | Sin pagos registrados no hay antigüedad de deuda que calcular. |
| RF09 (reporte de morosos) | RF08 | El reporte presenta el resultado de la clasificación. |
| RF12 (asignar instructor) | RF25 (registrar instructor), RF11 (disciplinas) | No se puede asignar un instructor ni crear una clase de una disciplina que no existe. |
| RF14 (inscribir a clase) | RF01, RF12, RF13 | Requiere alumno activo y clase existente en la grilla activa. |
| RF15 y RF16 (asistencia) | RF14 (inscripción) | El listado de la clase se arma con los alumnos inscriptos. |
| RF26 (agenda propia) | RF12 | La agenda se compone de las clases asignadas al instructor. |
| RF28 (reporte de ingresos) | RF06, RF07 | Se calcula sobre los pagos registrados. |
| RF29 (ocupación de clases) | RF14, RF16 | Se calcula sobre inscripciones y asistencias. |
| RF31 (consulta de auditoría) | RNF05 | Consulta el registro que el sistema genera. |

### 11.2 — Dependencias externas

| Dependencia | Descripción | Riesgo si no se cumple |
|---|---|---|
| Definiciones de la dirección de Vitalis | Reglas de mora por modalidad, parámetros de la consulta nutricional, política de acceso a datos de salud. | Bloquea las funcionalidades correspondientes. Ver puntos abiertos. |
| Datos de las planillas actuales | Carga inicial del padrón de alumnos y de la grilla vigente. | El sistema arranca vacío y la migración se hace manualmente. |
| Conectividad wifi del establecimiento | Uso del sistema desde tablet en el salón. | El registro de asistencia digital no sería viable en el momento de la clase. |
| Servicio de hosting | Publicación del sistema. | El sistema no sería accesible fuera de la red local. |
| Instancia de n8n operativa | Ejecución de las automatizaciones descriptas en la sección 9.4. | Las tareas periódicas dejarían de ejecutarse solas. El sistema sigue funcionando y la información se obtiene consultando los reportes desde la interfaz. |
| Servicio de correo saliente | Envío de los reportes periódicos a la dirección. | Los reportes se generan igual, pero deben descargarse desde la pantalla P16. |

---

## 12. Supuestos

Los siguientes supuestos fueron asumidos por el equipo ante la ausencia de una definición explícita. Cada uno debe validarse con la dirección de Vitalis antes de la implementación.

| ID | Supuesto | Impacto si resulta falso |
|---|---|---|
| **S01** | Cada alumno tiene una única modalidad de cobro vigente por vez. | Si un alumno pudiera combinar modalidades simultáneas, el modelo de datos y el cálculo de mora cambian. |
| **S02** | El período de una cuota mensual es el mes calendario. | Si el período se contara desde la fecha de alta de cada alumno, cambiaría el cálculo de vencimiento. |
| **S03** | La cantidad de disciplinas es acotada y de baja rotación. Se dan de alta y de baja pocas veces al año. | Si cambiaran con frecuencia, convendría revisar el diseño de la gestión de disciplinas. |
| **S04** | Un alumno puede estar inscripto en más de una clase de forma simultánea. | Si solo pudiera estar en una, la inscripción requeriría una validación adicional. |
| **S05** | El centro opera en una sola sede. | Un sistema multisede requeriría incorporar la entidad correspondiente en todo el modelo. |
| **S06** | Los instructores no son alumnos del centro. | Si lo fueran, una misma persona tendría dos registros y dos roles. |
| **S07** | El sistema no necesita registrar la firma ni el consentimiento del alumno en formato digital. | Si fuera necesario, habría que incorporar gestión documental. |
| **S08** | La información histórica de las planillas se migra una única vez, al inicio. | Una migración incremental requeriría herramientas específicas. |

---

## 13. Puntos abiertos

Cuestiones detectadas durante la evaluación de la Definition of Ready y durante la consolidación de este documento. Corresponden al criterio C20 de la DoR.

### 13.1 — Puntos resueltos

Los cinco puntos abiertos de la versión 1.1 fueron resueltos por acuerdo del equipo, con la validación de la dirección de Vitalis. Cada resolución quedó registrada como decisión en `integrantes.md`.

| ID | Punto abierto | Resolución adoptada | Decisión | Documentos impactados |
|---|---|---|---|---|
| **A1** | RF19 menciona "medidas" sin especificar cuáles se registran, si son obligatorias y en qué unidad. | Se definen los parámetros de la consulta en la regla RN26: peso, altura, perímetro de cintura, perímetro de cadera, porcentaje de masa grasa, objetivo y observaciones, con IMC calculado. El enunciado de RF19 no se modifica. | D11 | `docs/er-modelo.md`, `diagramas/er.puml`, `docs/diseño-ui.md` |
| **A2** | RF21 restringe el módulo nutricional "exclusivamente al rol nutricionista", mientras que RNF06 lo habilita también al Administrador. | Se separa el acceso en dos planos mediante la regla RN29: el contenido clínico corresponde a la Nutricionista (RF21) y la administración del módulo al Administrador, sin ver consultas (RNF06). Ningún requisito se modifica. | D10 | `docs/requisitos.md`, `docs/casos-de-uso.md`, `docs/diseño-ui.md` |
| **A3** | No estaba definido cómo se calcula la morosidad para la modalidad de cobro por clase. | La modalidad por clase no genera morosidad, porque el pago es anticipado. Estos alumnos se clasifican como inactivos tras 60 días sin pagos ni asistencias (RN25). | D12 | `docs/casos-de-uso.md`, `slicing.md` |
| **A4** | No estaba definido si el alumno-paciente puede consultar su propio historial nutricional. | El rol Alumno no accede al historial nutricional en la versión 1.0 (RN28). | D13 | `docs/diseño-ui.md` |
| **A5** | No estaba definido cómo se gestiona el consentimiento para registrar datos de salud de alumnos menores de edad. | Se exige tutor responsable y fecha de autorización antes de la primera consulta (RN27). | D14 | `docs/er-modelo.md`, `docs/casos-de-uso.md` |

### 13.2 — Fundamento de las resoluciones más sensibles

**A2 — Acceso al módulo nutricional.** La contradicción entre RF21 y RNF06 es real y ambos requisitos provienen del relevamiento, de modo que descartar uno habría implicado eliminar un requisito del cliente. La lectura que adoptamos entiende que los dos hablan de cosas distintas: RF21 protege la confidencialidad del dato de salud y RNF06 se refiere a la capacidad de administrar el módulo, que es una necesidad operativa real (alguien tiene que crear el usuario de la nutricionista y garantizar el resguardo de la información). Separar contenido de administración satisface ambos enunciados sin modificar ninguno, y es además la práctica habitual en sistemas que manejan historia clínica.

**A3 — Morosidad en modalidad por clase.** La pregunta original era si un alumno que paga por clase y no concurre durante 40 días es moroso. La respuesta surge de la naturaleza de la modalidad: si el pago se realiza en el momento de la clase, no hay deuda pendiente cuando el alumno deja de venir, simplemente deja de generar consumo. Clasificarlo como moroso produciría un reporte de cobranza con alumnos a los que no hay nada que reclamar, y eso degradaría la utilidad del reporte para la dirección. En cambio, sí resulta valioso identificar a estos alumnos como inactivos, porque señala un riesgo de baja sobre el que el centro puede actuar comercialmente.

**A5 — Consentimiento de menores.** Vitalis dicta disciplinas infantiles (Aeróbica Infantil y Kids Fit & Fun), de modo que la posibilidad de que un menor sea paciente del consultorio nutricional es concreta. La regla RN05 ya obliga a registrar un tutor responsable para todo alumno menor de 18 años, así que la resolución reutiliza ese dato y le agrega la fecha de autorización específica para el seguimiento nutricional, sin introducir una entidad nueva.

### 13.3 — Cuestiones diferidas

No se trata de bloqueos: son definiciones que el equipo decidió postergar por estar fuera del alcance de la versión 1.0.

| ID | Cuestión | Momento de resolución |
|---|---|---|
| **B1** | Vista de consulta del historial nutricional para el alumno-paciente. | Versión posterior, sujeta a un acuerdo sobre tratamiento de datos de salud. |
| **B2** | Política de retención y depuración del log de auditoría. | Antes de la puesta en producción. |
| **B3** | Procedimiento de carga inicial del padrón desde las planillas actuales. | Etapa de implementación. |

## 14. Matriz de trazabilidad

| Requisito | Módulo | Épica | Historia | Caso de uso | Prioridad |
|---|---|---|---|---|---|
| RF01 | M1 | E1 | HU-01 | CU-01 | Alta |
| RF02 | M1 | E1 | HU-01 | CU-01 | Alta |
| RF03 | M1 | E1 | HU-06 | CU-04 | Media |
| RF04 | M1 | E1 | HU-07 | CU-05 | Alta |
| RF05 | M1 | E1 | HU-08 | CU-06 | Media |
| RF06 | M2 | E2 | HU-02 | CU-02 | Alta |
| RF07 | M2 | E2 | HU-02, HU-11 | CU-02, CU-07 | Alta |
| RF08 | M2 | E2 | HU-03 | CU-03 | Alta |
| RF09 | M2 | E2 | HU-03 | CU-03 | Alta |
| RF10 | M2 | E2 | HU-10 | — | Media |
| RF11 | M3 | E3 | HU-12 | CU-08 | Alta |
| RF12 | M3 | E3 | HU-13 | CU-09 | Alta |
| RF13 | M3 | E3 | HU-14 | CU-10 | Media |
| RF14 | M3 | E3 | HU-15 | CU-11 | Alta |
| RF15 | M4 | E4 | HU-04 | CU-12 | Alta |
| RF16 | M4 | E4 | HU-04 | CU-12 | Alta |
| RF17 | M4 | E4 | HU-04 | CU-12 | Media |
| RF18 | M4 | E4 | HU-16 | — | Media |
| RF19 | M5 | E5 | HU-05a | CU-13 | Media |
| RF20 | M5 | E5 | HU-05b | CU-14 | Media |
| RF21 | M5 | E5 | HU-05a, HU-05b | CU-13, CU-14 | Alta |
| RF22 | M7 | E7 | HU-20 | CU-00 | Alta |
| RF23 | M7 | E7 | HU-21 | CU-16 | Alta |
| RF24 | M1 | E1 | HU-09 | — | Alta |
| RF25 | M6 | E6 | HU-18 | CU-15 | Alta |
| RF26 | M6 | E6 | HU-19 | — | Media |
| RF27 | M4 | E4 | HU-17 | — | Media |
| RF28 | M8 | E8 | HU-22 | — | Baja |
| RF29 | M8 | E8 | HU-23 | — | Baja |
| RF30 | M2 | E2 | HU-03 | CU-03 | Media |
| RF31 | M7 | E7 | HU-21 | — | Baja |

**Cobertura:** los 31 requisitos funcionales están asociados a al menos una historia de usuario. Las historias sin caso de uso corresponden a consultas simples que no justifican el desarrollo completo de flujos alternativos y de excepción.

---

## Historial de versiones

| Versión | Fecha | Cambio |
|---|---|---|
| 1.0 | 05/2026 | Versión del relevamiento inicial. RF01 a RF21 y RNF01 a RNF13. |
| 1.1 | 05/2026 | Normalización de identificadores sin guion (D1). Incorporación de RF22 a RF31 como requisitos adicionales. Formalización de reglas de negocio, restricciones, matriz de permisos, dependencias, supuestos, puntos abiertos y matriz de trazabilidad. |
| 1.2 | 05/2026 | Resolución de los puntos abiertos A1 a A5 (decisiones D10 a D14). Incorporación de las reglas RN25 a RN29. Ajuste de la matriz de permisos con el nivel de acceso administrativo. Ningún requisito funcional ni no funcional fue modificado en su enunciado. |
| 1.3 | 05/2026 | Incorporación de la restricción RE11 y de la sección 9.4 con la arquitectura tecnológica prevista, incluida la herramienta de automatización n8n (decisión D15). Precisión sobre la comunicación masiva como extensión futura. Ningún requisito funcional ni no funcional fue modificado. |

---

<sub>Escuela Superior de Comercio N° 49 "Justo José de Urquiza" — Desarrollo Web / Analista Funcional de Sistemas — 2026.</sub>
