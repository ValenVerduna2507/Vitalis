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

## 6. Épica E4 — Controlar asistencia

> Como centro de entrenamiento, necesitamos que cada instructor registre la asistencia de sus clases desde el salón, para dejar de depender de listas en papel y poder detectar ausencias reiteradas.

---

### HU-04 — Registrar asistencia a clase

| Campo | Detalle |
|---|---|
| Épica | E4 — Controlar asistencia |
| Módulo | M4 — Control de asistencia |
| Rol | Instructor |
| Requisitos | RF15, RF16, RF17 |
| Reglas de negocio | RN16, RN17, RN19 |
| Caso de uso | CU-12 |
| Slices | S5.1, S5.2, S5.3 |
| Prioridad | Alta |
| Estado DoR | Ready |
| Origen | Relevamiento inicial |

**Historia**

> Como instructor, quiero ver el listado de alumnos inscriptos en mi clase del día y poder marcar la asistencia, para llevar un registro digital sin depender de listas en papel.

**Criterios de aceptación**

1. El instructor ve únicamente las clases que tiene asignadas para el día.
2. Al seleccionar una clase, se muestra el listado de alumnos inscriptos con sus nombres.
3. Puede marcar a cada alumno como Presente, Ausente o Justificado.
4. Al confirmar, el sistema guarda la asistencia con fecha y hora del registro.
5. Si un alumno no está inscripto, el instructor puede agregarlo como asistente ocasional para esa clase.
6. No se puede registrar dos veces la asistencia del mismo alumno en la misma clase y fecha. Si ya fue registrada, el sistema muestra los valores cargados y permite corregirlos.
7. Si el instructor no tiene clases asignadas ese día, el sistema lo informa con un mensaje claro.

**INVEST**

| Criterio | Cumple | Observación |
|---|---|---|
| Independiente | Sí | La funcionalidad de asistencia puede desarrollarse sin que el módulo nutricional o el de pagos estén finalizados. |
| Negociable | Sí | Los estados de asistencia pueden ampliarse según necesidad del gimnasio. |
| Valiosa | Sí | Elimina las listas en papel, digitaliza el seguimiento de cada alumno y facilita la detección de ausencias reiteradas. |
| Estimable | Sí | La interfaz de toma de asistencia es simple y de complejidad estimable con claridad. |
| Pequeña | Sí | Cubre solo el registro de asistencia. El historial y los reportes de ausencias son historias separadas. |
| Verificable | Sí | Se puede verificar consultando la base de datos tras el registro o revisando el historial del alumno. |

**Observaciones**

- El criterio 5 fue el que motivó el requisito adicional RF27 y la decisión D7. En el modelo original, toda asistencia derivaba obligatoriamente de una inscripción, lo que hacía imposible registrar al asistente ocasional. El detalle de esa operación se desarrolla en HU-17.
- El criterio 6 aplica la regla RN19.
- Se divide en tres slices. S5.1 entrega solo el listado en pantalla, lo que ya evita que el instructor imprima la lista; S5.2 agrega Presente y Ausente, que son los dos estados de uso diario; S5.3 completa con Justificado y el sello de fecha y hora.
- La restricción RE05 condiciona el diseño: la pantalla se usa desde una tablet, de pie y con poco tiempo entre alumno y alumno.

---

### HU-17 — Registrar un asistente ocasional

| Campo | Detalle |
|---|---|
| Épica | E4 — Controlar asistencia |
| Módulo | M4 — Control de asistencia |
| Rol | Instructor |
| Requisitos | RF27 |
| Reglas de negocio | RN18, RN19 |
| Caso de uso | — |
| Slices | — |
| Prioridad | Media |
| Estado DoR | Ready |
| Origen | Incorporada por el equipo |

**Historia**

> Como instructor, quiero registrar la asistencia de un alumno activo que vino a mi clase sin estar inscripto, para que quede constancia de que participó y no tener que anotarlo aparte en un papel.

**Criterios de aceptación**

1. Desde la pantalla de toma de asistencia, el instructor puede buscar un alumno que no figura en el listado de inscriptos.
2. La búsqueda solo devuelve alumnos en estado Activo.
3. Al agregarlo, el alumno aparece en el listado de esa clase marcado como asistente ocasional.
4. El registro de asistencia se guarda con el mismo tratamiento que el de un alumno inscripto, incluyendo fecha y hora.
5. Agregar un asistente ocasional no genera una inscripción permanente: el alumno no aparece en el listado de la clase del día siguiente.
6. Si el alumno buscado está dado de baja, el sistema lo informa e impide agregarlo.
7. Las asistencias ocasionales se distinguen de las regulares en el historial del alumno.

**INVEST**

| Criterio | Cumple | Observación |
|---|---|---|
| Independiente | Sí | Se apoya en la pantalla de HU-04 pero su lógica es propia y puede desarrollarse después. |
| Negociable | Sí | Si el asistente ocasional debe generar o no una notificación a recepción quedó como definición abierta a la operación. |
| Valiosa | Sí | Es una situación cotidiana en el centro: alumnos que cambian de horario por un día. Hoy se anota en papel o directamente se pierde. |
| Estimable | Sí | Búsqueda de alumno más alta de asistencia sin inscripción asociada. |
| Pequeña | Sí | Solo el alta del asistente ocasional. |
| Verificable | Sí | Se comprueba agregando un alumno no inscripto y verificando que no aparece al día siguiente. |

**Observaciones**

- Esta historia se desprendió del criterio 5 de HU-04 durante el refinamiento. El equipo la separó porque implicaba un cambio en el modelo de datos que HU-04 por sí sola no justificaba.
- El criterio 7 tiene consecuencia directa en HU-23: la ocupación real de una clase incluye a los ocasionales, pero la cantidad de inscriptos no.

---

### HU-16 — Consultar el historial de asistencia de un alumno

| Campo | Detalle |
|---|---|
| Épica | E4 — Controlar asistencia |
| Módulo | M4 — Control de asistencia |
| Rol | Recepcionista, Administrador; Instructor y Alumno con alcance parcial |
| Requisitos | RF18 |
| Reglas de negocio | RN16 |
| Caso de uso | — |
| Slices | — |
| Prioridad | Media |
| Estado DoR | Ready |
| Origen | Incorporada por el equipo |

**Historia**

> Como recepcionista, quiero consultar el historial de asistencia de un alumno, para responder consultas de los familiares y detectar a quienes dejaron de venir antes de que se den de baja.

**Criterios de aceptación**

1. El historial se accede desde la ficha del alumno.
2. Se muestran fecha, clase, disciplina, instructor y estado de asistencia, del más reciente al más antiguo.
3. Se puede filtrar por rango de fechas y por disciplina.
4. Se muestra un resumen con la cantidad de presentes, ausentes y justificados del período consultado.
5. El sistema destaca a los alumnos con más de dos semanas consecutivas sin asistir.
6. Si el alumno no tiene asistencias registradas, el sistema lo informa e indica desde qué fecha está inscripto.
7. El instructor solo ve las asistencias correspondientes a sus propias clases. El alumno solo ve las propias.

**INVEST**

| Criterio | Cumple | Observación |
|---|---|---|
| Independiente | Sí | Es una consulta de solo lectura sobre los datos que genera HU-04. |
| Negociable | Sí | El umbral de dos semanas del criterio 5 es un parámetro ajustable. |
| Valiosa | Sí | Detectar al alumno que dejó de venir antes de que pida la baja permite a la dirección actuar comercialmente. |
| Estimable | Sí | Consulta con filtros y un cálculo de agregación simple. |
| Pequeña | Sí | Solo consulta individual. Los reportes agregados corresponden a la épica E8. |
| Verificable | Sí | Se comprueba registrando asistencias y verificando el historial y el resumen resultantes. |

**Observaciones**

- El criterio 5 no estaba en el requisito original. Se agregó a partir de una necesidad que la directora planteó en el relevamiento: enterarse de que un alumno dejó de venir cuando pide la baja ya es tarde.
- El criterio 7 aplica la matriz de permisos definida en `docs/requisitos.md`.

---

## 7. Épica E5 — Seguimiento nutricional

> Como centro de entrenamiento, necesitamos que la nutricionista registre y consulte la evolución de sus pacientes en un módulo aislado, para eliminar los registros físicos y garantizar la confidencialidad de la información de salud.

---

### HU-05a — Registrar una consulta nutricional

| Campo | Detalle |
|---|---|
| Épica | E5 — Seguimiento nutricional |
| Módulo | M5 — Seguimiento nutricional |
| Rol | Nutricionista |
| Requisitos | RF19, RF21 |
| Reglas de negocio | RN20, RN21, RN26, RN27, RN29 |
| Caso de uso | CU-13 |
| Slices | — |
| Prioridad | Media |
| Estado DoR | Ready |
| Origen | Relevamiento inicial (división de HU-05) |

**Historia**

> Como nutricionista, quiero registrar los datos de cada consulta de mis pacientes, para llevar un seguimiento digital sin usar registros físicos.

**Criterios de aceptación**

1. La nutricionista accede al sistema con su propio usuario y contraseña.
2. Solo puede ver y gestionar a sus propios pacientes. No tiene acceso a datos de pagos ni a la grilla general.
3. Puede registrar una consulta con fecha, peso en kilogramos, altura en centímetros, perímetro de cintura, perímetro de cadera, porcentaje de masa grasa, objetivo del paciente y observaciones.
4. Son obligatorios la fecha, el peso y el objetivo. La altura es obligatoria en la primera consulta y se arrastra a las siguientes, donde puede modificarse.
5. El índice de masa corporal se calcula automáticamente a partir del peso y la altura, y no se ingresa manualmente.
6. Solo se pueden registrar consultas de alumnos en estado Activo.
7. Si el paciente es menor de 18 años, el sistema exige que estén cargados el tutor responsable y la fecha de autorización antes de permitir la primera consulta.
8. Los datos de los pacientes no son visibles para instructores ni recepcionistas.
9. Si se ingresa un peso o una altura fuera de un rango razonable, el sistema pide confirmación antes de guardar.

**INVEST**

| Criterio | Cumple | Observación |
|---|---|---|
| Independiente | Sí | El módulo nutricional es funcionalmente independiente del de cuotas y asistencia. |
| Negociable | Sí | Los campos de cada consulta pueden ajustarse según los requerimientos específicos de la nutricionista. |
| Valiosa | Sí | Elimina los registros físicos y garantiza la confidencialidad del historial clínico de cada alumno. |
| Estimable | Sí | Con los parámetros definidos en la regla RN26, el formulario quedó acotado y estimable. |
| Pequeña | Sí | Cubre solo el registro de la consulta. La consulta del historial es HU-05b. |
| Verificable | Sí | Cada criterio puede comprobarse verificando el acceso por rol y los registros almacenados. |

**Observaciones**

- Esta historia surge de dividir HU-05, que la Definition of Ready había rechazado por agrupar el registro y la consulta en una sola unidad.
- El criterio 3 concreta la regla RN26 y la decisión D11. En la versión original de HU-05 el requisito hablaba genéricamente de "medidas", lo que impedía diseñar el formulario y estimar la historia.
- El criterio 7 aplica la regla RN27 y la decisión D14. Es relevante porque el centro dicta disciplinas infantiles.
- El criterio 9 se agregó a pedido de la nutricionista: un error de tipeo en el peso distorsiona toda la curva de evolución del paciente.
- Según la regla RN29 y la decisión D10, el Administrador puede crear el usuario de la nutricionista y asignarle pacientes, pero no accede al contenido de las consultas.

---

### HU-05b — Consultar la evolución de un paciente

| Campo | Detalle |
|---|---|
| Épica | E5 — Seguimiento nutricional |
| Módulo | M5 — Seguimiento nutricional |
| Rol | Nutricionista |
| Requisitos | RF20, RF21 |
| Reglas de negocio | RN20, RN26, RN28, RN29 |
| Caso de uso | CU-14 |
| Slices | — |
| Prioridad | Media |
| Estado DoR | Ready |
| Origen | Relevamiento inicial (división de HU-05) |

**Historia**

> Como nutricionista, quiero consultar la evolución histórica de cada paciente, para comparar los valores entre consultas y ajustar el plan alimentario con información concreta.

**Criterios de aceptación**

1. Se accede al historial desde la ficha del paciente.
2. Se listan todas las consultas registradas con fecha, peso, índice de masa corporal, perímetros, porcentaje de masa grasa y observaciones.
3. El listado se ordena de la consulta más reciente a la más antigua.
4. Se muestra la variación de peso y de índice de masa corporal respecto de la consulta anterior y respecto de la primera.
5. Se puede filtrar el historial por rango de fechas.
6. Si el paciente tiene una sola consulta registrada, el sistema lo indica y no muestra comparaciones.
7. El acceso está restringido al rol Nutricionista. Ningún otro rol accede al contenido de las consultas.
8. El rol Alumno no accede a su propio historial nutricional a través del sistema.

**INVEST**

| Criterio | Cumple | Observación |
|---|---|---|
| Independiente | Sí | Es una consulta de solo lectura sobre los datos que genera HU-05a. |
| Negociable | Sí | Qué comparaciones se muestran y sobre qué parámetros es ajustable con la nutricionista. |
| Valiosa | Sí | Es el objetivo del seguimiento: sin comparación entre consultas, registrar los datos no sirve para decidir. |
| Estimable | Sí | Consulta con filtros más el cálculo de variaciones entre registros. |
| Pequeña | Sí | Solo la consulta del historial. |
| Verificable | Sí | Se comprueba registrando dos consultas y verificando que la variación calculada es correcta. |

**Observaciones**

- El criterio 8 aplica la regla RN28 y la decisión D13. La devolución de resultados se realiza en el consultorio, a criterio de la nutricionista. La incorporación de una vista para el paciente quedó registrada como cuestión diferida B1 en `docs/requisitos.md`.
- El equipo evaluó incluir un gráfico de evolución y lo dejó fuera de la versión 1.0. La tabla con las variaciones cubre la necesidad; el gráfico es una mejora posterior.

---

## 8. Épica E6 — Gestionar instructores

> Como centro de entrenamiento, necesitamos administrar el plantel de instructores y que cada uno pueda ver su propia agenda, para asignarlos a las clases y que dejen de depender de recepción para saber qué les toca dictar.

---

### HU-18 — Registrar y dar de baja instructores

| Campo | Detalle |
|---|---|
| Épica | E6 — Gestionar instructores |
| Módulo | M6 — Gestión de instructores |
| Rol | Administrador |
| Requisitos | RF25 |
| Reglas de negocio | RN02, RN14 |
| Caso de uso | CU-15 |
| Slices | — |
| Prioridad | Alta |
| Estado DoR | Ready |
| Origen | Incorporada por el equipo |

**Historia**

> Como directora, quiero registrar a los instructores del centro con su especialidad y poder darlos de baja cuando dejan de trabajar acá, para asignarlos a las clases y conservar el registro de quién dictó cada clase.

**Criterios de aceptación**

1. Se registra un instructor con apellido, nombre, DNI, teléfono, email y especialidad o especialidades.
2. El sistema valida que el DNI no esté duplicado entre los instructores.
3. Se pueden modificar los datos y las especialidades de un instructor existente.
4. La baja es lógica: el instructor pasa a estado Inactivo y su historial se conserva.
5. Un instructor inactivo no aparece en la lista de instructores disponibles al crear o modificar una clase.
6. Si se intenta dar de baja a un instructor con clases asignadas en la grilla activa, el sistema informa cuáles son y solicita reasignarlas antes de confirmar.
7. Un instructor inactivo puede reactivarse conservando su registro.

**INVEST**

| Criterio | Cumple | Observación |
|---|---|---|
| Independiente | Sí | Es un mantenimiento autónomo. No requiere que existan clases. |
| Negociable | Sí | Qué datos se registran de cada instructor es ajustable. |
| Valiosa | Sí | Es prerrequisito de RF12: no se puede asignar a las clases un instructor que no está cargado. |
| Estimable | Sí | Alta, baja y modificación con validación de duplicados, análoga a HU-01. |
| Pequeña | Sí | Solo la gestión del plantel. La asignación a clases es HU-13. |
| Verificable | Sí | Se comprueba dando de baja a un instructor con clases y verificando que el sistema pide reasignarlas. |

**Observaciones**

- El módulo de instructores figuraba en el alcance del relevamiento y la entidad existía en el modelo, pero no había requisitos que definieran cómo se administra. De ahí surge el requisito adicional RF25.
- El criterio 6 es una diferencia con la baja de alumnos: dar de baja a un instructor deja clases sin dictar, así que el sistema no puede permitirlo sin advertencia. En cambio la baja de un alumno no bloquea nada.
- Los instructores no son alumnos del centro, según el supuesto S06. Si esa condición cambiara, una misma persona tendría dos registros.

---

### HU-19 — Consultar la agenda propia

| Campo | Detalle |
|---|---|
| Épica | E6 — Gestionar instructores |
| Módulo | M6 — Gestión de instructores |
| Rol | Instructor |
| Requisitos | RF26 |
| Reglas de negocio | RN12 |
| Caso de uso | — |
| Slices | — |
| Prioridad | Media |
| Estado DoR | Ready |
| Origen | Incorporada por el equipo |

**Historia**

> Como instructor, quiero ver mi agenda de clases de la semana, para saber qué tengo asignado sin tener que preguntar en recepción cada vez que cambia la grilla.

**Criterios de aceptación**

1. El instructor accede a su agenda desde el menú principal, sin necesidad de buscarse a sí mismo.
2. La agenda muestra las clases de la grilla activa que tiene asignadas, organizadas por día y horario.
3. De cada clase se muestran disciplina, día, horario, turno y cantidad de alumnos inscriptos.
4. Desde cada clase del día en curso se puede acceder directamente a la toma de asistencia.
5. El instructor no ve las clases asignadas a otros instructores.
6. Si el instructor no tiene clases asignadas en la grilla activa, el sistema lo informa con un mensaje claro.

**INVEST**

| Criterio | Cumple | Observación |
|---|---|---|
| Independiente | Sí | Es una consulta filtrada sobre las clases que crea HU-13. |
| Negociable | Sí | Si la agenda se muestra por semana o por día es una decisión de diseño ajustable. |
| Valiosa | Sí | Ataca directamente el problema P4: hoy el instructor no puede consultar nada por sí mismo. |
| Estimable | Sí | Consulta con filtro por instructor y por grilla activa. |
| Pequeña | Sí | Solo la vista de agenda. |
| Verificable | Sí | Se comprueba iniciando sesión como instructor y verificando que solo ve sus propias clases. |

**Observaciones**

- El criterio 4 conecta esta historia con HU-04. En la práctica, la agenda es la pantalla de entrada del instructor: desde ahí llega a la toma de asistencia con un solo paso.
- El criterio 5 depende del control de acceso por rol de la épica E7. Es una dependencia declarada.

---

## 9. Épica E7 — Seguridad, usuarios y roles

> Como centro de entrenamiento, necesitamos que cada persona acceda al sistema con su propio usuario y vea únicamente lo que le corresponde, para proteger la información sensible y saber quién realizó cada operación.

---

### HU-20 — Iniciar sesión en el sistema

| Campo | Detalle |
|---|---|
| Épica | E7 — Seguridad, usuarios y roles |
| Módulo | M7 — Seguridad, usuarios y roles |
| Rol | Todos |
| Requisitos | RF22 |
| Reglas de negocio | RN22, RN24 |
| Caso de uso | CU-00 |
| Slices | S6.1, S6.2, S6.3 |
| Prioridad | Alta |
| Estado DoR | Ready |
| Origen | Incorporada por el equipo |

**Historia**

> Como usuario del sistema, quiero iniciar sesión con mi propio usuario y contraseña, para acceder únicamente a las funciones que me corresponden y que quede registrado quién hizo cada operación.

**Criterios de aceptación**

1. El sistema solicita nombre de usuario y contraseña antes de permitir cualquier operación.
2. Si las credenciales son correctas, el usuario accede al panel principal correspondiente a su rol.
3. El menú muestra únicamente las opciones habilitadas para el rol del usuario.
4. Si las credenciales son incorrectas, el sistema muestra un mensaje genérico que no revela si el error está en el usuario o en la contraseña.
5. Un usuario desactivado no puede iniciar sesión, y el sistema le indica que contacte al administrador.
6. Las contraseñas se almacenan cifradas y no son visibles para ningún usuario, incluido el Administrador.
7. El usuario puede cerrar sesión de forma explícita desde cualquier pantalla.
8. Intentar acceder por dirección directa a una función no permitida devuelve un mensaje de acceso denegado.

**INVEST**

| Criterio | Cumple | Observación |
|---|---|---|
| Independiente | Sí | No depende de ninguna funcionalidad de negocio. Es la base sobre la que se apoyan las demás. |
| Negociable | Sí | La política de contraseñas y la duración de la sesión son ajustables. |
| Valiosa | Sí | Su valor no es visible por sí solo, pero sin identificación del usuario no se puede aplicar el control por roles ni el log de auditoría que exige RNF05. |
| Estimable | Sí | Autenticación con cifrado de contraseña y control de sesión, de alcance conocido. |
| Pequeña | Sí | Solo el acceso. La gestión de usuarios es HU-21. |
| Verificable | Sí | Se comprueba iniciando sesión con cada rol y verificando qué opciones ve y a qué accede. |

**Observaciones**

- Es la única historia del backlog que se planifica primero sin entregar valor de negocio directo. La justificación está en `slicing.md`: agregar la identificación del usuario después obliga a revisar todas las operaciones ya construidas.
- El criterio 4 es una práctica de seguridad: un mensaje que diga "el usuario no existe" permite averiguar qué usuarios son válidos.
- El criterio 8 corresponde al slice S6.3 y es lo que hace efectivo el requisito RNF06 sobre el módulo nutricional. Ocultar la opción del menú no alcanza.

---

### HU-21 — Gestionar usuarios y roles

| Campo | Detalle |
|---|---|
| Épica | E7 — Seguridad, usuarios y roles |
| Módulo | M7 — Seguridad, usuarios y roles |
| Rol | Administrador |
| Requisitos | RF23, RF31 |
| Reglas de negocio | RN22, RN23, RN24, RN29 |
| Caso de uso | CU-16 |
| Slices | — |
| Prioridad | Alta |
| Estado DoR | Ready |
| Origen | Incorporada por el equipo |

**Historia**

> Como directora, quiero crear los usuarios del sistema y asignarle a cada uno su rol, para que cada empleado acceda solo a lo que necesita y poder revisar después quién hizo cada operación.

**Criterios de aceptación**

1. El sistema lista los usuarios con nombre, rol, estado y fecha del último acceso.
2. Se puede crear un usuario indicando nombre, nombre de usuario, rol y una contraseña inicial.
3. Cada usuario tiene un único rol asignado, elegido entre Administrador, Recepcionista, Instructor, Nutricionista y Alumno.
4. El nombre de usuario no puede repetirse.
5. Se puede desactivar un usuario. El usuario desactivado no puede iniciar sesión, pero sus registros en el log de auditoría se conservan.
6. Se puede modificar el rol de un usuario existente, y el cambio queda registrado en el log de auditoría.
7. El sistema impide desactivar al último usuario con rol Administrador.
8. El Administrador puede consultar el log de auditoría filtrando por usuario, tipo de operación y rango de fechas.
9. El log de auditoría es de solo lectura: no puede modificarse ni eliminarse desde el sistema.

**INVEST**

| Criterio | Cumple | Observación |
|---|---|---|
| Independiente | Sí | Gestiona su propio conjunto de datos. Requiere que exista el mecanismo de autenticación de HU-20. |
| Negociable | Sí | Si el sistema debe forzar el cambio de la contraseña inicial en el primer acceso quedó como definición abierta. |
| Valiosa | Sí | Sin esta historia, cada alta de empleado dependería de una intervención técnica sobre la base de datos. |
| Estimable | Sí | Mantenimiento de usuarios más consulta filtrada del log. |
| Pequeña | Sí | El equipo evaluó separar la consulta del log en una historia propia y decidió mantenerla junta: son pocas pantallas y comparten el mismo contexto de administración. |
| Verificable | Sí | Se comprueba creando un usuario de cada rol, verificando el acceso resultante y consultando el log. |

**Observaciones**

- El criterio 7 evita un bloqueo del sistema: si se desactiva al último Administrador, nadie puede volver a crear usuarios.
- El criterio 9 aplica la regla RN23. Un log de auditoría modificable no cumple su función.
- Según la regla RN29 y la decisión D10, la gestión del usuario de la nutricionista y la asignación de sus pacientes corresponden a este rol, pero eso no habilita al Administrador a ver el contenido de las consultas nutricionales.

---

## 10. Épica E8 — Reportes de gestión

> Como centro de entrenamiento, necesitamos información agregada sobre ingresos y ocupación, para tomar decisiones comerciales con datos y no por intuición.

---

### HU-22 — Consultar el reporte de ingresos

| Campo | Detalle |
|---|---|
| Épica | E8 — Reportes de gestión |
| Módulo | M8 — Reportes de gestión |
| Rol | Administrador |
| Requisitos | RF28 |
| Reglas de negocio | RN06, RN08 |
| Caso de uso | — |
| Slices | — |
| Prioridad | Baja |
| Estado DoR | Ready |
| Origen | Incorporada por el equipo |

**Historia**

> Como directora, quiero ver cuánto ingresó el centro en un período discriminado por modalidad y por disciplina, para saber qué actividades sostienen el negocio y proyectar los meses siguientes.

**Criterios de aceptación**

1. Se selecciona un período indicando fecha desde y fecha hasta.
2. El reporte muestra el total recaudado en el período y la cantidad de pagos registrados.
3. El total se discrimina por modalidad de cobro y por disciplina.
4. Se muestra la comparación con el período inmediato anterior de igual duración.
5. El reporte puede exportarse en formato PDF.
6. Si no hay pagos registrados en el período seleccionado, el sistema lo informa y no genera un archivo vacío.
7. La fecha desde no puede ser posterior a la fecha hasta.

**INVEST**

| Criterio | Cumple | Observación |
|---|---|---|
| Independiente | Sí | Es una consulta de solo lectura sobre los pagos que registra HU-02. |
| Negociable | Sí | Qué agrupaciones y comparaciones incluye el reporte es ajustable con la dirección. |
| Valiosa | Sí | Hoy la proyección de ingresos no existe: la variabilidad de modalidades del problema P1 la hace inviable de calcular a mano. |
| Estimable | Sí | Consulta con agrupaciones y exportación, análoga a la de HU-03. |
| Pequeña | Sí | Un solo reporte. La ocupación de clases es HU-23. |
| Verificable | Sí | Se comprueba registrando pagos conocidos y verificando que los totales coinciden. |

**Observaciones**

- El reporte se calcula sobre el monto efectivamente cobrado en cada cuota, no sobre el monto vigente de la modalidad. Esto es consecuencia de la regla RN08 y de la decisión D8: si se calculara con los precios actuales, un aumento distorsionaría retroactivamente todos los períodos anteriores.
- Es una de las historias de menor prioridad porque depende de que haya un volumen de pagos cargado. Un reporte sobre dos meses de datos no permite proyectar nada.

---

### HU-23 — Consultar la ocupación de las clases

| Campo | Detalle |
|---|---|
| Épica | E8 — Reportes de gestión |
| Módulo | M8 — Reportes de gestión |
| Rol | Administrador |
| Requisitos | RF29 |
| Reglas de negocio | RN18, RN25 |
| Caso de uso | — |
| Slices | — |
| Prioridad | Baja |
| Estado DoR | Ready |
| Origen | Incorporada por el equipo |

**Historia**

> Como directora, quiero ver cuántos alumnos tiene inscriptos cada clase y qué porcentaje asiste realmente, para decidir qué horarios conviene sostener, reforzar o discontinuar.

**Criterios de aceptación**

1. El reporte lista las clases de la grilla activa con disciplina, día, horario, turno e instructor.
2. De cada clase se muestran la cantidad de alumnos inscriptos y el porcentaje promedio de asistencia del período seleccionado.
3. Las asistencias ocasionales se cuentan en el porcentaje de asistencia pero no en la cantidad de inscriptos.
4. Se puede ordenar el listado por cantidad de inscriptos o por porcentaje de asistencia.
5. Se puede filtrar por turno, por disciplina y por instructor.
6. El sistema destaca las clases con menos de tres inscriptos.
7. Se incluye un listado de alumnos inactivos: aquellos con modalidad por clase que no registran pagos ni asistencias en los últimos sesenta días.
8. Si la grilla activa no tiene clases cargadas, el sistema lo informa.

**INVEST**

| Criterio | Cumple | Observación |
|---|---|---|
| Independiente | Sí | Consulta de solo lectura sobre inscripciones y asistencias existentes. |
| Negociable | Sí | El umbral de tres inscriptos del criterio 6 es un parámetro ajustable. |
| Valiosa | Sí | Permite decidir sobre la grilla con datos. Hoy la decisión de abrir o cerrar un horario se toma por percepción. |
| Estimable | Sí | Consulta con agregaciones sobre dos tablas. |
| Pequeña | Sí | Un solo reporte. |
| Verificable | Sí | Se comprueba con una clase de inscriptos y asistencias conocidas, verificando el porcentaje calculado. |

**Observaciones**

- El criterio 3 aplica la distinción que introdujo HU-17. Una clase con pocos inscriptos pero muchos ocasionales tiene demanda real, y ese matiz se pierde si se cuentan juntos.
- El criterio 7 es la contrapartida de la decisión D12. Como los alumnos con modalidad por clase no aparecen en el reporte de morosos, este reporte es el que los vuelve visibles para la dirección, bajo la categoría de inactivos.
- El criterio 6 responde a una necesidad concreta: la directora quiere identificar rápido los horarios que no se sostienen.

---

## 11. Matriz de cobertura de requisitos

Verificación de que todo requisito funcional tiene al menos una historia asociada y que toda historia responde a un requisito.

| Requisito | Historias | Módulo |
|---|---|---|
| RF01 | HU-01 | M1 |
| RF02 | HU-01, HU-08 | M1 |
| RF03 | HU-06 | M1 |
| RF04 | HU-07 | M1 |
| RF05 | HU-08 | M1 |
| RF06 | HU-02 | M2 |
| RF07 | HU-02, HU-11 | M2 |
| RF08 | HU-03 | M2 |
| RF09 | HU-03 | M2 |
| RF10 | HU-02, HU-10 | M2 |
| RF11 | HU-12 | M3 |
| RF12 | HU-13 | M3 |
| RF13 | HU-14 | M3 |
| RF14 | HU-15 | M3 |
| RF15 | HU-04 | M4 |
| RF16 | HU-04 | M4 |
| RF17 | HU-04 | M4 |
| RF18 | HU-16 | M4 |
| RF19 | HU-05a | M5 |
| RF20 | HU-05b | M5 |
| RF21 | HU-05a, HU-05b | M5 |
| RF22 | HU-20 | M7 |
| RF23 | HU-21 | M7 |
| RF24 | HU-09 | M1 |
| RF25 | HU-18 | M6 |
| RF26 | HU-19 | M6 |
| RF27 | HU-04, HU-17 | M4 |
| RF28 | HU-22 | M8 |
| RF29 | HU-23 | M8 |
| RF30 | HU-03 | M2 |
| RF31 | HU-21 | M7 |

### Verificación

| Control | Resultado |
|---|---|
| Requisitos funcionales sin historia asociada | Ninguno. Los 31 requisitos están cubiertos. |
| Historias sin requisito asociado | Ninguna. Las 24 historias derivan de al menos un requisito. |
| Historias evaluadas contra la Definition of Ready | 24 de 24. |
| Historias en estado Ready | 24 de 24. |
| Historias con criterios de aceptación que contemplan camino alternativo o de error | 24 de 24, según el criterio C9. |

---

## Historial de versiones

| Versión | Fecha | Cambio |
|---|---|---|
| 1.0 | 05/2026 | Versión del relevamiento inicial. Cinco historias, una por módulo: HU-01 a HU-05. |
| 1.1 | 05/2026 | División de HU-05 en HU-05a y HU-05b por acción correctiva de la Definition of Ready. Incorporación de HU-06 a HU-23 para cubrir los requisitos sin historia asociada y los requisitos adicionales RF22 a RF31. Incorporación de la ficha de trazabilidad, del estado DoR y de la matriz de cobertura. |

---

<sub>Escuela Superior de Comercio N° 49 "Justo José de Urquiza" — Desarrollo Web / Analista Funcional de Sistemas — 2026.</sub>
