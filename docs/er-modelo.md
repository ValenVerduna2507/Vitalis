# Modelo entidad-relación

Sistema de Gestión Integral — Vitalis Centro de Entrenamiento
Equipo: Grupo 02
Versión: 1.1

---

## Índice

- [1. Alcance del modelo](#1-alcance-del-modelo)
- [2. Entidades del modelo](#2-entidades-del-modelo)
- [3. Cambios respecto del modelo preliminar](#3-cambios-respecto-del-modelo-preliminar)
- [4. Diccionario de datos](#4-diccionario-de-datos)
- [5. Relaciones y cardinalidades](#5-relaciones-y-cardinalidades)
- [6. Decisiones de diseño](#6-decisiones-de-diseño)
- [7. Reglas de integridad](#7-reglas-de-integridad)
- [8. Datos derivados](#8-datos-derivados)
- [9. Verificación contra los requisitos](#9-verificación-contra-los-requisitos)
- [10. Observaciones abiertas](#10-observaciones-abiertas)

---

## 1. Alcance del modelo

Este documento define el modelo conceptual de datos del sistema. Es la referencia única: ningún otro documento del repositorio define entidades, atributos ni relaciones por su cuenta.

El diagrama en código PlantUML se encuentra en [`diagramas/er.puml`](../diagramas/er.puml).

### Convenciones

| Convención | Criterio |
|---|---|
| Nombre de entidad | Singular, con mayúscula inicial. |
| Nombre de atributo | Minúscula, palabras separadas por guion bajo. |
| Clave primaria | `id_<entidad>`, de tipo entero autoincremental. |
| Clave foránea | Conserva el nombre de la clave primaria referenciada. |
| Obligatoriedad | Marcada con asterisco en el diagrama. |
| Baja lógica | Atributo `estado`, nunca eliminación física. |

---

## 2. Entidades del modelo

El modelo tiene 18 entidades, agrupadas por módulo.

| Módulo | Entidades |
|---|---|
| M1 — Gestión de alumnos | `Alumno`, `Tutor` |
| M2 — Gestión de cuotas y pagos | `Cuota`, `ModalidadCobro` |
| M3 — Planificación de actividades | `Disciplina`, `Clase`, `Grilla`, `Inscripcion` |
| M4 — Control de asistencia | `Asistencia` |
| M5 — Seguimiento nutricional | `Nutricionista`, `Paciente`, `SeguimientoNutricional` |
| M6 — Gestión de instructores | `Instructor`, `InstructorEspecialidad` |
| M7 — Seguridad y configuración | `Usuario`, `Rol`, `LogAuditoria`, `ParametroSistema` |

### 2.1 — Entidades del modelo preliminar

Las diez entidades identificadas en el relevamiento se conservan en su totalidad.

| Entidad | Descripción | Estado |
|---|---|---|
| `Alumno` | Persona inscripta en el centro. Es la entidad central del sistema. | Conservada, con atributos ampliados |
| `Cuota` | Registro de cada pago realizado por un alumno. | Conservada, con atributos ampliados |
| `ModalidadCobro` | Catálogo de modalidades de cobro habilitadas. | Conservada |
| `Clase` | Sesión de entrenamiento programada: día, horario, turno e instructor. | Conservada, con la disciplina normalizada |
| `Grilla` | Versión de la planificación de actividades. | Conservada |
| `Instructor` | Profesional que dicta clases. | Conservada, con la especialidad normalizada |
| `Inscripcion` | Relación entre un alumno y una clase. | Conservada |
| `Asistencia` | Registro de presencia de un alumno en una sesión concreta. | Conservada, con relaciones redefinidas |
| `SeguimientoNutricional` | Historial de consultas nutricionales. | Conservada, con atributos redefinidos |
| `Nutricionista` | Profesional de nutrición. | Conservada |

### 2.2 — Entidades incorporadas

| Entidad | Por qué se incorpora | Requisitos que la exigen |
|---|---|---|
| `Disciplina` | En el modelo preliminar la disciplina era un campo de texto dentro de `Clase`. Eso obliga a repetir el nombre en cada clase y a editarlas una por una si cambia. RF11 pide crear y gestionar disciplinas como entidades administrables. | RF11, RN15 |
| `Tutor` | RN05 obliga a registrar un familiar o tutor responsable para los alumnos menores de 18 años. El centro dicta disciplinas infantiles, y un mismo tutor puede tener varios hijos inscriptos. | RF01, RN05, RN27 |
| `Usuario` | El modelo preliminar no tenía dónde guardar credenciales. RNF04 exige contraseñas cifradas y RF22 exige autenticación. | RF22, RF23, RNF03, RNF04 |
| `Rol` | RNF03 define cuatro roles con permisos diferenciados y RN22 establece que cada usuario tiene un único rol. Modelarlo como catálogo evita repetir el nombre del rol como texto libre. | RF23, RNF03, RN22 |
| `LogAuditoria` | RNF05 obliga a registrar cada operación sensible y RF31 exige que ese registro sea consultable. Sin una entidad propia, no hay dónde guardarlo. | RF31, RNF05, RN23 |
| `InstructorEspecialidad` | Un instructor dicta más de una disciplina (Martín López dicta Funcional y Full Body). El atributo `especialidad` como texto único no lo representa. Es la tabla intermedia de una relación de muchos a muchos. | RF25 |
| `Paciente` | La decisión D10 establece que el Administrador asigna pacientes a la nutricionista sin ver el contenido de las consultas. Esa asignación existe antes de la primera consulta, de modo que no puede vivir dentro de `SeguimientoNutricional`. Además concentra la fecha de autorización del tutor que exige RN27. | RF19, RF21, RN21, RN27, RN29 |
| `ParametroSistema` | RF30 exige que el umbral de morosidad sea configurable por el Administrador. Un parámetro de negocio no debe estar escrito en el código. | RF30, RN09 |

---

## 3. Cambios respecto del modelo preliminar

Estos cambios modifican entidades que ya existían. Se documentan por separado porque afectan la lectura del diagrama original.

| # | Cambio | Entidad afectada | Justificación | Decisión |
|---|---|---|---|---|
| C1 | `Asistencia` deja de derivar exclusivamente de `Inscripcion`. Ahora referencia directamente a `Alumno` y a `Clase`, y conserva `id_inscripcion` como referencia opcional. | `Asistencia` | El criterio de aceptación de HU-04 permite registrar a un alumno activo no inscripto como asistente ocasional. Con el modelo anterior, toda asistencia requería una inscripción previa, de modo que ese caso era imposible de representar. | D7 |
| C2 | `Cuota` incorpora `id_modalidad` y conserva el monto efectivamente cobrado. | `Cuota` | La modalidad vigente vive en `Alumno`, pero un cambio de precio no debe reescribir la historia de pagos. Cada cuota guarda con qué modalidad y con qué monto fue cobrada. | D8 |
| C3 | `Clase` reemplaza el atributo de texto `disciplina` por la clave foránea `id_disciplina`. | `Clase` | Normalización. Permite modificar el nombre o la descripción de una disciplina sin editar cada clase. | — |
| C4 | `Instructor` reemplaza el atributo de texto `especialidad` por la relación de muchos a muchos con `Disciplina`. | `Instructor` | Un instructor dicta más de una disciplina. | — |
| C5 | `SeguimientoNutricional` reemplaza el atributo genérico `medidas : varchar` por atributos separados: altura, perímetro de cintura, perímetro de cadera, porcentaje de masa grasa y objetivo. | `SeguimientoNutricional` | El campo genérico impedía validar, comparar y graficar la evolución. La regla RN26 define los parámetros concretos. | D11 |
| C6 | `SeguimientoNutricional` referencia a `Paciente` en lugar de referenciar directamente a `Alumno` y a `Nutricionista`. | `SeguimientoNutricional` | La asignación de un paciente a la nutricionista existe antes de la primera consulta y es realizada por el Administrador. | D10, D14 |
| C7 | El atributo `estado` de `Cuota` cambia de significado. | `Cuota` | Ver la observación O1 en la sección 10. |

---

## 4. Diccionario de datos

Referencias: **PK** clave primaria, **FK** clave foránea, **U** valor único, **N** admite nulo.

### 4.1 — Módulo 1: gestión de alumnos

#### Alumno

Persona inscripta en el centro. Es la entidad central del sistema: toda la gestión de cuotas, asistencia y seguimiento nutricional gira en torno a ella.

| Atributo | Tipo | Clave | Obligatorio | Descripción |
|---|---|---|---|---|
| id_alumno | integer | PK | Sí | Identificador interno. |
| legajo | integer | U | Sí | Número correlativo asignado en el alta. No se reutiliza (RN04). |
| apellido | varchar(80) | | Sí | |
| nombre | varchar(80) | | Sí | |
| dni | varchar(15) | U | Sí | Identifica unívocamente al alumno (RN01). |
| fecha_nacimiento | date | | Sí | Determina si el alumno es menor de edad. |
| telefono | varchar(30) | | No | |
| email | varchar(120) | | No | |
| fecha_alta | date | | Sí | |
| estado | varchar(15) | | Sí | Activo o Baja. |
| fecha_baja | date | | N | Se completa al dar de baja (RF04). |
| motivo_baja | varchar(60) | | N | Motivo seleccionado de la lista definida (CU-05). |
| observacion_baja | text | | N | |
| id_modalidad | integer | FK | Sí | Modalidad de cobro vigente del alumno. |
| id_tutor | integer | FK | N | Obligatorio si el alumno es menor de 18 años (RN05). |

#### Tutor

Familiar o tutor responsable de un alumno menor de edad. Autoriza la participación y gestiona los trámites administrativos.

| Atributo | Tipo | Clave | Obligatorio | Descripción |
|---|---|---|---|---|
| id_tutor | integer | PK | Sí | |
| apellido | varchar(80) | | Sí | |
| nombre | varchar(80) | | Sí | |
| dni | varchar(15) | U | Sí | |
| telefono | varchar(30) | | Sí | Es el contacto de emergencia del alumno. |
| email | varchar(120) | | No | |
| parentesco | varchar(30) | | Sí | Madre, padre, tutor u otro. |

### 4.2 — Módulo 2: gestión de cuotas y pagos

#### ModalidadCobro

Catálogo de las modalidades de cobro habilitadas. Centraliza el monto base y la descripción de cada una.

| Atributo | Tipo | Clave | Obligatorio | Descripción |
|---|---|---|---|---|
| id_modalidad | integer | PK | Sí | |
| nombre | varchar(60) | U | Sí | |
| descripcion | varchar(200) | | No | |
| tipo | varchar(20) | | Sí | Mensual, PorClase o Combinada (RN06). |
| monto_base | decimal(12,2) | | Sí | Debe ser mayor a cero (RN10). |
| estado | varchar(15) | | Sí | Activa o Inactiva. No se elimina si tiene alumnos asignados. |

#### Cuota

Registro de cada pago realizado por un alumno.

| Atributo | Tipo | Clave | Obligatorio | Descripción |
|---|---|---|---|---|
| id_cuota | integer | PK | Sí | |
| id_alumno | integer | FK | Sí | |
| id_modalidad | integer | FK | Sí | Modalidad aplicada en este cobro (D8). |
| monto | decimal(12,2) | | Sí | Monto efectivamente cobrado. Mayor a cero (RN10). |
| periodo_mes | integer | | N | Mes abonado. Nulo en la modalidad por clase. |
| periodo_anio | integer | | N | Año abonado. Nulo en la modalidad por clase. |
| cantidad_clases | integer | | N | Cantidad de clases abonadas. Solo en la modalidad por clase. |
| fecha_pago | date | | Sí | |
| medio_pago | varchar(20) | | Sí | Efectivo o Transferencia (RN11). |
| estado | varchar(15) | | Sí | Registrada o Anulada. Ver observación O1. |
| id_usuario_registro | integer | FK | Sí | Usuario que registró el pago. |

### 4.3 — Módulo 3: planificación de actividades

#### Disciplina

Actividad que ofrece el centro.

| Atributo | Tipo | Clave | Obligatorio | Descripción |
|---|---|---|---|---|
| id_disciplina | integer | PK | Sí | |
| nombre | varchar(60) | U | Sí | Full Body, Funcional, Yoga, Pilates y demás. Pilates se incorpora al catálogo según la decisión D3. |
| descripcion | varchar(200) | | No | |
| publico | varchar(15) | | Sí | Adultos o Ninos. Condiciona el turno permitido (RN15). |
| estado | varchar(15) | | Sí | Activa o Inactiva. |

#### Grilla

Versión de la planificación de actividades. El atributo `activa` permite cambiar de grilla sin eliminar las anteriores.

| Atributo | Tipo | Clave | Obligatorio | Descripción |
|---|---|---|---|---|
| id_grilla | integer | PK | Sí | |
| nombre | varchar(60) | | Sí | Por ejemplo, Grilla regular o Grilla de agosto. |
| fecha_desde | date | | Sí | |
| fecha_hasta | date | | N | Nula mientras la grilla no tenga fin previsto. |
| activa | boolean | | Sí | Solo una grilla puede tener valor verdadero (RN12). |

#### Clase

Sesión de entrenamiento programada dentro de una grilla.

| Atributo | Tipo | Clave | Obligatorio | Descripción |
|---|---|---|---|---|
| id_clase | integer | PK | Sí | |
| id_disciplina | integer | FK | Sí | |
| id_grilla | integer | FK | Sí | |
| id_instructor | integer | FK | Sí | Instructor único a cargo (RN14). |
| turno | varchar(15) | | Sí | Manana o TardeNoche. |
| dia_semana | varchar(15) | | Sí | |
| hora_inicio | time | | Sí | |
| hora_fin | time | | Sí | Debe ser posterior a hora_inicio. |
| observacion | varchar(200) | | No | Se usa para registrar un ayudante, si lo hubiera. |
| estado | varchar(15) | | Sí | Activa o Inactiva. |

#### Inscripcion

Relación entre un alumno y una clase. Indica que el alumno participa de esa clase de forma regular.

| Atributo | Tipo | Clave | Obligatorio | Descripción |
|---|---|---|---|---|
| id_inscripcion | integer | PK | Sí | |
| id_alumno | integer | FK | Sí | |
| id_clase | integer | FK | Sí | |
| fecha_inscripcion | date | | Sí | |
| fecha_baja | date | | N | Se completa al dar de baja la inscripción. |
| estado | varchar(15) | | Sí | Activa o Baja. |

### 4.4 — Módulo 4: control de asistencia

#### Asistencia

Registro de presencia de un alumno en una sesión concreta de una clase.

| Atributo | Tipo | Clave | Obligatorio | Descripción |
|---|---|---|---|---|
| id_asistencia | integer | PK | Sí | |
| id_alumno | integer | FK | Sí | Referencia directa al alumno (D7). |
| id_clase | integer | FK | Sí | |
| id_inscripcion | integer | FK | N | Nulo cuando se trata de un asistente ocasional (RN18). |
| fecha | date | | Sí | |
| hora_registro | time | | Sí | Momento en que el instructor registró la asistencia (RF17). |
| estado | varchar(15) | | Sí | Presente, Ausente o Justificado (RN16). |
| es_ocasional | boolean | | Sí | Verdadero si el alumno no estaba inscripto en la clase. |
| id_instructor | integer | FK | Sí | Instructor que efectuó el registro. |

### 4.5 — Módulo 5: seguimiento nutricional

#### Nutricionista

Profesional de nutrición con acceso exclusivo al módulo de seguimiento.

| Atributo | Tipo | Clave | Obligatorio | Descripción |
|---|---|---|---|---|
| id_nutricionista | integer | PK | Sí | |
| apellido | varchar(80) | | Sí | |
| nombre | varchar(80) | | Sí | |
| matricula | varchar(30) | U | Sí | |
| email | varchar(120) | | No | |
| estado | varchar(15) | | Sí | Activa o Inactiva. |

#### Paciente

Alumno asignado al consultorio nutricional. La asignación es realizada por el Administrador y existe antes de la primera consulta.

| Atributo | Tipo | Clave | Obligatorio | Descripción |
|---|---|---|---|---|
| id_paciente | integer | PK | Sí | |
| id_alumno | integer | FK, U | Sí | Un alumno tiene a lo sumo una ficha de paciente. |
| id_nutricionista | integer | FK | Sí | |
| fecha_alta | date | | Sí | Fecha de asignación al consultorio. |
| fecha_autorizacion_tutor | date | | N | Obligatoria si el alumno es menor de 18 años (RN27). |
| estado | varchar(15) | | Sí | Activo o Inactivo. |

#### SeguimientoNutricional

Cada consulta registrada por la nutricionista. Contiene el contenido clínico del módulo, accesible únicamente por el rol Nutricionista (RN29).

| Atributo | Tipo | Clave | Obligatorio | Descripción |
|---|---|---|---|---|
| id_seguimiento | integer | PK | Sí | |
| id_paciente | integer | FK | Sí | |
| fecha | date | | Sí | |
| peso | decimal(5,2) | | Sí | En kilogramos (RN26). |
| altura | decimal(5,2) | | Sí | En centímetros. Obligatoria en la primera consulta, luego se arrastra. |
| per_cintura | decimal(5,2) | | N | En centímetros. |
| per_cadera | decimal(5,2) | | N | En centímetros. |
| masa_grasa | decimal(5,2) | | N | Porcentaje. |
| objetivo | varchar(200) | | Sí | Objetivo acordado con el paciente. |
| observaciones | text | | No | |

### 4.6 — Módulo 6: gestión de instructores

#### Instructor

Profesional que dicta clases.

| Atributo | Tipo | Clave | Obligatorio | Descripción |
|---|---|---|---|---|
| id_instructor | integer | PK | Sí | |
| apellido | varchar(80) | | Sí | |
| nombre | varchar(80) | | Sí | |
| dni | varchar(15) | U | Sí | |
| telefono | varchar(30) | | No | |
| email | varchar(120) | | No | |
| fecha_alta | date | | Sí | |
| estado | varchar(15) | | Sí | Activo o Inactivo. Baja lógica (RN02). |

#### InstructorEspecialidad

Tabla intermedia entre `Instructor` y `Disciplina`. Registra qué disciplinas dicta cada instructor.

| Atributo | Tipo | Clave | Obligatorio | Descripción |
|---|---|---|---|---|
| id_instructor | integer | PK, FK | Sí | |
| id_disciplina | integer | PK, FK | Sí | |

### 4.7 — Módulo 7: seguridad y configuración

#### Rol

Catálogo de perfiles de acceso.

| Atributo | Tipo | Clave | Obligatorio | Descripción |
|---|---|---|---|---|
| id_rol | integer | PK | Sí | |
| nombre | varchar(30) | U | Sí | Administrador, Recepcionista, Instructor, Nutricionista o Alumno. |
| descripcion | varchar(200) | | No | |

#### Usuario

Cuenta de acceso al sistema. Cada usuario tiene un único rol (RN22).

| Atributo | Tipo | Clave | Obligatorio | Descripción |
|---|---|---|---|---|
| id_usuario | integer | PK | Sí | |
| nombre_usuario | varchar(40) | U | Sí | |
| hash_contrasena | varchar(255) | | Sí | Contraseña almacenada cifrada (RNF04). |
| id_rol | integer | FK | Sí | |
| id_alumno | integer | FK | N | Vincula la cuenta con el alumno, si el rol es Alumno. |
| id_instructor | integer | FK | N | Vincula la cuenta con el instructor, si el rol es Instructor. |
| id_nutricionista | integer | FK | N | Vincula la cuenta con la nutricionista, si el rol es Nutricionista. |
| estado | varchar(15) | | Sí | Activo o Inactivo. Un usuario inactivo no puede iniciar sesión (RN24). |
| ultimo_acceso | datetime | | N | |

#### LogAuditoria

Registro de las operaciones sensibles. Es de solo lectura: no se modifica ni se elimina (RN23).

| Atributo | Tipo | Clave | Obligatorio | Descripción |
|---|---|---|---|---|
| id_log | integer | PK | Sí | |
| id_usuario | integer | FK | Sí | Usuario que ejecutó la operación. |
| fecha_hora | datetime | | Sí | |
| tipo_operacion | varchar(30) | | Sí | Alta, Modificacion, Baja, Reactivacion, Pago, AccesoDenegado. |
| entidad_afectada | varchar(40) | | Sí | Nombre de la entidad sobre la que se operó. |
| id_registro_afectado | integer | | N | Identificador del registro afectado. |
| valor_anterior | text | | N | Contenido previo, en las modificaciones. |
| valor_nuevo | text | | N | Contenido posterior, en las modificaciones. |

#### ParametroSistema

Parámetros de negocio configurables por el Administrador.

| Atributo | Tipo | Clave | Obligatorio | Descripción |
|---|---|---|---|---|
| id_parametro | integer | PK | Sí | |
| clave | varchar(50) | U | Sí | Por ejemplo, umbral_mora_dias o umbral_inactividad_dias. |
| valor | varchar(100) | | Sí | |
| descripcion | varchar(200) | | No | |
| id_usuario_modificacion | integer | FK | N | Último usuario que modificó el parámetro. |
| fecha_modificacion | datetime | | N | |

Valores iniciales previstos:

| Clave | Valor | Requisito |
|---|---|---|
| umbral_mora_dias | 30 | RF08, RF30, RN09 |
| umbral_inactividad_dias | 60 | RN25 |
| umbral_ocupacion_minima | 3 | HU-23 |
| umbral_ausencia_semanas | 2 | HU-16 |

---

## 5. Relaciones y cardinalidades

### 5.1 — Listado de relaciones

| # | Entidad origen | Relación | Entidad destino | Cardinalidad | Descripción |
|---|---|---|---|---|---|
| R01 | Alumno | abona | Cuota | 1 a N (0..N) | Un alumno puede no tener pagos registrados o tener muchos. Cada cuota pertenece a un único alumno. |
| R02 | ModalidadCobro | es asignada a | Alumno | 1 a N (0..N) | Cada alumno tiene una única modalidad vigente. Una modalidad puede tener muchos alumnos o ninguno. |
| R03 | ModalidadCobro | se aplica en | Cuota | 1 a N (0..N) | Cada cuota conserva la modalidad con la que fue cobrada (D8). |
| R04 | Tutor | es responsable de | Alumno | 1 a N (0..N) | Un tutor puede tener varios hijos inscriptos. Un alumno tiene a lo sumo un tutor. |
| R05 | Alumno | realiza | Inscripcion | 1 a N (0..N) | |
| R06 | Clase | recibe | Inscripcion | 1 a N (0..N) | |
| R07 | Disciplina | se dicta en | Clase | 1 a N (0..N) | Cada clase corresponde a una única disciplina. |
| R08 | Grilla | contiene | Clase | 1 a N (0..N) | Cada clase pertenece a una única grilla. |
| R09 | Instructor | dicta | Clase | 1 a N (0..N) | Cada clase tiene un único instructor a cargo (RN14). |
| R10 | Instructor | tiene especialidad en | Disciplina | N a M | A través de `InstructorEspecialidad`. |
| R11 | Alumno | registra | Asistencia | 1 a N (0..N) | Relación directa, que habilita al asistente ocasional (D7). |
| R12 | Clase | genera | Asistencia | 1 a N (0..N) | |
| R13 | Inscripcion | respalda | Asistencia | 1 a N (0..N), opcional | Nula en las asistencias ocasionales. |
| R14 | Instructor | efectúa | Asistencia | 1 a N (0..N) | Registra quién tomó la asistencia. |
| R15 | Alumno | es | Paciente | 1 a 1 (0..1) | Un alumno tiene a lo sumo una ficha de paciente. |
| R16 | Nutricionista | atiende a | Paciente | 1 a N (0..N) | |
| R17 | Paciente | tiene | SeguimientoNutricional | 1 a N (0..N) | Cada consulta pertenece a un único paciente. |
| R18 | Rol | agrupa | Usuario | 1 a N (0..N) | Cada usuario tiene un único rol (RN22). |
| R19 | Usuario | corresponde a | Alumno | 1 a 1 (0..1), opcional | Vincula la cuenta con la persona. |
| R20 | Usuario | corresponde a | Instructor | 1 a 1 (0..1), opcional | |
| R21 | Usuario | corresponde a | Nutricionista | 1 a 1 (0..1), opcional | |
| R22 | Usuario | genera | LogAuditoria | 1 a N (0..N) | |
| R23 | Usuario | registra | Cuota | 1 a N (0..N) | Trazabilidad de quién cobró cada cuota. |
| R24 | Usuario | modifica | ParametroSistema | 1 a N (0..N), opcional | |

### 5.2 — Esquema de relaciones principales

```text
                          Tutor
                            │ 1
                            │
                            │ 0..N
    ModalidadCobro ─────── Alumno ───────────── Paciente ──── Nutricionista
          1  │              │  1                 0..1 │ 1          1
             │              │                         │
        0..N │         ┌────┼────┬──────────┐    0..N │
             │         │    │    │          │         │
           Cuota   Inscripcion   │      Asistencia   SeguimientoNutricional
                        │  0..N  │        0..N
                   0..N │        │
                        │        │
                      Clase ─────┘
                        │ 0..N
          ┌─────────────┼─────────────┐
          │             │             │
       Grilla      Disciplina     Instructor
          1             1              1
                        │              │
                        └── N a M ─────┘
                        InstructorEspecialidad


    Rol ──── Usuario ──── LogAuditoria
     1        0..N            0..N
              │
              └──── ParametroSistema
```

### 5.3 — Relaciones que merecen atención

**Asistencia con tres orígenes.** `Asistencia` referencia a `Alumno`, a `Clase` y opcionalmente a `Inscripcion`. La referencia a la inscripción no es redundante: distingue al alumno que participa regularmente de la clase del que se presentó una vez. Cuando `id_inscripcion` es nulo y `es_ocasional` es verdadero, se trata de un asistente ocasional.

**Doble referencia a ModalidadCobro.** `Alumno` apunta a la modalidad vigente y `Cuota` a la modalidad aplicada. Puede parecer redundante, pero no lo es: si un alumno cambia de modalidad o si la dirección actualiza un precio, las cuotas anteriores deben conservar las condiciones con las que fueron cobradas. Sin esta separación, el reporte de ingresos de HU-22 devolvería cifras distintas cada vez que cambia una tarifa.

**Usuario con tres referencias opcionales.** Un usuario puede corresponder a un alumno, a un instructor o a una nutricionista, y a lo sumo a uno de los tres. Los usuarios con rol Administrador y Recepcionista no tienen ninguna de las tres referencias completada, porque no existe una entidad de negocio que los represente.

---

## 6. Decisiones de diseño

### 6.1 — Decisiones del modelo preliminar, conservadas

**Separación entre Inscripcion y Asistencia.** La inscripción registra que un alumno participa de una clase de forma regular; la asistencia registra si efectivamente estuvo presente en cada sesión. Esta separación permite distinguir a los alumnos inscriptos que no asisten de los que simplemente no han pagado. Es información valiosa para la dirección, y es la base del reporte de ocupación de HU-23.

**El atributo activa en Grilla.** El sistema puede tener múltiples versiones de planificación almacenadas, pero solo una activa a la vez. Esto resuelve la necesidad observada en las planillas relevadas, donde coexistían una grilla regular y una grilla de agosto sin trazabilidad entre ellas.

**ModalidadCobro como entidad separada.** Las modalidades de cobro son datos de negocio que la dirección puede modificar: nuevos precios, nuevas combinaciones. Modelarlas como entidad independiente, en lugar de un campo de texto en `Alumno`, permite actualizarlas sin tocar los registros de cada alumno.

**Nutricionista como entidad separada de Instructor.** Aunque ambos son profesionales del centro, sus roles, accesos y datos son completamente distintos. Mantenerlos en entidades separadas simplifica el control de acceso por roles que exigen RNF03 y RNF06, y evita mezclar información sensible con información operativa.

### 6.2 — Decisiones incorporadas

**Baja lógica en todas las entidades de negocio.** Ninguna entidad se elimina físicamente. `Alumno`, `Instructor`, `Disciplina`, `Clase`, `ModalidadCobro`, `Nutricionista`, `Paciente` y `Usuario` tienen atributo `estado`. La regla RN02 lo exige para alumnos; el equipo lo extendió al resto por el mismo motivo: una eliminación física rompe la trazabilidad de todo lo que referencia ese registro.

**El IMC no se almacena.** Es un valor derivado del peso y la altura de cada consulta. Guardarlo abriría la posibilidad de que quede desactualizado si se corrige alguno de los dos valores. Ver la sección 8.

**La morosidad no se almacena.** No existe un atributo `es_moroso` en `Alumno`. La condición se calcula comparando la fecha del último pago con el umbral configurado en `ParametroSistema`. Un atributo almacenado exigiría un proceso que lo actualice todos los días y quedaría desincronizado ante cualquier falla.

**Paciente como entidad intermedia.** La alternativa era que `SeguimientoNutricional` referenciara directamente a `Alumno` y a `Nutricionista`, como en el modelo preliminar. El problema es que la asignación de un paciente al consultorio existe **antes** de la primera consulta: el Administrador la realiza sin ver contenido clínico, según la decisión D10. Además, la fecha de autorización del tutor que exige RN27 corresponde al paciente, no a cada consulta. Con `Paciente` como entidad, la separación entre administración y contenido clínico que establece RN29 queda reflejada en la estructura de datos y no solo en la capa de permisos.

**ParametroSistema en lugar de constantes.** Los umbrales de mora, inactividad, ocupación mínima y ausencia reiterada son decisiones comerciales de la dirección. La restricción RE04 advierte que la dirección los modifica sin previo aviso al equipo técnico. Guardarlos como registros configurables evita que cada ajuste requiera una intervención sobre el código.

**Tutor como entidad y no como atributos de Alumno.** Un mismo tutor puede tener varios hijos inscriptos, situación habitual en Aeróbica Infantil y Kids Fit and Fun. Con los datos del tutor dentro de `Alumno`, cambiar un teléfono obligaría a modificar varios registros y podrían quedar desincronizados.

### 6.3 — Alternativas evaluadas y descartadas

| Alternativa | Por qué se descartó |
|---|---|
| Guardar el rol como texto dentro de `Usuario`. | Impide validar los valores y obliga a comparar cadenas en cada control de acceso. |
| Modelar los permisos como una tabla `RolPermiso`. | Con cinco roles fijos y una matriz de permisos estable, agrega complejidad sin beneficio. Se reevaluaría si los roles fueran configurables. |
| Una entidad `Persona` común a `Alumno`, `Instructor` y `Nutricionista`. | Aporta elegancia teórica pero complica todas las consultas del sistema. El supuesto S06 establece que los instructores no son alumnos, de modo que la superposición es marginal. |
| Guardar el estado de cuenta del alumno como atributo. | Genera el mismo problema que almacenar la morosidad: quedaría desactualizado. |
| Registrar la asistencia solo por inscripción. | Es el modelo preliminar. No permite representar al asistente ocasional que exige RF27. |

---

## 7. Reglas de integridad

### 7.1 — Integridad de entidad

| Entidad | Restricción |
|---|---|
| Alumno | `dni` único en todo el padrón, incluidos los alumnos dados de baja (RN01). `legajo` único y no reutilizable (RN04). |
| Instructor | `dni` único entre los instructores. |
| Nutricionista | `matricula` única. |
| Usuario | `nombre_usuario` único. |
| Disciplina | `nombre` único. |
| ModalidadCobro | `nombre` único. |
| Rol | `nombre` único. |
| Paciente | `id_alumno` único: un alumno tiene a lo sumo una ficha de paciente. |
| ParametroSistema | `clave` única. |
| Asistencia | Combinación única de `id_alumno`, `id_clase` y `fecha` (RN19). |
| InstructorEspecialidad | Clave primaria compuesta por `id_instructor` e `id_disciplina`. |

### 7.2 — Integridad referencial

| Regla | Comportamiento |
|---|---|
| Ninguna clave foránea admite eliminación en cascada. | Las bajas son lógicas, de modo que no hay eliminaciones que propagar. |
| `Alumno.id_modalidad` | No se puede desactivar una modalidad con alumnos asignados sin reasignarlos previamente (CU-07, E1). |
| `Clase.id_instructor` | No se puede dar de baja un instructor con clases asignadas en la grilla activa (CU-15, E1). |
| `Clase.id_disciplina` | No se puede eliminar una disciplina con clases asociadas. Solo desactivarla (CU-08, E1). |
| `Asistencia.id_inscripcion` | Admite nulo. Dar de baja una inscripción no elimina las asistencias asociadas (HU-15, criterio 8). |
| `LogAuditoria` | No admite modificación ni eliminación de registros (RN23). |

### 7.3 — Restricciones de dominio

| Entidad y atributo | Restricción |
|---|---|
| Alumno.estado | Activo, Baja. |
| Cuota.monto | Mayor a cero (RN10). |
| Cuota.medio_pago | Efectivo, Transferencia (RN11). |
| Cuota.estado | Registrada, Anulada. |
| ModalidadCobro.tipo | Mensual, PorClase, Combinada (RN06). |
| ModalidadCobro.monto_base | Mayor a cero. |
| Disciplina.publico | Adultos, Ninos. |
| Clase.turno | Manana, TardeNoche. |
| Clase.hora_fin | Posterior a `hora_inicio`. |
| Asistencia.estado | Presente, Ausente, Justificado (RN16). |
| Grilla.activa | Solo un registro con valor verdadero en toda la tabla (RN12). |
| SeguimientoNutricional.peso | Mayor a cero. |
| SeguimientoNutricional.altura | Mayor a cero. |

### 7.4 — Reglas que el modelo no puede expresar por sí solo

Estas reglas requieren validación en la lógica de la aplicación.

| Regla | Descripción |
|---|---|
| RN05 | Si `Alumno.fecha_nacimiento` indica menos de 18 años, `id_tutor` es obligatorio. |
| RN12 | Una única grilla con `activa` en verdadero. |
| RN13 | Una inscripción solo puede apuntar a una clase de la grilla activa. |
| RN14 | Un instructor no puede tener dos clases superpuestas en día y horario dentro de la misma grilla. |
| RN15 | Una disciplina con `publico` igual a Ninos solo puede asociarse a clases del turno tarde. |
| RN17 | Un instructor solo registra asistencia de sus propias clases y del día en curso. |
| RN21 | Solo los alumnos con `estado` Activo pueden ser dados de alta como pacientes. |
| RN27 | Si el paciente es menor de 18 años, `fecha_autorizacion_tutor` es obligatoria antes de la primera consulta. |

---

## 8. Datos derivados

Estos valores no se almacenan: se calculan en el momento de la consulta.

| Dato derivado | Cálculo | Dónde se usa |
|---|---|---|
| Estado de cuenta del alumno | Compara la fecha de la última cuota registrada con la fecha actual, contra el parámetro `umbral_mora_dias`. | HU-02, HU-03, HU-10 |
| Días de mora | Diferencia entre la fecha actual y la fecha de la última cuota. | HU-03 |
| Monto adeudado | Cantidad de períodos vencidos multiplicada por el monto de la modalidad vigente. | HU-03 |
| Alumno inactivo | Alumno con modalidad de tipo PorClase sin cuotas ni asistencias en el período definido por `umbral_inactividad_dias` (RN25). | HU-23 |
| Índice de masa corporal | Peso dividido por el cuadrado de la altura expresada en metros. | HU-05a, HU-05b |
| Variación de peso e IMC | Diferencia respecto de la consulta anterior y respecto de la primera. | HU-05b |
| Edad del alumno | Diferencia entre la fecha actual y `fecha_nacimiento`. | HU-01, HU-05a |
| Ocupación de una clase | Cantidad de inscripciones activas y promedio de asistencias con estado Presente sobre el total de sesiones del período. | HU-23 |
| Total recaudado por período | Suma de `Cuota.monto` en el rango de fechas, agrupada por modalidad y por disciplina. | HU-22 |
| Historial de cambios de un alumno | Registros de `LogAuditoria` filtrados por entidad `Alumno` e `id_registro_afectado`. | HU-06, CU-04 |

---

## 9. Verificación contra los requisitos

Cada requisito funcional debe estar soportado por el modelo. Esta tabla verifica esa condición.

| Requisito | Entidades que lo soportan |
|---|---|
| RF01 Registrar alumno | Alumno, Tutor, ModalidadCobro |
| RF02 Validar DNI único | Alumno |
| RF03 Modificar alumno con historial | Alumno, LogAuditoria |
| RF04 Baja lógica | Alumno |
| RF05 Reactivación | Alumno |
| RF06 Registrar pago | Cuota, Usuario |
| RF07 Modalidades variables | ModalidadCobro, Cuota, Alumno |
| RF08 Detección de morosos | Cuota, ParametroSistema (dato derivado) |
| RF09 Reporte de morosos | Cuota, Alumno, ModalidadCobro |
| RF10 Historial de pagos | Cuota |
| RF11 Gestionar disciplinas | Disciplina |
| RF12 Asignar instructor | Clase, Instructor |
| RF13 Versiones de grilla | Grilla, Clase |
| RF14 Inscribir a clase | Inscripcion, Alumno, Clase |
| RF15 Listado de inscriptos | Inscripcion, Clase, Alumno |
| RF16 Registrar asistencia | Asistencia |
| RF17 Fecha y hora del registro | Asistencia |
| RF18 Historial de asistencia | Asistencia |
| RF19 Registrar consulta nutricional | Paciente, SeguimientoNutricional |
| RF20 Historial de evolución | SeguimientoNutricional |
| RF21 Acceso restringido | Usuario, Rol, Paciente |
| RF22 Autenticación | Usuario, Rol |
| RF23 Gestión de usuarios | Usuario, Rol |
| RF24 Búsqueda de alumnos | Alumno |
| RF25 Gestión de instructores | Instructor, InstructorEspecialidad |
| RF26 Agenda propia | Clase, Instructor, Grilla |
| RF27 Asistente ocasional | Asistencia |
| RF28 Reporte de ingresos | Cuota, ModalidadCobro, Disciplina |
| RF29 Ocupación de clases | Inscripcion, Asistencia, Clase |
| RF30 Umbral configurable | ParametroSistema |
| RF31 Consulta del log | LogAuditoria, Usuario |

### Resultado

| Control | Resultado |
|---|---|
| Requisitos funcionales soportados por el modelo | 31 de 31 |
| Entidades sin requisito que las justifique | Ninguna |
| Entidades del modelo preliminar conservadas | 10 de 10 |
| Entidades incorporadas | 8 |

---

## 10. Observaciones abiertas

| ID | Observación | Estado |
|---|---|---|
| **O1** | El atributo `estado` de `Cuota` figuraba en el modelo preliminar con la descripción "permite identificar si el pago está al día, pendiente o si el alumno es moroso". Esa descripción corresponde al estado de cuenta del **alumno**, que es un dato derivado, y no al de una cuota individual: una cuota registrada es un hecho consumado, no tiene estados de mora. El equipo conservó el atributo pero le asignó un dominio propio: Registrada o Anulada, que es lo que necesita el mecanismo de corrección de pagos mal cargados previsto en HU-10. La morosidad se calcula, según se detalla en la sección 8. |
| **O2** | El modelo no contempla el cupo máximo por clase. Ninguna historia ni requisito lo pide, y la dirección no lo mencionó en el relevamiento. Si más adelante se decidiera limitar la cantidad de inscriptos, alcanzaría con agregar un atributo `cupo_maximo` a `Clase` y una validación en CU-11. Queda registrado para no descubrirlo tarde. |
| **O3** | La política de retención del `LogAuditoria` no está definida. Corresponde a la cuestión diferida B2 de `docs/requisitos.md`, a resolver antes de la puesta en producción. |

---

## Historial de versiones

| Versión | Fecha | Cambio |
|---|---|---|
| 1.0 | 05/2026 | Modelo del relevamiento inicial. Diez entidades con sus atributos principales y relaciones. |
| 1.1 | 05/2026 | Incorporación de ocho entidades para cubrir seguridad, disciplinas, tutores, especialidades, pacientes, auditoría y parámetros. Redefinición de las relaciones de `Asistencia` (D7), de la doble referencia a `ModalidadCobro` (D8) y de los atributos de `SeguimientoNutricional` (D11). Incorporación del diccionario de datos, las reglas de integridad, los datos derivados y la verificación contra requisitos. |

---

<sub>Escuela Superior de Comercio N° 49 "Justo José de Urquiza" — Desarrollo Web / Analista Funcional de Sistemas — 2026.</sub>
