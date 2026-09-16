# Historias de usuario

Sistema de Gestión Integral — Vitalis Centro de Entrenamiento
Equipo: Grupo 02
Versión: 1.1

---

## Índice

- [1. Cómo leer este documento](#1-cómo-leer-este-documento)
- [2. Resumen del backlog](#2-resumen-del-backlog)
- [3. Épica E1 — Gestionar alumnos](#3-épica-e1--gestionar-alumnos)
- [4. Épica E2 — Gestionar cuotas y pagos](#4-épica-e2--gestionar-cuotas-y-pagos)
- [5. Épica E3 — Planificar actividades](#5-épica-e3--planificar-actividades)
- [6. Épica E4 — Controlar asistencia](#6-épica-e4--controlar-asistencia)
- [7. Épica E5 — Seguimiento nutricional](#7-épica-e5--seguimiento-nutricional)
- [8. Épica E6 — Gestionar instructores](#8-épica-e6--gestionar-instructores)
- [9. Épica E7 — Seguridad, usuarios y roles](#9-épica-e7--seguridad-usuarios-y-roles)
- [10. Épica E8 — Reportes de gestión](#10-épica-e8--reportes-de-gestión)
- [11. Matriz de cobertura de requisitos](#11-matriz-de-cobertura-de-requisitos)

---

## 1. Cómo leer este documento

Cada historia se presenta con la misma estructura:

| Elemento | Contenido |
|---|---|
| Ficha | Épica, módulo, rol, requisitos, caso de uso, slices, prioridad y estado frente a la Definition of Ready. |
| Historia | Enunciado en formato `Como <rol>, quiero <funcionalidad>, para <beneficio>`. |
| Criterios de aceptación | Lista verificable. Incluye siempre al menos un camino alternativo o de error, según el criterio C9 de la DoR. |
| INVEST | Evaluación de los seis criterios con su justificación. |
| Observaciones | Decisiones de diseño, dependencias y cuestiones registradas durante el refinamiento. |

### Origen de las historias

| Origen | Historias |
|---|---|
| Relevamiento inicial | HU-01, HU-02, HU-03, HU-04 y HU-05 (esta última dividida en HU-05a y HU-05b). |
| Incorporadas por el equipo | HU-06 a HU-23. |

Las historias incorporadas por el equipo derivan de requisitos existentes que no tenían historia asociada, o de los requisitos adicionales RF22 a RF31. Ninguna historia se creó sin un requisito que la justifique.

### Datos de ejemplo

Los ejemplos de este documento usan un conjunto fijo de datos, que se mantiene en todos los artefactos del repositorio.

| Rol | Nombre |
|---|---|
| Directora | Andrea Sosa |
| Recepcionista | Julieta Ferreyra |
| Instructor de Funcional y Full Body | Martín López |
| Instructora de Yoga y GAP | Sofía Martínez |
| Instructor de Full Body y Rutina Personalizada | Lucas Fernández |
| Instructora de Zumba y Aeróbica Infantil | Carla Giménez |
| Instructor de Kids Fit and Fun y Funcional | Diego Ríos |
| Nutricionista | Paula Ibarra |

### Prioridad

| Nivel | Significado |
|---|---|
| Alta | Necesaria para que el módulo cumpla su propósito. |
| Media | Completa la operación, puede diferirse a una iteración posterior. |
| Baja | Mejora la gestión pero no condiciona la operación diaria. |

---

## 2. Resumen del backlog

| ID | Historia | Épica | Rol | Requisitos | Prioridad | Estado DoR |
|---|---|---|---|---|---|---|
| HU-01 | Registrar un nuevo alumno | E1 | Recepcionista | RF01, RF02 | Alta | Ready |
| HU-06 | Modificar los datos de un alumno | E1 | Recepcionista | RF03 | Media | Ready |
| HU-07 | Dar de baja a un alumno | E1 | Recepcionista | RF04 | Alta | Ready |
| HU-08 | Reactivar un alumno dado de baja | E1 | Recepcionista | RF05 | Media | Ready |
| HU-09 | Buscar y listar alumnos | E1 | Recepcionista | RF24 | Alta | Ready |
| HU-02 | Registrar el pago de una cuota | E2 | Recepcionista | RF06, RF07 | Alta | Ready |
| HU-03 | Consultar alumnos morosos | E2 | Administrador | RF08, RF09, RF30 | Alta | Ready |
| HU-10 | Consultar el historial de pagos de un alumno | E2 | Recepcionista | RF10 | Media | Ready |
| HU-11 | Gestionar las modalidades de cobro | E2 | Administrador | RF07 | Alta | Ready |
| HU-12 | Gestionar disciplinas | E3 | Administrador | RF11 | Alta | Ready |
| HU-13 | Crear una clase y asignar instructor | E3 | Administrador | RF12 | Alta | Ready |
| HU-14 | Gestionar versiones de grilla | E3 | Administrador | RF13 | Media | Ready |
| HU-15 | Inscribir un alumno a una clase | E3 | Recepcionista | RF14 | Alta | Ready |
| HU-04 | Registrar asistencia a clase | E4 | Instructor | RF15, RF16, RF17 | Alta | Ready |
| HU-16 | Consultar el historial de asistencia de un alumno | E4 | Recepcionista | RF18 | Media | Ready |
| HU-17 | Registrar un asistente ocasional | E4 | Instructor | RF27 | Media | Ready |
| HU-05a | Registrar una consulta nutricional | E5 | Nutricionista | RF19, RF21 | Media | Ready |
| HU-05b | Consultar la evolución de un paciente | E5 | Nutricionista | RF20, RF21 | Media | Ready |
| HU-18 | Registrar y dar de baja instructores | E6 | Administrador | RF25 | Alta | Ready |
| HU-19 | Consultar la agenda propia | E6 | Instructor | RF26 | Media | Ready |
| HU-20 | Iniciar sesión en el sistema | E7 | Todos | RF22 | Alta | Ready |
| HU-21 | Gestionar usuarios y roles | E7 | Administrador | RF23, RF31 | Alta | Ready |
| HU-22 | Consultar el reporte de ingresos | E8 | Administrador | RF28 | Baja | Ready |
| HU-23 | Consultar la ocupación de las clases | E8 | Administrador | RF29 | Baja | Ready |

**Total: 24 historias.**

---

## 3. Épica E1 — Gestionar alumnos

> Como centro de entrenamiento, necesitamos administrar el padrón de alumnos de forma digital y centralizada, para eliminar la planilla de Excel y dejar de perder historial cuando un alumno se da de baja.

---

### HU-01 — Registrar un nuevo alumno

| Campo | Detalle |
|---|---|
| Épica | E1 — Gestionar alumnos |
| Módulo | M1 — Gestión de alumnos |
| Rol | Recepcionista (también Administrador) |
| Requisitos | RF01, RF02 |
| Reglas de negocio | RN01, RN04, RN05, RN06 |
| Caso de uso | CU-01 |
| Slices | S1.1, S1.2, S1.3 |
| Prioridad | Alta |
| Estado DoR | Ready |
| Origen | Relevamiento inicial |

**Historia**

> Como recepcionista, quiero registrar un nuevo alumno con sus datos personales y la actividad elegida, para tener un padrón digital centralizado que evite duplicados y errores.

**Criterios de aceptación**

1. Se ingresan nombre, apellido, DNI, teléfono, email y actividad o actividades (por ejemplo, Pilates, Entrenamiento o ambas).
2. Si el DNI ya existe en el padrón, el sistema muestra un mensaje claro, exhibe los datos del alumno existente e impide continuar.
3. Al confirmar el alta, el sistema asigna un número de legajo correlativo y muestra una confirmación.
4. El alumno queda en estado Activo y aparece en el padrón general.
5. No se puede confirmar el alta si hay campos obligatorios vacíos. Los campos obligatorios son nombre, apellido, DNI y al menos una actividad.
6. Se selecciona la modalidad de cobro del alumno entre las modalidades vigentes.
7. Si el alumno es menor de 18 años, el sistema exige registrar el nombre y el teléfono del familiar o tutor responsable.

**INVEST**

| Criterio | Cumple | Observación |
|---|---|---|
| Independiente | Sí | No depende de ninguna otra historia para implementarse. Es el punto de entrada del sistema. |
| Negociable | Sí | Los campos del formulario pueden ajustarse según lo que la dirección defina como mínimo necesario. |
| Valiosa | Sí | Elimina el registro manual en planilla, evita duplicados y habilita el seguimiento digital del alumno. |
| Estimable | Sí | El formulario y la lógica de validación de DNI son de complejidad conocida. |
| Pequeña | Sí | Cubre solo el alta. La gestión de cuotas y la inscripción a clases son historias separadas. |
| Verificable | Sí | Los criterios de aceptación son concretos y comprobables con casos de prueba específicos. |

**Observaciones**

- Es la historia de referencia del backlog. Cuando el equipo duda de si otra historia está bien escrita, la compara con esta.
- El criterio 7 se agregó durante el refinamiento, a partir de la regla RN05. El enunciado original no contemplaba a los alumnos menores, que existen en las disciplinas Aeróbica Infantil y Kids Fit and Fun.
- La división en slices responde a la necesidad de empezar a migrar el padrón desde la primera semana. Ver `slicing.md`.

---

### HU-06 — Modificar los datos de un alumno

| Campo | Detalle |
|---|---|
| Épica | E1 — Gestionar alumnos |
| Módulo | M1 — Gestión de alumnos |
| Rol | Recepcionista (también Administrador) |
| Requisitos | RF03 |
| Reglas de negocio | RN01, RN23 |
| Caso de uso | CU-04 |
| Slices | — |
| Prioridad | Media |
| Estado DoR | Ready |
| Origen | Incorporada por el equipo |

**Historia**

> Como recepcionista, quiero modificar los datos de un alumno ya registrado y que el sistema guarde qué se cambió, para mantener el padrón actualizado sin perder el rastro de las correcciones.

**Criterios de aceptación**

1. Se accede a la edición desde la ficha del alumno.
2. Se pueden modificar teléfono, email, actividades y modalidad de cobro.
3. El DNI solo puede modificarse por el rol Administrador, y el sistema vuelve a validar que no exista otro alumno con ese número.
4. Al guardar, el sistema registra en el historial de cambios qué campo se modificó, el valor anterior, el valor nuevo, el usuario y la fecha y hora.
5. El historial de cambios es consultable desde la misma ficha del alumno.
6. Si no se modificó ningún campo, el sistema informa que no hay cambios para guardar y no genera un registro en el historial.

**INVEST**

| Criterio | Cumple | Observación |
|---|---|---|
| Independiente | Sí | Reutiliza el formulario de HU-01 en modo edición. No requiere que ninguna otra historia esté terminada. |
| Negociable | Sí | Qué campos son editables por cada rol es una decisión de la dirección. |
| Valiosa | Sí | Sin esta historia el padrón se desactualiza en pocas semanas: los alumnos cambian de teléfono y de actividad con frecuencia. |
| Estimable | Sí | El único punto de incertidumbre era el historial de cambios, resuelto al reutilizar el log de auditoría de RNF05. |
| Pequeña | Sí | Solo modificación de datos. La baja y la reactivación son historias separadas. |
| Verificable | Sí | Se comprueba modificando un campo y consultando el historial resultante. |

**Observaciones**

- El requisito RF03 exige registrar el historial de cambios. El equipo decidió no crear una estructura propia para eso: se reutiliza el log de auditoría que RNF05 ya obliga a implementar, filtrado por alumno.
- La restricción del criterio 3 surge de RN01. El DNI es la identidad del alumno, así que modificarlo es una operación excepcional que corresponde al Administrador.

---

### HU-07 — Dar de baja a un alumno

| Campo | Detalle |
|---|---|
| Épica | E1 — Gestionar alumnos |
| Módulo | M1 — Gestión de alumnos |
| Rol | Recepcionista (también Administrador) |
| Requisitos | RF04 |
| Reglas de negocio | RN02, RN03 |
| Caso de uso | CU-05 |
| Slices | — |
| Prioridad | Alta |
| Estado DoR | Ready |
| Origen | Incorporada por el equipo |

**Historia**

> Como recepcionista, quiero dar de baja a un alumno que deja de asistir, indicando la fecha y el motivo, para sacarlo del padrón activo sin perder su historial de pagos y asistencias.

**Criterios de aceptación**

1. Se accede a la baja desde la ficha del alumno.
2. El sistema solicita el motivo de la baja, elegido de una lista, y permite agregar una observación opcional.
3. La fecha de baja se registra automáticamente con la fecha del día, y puede ajustarse manualmente.
4. Al confirmar, el alumno pasa a estado Baja y deja de aparecer en el padrón activo y en el reporte de morosos.
5. El registro del alumno no se elimina. Su historial de pagos, inscripciones y asistencias se conserva completo.
6. El sistema pide una confirmación explícita antes de ejecutar la baja.
7. Si el alumno tiene cuotas impagas, el sistema lo informa antes de confirmar, pero permite continuar con la baja.

**INVEST**

| Criterio | Cumple | Observación |
|---|---|---|
| Independiente | Sí | Solo cambia el estado del alumno y agrega fecha y motivo. No depende de otras historias. |
| Negociable | Sí | Los motivos de baja de la lista son una definición de negocio ajustable. |
| Valiosa | Sí | Resuelve el problema P2: la planilla actual obliga a elegir entre borrar el registro y perder el historial, o conservarlo y ensuciar el padrón. |
| Estimable | Sí | Es un cambio de estado con dos campos adicionales. |
| Pequeña | Sí | No incluye la reactivación, que es HU-08. |
| Verificable | Sí | Se comprueba dando de baja a un alumno y verificando que no aparece en el padrón activo pero sí conserva sus registros. |

**Observaciones**

- El criterio 7 se agregó durante el refinamiento. El equipo discutió si una cuota impaga debía impedir la baja y decidió que no: si el alumno dejó de venir, el centro necesita registrarlo aunque la deuda quede pendiente. El aviso es informativo, no bloqueante.
- Motivos de baja definidos con la dirección: mudanza, motivos económicos, lesión o problema de salud, falta de tiempo, cambio de gimnasio, sin especificar.

---

### HU-08 — Reactivar un alumno dado de baja

| Campo | Detalle |
|---|---|
| Épica | E1 — Gestionar alumnos |
| Módulo | M1 — Gestión de alumnos |
| Rol | Recepcionista (también Administrador) |
| Requisitos | RF05 |
| Reglas de negocio | RN02, RN03, RN04 |
| Caso de uso | CU-06 |
| Slices | — |
| Prioridad | Media |
| Estado DoR | Ready |
| Origen | Incorporada por el equipo |

**Historia**

> Como recepcionista, quiero reactivar a un alumno que vuelve al centro después de haberse dado de baja, para que retome su actividad conservando su legajo y su historial sin volver a cargar todos sus datos.

**Criterios de aceptación**

1. Al intentar registrar un alumno cuyo DNI corresponde a uno dado de baja, el sistema lo detecta y ofrece reactivarlo en lugar de crear un registro nuevo.
2. También se puede reactivar desde la búsqueda de alumnos, filtrando por estado Baja.
3. Al reactivar, el alumno vuelve a estado Activo y conserva su número de legajo original.
4. El historial previo de pagos, inscripciones y asistencias queda disponible en su ficha.
5. El sistema permite revisar y actualizar los datos de contacto y la modalidad de cobro durante la reactivación.
6. La reactivación queda registrada en el log de auditoría con fecha, hora y usuario.
7. Las inscripciones a clases anteriores no se restauran automáticamente: el alumno debe volver a inscribirse.

**INVEST**

| Criterio | Cumple | Observación |
|---|---|---|
| Independiente | Sí | Es la operación inversa de HU-07 sobre el mismo campo de estado. Puede desarrollarse en paralelo. |
| Negociable | Sí | Si conviene restaurar o no las inscripciones previas fue una decisión discutida con la dirección. |
| Valiosa | Sí | Vitalis tiene alta rotación estacional. Reactivar en dos pasos evita recargar todos los datos y preserva la antigüedad del alumno. |
| Estimable | Sí | Cambio de estado más la detección del DNI existente. |
| Pequeña | Sí | Solo la reactivación. |
| Verificable | Sí | Se comprueba dando de baja a un alumno, reactivándolo y verificando que conserva legajo e historial. |

**Observaciones**

- El criterio 7 surge de una consulta a la dirección: la grilla cambia entre temporadas, de modo que restaurar automáticamente inscripciones de meses atrás anotaría al alumno en clases que pueden haber cambiado de horario o dejado de existir. Es consistente con RN13.
- El criterio 1 conecta esta historia con HU-01. La validación de DNI duplicado de RF02 pasa a tener dos salidas: bloquear si el alumno está activo, ofrecer reactivación si está de baja.

---

### HU-09 — Buscar y listar alumnos

| Campo | Detalle |
|---|---|
| Épica | E1 — Gestionar alumnos |
| Módulo | M1 — Gestión de alumnos |
| Rol | Recepcionista, Administrador; Instructor con acceso de consulta |
| Requisitos | RF24 |
| Reglas de negocio | — |
| Caso de uso | — |
| Slices | — |
| Prioridad | Alta |
| Estado DoR | Ready |
| Origen | Incorporada por el equipo |

**Historia**

> Como recepcionista, quiero buscar un alumno por apellido, DNI o legajo y filtrar el padrón, para encontrar rápidamente a quien tengo enfrente en el mostrador sin recorrer una lista de más de doscientos registros.

**Criterios de aceptación**

1. Se puede buscar por apellido, DNI o número de legajo desde un único campo de búsqueda.
2. La búsqueda por apellido admite coincidencias parciales y no distingue mayúsculas, minúsculas ni acentos.
3. El listado se puede filtrar por estado (Activo, Baja o todos), por actividad y por turno.
4. El listado muestra legajo, apellido y nombre, DNI, actividad, estado y situación de cuenta.
5. Por defecto, el listado muestra únicamente alumnos activos.
6. Si la búsqueda no arroja resultados, el sistema muestra un mensaje indicándolo y ofrece la opción de registrar un nuevo alumno.
7. Desde cada fila del listado se accede a la ficha completa del alumno.

**INVEST**

| Criterio | Cumple | Observación |
|---|---|---|
| Independiente | Sí | Consulta la tabla que HU-01 llena, pero puede desarrollarse en paralelo usando datos de prueba. |
| Negociable | Sí | Qué columnas muestra el listado y qué filtros se ofrecen son ajustables según el uso real. |
| Valiosa | Sí | Sin búsqueda, un padrón de más de doscientos alumnos no es operable. Es lo que convierte los datos cargados en información utilizable. |
| Estimable | Sí | Consulta con filtros sobre una única tabla. |
| Pequeña | Sí | Solo búsqueda y listado. No incluye edición ni exportación. |
| Verificable | Sí | Se comprueba buscando por cada criterio y verificando los resultados esperados. |

**Observaciones**

- Esta historia no existía en el relevamiento. Surgió al planificar la iteración 1: se podía cargar el padrón pero no encontrar a nadie. Motivó la incorporación del requisito adicional RF24.
- El criterio 6 aplica el requisito RNF09: el mensaje no solo informa que no hay resultados, sino que ofrece la acción siguiente.
- El criterio 2 responde a un caso concreto del mostrador: la recepcionista escribe "gomez" y el alumno está cargado como "Gómez".

---

## 4. Épica E2 — Gestionar cuotas y pagos

> Como centro de entrenamiento, necesitamos registrar los pagos y detectar automáticamente a los alumnos con cuota vencida, para dejar de revisar la planilla fila por fila y poder reclamar la deuda a tiempo.

---

### HU-11 — Gestionar las modalidades de cobro

| Campo | Detalle |
|---|---|
| Épica | E2 — Gestionar cuotas y pagos |
| Módulo | M2 — Gestión de cuotas y pagos |
| Rol | Administrador |
| Requisitos | RF07 |
| Reglas de negocio | RN06, RN07, RN08 |
| Caso de uso | CU-07 |
| Slices | — |
| Prioridad | Alta |
| Estado DoR | Ready |
| Origen | Incorporada por el equipo |

**Historia**

> Como directora, quiero administrar las modalidades de cobro y sus montos desde el sistema, para actualizar los precios cuando lo decido sin depender de que alguien modifique el programa.

**Criterios de aceptación**

1. El sistema lista las modalidades vigentes con su descripción y su monto base.
2. Se puede crear una modalidad indicando nombre, descripción, tipo (mensual fija, por clase o combinada) y monto base.
3. Se puede modificar el monto base de una modalidad existente.
4. Al modificar un monto, el sistema advierte cuántos alumnos tienen esa modalidad asignada antes de confirmar.
5. Un cambio de monto no altera las cuotas ya registradas: cada cuota conserva el monto con el que fue cobrada.
6. Una modalidad asignada a por lo menos un alumno no se puede eliminar, solo desactivar. Al desactivarla, deja de ofrecerse en el alta pero los alumnos que la tienen la conservan.
7. El monto base debe ser mayor a cero.

**INVEST**

| Criterio | Cumple | Observación |
|---|---|---|
| Independiente | Sí | Gestiona su propio catálogo. No requiere que el registro de pagos esté terminado. |
| Negociable | Sí | La estructura de precios es una decisión comercial que la dirección puede cambiar en cualquier momento. |
| Valiosa | Sí | Responde a la restricción RE04: la dirección modifica precios sin avisar al equipo técnico. Sin esta historia, cada aumento requeriría tocar el código. |
| Estimable | Sí | Es un mantenimiento de catálogo con validaciones simples. |
| Pequeña | Sí | Solo el catálogo de modalidades. La aplicación de la modalidad al cobro es HU-02. |
| Verificable | Sí | Se comprueba creando una modalidad, asignándola a un alumno y modificando su monto. |

**Observaciones**

- Los valores vigentes a mayo de 2026 son: mensual fija 28.000 pesos, por clase 4.500 pesos y combinada 38.000 pesos (regla RN07).
- El criterio 5 es la razón de la decisión D8: la modalidad y el monto se guardan también en cada cuota, no solo en el alumno. Sin eso, un aumento de precio reescribiría la historia de pagos.
- El criterio 6 evita el problema clásico de perder la referencia de las cuotas históricas al eliminar un catálogo.

---

### HU-02 — Registrar el pago de una cuota

| Campo | Detalle |
|---|---|
| Épica | E2 — Gestionar cuotas y pagos |
| Módulo | M2 — Gestión de cuotas y pagos |
| Rol | Recepcionista (también Administrador) |
| Requisitos | RF06, RF07, RF10 |
| Reglas de negocio | RN03, RN06, RN08, RN10, RN11 |
| Caso de uso | CU-02 |
| Slices | S2.1, S2.2, S2.3 |
| Prioridad | Alta |
| Estado DoR | Ready |
| Origen | Relevamiento inicial |

**Historia**

> Como recepcionista, quiero registrar el pago de la cuota de un alumno indicando el monto y el medio de pago, para mantener el control de ingresos y saber quién está al día.

**Criterios de aceptación**

1. Se busca al alumno por apellido, DNI o número de legajo.
2. Se ingresan monto, fecha, período abonado y medio de pago (efectivo o transferencia).
3. El sistema sugiere el monto según la modalidad de cobro vigente del alumno, y ese monto puede modificarse.
4. El sistema registra el pago y muestra el estado de cuenta actualizado del alumno.
5. El historial de pagos del alumno refleja el nuevo registro de forma inmediata.
6. Si el alumno está dado de baja, el sistema no permite registrar el pago e informa el motivo, sugiriendo reactivarlo primero.
7. Si el monto ingresado es cero o negativo, el sistema rechaza el valor e indica que debe ser mayor a cero.
8. El pago queda asociado a la modalidad y al monto vigentes al momento del cobro.

**INVEST**

| Criterio | Cumple | Observación |
|---|---|---|
| Independiente | Sí | El módulo de pagos puede desarrollarse independientemente del de asistencia o del nutricional. |
| Negociable | Sí | Los medios de pago habilitados y los períodos disponibles son decisiones de negocio ajustables. |
| Valiosa | Sí | Reemplaza el registro manual en Excel y permite detectar automáticamente alumnos morosos. |
| Estimable | Sí | El formulario de pago y la lógica de estado de cuenta son de complejidad conocida. |
| Pequeña | Sí | Cubre solo el acto de registrar el pago. El reporte de morosos es una historia separada. |
| Verificable | Sí | Cada criterio de aceptación puede comprobarse con casos de prueba concretos. |

**Observaciones**

- El criterio 8 se agregó durante el refinamiento, a partir de la regla RN08 y de la decisión D8.
- La historia se divide en tres slices, uno por modalidad de cobro. S2.1 cubre la modalidad mensual fija, que alcanza a la mayoría del padrón, y es la que entra primero al plan de entrega. Ver `slicing.md`.
- El requisito RNF08 limita este flujo a un máximo de cuatro pasos desde la búsqueda del alumno hasta la confirmación. El diseño de pantalla debe respetarlo.

---

### HU-10 — Consultar el historial de pagos de un alumno

| Campo | Detalle |
|---|---|
| Épica | E2 — Gestionar cuotas y pagos |
| Módulo | M2 — Gestión de cuotas y pagos |
| Rol | Recepcionista, Administrador; Alumno sobre sus propios pagos |
| Requisitos | RF10 |
| Reglas de negocio | RN08 |
| Caso de uso | — |
| Slices | — |
| Prioridad | Media |
| Estado DoR | Ready |
| Origen | Incorporada por el equipo |

**Historia**

> Como recepcionista, quiero ver todos los pagos que registró un alumno con su fecha, monto y período, para responder en el momento cuando alguien pregunta si tiene la cuota al día.

**Criterios de aceptación**

1. El historial se accede desde la ficha del alumno.
2. Se muestran fecha de pago, período abonado, monto, modalidad aplicada y medio de pago, ordenados del más reciente al más antiguo.
3. El encabezado muestra el estado de cuenta actual: al día, o moroso con la cantidad de días de mora y el monto adeudado.
4. Se puede filtrar el historial por año.
5. Si el alumno no registra pagos, el sistema informa que no hay pagos registrados e indica la fecha de alta del alumno.
6. El rol Alumno accede únicamente a su propio historial, en modo de solo lectura.

**INVEST**

| Criterio | Cumple | Observación |
|---|---|---|
| Independiente | Sí | Es una consulta de solo lectura sobre datos que HU-02 genera. Se desarrolla con datos de prueba. |
| Negociable | Sí | Las columnas mostradas y los filtros disponibles son ajustables. |
| Valiosa | Sí | Responde la consulta más frecuente del mostrador. Hoy obliga a buscar en la planilla mientras el alumno espera. |
| Estimable | Sí | Consulta con filtro sobre una única tabla. |
| Pequeña | Sí | Solo consulta. No incluye la anulación ni la modificación de pagos. |
| Verificable | Sí | Se comprueba registrando pagos y verificando que aparecen con los datos correctos. |

**Observaciones**

- El criterio 6 concreta el alcance del rol Alumno definido en la decisión D5.
- El equipo evaluó incluir la anulación de un pago mal registrado y decidió dejarla fuera de la versión 1.0. La corrección se resuelve registrando un ajuste, no borrando el registro original, para no romper el criterio de trazabilidad.

---

### HU-03 — Consultar alumnos morosos

| Campo | Detalle |
|---|---|
| Épica | E2 — Gestionar cuotas y pagos |
| Módulo | M2 — Gestión de cuotas y pagos |
| Rol | Administrador |
| Requisitos | RF08, RF09, RF30 |
| Reglas de negocio | RN09, RN25 |
| Caso de uso | CU-03 |
| Slices | S3.1, S3.2, S3.3, S3.4 |
| Prioridad | Alta |
| Estado DoR | Ready |
| Origen | Relevamiento inicial |

**Historia**

> Como directora del gimnasio, quiero ver el listado de alumnos con cuota vencida filtrado por actividad, para tomar acciones de cobranza a tiempo y conocer el impacto financiero de la mora.

**Criterios de aceptación**

1. El sistema lista automáticamente a los alumnos con más de treinta días sin pago registrado.
2. El listado muestra nombre, actividad, días de mora y monto adeudado.
3. Se puede filtrar por actividad (Pilates, Entrenamiento y demás) y por turno.
4. El listado puede exportarse en formato PDF.
5. Si no hay alumnos morosos, el sistema muestra el mensaje "Sin alumnos morosos al día de hoy" y no genera un PDF vacío.
6. El umbral de días es configurable por el Administrador, con treinta días como valor por defecto.
7. Los alumnos con modalidad de cobro por clase no se incluyen en el listado de morosos.
8. Los alumnos dados de baja no figuran en el listado.

**INVEST**

| Criterio | Cumple | Observación |
|---|---|---|
| Independiente | Sí | La lógica de detección de mora puede desarrollarse y probarse independientemente de otros módulos. |
| Negociable | Sí | El umbral de días para considerar mora es un parámetro ajustable por la dirección. |
| Valiosa | Sí | Hoy este análisis requiere revisar manualmente cada fila de la planilla. El sistema lo automatiza completamente. |
| Estimable | Sí | La consulta y el reporte tienen complejidad acotada y definida. |
| Pequeña | Sí | Cubre solo la consulta y la exportación. Las notificaciones automáticas a alumnos son una historia futura. |
| Verificable | Sí | Los criterios son comprobables con datos de prueba que incluyan alumnos al día y morosos. |

**Observaciones**

- El criterio 7 resuelve el punto abierto A3, que la Definition of Ready había dejado pendiente. Según la decisión D12 y la regla RN25, la modalidad por clase no genera morosidad porque el pago es anticipado. Estos alumnos se clasifican como inactivos tras sesenta días sin pagos ni asistencias, dato que se expone en la historia HU-23.
- El criterio 6 corresponde al requisito adicional RF30 y a la decisión D6.
- La historia se divide en cuatro slices. S3.1, el listado en pantalla, concentra la mayor parte del valor: la exportación a PDF de S3.3 se ubica después porque exportar un listado que todavía no se puede ver no tiene sentido.

---

## 5. Épica E3 — Planificar actividades

> Como centro de entrenamiento, necesitamos administrar las disciplinas, los horarios y la asignación de instructores con soporte de múltiples versiones de grilla, para poder reorganizar la temporada sin perder la planificación anterior.

---

### HU-12 — Gestionar disciplinas

| Campo | Detalle |
|---|---|
| Épica | E3 — Planificar actividades |
| Módulo | M3 — Planificación de actividades |
| Rol | Administrador |
| Requisitos | RF11 |
| Reglas de negocio | RN15 |
| Caso de uso | CU-08 |
| Slices | — |
| Prioridad | Alta |
| Estado DoR | Ready |
| Origen | Incorporada por el equipo |

**Historia**

> Como directora, quiero administrar el catálogo de disciplinas que ofrece el centro, para poder incorporar una actividad nueva o discontinuar una existente sin perder el historial de las clases ya dictadas.

**Criterios de aceptación**

1. El sistema lista las disciplinas con su nombre, descripción, turno habitual y estado.
2. Se puede crear una disciplina indicando nombre, descripción y si está dirigida a adultos o a niños.
3. El nombre de la disciplina no puede repetirse.
4. Se puede modificar la descripción y el estado de una disciplina existente.
5. Una disciplina con clases asociadas no se puede eliminar, solo desactivar. Al desactivarla, deja de ofrecerse al crear clases nuevas, pero las clases existentes se conservan.
6. Las disciplinas dirigidas a niños solo pueden asignarse a clases del turno tarde.

**INVEST**

| Criterio | Cumple | Observación |
|---|---|---|
| Independiente | Sí | Es un catálogo autónomo. No requiere que existan clases ni instructores. |
| Negociable | Sí | Qué atributos describen una disciplina puede ajustarse. |
| Valiosa | Sí | Es el prerrequisito de toda la planificación. Sin disciplinas no se pueden crear clases. |
| Estimable | Sí | Mantenimiento de catálogo con validaciones simples. |
| Pequeña | Sí | Solo el catálogo. La creación de clases es HU-13. |
| Verificable | Sí | Se comprueba creando una disciplina y verificando que aparece disponible al crear una clase. |

**Observaciones**

- Durante el modelado se decidió que `Disciplina` fuera una entidad separada y no un campo de texto dentro de `Clase`. El motivo es el mismo que en las modalidades de cobro: cambiar el nombre o la descripción de una disciplina no debe obligar a modificar cada clase.
- El criterio 6 aplica la regla RN15. Las disciplinas infantiles Aeróbica Infantil y Kids Fit and Fun se dictan únicamente en el turno tarde.

---

### HU-13 — Crear una clase y asignar instructor

| Campo | Detalle |
|---|---|
| Épica | E3 — Planificar actividades |
| Módulo | M3 — Planificación de actividades |
| Rol | Administrador |
| Requisitos | RF12 |
| Reglas de negocio | RN12, RN14, RN15 |
| Caso de uso | CU-09 |
| Slices | — |
| Prioridad | Alta |
| Estado DoR | Ready |
| Origen | Incorporada por el equipo |

**Historia**

> Como directora, quiero crear una clase indicando disciplina, día, horario, turno e instructor a cargo, para armar la grilla del centro y que cada profesor sepa qué le toca dictar.

**Criterios de aceptación**

1. Se crea una clase seleccionando disciplina, día de la semana, hora de inicio, hora de fin, turno e instructor.
2. La lista de instructores disponibles solo muestra instructores activos.
3. Si el instructor seleccionado ya tiene otra clase que se superpone en día y horario, el sistema lo informa e impide guardar.
4. La clase se crea siempre dentro de la grilla activa.
5. Se puede modificar una clase existente y reasignarle otro instructor.
6. Una clase con alumnos inscriptos no se puede eliminar, solo desactivar, y el sistema informa cuántos alumnos quedarían afectados.
7. La hora de fin debe ser posterior a la hora de inicio.

**INVEST**

| Criterio | Cumple | Observación |
|---|---|---|
| Independiente | Sí | Depende de que existan disciplinas e instructores, pero eso es una dependencia de datos que se resuelve con registros de prueba. |
| Negociable | Sí | Si una clase puede tener más de un instructor fue una decisión discutida con la dirección. |
| Valiosa | Sí | Es lo que reemplaza la planilla de planificación de actividades. |
| Estimable | Sí | El único punto de cuidado es la validación de superposición horaria, que está acotada. |
| Pequeña | Sí | Solo la creación de clases. El versionado de grilla es HU-14. |
| Verificable | Sí | Se comprueba creando dos clases superpuestas para el mismo instructor y verificando que el sistema lo impide. |

**Observaciones**

- El criterio 3 aplica la regla RN14. La superposición se detectó como un problema real: en la planilla actual nada impide asignar al mismo profesor en dos salones a la misma hora.
- Se acordó con la dirección que cada clase tiene un único instructor a cargo. Si eventualmente hubiera un ayudante, se registra en la observación de la clase y no como un segundo instructor.

---

### HU-14 — Gestionar versiones de grilla

| Campo | Detalle |
|---|---|
| Épica | E3 — Planificar actividades |
| Módulo | M3 — Planificación de actividades |
| Rol | Administrador |
| Requisitos | RF13 |
| Reglas de negocio | RN12, RN13 |
| Caso de uso | CU-10 |
| Slices | S4.1, S4.2, S4.3 |
| Prioridad | Media |
| Estado DoR | Ready |
| Origen | Incorporada por el equipo |

**Historia**

> Como directora, quiero armar la grilla de la temporada siguiente sin pisar la que está funcionando, para reorganizar horarios con tiempo y poder volver a consultar cómo estaba organizada la temporada anterior.

**Criterios de aceptación**

1. El sistema lista las grillas existentes indicando nombre, vigencia y cuál está activa.
2. Se puede crear una grilla nueva a partir de una copia de la grilla activa, con todas sus clases e instructores.
3. Se puede editar una grilla que no está activa sin afectar la operación en curso.
4. Se puede activar una grilla, momento en el cual la anterior pasa a modo de solo lectura.
5. Solo puede haber una grilla activa por vez.
6. Al activar una grilla nueva, el sistema advierte cuántos alumnos quedan con inscripciones en clases que no existen en la grilla entrante.
7. Las grillas anteriores no se eliminan y siguen siendo consultables.

**INVEST**

| Criterio | Cumple | Observación |
|---|---|---|
| Independiente | Sí | Opera sobre la estructura de grillas. Puede desarrollarse una vez que existe la gestión de clases. |
| Negociable | Sí | Qué ocurre con las inscripciones al cambiar de grilla fue objeto de discusión con la dirección. |
| Valiosa | Sí | Resuelve el problema P3: hoy las versiones de grilla conviven en archivos sueltos sin trazabilidad. |
| Estimable | Sí | Tras dividirla en tres slices, cada parte quedó acotada. Sin dividir no se podía estimar. |
| Pequeña | Sí | Se divide en tres slices, cada uno entregable por separado. |
| Verificable | Sí | Se comprueba creando una grilla nueva, activándola y verificando que la anterior queda en solo lectura. |

**Observaciones**

- El slice S4.1 implementa una grilla única activa, que cubre la operación habitual del centro. El versionado real, S4.2 y S4.3, se entrega después porque solo hace falta dos o tres veces al año.
- La entidad `Grilla` existe en el modelo desde el principio aunque en S4.1 tenga un único registro. Dividir en slices no significa ignorar lo que viene, sino no construirlo todavía.
- El criterio 6 responde a una pregunta concreta de la directora: al cambiar la grilla de agosto, ¿qué pasa con los alumnos anotados en un horario que ya no existe? El sistema no los desinscribe automáticamente: los informa para que recepción los recontacte.

---

### HU-15 — Inscribir un alumno a una clase

| Campo | Detalle |
|---|---|
| Épica | E3 — Planificar actividades |
| Módulo | M3 — Planificación de actividades |
| Rol | Recepcionista (también Administrador) |
| Requisitos | RF14 |
| Reglas de negocio | RN03, RN13 |
| Caso de uso | CU-11 |
| Slices | — |
| Prioridad | Alta |
| Estado DoR | Ready |
| Origen | Incorporada por el equipo |

**Historia**

> Como recepcionista, quiero inscribir a un alumno en las clases que va a cursar, para que el instructor tenga su listado armado y se pueda controlar la asistencia.

**Criterios de aceptación**

1. Se accede a la inscripción desde la ficha del alumno o desde la clase.
2. Solo se pueden inscribir alumnos en estado Activo.
3. Solo se ofrecen clases pertenecientes a la grilla activa.
4. Si el alumno está dado de baja, el sistema lo informa e impide la inscripción, sugiriendo reactivarlo primero.
5. Si el alumno ya está inscripto en esa clase, el sistema lo informa y no genera una inscripción duplicada.
6. Un alumno puede estar inscripto en varias clases simultáneamente.
7. El sistema advierte si el alumno se inscribe en dos clases superpuestas en día y horario, pero permite continuar.
8. Se puede dar de baja una inscripción, conservando el historial de asistencias ya registradas.

**INVEST**

| Criterio | Cumple | Observación |
|---|---|---|
| Independiente | Sí | Vincula alumnos y clases, ambos existentes. La lógica de inscripción es propia. |
| Negociable | Sí | Si se permite o no la superposición horaria del alumno fue una decisión de negocio. |
| Valiosa | Sí | Es el eslabón que conecta el padrón con la grilla y habilita todo el control de asistencia. |
| Estimable | Sí | Alta de una relación con validaciones acotadas. |
| Pequeña | Sí | Solo la inscripción. El registro de asistencia es HU-04. |
| Verificable | Sí | Se comprueba inscribiendo un alumno y verificando que aparece en el listado de la clase. |

**Observaciones**

- El criterio 7 es una advertencia y no un bloqueo. La dirección aclaró que hay alumnos que se anotan en dos clases del mismo horario para elegir según el día, y el sistema no debe impedirlo.
- El criterio 8 conecta con la decisión D7. Dar de baja una inscripción no borra las asistencias registradas: el alumno estuvo en esas clases y el dato debe conservarse.

---
