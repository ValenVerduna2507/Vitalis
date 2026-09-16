# Casos de uso

Sistema de Gestión Integral — Vitalis Centro de Entrenamiento
Equipo: Grupo 02
Versión: 1.1

---

## Índice

- [1. Cómo leer este documento](#1-cómo-leer-este-documento)
- [2. Actores del sistema](#2-actores-del-sistema)
- [3. Relaciones UML utilizadas](#3-relaciones-uml-utilizadas)
- [4. Listado de casos de uso](#4-listado-de-casos-de-uso)
- [5. Módulo transversal: seguridad](#5-módulo-transversal-seguridad)
- [6. Módulo 1: gestión de alumnos](#6-módulo-1-gestión-de-alumnos)
- [7. Módulo 2: gestión de cuotas y pagos](#7-módulo-2-gestión-de-cuotas-y-pagos)
- [8. Módulo 3: planificación de actividades](#8-módulo-3-planificación-de-actividades)
- [9. Módulo 4: control de asistencia](#9-módulo-4-control-de-asistencia)
- [10. Módulo 5: seguimiento nutricional](#10-módulo-5-seguimiento-nutricional)
- [11. Módulo 6: gestión de instructores](#11-módulo-6-gestión-de-instructores)
- [12. Matriz de trazabilidad](#12-matriz-de-trazabilidad)

---

## 1. Cómo leer este documento

Cada caso de uso se documenta con la estructura propuesta por Alistair Cockburn, adaptada al formato de la cátedra.

| Campo | Contenido |
|---|---|
| ID y nombre | Identificador estable y nombre en infinitivo. |
| Descripción | Qué logra el actor con este caso de uso. |
| Actor principal | Quien inicia el caso de uso y obtiene el valor. |
| Actores secundarios | Quienes participan sin iniciarlo. |
| Precondiciones | Qué debe ser verdadero antes de comenzar. |
| Postcondiciones | En qué estado queda el sistema al terminar, en éxito y en fallo. |
| Flujo normal | Secuencia principal, con la acción del actor y la reacción del sistema. |
| Flujos alternativos | Caminos válidos distintos del principal. |
| Excepciones | Situaciones de error y cómo responde el sistema. |
| Reglas de negocio | Reglas de `docs/requisitos.md` que se aplican. |
| Rendimiento | Exigencia de tiempo de respuesta. |
| Frecuencia estimada | Cuántas veces se ejecuta y en qué momentos. |
| Importancia | Vital, Alta o Media, según su peso en la operación. |
| Comentarios | Decisiones de diseño y observaciones del refinamiento. |

**Diferencia con las historias de usuario.** La historia responde *qué* necesita el usuario y *para qué*. El caso de uso responde *cómo* interactúa con el sistema, paso a paso, incluyendo qué pasa cuando algo sale mal. Una historia puede no tener caso de uso si es una consulta simple sin flujos alternativos relevantes.

---

## 2. Actores del sistema

| Actor | Tipo | Descripción |
|---|---|---|
| **Administrador** | Primario | Propietaria y directora del centro. Configura el sistema, consulta reportes y gestiona usuarios. |
| **Recepcionista** | Primario | Personal administrativo. Es el usuario más frecuente: opera el padrón, los cobros y las inscripciones. |
| **Instructor** | Primario | Profesor a cargo de una o más clases. Registra asistencia y consulta su agenda. |
| **Nutricionista** | Primario | Profesional del consultorio nutricional. Opera exclusivamente en el módulo M5. |
| **Alumno** | Primario | Beneficiario del servicio. Accede en modo de solo consulta a su propia información. |
| **Sistema de auditoría** | Secundario | Componente interno que registra las operaciones sensibles exigidas por RNF05. No es una persona. |

> El Administrador puede ejecutar cualquier caso de uso de los roles operativos, con la excepción del módulo nutricional. Para no repetir esa aclaración en cada ficha, se indica únicamente el actor habitual de cada caso.

---

## 3. Relaciones UML utilizadas

### 3.1 — Relaciones aplicadas

| Relación | Significado | Uso en este sistema |
|---|---|---|
| **Asociación** | El actor participa en el caso de uso. | Todas las líneas entre actores y casos de uso. |
| **`<<include>>`** | El caso base **siempre** ejecuta el caso incluido. La flecha va del caso base hacia el incluido. | Todos los casos de uso incluyen CU-00 Autenticar usuario, porque ninguna operación se ejecuta sin sesión iniciada. |
| **`<<extend>>`** | El caso extensión se ejecuta **solo en ciertas condiciones**, en un punto de extensión del caso base. La flecha va del caso extensión hacia el caso base. | Tres casos: exportación del reporte, registro de asistente ocasional y reactivación durante el alta. |

### 3.2 — Relaciones `<<include>>` del sistema

| Caso base | Caso incluido | Motivo |
|---|---|---|
| Todos los casos de uso | CU-00 Autenticar usuario | Ninguna operación se ejecuta sin sesión iniciada (RF22, RNF03). |
| CU-01 Registrar alumno | CU-00 | El alta requiere un usuario identificado para el log de auditoría. |
| CU-02 Registrar pago | CU-00 | Ídem. |
| CU-12 Registrar asistencia | CU-00 | El sistema debe saber qué instructor registró la asistencia. |

### 3.3 — Relaciones `<<extend>>` del sistema

| Caso extensión | Caso base | Punto de extensión | Condición |
|---|---|---|---|
| Exportar reporte a PDF | CU-03 Consultar alumnos morosos | Tras visualizar el listado. | El usuario decide exportar. La consulta es válida sin exportar. |
| Registrar asistente ocasional | CU-12 Registrar asistencia | Durante la toma de asistencia. | Se presenta un alumno activo que no está inscripto en la clase. |
| CU-06 Reactivar alumno | CU-01 Registrar alumno | Al validar el DNI ingresado. | El DNI corresponde a un alumno en estado Baja. |

### 3.4 — Corrección respecto del diagrama preliminar

El diagrama de casos de uso de la presentación preliminar establecía una relación `<<extend>>` entre **CU-02 Registrar pago** y **CU-01 Registrar alumno**, justificada en que el pago se realiza sobre un alumno ya existente.

El equipo revisó esa relación y la retiró. En UML, `<<extend>>` indica que un caso de uso **opcionalmente amplía el comportamiento** de otro en un punto de extensión de su flujo: el caso base se ejecuta completo por sí solo, y la extensión puede o no ocurrir durante esa misma ejecución. Registrar un pago no ocurre durante el alta de un alumno ni la amplía: es una operación independiente, disparada por un evento distinto, en un momento distinto y muchas veces a lo largo del año.

Lo que existe entre ambos es una **dependencia de datos**: no se puede cobrar a un alumno que no está registrado. Eso se expresa como **precondición de CU-02**, no como relación entre casos de uso.

En reemplazo, se incorporó un `<<extend>>` que sí cumple la semántica: **CU-06 Reactivar alumno extiende a CU-01 Registrar alumno**. Cuando la recepcionista carga un DNI que corresponde a un alumno dado de baja, el flujo del alta se desvía hacia la reactivación. Esto ocurre dentro de la misma ejecución del caso base y solo bajo esa condición, que es exactamente lo que la relación describe.

Esta corrección se aplica también en `diagramas/casos-de-uso.puml`.

---

## 4. Listado de casos de uso

| ID | Caso de uso | Módulo | Actor principal | Historia | Importancia |
|---|---|---|---|---|---|
| CU-00 | Autenticar usuario | M7 | Todos | HU-20 | Vital |
| CU-01 | Registrar alumno | M1 | Recepcionista | HU-01 | Vital |
| CU-04 | Modificar datos de alumno | M1 | Recepcionista | HU-06 | Media |
| CU-05 | Dar de baja alumno | M1 | Recepcionista | HU-07 | Alta |
| CU-06 | Reactivar alumno | M1 | Recepcionista | HU-08 | Media |
| CU-02 | Registrar pago de cuota | M2 | Recepcionista | HU-02 | Vital |
| CU-03 | Consultar alumnos morosos | M2 | Administrador | HU-03 | Alta |
| CU-07 | Gestionar modalidades de cobro | M2 | Administrador | HU-11 | Alta |
| CU-08 | Gestionar disciplinas | M3 | Administrador | HU-12 | Alta |
| CU-09 | Crear clase y asignar instructor | M3 | Administrador | HU-13 | Alta |
| CU-10 | Gestionar versiones de grilla | M3 | Administrador | HU-14 | Media |
| CU-11 | Inscribir alumno a clase | M3 | Recepcionista | HU-15 | Alta |
| CU-12 | Registrar asistencia | M4 | Instructor | HU-04, HU-17 | Alta |
| CU-13 | Registrar consulta nutricional | M5 | Nutricionista | HU-05a | Media |
| CU-14 | Consultar evolución del paciente | M5 | Nutricionista | HU-05b | Media |
| CU-15 | Gestionar instructores | M6 | Administrador | HU-18 | Alta |
| CU-16 | Gestionar usuarios y roles | M7 | Administrador | HU-21 | Alta |

**Total: 17 casos de uso.**

Las historias HU-09, HU-10, HU-16, HU-19, HU-22 y HU-23 no tienen caso de uso desarrollado. Son consultas de solo lectura cuyo flujo se agota en seleccionar un filtro y leer el resultado, sin flujos alternativos ni excepciones que justifiquen una ficha completa.

---

## 5. Módulo transversal: seguridad

### CU-00 — Autenticar usuario

| Campo | Detalle |
|---|---|
| ID | CU-00 |
| Nombre | Autenticar usuario |
| Descripción | El usuario accede al sistema con sus credenciales. El sistema valida la identidad, determina su rol y habilita únicamente las funciones que le corresponden. Este caso de uso es incluido por todos los demás. |
| Actor principal | Cualquier usuario del sistema |
| Actores secundarios | Sistema de auditoría |
| Historia asociada | HU-20 |
| Requisitos | RF22, RNF03, RNF04 |
| Reglas de negocio | RN22, RN24 |
| Rendimiento | La validación de credenciales debe completarse en un máximo de 3 segundos. |
| Frecuencia estimada | Alta. Entre 10 y 20 inicios de sesión diarios, con picos al abrir y al cerrar el centro. |
| Importancia | Vital |

**Precondiciones**

- El usuario tiene una cuenta creada en el sistema.
- La cuenta se encuentra en estado Activo.

**Postcondiciones**

- Éxito: se abre una sesión asociada al usuario y a su rol. Se muestra el panel principal correspondiente.
- Éxito: el acceso queda registrado con fecha y hora.
- Fallo: no se abre sesión. El sistema permanece en la pantalla de acceso.

**Flujo normal**

| # | Acción (actor) | Reacción (sistema) |
|---|---|---|
| 1 | El usuario accede a la dirección del sistema. | El sistema muestra la pantalla de acceso con los campos de usuario y contraseña. |
| 2 | Ingresa su nombre de usuario y su contraseña, y confirma. | El sistema valida las credenciales contra el registro de usuarios. |
| 3 | — | El sistema identifica el rol asociado, abre la sesión y registra el acceso. |
| 4 | — | El sistema muestra el panel principal con el menú correspondiente al rol. |

**Flujos alternativos**

| # | Situación | Comportamiento |
|---|---|---|
| A1 | El usuario ya tiene una sesión abierta en el mismo navegador. | El sistema lo redirige directamente al panel principal sin volver a solicitar credenciales. |
| A2 | El usuario cierra sesión de forma explícita. | El sistema finaliza la sesión, la registra y vuelve a la pantalla de acceso. |

**Excepciones**

| # | Situación | Respuesta del sistema |
|---|---|---|
| E1 | Las credenciales son incorrectas. | Muestra el mensaje "Usuario o contraseña incorrectos" sin precisar cuál de los dos falló, y permanece en la pantalla de acceso. |
| E2 | El usuario existe pero está desactivado. | Informa que la cuenta no está habilitada y sugiere contactar al administrador. No abre sesión. |
| E3 | El usuario intenta acceder por dirección directa a una función sin sesión iniciada. | Redirige a la pantalla de acceso y, tras autenticarse, lo lleva a la función solicitada si su rol lo permite. |
| E4 | El usuario autenticado intenta acceder por dirección directa a una función no permitida para su rol. | Muestra un mensaje de acceso denegado y lo devuelve a su panel principal. Registra el intento en el log de auditoría. |
| E5 | Falla la conexión con la base de datos. | Informa que el servicio no está disponible temporalmente y sugiere reintentar. |

**Comentarios**

- La respuesta genérica de E1 es deliberada: un mensaje que distinga entre usuario inexistente y contraseña incorrecta permitiría averiguar qué nombres de usuario son válidos.
- E4 es lo que hace efectivo el requisito RNF06 sobre el módulo nutricional. Ocultar la opción del menú no constituye control de acceso.
- Este caso de uso es incluido por todos los demás mediante `<<include>>`. Para no saturar el diagrama, en `diagramas/casos-de-uso.puml` la relación se dibuja únicamente sobre los casos de uso representativos de cada módulo.

---

## 6. Módulo 1: gestión de alumnos

### CU-01 — Registrar alumno

| Campo | Detalle |
|---|---|
| ID | CU-01 |
| Nombre | Registrar alumno |
| Descripción | La recepcionista registra un nuevo alumno en el sistema ingresando sus datos personales y la actividad o actividades en las que participará. El sistema valida que el DNI no esté duplicado y genera un número de legajo único. |
| Actor principal | Recepcionista |
| Actores secundarios | Sistema de validación de DNI (interno), Sistema de auditoría |
| Historia asociada | HU-01 |
| Requisitos | RF01, RF02 |
| Reglas de negocio | RN01, RN04, RN05, RN06 |
| Rendimiento | Los pasos 1 al 4 deben completarse en un máximo de 3 segundos por paso bajo condiciones normales. |
| Frecuencia estimada | Variable. Picos esperados en inicio de temporada o lanzamiento de nuevas disciplinas. |
| Importancia | Vital |

**Precondiciones**

- La recepcionista está autenticada en el sistema.
- El alumno no existe previamente en el padrón, o existe en estado Baja.

**Postcondiciones**

- Éxito: el alumno queda registrado en estado Activo con un número de legajo asignado.
- Éxito: el padrón general refleja el nuevo registro de forma inmediata.
- Éxito: el alta queda registrada en el log de auditoría.
- Fallo: no se registra ningún alumno. El sistema no almacena datos parciales.

**Flujo normal**

| # | Acción (actor) | Reacción (sistema) |
|---|---|---|
| 1 | La recepcionista selecciona "Nuevo alumno". | El sistema muestra el formulario de alta. |
| 2 | Ingresa nombre, apellido, DNI, teléfono, email y actividad o actividades. | El sistema valida que todos los campos obligatorios estén completos. |
| 3 | Selecciona la modalidad de cobro (mensual, por clase o combinada). | El sistema registra la modalidad asociada al alumno y muestra el monto vigente. |
| 4 | Confirma el alta. | El sistema verifica que el DNI no esté duplicado. Si es único, asigna número de legajo, registra el alta y muestra confirmación. |

**Flujos alternativos**

| # | Situación | Comportamiento |
|---|---|---|
| A1 | El alumno es menor de 18 años, según la fecha de nacimiento ingresada. | El sistema habilita los campos de familiar o tutor responsable y los marca como obligatorios. El flujo continúa en el paso 3. |
| A2 | El DNI ingresado corresponde a un alumno en estado Baja. | El sistema ofrece reactivar el registro existente. Si la recepcionista acepta, el flujo continúa en CU-06. Si rechaza, el alta no se realiza. |
| A3 | La recepcionista cancela el alta antes de confirmar. | El sistema descarta los datos ingresados y vuelve al padrón sin registrar nada. |

**Excepciones**

| # | Situación | Respuesta del sistema |
|---|---|---|
| E1 | El DNI ingresado ya existe en el sistema y corresponde a un alumno activo. | Muestra un mensaje indicando que el DNI está registrado, exhibe los datos del alumno existente y no realiza el alta. |
| E2 | Faltan campos obligatorios. | Resalta los campos incompletos e impide avanzar hasta que se completen. |
| E3 | Falla de conexión al confirmar. | Muestra un aviso de error de conexión y no registra el alta parcialmente. Los datos ingresados se conservan en el formulario para reintentar. |
| E4 | El DNI ingresado no tiene un formato válido. | Indica el formato esperado e impide avanzar. |

**Comentarios**

- Este caso de uso es el punto de entrada al sistema para cualquier alumno. Un alta incorrecta o duplicada contamina todos los módulos posteriores: pagos, asistencia e historial.
- El flujo alternativo A2 es el punto de extensión de la relación `<<extend>>` con CU-06.
- El flujo alternativo A1 se incorporó a partir de la regla RN05. La versión preliminar de este caso de uso no contemplaba alumnos menores, que existen en las disciplinas Aeróbica Infantil y Kids Fit and Fun.

---

### CU-04 — Modificar datos de alumno

| Campo | Detalle |
|---|---|
| ID | CU-04 |
| Nombre | Modificar datos de alumno |
| Descripción | La recepcionista actualiza los datos de un alumno existente. El sistema conserva el valor anterior de cada campo modificado en el historial de cambios. |
| Actor principal | Recepcionista |
| Actores secundarios | Sistema de auditoría |
| Historia asociada | HU-06 |
| Requisitos | RF03 |
| Reglas de negocio | RN01, RN23 |
| Rendimiento | El guardado debe reflejarse en un máximo de 3 segundos. |
| Frecuencia estimada | Media. Entre 5 y 15 modificaciones semanales, principalmente cambios de teléfono y de actividad. |
| Importancia | Media |

**Precondiciones**

- La recepcionista está autenticada.
- El alumno existe en el sistema.

**Postcondiciones**

- Éxito: los datos del alumno quedan actualizados.
- Éxito: el historial de cambios registra qué campo se modificó, su valor anterior, su valor nuevo, el usuario y la fecha y hora.
- Fallo: los datos permanecen sin cambios y no se genera ningún registro en el historial.

**Flujo normal**

| # | Acción (actor) | Reacción (sistema) |
|---|---|---|
| 1 | Busca al alumno y accede a su ficha. | El sistema muestra los datos actuales del alumno. |
| 2 | Selecciona "Editar". | El sistema habilita los campos modificables según el rol del usuario. |
| 3 | Modifica los campos necesarios. | El sistema valida el formato de cada campo a medida que se completa. |
| 4 | Confirma los cambios. | El sistema guarda los datos, registra el cambio en el historial y muestra confirmación. |

**Flujos alternativos**

| # | Situación | Comportamiento |
|---|---|---|
| A1 | El usuario tiene rol Administrador y modifica el DNI. | El sistema vuelve a validar que el nuevo DNI no exista en el padrón antes de guardar. |
| A2 | Se modifica la modalidad de cobro. | El sistema advierte que el cambio rige para los próximos pagos y no altera las cuotas ya registradas. |
| A3 | El usuario consulta el historial de cambios. | El sistema muestra la lista de modificaciones del alumno, de la más reciente a la más antigua. |

**Excepciones**

| # | Situación | Respuesta del sistema |
|---|---|---|
| E1 | No se modificó ningún campo. | Informa que no hay cambios para guardar y no genera un registro en el historial. |
| E2 | El nuevo DNI ingresado por el Administrador ya pertenece a otro alumno. | Muestra el conflicto, exhibe los datos del alumno existente e impide guardar. |
| E3 | El usuario con rol Recepcionista intenta modificar el DNI. | El campo se muestra deshabilitado, con la indicación de que solo el Administrador puede modificarlo. |
| E4 | Falla de conexión al confirmar. | Conserva los datos ingresados y ofrece reintentar. No guarda modificaciones parciales. |

**Comentarios**

- El requisito RF03 exige registrar el historial de cambios. El equipo decidió reutilizar el log de auditoría exigido por RNF05, filtrado por alumno, en lugar de crear una estructura de historial propia.
- La restricción sobre el DNI proviene de RN01: el DNI es la identidad del alumno, de modo que modificarlo es una operación excepcional.

---

### CU-05 — Dar de baja alumno

| Campo | Detalle |
|---|---|
| ID | CU-05 |
| Nombre | Dar de baja alumno |
| Descripción | La recepcionista registra la baja de un alumno que deja de asistir, indicando la fecha y el motivo. La baja es lógica: el registro y todo su historial se conservan. |
| Actor principal | Recepcionista |
| Actores secundarios | Sistema de auditoría |
| Historia asociada | HU-07 |
| Requisitos | RF04 |
| Reglas de negocio | RN02, RN03 |
| Rendimiento | La confirmación debe reflejarse en un máximo de 3 segundos. |
| Frecuencia estimada | Media. Entre 10 y 20 bajas mensuales, con picos en los meses de menor actividad. |
| Importancia | Alta |

**Precondiciones**

- La recepcionista está autenticada.
- El alumno existe y se encuentra en estado Activo.

**Postcondiciones**

- Éxito: el alumno pasa a estado Baja con fecha y motivo registrados.
- Éxito: el alumno deja de figurar en el padrón activo, en el reporte de morosos y en los listados de clases.
- Éxito: su historial de pagos, inscripciones y asistencias se conserva íntegro.
- Fallo: el alumno permanece en estado Activo.

**Flujo normal**

| # | Acción (actor) | Reacción (sistema) |
|---|---|---|
| 1 | Busca al alumno y accede a su ficha. | El sistema muestra los datos y el estado actual del alumno. |
| 2 | Selecciona "Dar de baja". | El sistema muestra el formulario de baja con la fecha del día precargada y la lista de motivos. |
| 3 | Selecciona el motivo y, opcionalmente, agrega una observación. | El sistema valida que el motivo esté seleccionado. |
| 4 | Confirma la baja. | El sistema solicita confirmación explícita, registra la baja, actualiza el estado y muestra el resultado. |

**Flujos alternativos**

| # | Situación | Comportamiento |
|---|---|---|
| A1 | El alumno tiene cuotas impagas. | El sistema informa el monto adeudado antes de confirmar. La recepcionista puede continuar con la baja de todos modos. |
| A2 | El alumno tiene inscripciones activas a clases. | El sistema informa en qué clases estaba inscripto y da de baja esas inscripciones al confirmar. |
| A3 | La recepcionista ajusta manualmente la fecha de baja. | El sistema acepta la fecha siempre que no sea posterior al día en curso. |
| A4 | La recepcionista cancela antes de confirmar. | El alumno permanece en estado Activo y no se registra nada. |

**Excepciones**

| # | Situación | Respuesta del sistema |
|---|---|---|
| E1 | El alumno ya se encuentra en estado Baja. | Informa la situación e indica la fecha de baja registrada. No permite ejecutar la operación nuevamente. |
| E2 | No se seleccionó un motivo. | Impide confirmar e indica que el motivo es obligatorio. |
| E3 | La fecha de baja ingresada es posterior al día en curso. | Rechaza el valor e indica que la fecha no puede ser futura. |
| E4 | Falla de conexión al confirmar. | No registra la baja y ofrece reintentar. |

**Comentarios**

- El flujo A1 fue objeto de discusión en el refinamiento. El equipo decidió que la deuda no bloquee la baja: si el alumno dejó de venir, el centro necesita registrarlo aunque la cuota quede impaga. El aviso es informativo.
- Los motivos de baja definidos con la dirección son: mudanza, motivos económicos, lesión o problema de salud, falta de tiempo, cambio de gimnasio, sin especificar.
- La baja lógica resuelve el problema P2 del relevamiento: la planilla actual obliga a elegir entre borrar el registro y perder el historial, o conservarlo y ensuciar el padrón.

---

### CU-06 — Reactivar alumno

| Campo | Detalle |
|---|---|
| ID | CU-06 |
| Nombre | Reactivar alumno |
| Descripción | La recepcionista reincorpora a un alumno que había sido dado de baja. El alumno recupera el estado Activo conservando su número de legajo y todo su historial. Este caso de uso extiende a CU-01. |
| Actor principal | Recepcionista |
| Actores secundarios | Sistema de auditoría |
| Historia asociada | HU-08 |
| Requisitos | RF05, RF02 |
| Reglas de negocio | RN02, RN03, RN04, RN13 |
| Rendimiento | La reactivación debe reflejarse en un máximo de 3 segundos. |
| Frecuencia estimada | Media-baja. Entre 5 y 15 reactivaciones mensuales, concentradas al inicio de temporada. |
| Importancia | Media |

**Precondiciones**

- La recepcionista está autenticada.
- El alumno existe en el sistema y se encuentra en estado Baja.

**Postcondiciones**

- Éxito: el alumno vuelve a estado Activo conservando su número de legajo original.
- Éxito: su historial previo de pagos, inscripciones y asistencias queda disponible en la ficha.
- Éxito: la reactivación queda registrada en el log de auditoría.
- Fallo: el alumno permanece en estado Baja.

**Flujo normal**

| # | Acción (actor) | Reacción (sistema) |
|---|---|---|
| 1 | Busca al alumno filtrando por estado Baja, o intenta darlo de alta y el sistema detecta el DNI existente. | El sistema muestra la ficha del alumno con su historial y ofrece la opción de reactivarlo. |
| 2 | Selecciona "Reactivar". | El sistema muestra los datos registrados para su revisión. |
| 3 | Actualiza los datos de contacto y la modalidad de cobro si corresponde. | El sistema valida el formato de los campos modificados. |
| 4 | Confirma la reactivación. | El sistema cambia el estado a Activo, conserva el legajo, registra la operación y muestra confirmación. |

**Flujos alternativos**

| # | Situación | Comportamiento |
|---|---|---|
| A1 | El caso de uso se inicia desde CU-01, al detectarse un DNI dado de baja. | El sistema traslada al formulario de reactivación los datos ya ingresados en el alta y continúa en el paso 2. |
| A2 | La modalidad de cobro que tenía el alumno fue desactivada. | El sistema lo informa y solicita seleccionar una modalidad vigente antes de confirmar. |
| A3 | El alumno tenía cuotas impagas al momento de la baja. | El sistema informa el monto pendiente. La reactivación se realiza igualmente. |

**Excepciones**

| # | Situación | Respuesta del sistema |
|---|---|---|
| E1 | El alumno ya se encuentra en estado Activo. | Informa que el alumno está activo y no ejecuta la reactivación. |
| E2 | Falla de conexión al confirmar. | No modifica el estado y ofrece reintentar. |

**Comentarios**

- Las inscripciones a clases anteriores no se restauran automáticamente. La grilla pudo haber cambiado entre la baja y la reactivación, de modo que restaurarlas anotaría al alumno en clases que ya no existen o cambiaron de horario. Es consistente con la regla RN13.
- Este caso de uso es la relación `<<extend>>` de CU-01. El punto de extensión es la validación de DNI del paso 4 del flujo normal de CU-01.
- Conservar el número de legajo original es lo que permite mantener la antigüedad del alumno y la continuidad de su historial, conforme a la regla RN04.

---

## 7. Módulo 2: gestión de cuotas y pagos

### CU-02 — Registrar pago de cuota

| Campo | Detalle |
|---|---|
| ID | CU-02 |
| Nombre | Registrar pago de cuota |
| Descripción | La recepcionista registra el pago de la cuota de un alumno activo, especificando el monto, el período, la fecha y el medio de pago. El sistema actualiza el estado de cuenta del alumno. |
| Actor principal | Recepcionista |
| Actores secundarios | Sistema de auditoría |
| Historia asociada | HU-02 |
| Requisitos | RF06, RF07, RF10 |
| Reglas de negocio | RN03, RN06, RN08, RN10, RN11 |
| Rendimiento | La confirmación del pago debe reflejarse en el sistema en un máximo de 3 segundos. |
| Frecuencia estimada | Alta. Se estima entre 30 y 60 registros diarios en períodos de cobro mensual. |
| Importancia | Vital |

**Precondiciones**

- La recepcionista está autenticada.
- El alumno existe en el sistema y su estado es Activo.
- El alumno tiene una modalidad de cobro vigente asignada.

**Postcondiciones**

- Éxito: el pago queda registrado y el estado de cuenta del alumno se actualiza.
- Éxito: el historial de pagos refleja el nuevo registro.
- Éxito: la cuota conserva el monto y la modalidad aplicados al momento del cobro.
- Fallo: no se registra ningún pago y el estado de cuenta no se modifica.

**Flujo normal**

| # | Acción (actor) | Reacción (sistema) |
|---|---|---|
| 1 | La recepcionista busca al alumno por apellido o número de legajo. | El sistema muestra los resultados de búsqueda. |
| 2 | Selecciona al alumno y accede a su ficha. | El sistema muestra los datos del alumno y su estado de cuenta actual (al día, moroso, días de mora). |
| 3 | Selecciona "Registrar pago". | El sistema muestra el formulario de pago con el monto sugerido según la modalidad del alumno. |
| 4 | Ingresa monto, período, fecha y medio de pago. Confirma. | El sistema registra el pago, actualiza el estado de cuenta a "Al día" y muestra confirmación. |

**Flujos alternativos**

| # | Situación | Comportamiento |
|---|---|---|
| A1 | La recepcionista modifica el monto sugerido. | El sistema acepta el valor ingresado, siempre que sea mayor a cero, y lo registra como monto efectivamente cobrado. |
| A2 | El alumno tiene modalidad por clase. | El sistema solicita la cantidad de clases abonadas y calcula el monto multiplicando por el valor unitario vigente. |
| A3 | El alumno tiene modalidad combinada. | El sistema muestra el monto de la tarifa combinada correspondiente a las actividades del alumno. |
| A4 | El alumno registra deuda de períodos anteriores. | El sistema lo informa y permite seleccionar a qué período se imputa el pago. |

**Excepciones**

| # | Situación | Respuesta del sistema |
|---|---|---|
| E1 | El alumno tiene estado Baja. | No permite registrar el pago e informa que el alumno está dado de baja. Sugiere reactivarlo primero. |
| E2 | El monto ingresado es cero o negativo. | Rechaza el valor e indica que el monto debe ser mayor a cero. |
| E3 | Falla de conexión al confirmar. | Conserva los datos ingresados y muestra un aviso para reintentar. No registra el pago parcialmente. |
| E4 | Ya existe un pago registrado para ese alumno y ese período. | Informa la situación, muestra el pago existente y solicita confirmación antes de registrar un segundo pago del mismo período. |

**Comentarios**

- La detección automática de mora de RF08 depende directamente de los datos registrados en este caso de uso.
- El pago se asocia a la modalidad y al monto vigentes al momento del cobro, conforme a la regla RN08 y a la decisión D8. Un aumento posterior de precios no modifica las cuotas ya registradas.
- El requisito RNF08 limita este flujo a un máximo de cuatro pasos desde la búsqueda del alumno hasta la confirmación. El flujo normal documentado tiene exactamente cuatro.
- La versión preliminar del diagrama establecía una relación `<<extend>>` entre este caso de uso y CU-01. Esa relación fue retirada por el equipo. El detalle de la corrección está en la sección 3.4.

---

### CU-03 — Consultar alumnos morosos

| Campo | Detalle |
|---|---|
| ID | CU-03 |
| Nombre | Consultar alumnos morosos |
| Descripción | La directora o el administrador consulta el listado de alumnos con cuota vencida, con posibilidad de filtrar por actividad y exportar el resultado en PDF. |
| Actor principal | Administrador (directora) |
| Actores secundarios | Ninguno |
| Historia asociada | HU-03 |
| Requisitos | RF08, RF09, RF30 |
| Reglas de negocio | RN09, RN25 |
| Rendimiento | El sistema debe mostrar el listado en un máximo de 3 segundos, incluso con 500 alumnos registrados. |
| Frecuencia estimada | Baja-media. Se estima entre 5 y 15 consultas mensuales por parte de la dirección. |
| Importancia | Alta |

**Precondiciones**

- El usuario tiene rol Administrador.
- Existe al menos un alumno registrado en el sistema con pagos históricos.

**Postcondiciones**

- Éxito: el usuario visualiza el listado de alumnos morosos con los datos solicitados.
- Éxito (exportación): se genera un archivo PDF descargable con el listado.
- Fallo: el sistema no modifica ningún dato. La consulta es de solo lectura.

**Flujo normal**

| # | Acción (actor) | Reacción (sistema) |
|---|---|---|
| 1 | El director accede a la sección "Reportes" y selecciona "Alumnos morosos". | El sistema ejecuta la consulta con el criterio predeterminado (más de 30 días sin pago) y muestra el listado completo. |
| 2 | Opcionalmente, aplica filtros por actividad o por turno. | El sistema actualiza el listado según los filtros seleccionados. |
| 3 | Revisa el listado con nombre, actividad, días de mora y monto adeudado. | El sistema mantiene la vista actualizada. |
| 4 | Selecciona "Exportar PDF". | El sistema genera un archivo PDF con el listado filtrado y lo pone a disposición para descarga. |

**Flujos alternativos**

| # | Situación | Comportamiento |
|---|---|---|
| A1 | El usuario modifica el umbral de días de mora. | El sistema recalcula el listado con el nuevo umbral y lo guarda como valor predeterminado para consultas futuras. |
| A2 | El usuario consulta el listado sin exportarlo. | El caso de uso finaliza correctamente en el paso 3. La exportación es opcional. |
| A3 | El usuario ordena el listado por días de mora o por monto adeudado. | El sistema reordena la vista sin volver a consultar la base. |

**Excepciones**

| # | Situación | Respuesta del sistema |
|---|---|---|
| E1 | No hay alumnos morosos con los filtros aplicados. | Muestra el mensaje "Sin alumnos morosos al día de hoy" y no genera un PDF vacío. |
| E2 | Falla la generación del PDF. | Informa el error y sugiere reintentar. El listado en pantalla sigue disponible. |
| E3 | El umbral de días ingresado es cero o negativo. | Rechaza el valor e indica que debe ser un número mayor a cero. |

**Comentarios**

- E1 ilustra un requisito de comunicación que no fue modelado explícitamente en los requisitos funcionales pero que surge naturalmente al desarrollar el caso de uso: el sistema debe informar explícitamente cuándo no hay resultados, para que el usuario no interprete que hay un error.
- Los alumnos con modalidad de cobro por clase no se incluyen en el listado, conforme a la regla RN25 y a la decisión D12. El pago en esa modalidad es anticipado, de modo que no genera deuda. Esos alumnos se hacen visibles como inactivos en el reporte de ocupación (HU-23).
- Los alumnos dados de baja no figuran en el listado, aun cuando registren deuda pendiente.
- El paso 4 constituye el punto de extensión de la relación `<<extend>>` "Exportar reporte a PDF". La consulta es un caso de uso completo sin ejecutarlo.

---

### CU-07 — Gestionar modalidades de cobro

| Campo | Detalle |
|---|---|
| ID | CU-07 |
| Nombre | Gestionar modalidades de cobro |
| Descripción | El administrador administra el catálogo de modalidades de cobro y sus montos base, de modo que la dirección pueda actualizar precios sin intervención técnica. |
| Actor principal | Administrador |
| Actores secundarios | Sistema de auditoría |
| Historia asociada | HU-11 |
| Requisitos | RF07 |
| Reglas de negocio | RN06, RN07, RN08, RN10 |
| Rendimiento | El guardado debe reflejarse en un máximo de 3 segundos. |
| Frecuencia estimada | Baja. Entre 3 y 6 actualizaciones anuales, coincidentes con ajustes de precio. |
| Importancia | Alta |

**Precondiciones**

- El usuario tiene rol Administrador.

**Postcondiciones**

- Éxito: la modalidad queda creada o actualizada y disponible para su asignación a alumnos.
- Éxito: las cuotas ya registradas conservan el monto con el que fueron cobradas.
- Fallo: el catálogo permanece sin cambios.

**Flujo normal**

| # | Acción (actor) | Reacción (sistema) |
|---|---|---|
| 1 | Accede a "Configuración" y selecciona "Modalidades de cobro". | El sistema lista las modalidades vigentes con su tipo, descripción, monto base y cantidad de alumnos asignados. |
| 2 | Selecciona "Nueva modalidad" o elige una existente para editar. | El sistema muestra el formulario correspondiente. |
| 3 | Ingresa o modifica nombre, descripción, tipo y monto base. | El sistema valida que el monto sea mayor a cero y que el nombre no se repita. |
| 4 | Confirma. | El sistema guarda los cambios, registra la operación y muestra confirmación. |

**Flujos alternativos**

| # | Situación | Comportamiento |
|---|---|---|
| A1 | Se modifica el monto de una modalidad con alumnos asignados. | El sistema advierte cuántos alumnos quedarán afectados en sus próximos pagos y solicita confirmación. |
| A2 | Se desactiva una modalidad. | El sistema deja de ofrecerla en el alta de alumnos, pero los alumnos que la tienen asignada la conservan hasta que se les cambie manualmente. |
| A3 | Se reactiva una modalidad desactivada. | El sistema vuelve a ofrecerla en el alta. |

**Excepciones**

| # | Situación | Respuesta del sistema |
|---|---|---|
| E1 | Se intenta eliminar una modalidad asignada a al menos un alumno. | Impide la eliminación, indica cuántos alumnos la tienen asignada y ofrece desactivarla en su lugar. |
| E2 | El monto ingresado es cero o negativo. | Rechaza el valor e indica que debe ser mayor a cero. |
| E3 | El nombre de la modalidad ya existe. | Informa el conflicto e impide guardar. |

**Comentarios**

- Este caso de uso responde a la restricción RE04: la dirección modifica precios sin previo aviso al equipo técnico. Sin él, cada aumento requeriría intervenir el código.
- Los valores vigentes a mayo de 2026 son: mensual fija 28.000 pesos, por clase 4.500 pesos y combinada 38.000 pesos.
- La imposibilidad de eliminar una modalidad en uso (E1) evita perder la referencia de las cuotas históricas, que es la base del reporte de ingresos de HU-22.

---

## 8. Módulo 3: planificación de actividades

### CU-08 — Gestionar disciplinas

| Campo | Detalle |
|---|---|
| ID | CU-08 |
| Nombre | Gestionar disciplinas |
| Descripción | El administrador administra el catálogo de disciplinas que ofrece el centro, incorporando actividades nuevas o discontinuando existentes sin perder el historial de clases dictadas. |
| Actor principal | Administrador |
| Actores secundarios | Sistema de auditoría |
| Historia asociada | HU-12 |
| Requisitos | RF11 |
| Reglas de negocio | RN15 |
| Rendimiento | El guardado debe reflejarse en un máximo de 3 segundos. |
| Frecuencia estimada | Baja. Entre 2 y 5 altas o bajas de disciplina por año. |
| Importancia | Alta |

**Precondiciones**

- El usuario tiene rol Administrador.

**Postcondiciones**

- Éxito: la disciplina queda creada o actualizada y disponible para asociarse a clases.
- Fallo: el catálogo permanece sin cambios.

**Flujo normal**

| # | Acción (actor) | Reacción (sistema) |
|---|---|---|
| 1 | Accede a "Planificación" y selecciona "Disciplinas". | El sistema lista las disciplinas con nombre, descripción, público destinatario y estado. |
| 2 | Selecciona "Nueva disciplina" o elige una existente. | El sistema muestra el formulario correspondiente. |
| 3 | Ingresa nombre, descripción y si está dirigida a adultos o a niños. | El sistema valida que el nombre no se repita. |
| 4 | Confirma. | El sistema guarda la disciplina y muestra confirmación. |

**Flujos alternativos**

| # | Situación | Comportamiento |
|---|---|---|
| A1 | Se desactiva una disciplina con clases asociadas. | El sistema deja de ofrecerla al crear clases nuevas, pero las clases existentes se conservan y siguen dictándose hasta el fin de la grilla. |
| A2 | Se modifica la descripción de una disciplina. | El cambio se refleja en todas las clases asociadas, sin necesidad de editarlas una por una. |

**Excepciones**

| # | Situación | Respuesta del sistema |
|---|---|---|
| E1 | Se intenta eliminar una disciplina con clases asociadas. | Impide la eliminación, indica cuántas clases la utilizan y ofrece desactivarla. |
| E2 | El nombre de la disciplina ya existe. | Informa el conflicto e impide guardar. |
| E3 | Se intenta asignar una disciplina infantil a una clase del turno mañana. | Impide la operación e indica que las disciplinas infantiles se dictan únicamente en el turno tarde. |

**Comentarios**

- Durante el modelado se decidió que `Disciplina` fuera una entidad separada y no un campo de texto dentro de `Clase`. El flujo alternativo A2 muestra la consecuencia práctica de esa decisión.
- La excepción E3 aplica la regla RN15.

---

### CU-09 — Crear clase y asignar instructor

| Campo | Detalle |
|---|---|
| ID | CU-09 |
| Nombre | Crear clase y asignar instructor |
| Descripción | El administrador crea una clase indicando disciplina, día, horario y turno, y le asigna un instructor. El sistema valida que el instructor no tenga otra clase superpuesta. |
| Actor principal | Administrador |
| Actores secundarios | Sistema de auditoría |
| Historia asociada | HU-13 |
| Requisitos | RF12 |
| Reglas de negocio | RN12, RN14, RN15 |
| Rendimiento | La validación de superposición y el guardado deben completarse en un máximo de 3 segundos. |
| Frecuencia estimada | Baja durante la temporada, alta al armar una grilla nueva. Entre 20 y 40 clases por grilla. |
| Importancia | Alta |

**Precondiciones**

- El usuario tiene rol Administrador.
- Existe al menos una disciplina activa.
- Existe al menos un instructor activo.
- Existe una grilla activa.

**Postcondiciones**

- Éxito: la clase queda creada dentro de la grilla activa con su instructor asignado.
- Éxito: la clase aparece en la agenda del instructor y queda disponible para inscripciones.
- Fallo: no se crea la clase.

**Flujo normal**

| # | Acción (actor) | Reacción (sistema) |
|---|---|---|
| 1 | Accede a "Planificación" y selecciona "Nueva clase". | El sistema muestra el formulario con las disciplinas activas y los instructores activos. |
| 2 | Selecciona disciplina, día de la semana, hora de inicio, hora de fin y turno. | El sistema valida que la hora de fin sea posterior a la de inicio. |
| 3 | Selecciona el instructor a cargo. | El sistema verifica que el instructor no tenga otra clase superpuesta en ese día y horario. |
| 4 | Confirma. | El sistema crea la clase dentro de la grilla activa y muestra confirmación. |

**Flujos alternativos**

| # | Situación | Comportamiento |
|---|---|---|
| A1 | Se modifica una clase existente. | El sistema aplica las mismas validaciones y actualiza la clase. Si cambió el instructor, la clase pasa a la agenda del nuevo. |
| A2 | Se desactiva una clase con alumnos inscriptos. | El sistema informa cuántos alumnos quedan afectados y solicita confirmación antes de continuar. |
| A3 | Se crea la clase sobre una grilla no activa, en preparación de la próxima temporada. | El sistema la registra en esa grilla, sin afectar la operación en curso. |

**Excepciones**

| # | Situación | Respuesta del sistema |
|---|---|---|
| E1 | El instructor seleccionado ya tiene una clase superpuesta en día y horario. | Muestra cuál es la clase en conflicto e impide guardar. |
| E2 | La hora de fin es anterior o igual a la hora de inicio. | Rechaza el valor e indica el error. |
| E3 | Se selecciona una disciplina infantil en el turno mañana. | Impide la operación e indica que las disciplinas infantiles se dictan únicamente en el turno tarde. |
| E4 | Se intenta eliminar una clase con inscripciones. | Impide la eliminación y ofrece desactivarla. |

**Comentarios**

- La validación de E1 responde a un problema real: en la planilla actual nada impide asignar al mismo profesor en dos salones a la misma hora.
- Se acordó con la dirección que cada clase tiene un único instructor a cargo, conforme a la regla RN14. Si eventualmente hubiera un ayudante, se registra en la observación de la clase.

---

### CU-10 — Gestionar versiones de grilla

| Campo | Detalle |
|---|---|
| ID | CU-10 |
| Nombre | Gestionar versiones de grilla |
| Descripción | El administrador crea, edita y activa versiones de la grilla de clases, de modo que la planificación de la temporada siguiente pueda prepararse sin afectar la que está en uso ni perder las anteriores. |
| Actor principal | Administrador |
| Actores secundarios | Sistema de auditoría |
| Historia asociada | HU-14 |
| Requisitos | RF13 |
| Reglas de negocio | RN12, RN13 |
| Rendimiento | La copia de una grilla completa debe completarse en un máximo de 5 segundos. La activación, en un máximo de 3 segundos. |
| Frecuencia estimada | Muy baja. Entre 2 y 4 cambios de grilla por año. |
| Importancia | Media |

**Precondiciones**

- El usuario tiene rol Administrador.
- Existe al menos una grilla en el sistema.

**Postcondiciones**

- Éxito: la nueva grilla queda creada, o la grilla seleccionada queda activa.
- Éxito: al activar una grilla, la anterior pasa a modo de solo lectura sin perder sus datos.
- Éxito: existe una única grilla activa.
- Fallo: la grilla activa no cambia.

**Flujo normal**

| # | Acción (actor) | Reacción (sistema) |
|---|---|---|
| 1 | Accede a "Planificación" y selecciona "Grillas". | El sistema lista las grillas con nombre, vigencia, cantidad de clases y cuál está activa. |
| 2 | Selecciona "Nueva grilla a partir de la activa". | El sistema solicita nombre y período de vigencia de la grilla nueva. |
| 3 | Confirma la creación. | El sistema copia todas las clases e instructores de la grilla activa a la grilla nueva, que queda en estado inactiva y editable. |
| 4 | Ajusta las clases de la grilla nueva según la temporada. | El sistema aplica las validaciones de CU-09 sobre la grilla en edición. |
| 5 | Selecciona "Activar" sobre la grilla nueva. | El sistema advierte los efectos del cambio, solicita confirmación, activa la grilla nueva y pasa la anterior a solo lectura. |

**Flujos alternativos**

| # | Situación | Comportamiento |
|---|---|---|
| A1 | Se crea una grilla vacía en lugar de copiar la activa. | El sistema crea la grilla sin clases. El administrador las carga con CU-09. |
| A2 | Se consulta una grilla anterior. | El sistema la muestra en modo de solo lectura, sin permitir modificaciones. |
| A3 | El administrador cancela la activación tras ver la advertencia. | La grilla activa no cambia y la nueva permanece editable. |

**Excepciones**

| # | Situación | Respuesta del sistema |
|---|---|---|
| E1 | Al activar una grilla, hay alumnos inscriptos en clases que no existen en la grilla entrante. | Informa cuántos alumnos y en qué clases quedan afectados, y solicita confirmación. Si se confirma, esas inscripciones quedan sin clase asociada y se listan para que recepción las reasigne. |
| E2 | Se intenta editar una grilla no activa que ya venció. | Impide la edición e indica que las grillas vencidas son de solo lectura. |
| E3 | Se intenta eliminar una grilla. | Impide la operación. Las grillas no se eliminan, conforme a la regla RN12. |
| E4 | Falla la copia de la grilla. | No crea la grilla nueva y ofrece reintentar. No deja copias parciales. |

**Comentarios**

- Este caso de uso resuelve el problema P3 del relevamiento: hoy las versiones de grilla conviven en archivos sueltos, sin trazabilidad.
- La excepción E1 responde a una pregunta concreta de la directora: al cambiar la grilla de agosto, qué pasa con los alumnos anotados en un horario que ya no existe. El sistema no los desinscribe en silencio, los informa para que recepción los recontacte.
- La historia asociada se divide en tres slices. El primero implementa una grilla única activa, que cubre la operación habitual del centro; el versionado real se entrega después. Ver `slicing.md`.

---

### CU-11 — Inscribir alumno a clase

| Campo | Detalle |
|---|---|
| ID | CU-11 |
| Nombre | Inscribir alumno a clase |
| Descripción | La recepcionista inscribe a un alumno activo en una clase de la grilla activa, de modo que figure en el listado del instructor y pueda registrarse su asistencia. |
| Actor principal | Recepcionista |
| Actores secundarios | Sistema de auditoría |
| Historia asociada | HU-15 |
| Requisitos | RF14 |
| Reglas de negocio | RN03, RN13 |
| Rendimiento | La inscripción debe confirmarse en un máximo de 3 segundos. |
| Frecuencia estimada | Media-alta. Entre 10 y 30 inscripciones semanales, con picos al inicio de temporada. |
| Importancia | Alta |

**Precondiciones**

- La recepcionista está autenticada.
- El alumno existe y se encuentra en estado Activo.
- Existe una grilla activa con al menos una clase.

**Postcondiciones**

- Éxito: el alumno queda inscripto en la clase y figura en el listado del instructor.
- Éxito: la clase queda disponible en la ficha del alumno.
- Fallo: no se genera la inscripción.

**Flujo normal**

| # | Acción (actor) | Reacción (sistema) |
|---|---|---|
| 1 | Accede a la ficha del alumno y selecciona "Inscribir a clase". | El sistema muestra las clases de la grilla activa, agrupadas por día y turno. |
| 2 | Selecciona una o varias clases. | El sistema verifica que el alumno esté activo y que no esté ya inscripto en esas clases. |
| 3 | Confirma la inscripción. | El sistema registra la inscripción con su fecha y muestra confirmación. |

**Flujos alternativos**

| # | Situación | Comportamiento |
|---|---|---|
| A1 | La inscripción se inicia desde la clase en lugar de la ficha del alumno. | El sistema muestra el buscador de alumnos activos y continúa en el paso 2. |
| A2 | El alumno se inscribe en dos clases superpuestas en día y horario. | El sistema muestra una advertencia y permite continuar. La superposición no está prohibida. |
| A3 | Se da de baja una inscripción existente. | El sistema la desactiva y conserva las asistencias ya registradas. |

**Excepciones**

| # | Situación | Respuesta del sistema |
|---|---|---|
| E1 | El alumno está en estado Baja. | Impide la inscripción, informa el motivo y sugiere reactivarlo primero. |
| E2 | El alumno ya está inscripto en esa clase. | Informa la situación y no genera una inscripción duplicada. |
| E3 | La clase seleccionada está desactivada. | No la muestra en el listado de opciones disponibles. |
| E4 | Falla de conexión al confirmar. | No registra la inscripción y ofrece reintentar. |

**Comentarios**

- El flujo A2 es una advertencia y no un bloqueo. La dirección aclaró que hay alumnos que se anotan en dos clases del mismo horario para elegir según el día.
- Solo se ofrecen clases de la grilla activa, conforme a la regla RN13. Inscribir en una grilla no activa dejaría al alumno anotado en un horario que todavía no rige.
- Este caso de uso es el eslabón que conecta el padrón con la grilla y habilita todo el control de asistencia.

---

## 9. Módulo 4: control de asistencia

### CU-12 — Registrar asistencia

| Campo | Detalle |
|---|---|
| ID | CU-12 |
| Nombre | Registrar asistencia |
| Descripción | El instructor consulta el listado de alumnos inscriptos en su clase del día y registra la asistencia de cada uno. Puede además incorporar a un alumno activo que se presentó sin estar inscripto. |
| Actor principal | Instructor |
| Actores secundarios | Sistema de auditoría |
| Historia asociada | HU-04, HU-17 |
| Requisitos | RF15, RF16, RF17, RF27 |
| Reglas de negocio | RN16, RN17, RN18, RN19 |
| Rendimiento | El listado debe cargarse en un máximo de 3 segundos. El guardado, en un máximo de 3 segundos desde la confirmación. |
| Frecuencia estimada | Muy alta. Entre 15 y 25 registros diarios, uno por clase dictada. |
| Importancia | Alta |

**Precondiciones**

- El instructor está autenticado.
- El instructor tiene al menos una clase asignada en la grilla activa para el día en curso.
- La clase tiene alumnos inscriptos, o el instructor incorporará asistentes ocasionales.

**Postcondiciones**

- Éxito: queda registrada la asistencia de cada alumno con su estado, la fecha y la hora.
- Éxito: los registros quedan disponibles en el historial de asistencia de cada alumno y en el reporte de ocupación.
- Fallo: no se registra ninguna asistencia. El sistema no guarda registros parciales.

**Flujo normal**

| # | Acción (actor) | Reacción (sistema) |
|---|---|---|
| 1 | El instructor accede a su agenda. | El sistema muestra únicamente las clases que tiene asignadas para el día en curso. |
| 2 | Selecciona una clase. | El sistema muestra el listado de alumnos inscriptos, ordenado alfabéticamente, con el estado de asistencia sin definir. |
| 3 | Marca a cada alumno como Presente, Ausente o Justificado. | El sistema registra la selección de cada alumno en la vista, sin guardar todavía. |
| 4 | Confirma el registro. | El sistema guarda la asistencia de todos los alumnos con la fecha y la hora del registro, y muestra confirmación. |

**Flujos alternativos**

| # | Situación | Comportamiento |
|---|---|---|
| A1 | Se presenta un alumno activo que no figura en el listado. | El instructor lo busca y lo agrega como asistente ocasional. El sistema lo incorpora al listado de esa clase, marcado como ocasional, sin generar una inscripción permanente. |
| A2 | La asistencia de esa clase y fecha ya fue registrada. | El sistema muestra los valores cargados y permite corregirlos. Al confirmar, actualiza el registro existente en lugar de crear uno nuevo. |
| A3 | El instructor marca a todos los alumnos como presentes con una sola acción. | El sistema aplica el estado Presente a todo el listado y permite modificar individualmente los que corresponda. |
| A4 | El instructor sale de la pantalla sin confirmar. | El sistema descarta las marcas no confirmadas y no registra nada. |

**Excepciones**

| # | Situación | Respuesta del sistema |
|---|---|---|
| E1 | El instructor no tiene clases asignadas para el día. | Informa que no hay clases programadas para la fecha y no muestra el listado. |
| E2 | El alumno buscado para agregar como ocasional está en estado Baja. | Informa que el alumno está dado de baja e impide agregarlo. |
| E3 | La clase no tiene alumnos inscriptos. | Muestra el listado vacío con la indicación correspondiente y habilita la incorporación de asistentes ocasionales. |
| E4 | Falla la conexión al confirmar. | Conserva las marcas realizadas en pantalla y ofrece reintentar. No guarda registros parciales. |
| E5 | El instructor intenta acceder a una clase que no tiene asignada. | Deniega el acceso y registra el intento en el log de auditoría. |

**Comentarios**

- El flujo A1 corresponde al requisito adicional RF27 y a la decisión D7, y constituye el punto de extensión de la relación `<<extend>>` "Registrar asistente ocasional". Se documenta dentro de este caso de uso porque ocurre durante la misma ejecución del flujo base.
- La distinción entre asistencia regular y ocasional se conserva en el historial, y tiene efecto en el reporte de ocupación de HU-23: una clase con pocos inscriptos pero muchos ocasionales tiene demanda real.
- La restricción RE05 condiciona el diseño de la pantalla: se usa desde una tablet, de pie y con poco tiempo entre alumno y alumno. De ahí el flujo A3.
- La excepción E4 es especialmente relevante porque la conectividad wifi del salón puede ser inestable.

---

## 10. Módulo 5: seguimiento nutricional

### CU-13 — Registrar consulta nutricional

| Campo | Detalle |
|---|---|
| ID | CU-13 |
| Nombre | Registrar consulta nutricional |
| Descripción | La nutricionista registra los datos de una consulta de seguimiento de un alumno-paciente. El módulo es de acceso exclusivo de este rol. |
| Actor principal | Nutricionista |
| Actores secundarios | Sistema de auditoría |
| Historia asociada | HU-05a |
| Requisitos | RF19, RF21 |
| Reglas de negocio | RN20, RN21, RN26, RN27, RN29 |
| Rendimiento | El guardado debe reflejarse en un máximo de 3 segundos. |
| Frecuencia estimada | Baja-media. Entre 6 y 12 consultas semanales, concentradas los martes de 15:00 a 18:00. |
| Importancia | Media |

**Precondiciones**

- La nutricionista está autenticada con su propio usuario.
- El alumno-paciente existe y se encuentra en estado Activo.
- El alumno figura entre los pacientes asignados a la nutricionista.
- Si el paciente es menor de 18 años, están registrados el tutor responsable y la fecha de autorización.

**Postcondiciones**

- Éxito: la consulta queda registrada con todos sus parámetros y el índice de masa corporal calculado.
- Éxito: la consulta queda disponible en el historial de evolución del paciente.
- Éxito: la operación queda registrada en el log de auditoría del módulo.
- Fallo: no se registra la consulta y el historial permanece sin cambios.

**Flujo normal**

| # | Acción (actor) | Reacción (sistema) |
|---|---|---|
| 1 | La nutricionista accede al módulo y selecciona un paciente de su listado. | El sistema muestra la ficha del paciente con la fecha de su última consulta. |
| 2 | Selecciona "Nueva consulta". | El sistema muestra el formulario con la fecha del día precargada y la altura registrada en la consulta anterior. |
| 3 | Ingresa peso, perímetros de cintura y cadera, porcentaje de masa grasa, objetivo y observaciones. | El sistema calcula el índice de masa corporal a partir del peso y la altura, y lo muestra en pantalla. |
| 4 | Confirma la consulta. | El sistema guarda el registro, lo incorpora al historial del paciente y muestra confirmación. |

**Flujos alternativos**

| # | Situación | Comportamiento |
|---|---|---|
| A1 | Es la primera consulta del paciente. | El sistema exige el ingreso de la altura, que no puede arrastrarse de una consulta anterior. |
| A2 | La altura del paciente cambió respecto de la consulta anterior. | La nutricionista la modifica y el sistema recalcula el índice de masa corporal con el nuevo valor. |
| A3 | La nutricionista deja en blanco los perímetros y el porcentaje de masa grasa. | El sistema acepta la consulta: esos campos son opcionales según la regla RN26. |
| A4 | El alumno no figura entre los pacientes asignados. | La nutricionista solicita al Administrador que se lo asigne. El Administrador realiza la asignación sin acceder al contenido de las consultas. |

**Excepciones**

| # | Situación | Respuesta del sistema |
|---|---|---|
| E1 | Un usuario con rol Recepcionista o Instructor intenta acceder al módulo. | Deniega el acceso, muestra un mensaje de permiso insuficiente y registra el intento en el log de auditoría. |
| E2 | El alumno seleccionado está en estado Baja. | Impide registrar la consulta e informa el motivo. |
| E3 | El paciente es menor de 18 años y no tiene registrados el tutor responsable ni la fecha de autorización. | Impide crear la primera consulta e indica qué datos faltan cargar. |
| E4 | El peso o la altura ingresados están fuera de un rango razonable. | Solicita confirmación explícita antes de guardar, para descartar un error de tipeo. |
| E5 | Faltan campos obligatorios (fecha, peso u objetivo). | Resalta los campos incompletos e impide confirmar. |
| E6 | Falla de conexión al confirmar. | Conserva los datos ingresados y ofrece reintentar. No registra la consulta parcialmente. |

**Comentarios**

- Los parámetros del paso 3 corresponden a la regla RN26 y a la decisión D11. La versión preliminar del requisito RF19 hablaba genéricamente de "medidas", lo que impedía diseñar el formulario y estimar la historia. Ese fue el motivo por el que la Definition of Ready rechazó HU-05.
- La excepción E3 aplica la regla RN27 y la decisión D14. Es relevante porque el centro dicta disciplinas infantiles.
- La excepción E4 se incorporó a pedido de la nutricionista: un error de tipeo en el peso distorsiona toda la curva de evolución del paciente.
- Según la regla RN29 y la decisión D10, el Administrador puede crear el usuario de la nutricionista y asignarle pacientes, pero no accede al contenido clínico de las consultas. El flujo A4 refleja esa separación.

---

### CU-14 — Consultar evolución del paciente

| Campo | Detalle |
|---|---|
| ID | CU-14 |
| Nombre | Consultar evolución del paciente |
| Descripción | La nutricionista consulta el historial completo de consultas de un paciente y las variaciones de sus parámetros entre una consulta y otra. |
| Actor principal | Nutricionista |
| Actores secundarios | Ninguno |
| Historia asociada | HU-05b |
| Requisitos | RF20, RF21 |
| Reglas de negocio | RN20, RN26, RN28, RN29 |
| Rendimiento | El historial debe cargarse en un máximo de 3 segundos. |
| Frecuencia estimada | Baja-media. Una consulta por paciente atendido, entre 6 y 12 semanales. |
| Importancia | Media |

**Precondiciones**

- La nutricionista está autenticada con su propio usuario.
- El paciente tiene al menos una consulta registrada.

**Postcondiciones**

- Éxito: se visualiza el historial completo con las variaciones calculadas.
- Fallo: el sistema no modifica ningún dato. La consulta es de solo lectura.

**Flujo normal**

| # | Acción (actor) | Reacción (sistema) |
|---|---|---|
| 1 | Accede al módulo y selecciona un paciente de su listado. | El sistema muestra la ficha del paciente. |
| 2 | Selecciona "Historial de evolución". | El sistema lista todas las consultas registradas, de la más reciente a la más antigua. |
| 3 | Revisa los valores de cada consulta. | El sistema muestra, junto a cada registro, la variación de peso y de índice de masa corporal respecto de la consulta anterior y respecto de la primera. |
| 4 | Opcionalmente, filtra por rango de fechas. | El sistema actualiza el listado y recalcula las variaciones sobre el período seleccionado. |

**Flujos alternativos**

| # | Situación | Comportamiento |
|---|---|---|
| A1 | El paciente tiene una única consulta registrada. | El sistema muestra el registro sin columnas de variación e indica que no hay consultas previas para comparar. |
| A2 | La nutricionista accede al historial desde el formulario de una consulta nueva. | El sistema muestra los últimos valores registrados como referencia, sin salir del formulario. |

**Excepciones**

| # | Situación | Respuesta del sistema |
|---|---|---|
| E1 | Un usuario de otro rol intenta acceder al historial. | Deniega el acceso y registra el intento en el log de auditoría. |
| E2 | El paciente no tiene consultas registradas. | Informa que no hay consultas y ofrece registrar la primera. |
| E3 | Un usuario con rol Alumno intenta acceder a su propio historial nutricional. | Deniega el acceso. La opción no figura en su menú. |

**Comentarios**

- La excepción E3 aplica la regla RN28 y la decisión D13: el alumno no accede a su historial nutricional en la versión 1.0. La devolución se realiza en el consultorio, a criterio de la nutricionista. La incorporación de una vista para el paciente quedó registrada como cuestión diferida B1 en `docs/requisitos.md`.
- El equipo evaluó incluir un gráfico de evolución y lo dejó fuera de la versión 1.0. La tabla con las variaciones cubre la necesidad clínica; el gráfico es una mejora posterior.

---

## 11. Módulo 6: gestión de instructores

### CU-15 — Gestionar instructores

| Campo | Detalle |
|---|---|
| ID | CU-15 |
| Nombre | Gestionar instructores |
| Descripción | El administrador registra, modifica y da de baja a los instructores del centro, de modo que puedan asignarse a las clases y se conserve el registro de quién dictó cada una. |
| Actor principal | Administrador |
| Actores secundarios | Sistema de auditoría |
| Historia asociada | HU-18 |
| Requisitos | RF25 |
| Reglas de negocio | RN02, RN14 |
| Rendimiento | El guardado debe reflejarse en un máximo de 3 segundos. |
| Frecuencia estimada | Baja. Entre 3 y 8 altas o bajas por año. |
| Importancia | Alta |

**Precondiciones**

- El usuario tiene rol Administrador.

**Postcondiciones**

- Éxito: el instructor queda registrado o actualizado y disponible para asignarse a clases.
- Éxito (baja): el instructor pasa a estado Inactivo y deja de ofrecerse al crear clases, conservando su historial.
- Fallo: el plantel permanece sin cambios.

**Flujo normal**

| # | Acción (actor) | Reacción (sistema) |
|---|---|---|
| 1 | Accede a "Instructores". | El sistema lista el plantel con apellido, nombre, especialidades, estado y cantidad de clases asignadas. |
| 2 | Selecciona "Nuevo instructor" o elige uno existente. | El sistema muestra el formulario correspondiente. |
| 3 | Ingresa apellido, nombre, DNI, teléfono, email y especialidades. | El sistema valida que el DNI no esté duplicado entre los instructores. |
| 4 | Confirma. | El sistema guarda el registro y muestra confirmación. |

**Flujos alternativos**

| # | Situación | Comportamiento |
|---|---|---|
| A1 | Se da de baja a un instructor sin clases asignadas en la grilla activa. | El sistema cambia el estado a Inactivo directamente, previa confirmación. |
| A2 | Se reactiva un instructor inactivo. | El sistema lo devuelve a estado Activo conservando su registro y su historial de clases dictadas. |
| A3 | Se modifican las especialidades de un instructor. | El cambio no afecta las clases ya asignadas: la especialidad es informativa y no restringe la asignación. |

**Excepciones**

| # | Situación | Respuesta del sistema |
|---|---|---|
| E1 | Se intenta dar de baja a un instructor con clases asignadas en la grilla activa. | Muestra el listado de clases afectadas e impide confirmar la baja hasta que sean reasignadas a otro instructor. |
| E2 | El DNI ingresado ya pertenece a otro instructor. | Informa el conflicto, exhibe el registro existente e impide guardar. |
| E3 | Faltan campos obligatorios. | Resalta los campos incompletos e impide confirmar. |
| E4 | Se intenta eliminar físicamente un instructor. | Impide la operación. La baja es siempre lógica, conforme a la regla RN02. |

**Comentarios**

- La excepción E1 es la diferencia central respecto de la baja de alumnos (CU-05): dar de baja a un instructor deja clases sin dictar, de modo que el sistema no puede permitirlo sin resolver primero la reasignación. La baja de un alumno, en cambio, no bloquea nada.
- Los instructores no son alumnos del centro, según el supuesto S06 de `docs/requisitos.md`. Si esa condición cambiara, una misma persona tendría dos registros independientes.
- Este caso de uso es prerrequisito de CU-09: no se puede asignar a una clase un instructor que no está cargado en el sistema.

---

### CU-16 — Gestionar usuarios y roles

| Campo | Detalle |
|---|---|
| ID | CU-16 |
| Nombre | Gestionar usuarios y roles |
| Descripción | El administrador crea los usuarios del sistema, les asigna un rol y consulta el log de auditoría de las operaciones sensibles. |
| Actor principal | Administrador |
| Actores secundarios | Sistema de auditoría |
| Historia asociada | HU-21 |
| Requisitos | RF23, RF31 |
| Reglas de negocio | RN22, RN23, RN24, RN29 |
| Rendimiento | El guardado debe reflejarse en un máximo de 3 segundos. La consulta del log, en un máximo de 3 segundos. |
| Frecuencia estimada | Baja. Entre 5 y 10 operaciones anuales sobre usuarios. Consulta del log, esporádica. |
| Importancia | Alta |

**Precondiciones**

- El usuario tiene rol Administrador.

**Postcondiciones**

- Éxito: el usuario queda creado, modificado o desactivado, con su rol asignado.
- Éxito: el cambio queda registrado en el log de auditoría.
- Fallo: el conjunto de usuarios permanece sin cambios.

**Flujo normal**

| # | Acción (actor) | Reacción (sistema) |
|---|---|---|
| 1 | Accede a "Configuración" y selecciona "Usuarios". | El sistema lista los usuarios con nombre, rol, estado y fecha del último acceso. |
| 2 | Selecciona "Nuevo usuario" o elige uno existente. | El sistema muestra el formulario correspondiente. |
| 3 | Ingresa nombre, nombre de usuario, rol y contraseña inicial. | El sistema valida que el nombre de usuario no se repita. |
| 4 | Confirma. | El sistema crea el usuario con la contraseña almacenada cifrada, registra la operación y muestra confirmación. |

**Flujos alternativos**

| # | Situación | Comportamiento |
|---|---|---|
| A1 | Se modifica el rol de un usuario existente. | El sistema aplica el nuevo rol, registra el cambio en el log de auditoría y el usuario ve el menú actualizado en su próximo acceso. |
| A2 | Se desactiva un usuario. | El sistema le impide iniciar sesión y conserva sus registros previos en el log de auditoría. |
| A3 | El administrador consulta el log de auditoría. | El sistema muestra las operaciones registradas, filtrables por usuario, tipo de operación y rango de fechas. |
| A4 | Se crea el usuario de la nutricionista y se le asignan pacientes. | El sistema realiza la asignación sin exponer el contenido de las consultas al Administrador. |

**Excepciones**

| # | Situación | Respuesta del sistema |
|---|---|---|
| E1 | El nombre de usuario ya existe. | Informa el conflicto e impide guardar. |
| E2 | Se intenta desactivar al último usuario con rol Administrador. | Impide la operación e indica que debe existir al menos un administrador activo. |
| E3 | Se intenta modificar o eliminar un registro del log de auditoría. | La operación no está disponible. El log es de solo lectura, conforme a la regla RN23. |
| E4 | Un usuario de otro rol intenta acceder a la gestión de usuarios. | Deniega el acceso y registra el intento. |

**Comentarios**

- La excepción E2 evita un bloqueo del sistema: si se desactivara al último Administrador, nadie podría volver a crear usuarios.
- La excepción E3 aplica la regla RN23. Un log de auditoría modificable no cumple su función de trazabilidad.
- El flujo A4 concreta la separación establecida por la regla RN29 y la decisión D10: la administración del módulo nutricional corresponde a este rol, pero eso no habilita el acceso al contenido clínico.

---

## 12. Matriz de trazabilidad

| Caso de uso | Historia | Requisitos | Reglas de negocio | Módulo | Importancia |
|---|---|---|---|---|---|
| CU-00 Autenticar usuario | HU-20 | RF22 | RN22, RN24 | M7 | Vital |
| CU-01 Registrar alumno | HU-01 | RF01, RF02 | RN01, RN04, RN05, RN06 | M1 | Vital |
| CU-04 Modificar datos de alumno | HU-06 | RF03 | RN01, RN23 | M1 | Media |
| CU-05 Dar de baja alumno | HU-07 | RF04 | RN02, RN03 | M1 | Alta |
| CU-06 Reactivar alumno | HU-08 | RF05, RF02 | RN02, RN03, RN04, RN13 | M1 | Media |
| CU-02 Registrar pago de cuota | HU-02 | RF06, RF07, RF10 | RN03, RN06, RN08, RN10, RN11 | M2 | Vital |
| CU-03 Consultar alumnos morosos | HU-03 | RF08, RF09, RF30 | RN09, RN25 | M2 | Alta |
| CU-07 Gestionar modalidades de cobro | HU-11 | RF07 | RN06, RN07, RN08, RN10 | M2 | Alta |
| CU-08 Gestionar disciplinas | HU-12 | RF11 | RN15 | M3 | Alta |
| CU-09 Crear clase y asignar instructor | HU-13 | RF12 | RN12, RN14, RN15 | M3 | Alta |
| CU-10 Gestionar versiones de grilla | HU-14 | RF13 | RN12, RN13 | M3 | Media |
| CU-11 Inscribir alumno a clase | HU-15 | RF14 | RN03, RN13 | M3 | Alta |
| CU-12 Registrar asistencia | HU-04, HU-17 | RF15, RF16, RF17, RF27 | RN16, RN17, RN18, RN19 | M4 | Alta |
| CU-13 Registrar consulta nutricional | HU-05a | RF19, RF21 | RN20, RN21, RN26, RN27, RN29 | M5 | Media |
| CU-14 Consultar evolución del paciente | HU-05b | RF20, RF21 | RN20, RN26, RN28, RN29 | M5 | Media |
| CU-15 Gestionar instructores | HU-18 | RF25 | RN02, RN14 | M6 | Alta |
| CU-16 Gestionar usuarios y roles | HU-21 | RF23, RF31 | RN22, RN23, RN24, RN29 | M7 | Alta |

### Verificación

| Control | Resultado |
|---|---|
| Casos de uso desarrollados | 17 |
| Casos de uso con flujo normal, flujos alternativos y excepciones | 17 de 17 |
| Casos de uso vinculados a al menos una historia | 17 de 17 |
| Casos de uso vinculados a al menos un requisito funcional | 17 de 17 |
| Requisitos funcionales representados en casos de uso | 24 de 31 |
| Requisitos sin caso de uso | RF10 parcial, RF18, RF24, RF26, RF28, RF29. Corresponden a consultas de solo lectura cubiertas por historias sin ficha de caso de uso. |

---

## Historial de versiones

| Versión | Fecha | Cambio |
|---|---|---|
| 1.0 | 05/2026 | Versión del relevamiento inicial. Tres casos de uso desarrollados: CU-01, CU-02 y CU-03. |
| 1.1 | 05/2026 | Incorporación de CU-00 y CU-04 a CU-16. Agregado de flujos alternativos y reglas de negocio a los casos de uso existentes. Corrección de la relación `<<extend>>` entre CU-02 y CU-01, reemplazada por la relación entre CU-06 y CU-01. Incorporación de la matriz de trazabilidad. |

---

<sub>Escuela Superior de Comercio N° 49 "Justo José de Urquiza" — Desarrollo Web / Analista Funcional de Sistemas — 2026.</sub>
