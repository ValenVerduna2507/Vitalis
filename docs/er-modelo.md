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



---

<sub>Escuela Superior de Comercio N° 49 "Justo José de Urquiza" — Desarrollo Web / Analista Funcional de Sistemas — 2026.</sub>
