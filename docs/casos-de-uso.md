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
