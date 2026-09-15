# Sistema de Gestión Integral — Vitalis Centro de Entrenamiento

Repositorio de documentación de análisis funcional del **Sistema de Gestión Integral para Vitalis Centro de Entrenamiento**, desarrollado como trabajo práctico de la materia **Desarrollo Web — Analista Funcional de Sistemas** de la Escuela Superior de Comercio N° 49 "Justo José de Urquiza" (Rosario, Santa Fe).

---

## Índice

- [1. ¿Qué es Vitalis?](#1-qué-es-vitalis)
- [2. Problema a resolver](#2-problema-a-resolver)
- [3. Objetivo del sistema](#3-objetivo-del-sistema)
- [4. Alcance](#4-alcance)
- [5. Actores principales](#5-actores-principales)
- [6. Módulos del sistema](#6-módulos-del-sistema)
- [7. Tecnologías y herramientas previstas](#7-tecnologías-y-herramientas-previstas)
- [8. Estructura del repositorio](#8-estructura-del-repositorio)
- [9. Documentación disponible](#9-documentación-disponible)
- [10. Diagramas](#10-diagramas)
- [11. Estado del proyecto](#11-estado-del-proyecto)
- [12. Integrantes](#12-integrantes)
- [13. Convenciones de trabajo](#13-convenciones-de-trabajo)

---

## 1. ¿Qué es Vitalis?

Vitalis es un centro de entrenamiento físico ubicado en **Pueblo Esther, provincia de Santa Fe**. Ofrece una amplia variedad de disciplinas en dos franjas horarias (turno mañana y turno tarde-noche) y complementa su propuesta con el servicio de una nutricionista.

**Disciplinas relevadas:**

| Disciplina | Perfil de alumno | Turnos habituales |
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

**Dimensión aproximada de la operación:**

| Indicador | Valor |
|---|---|
| Alumnos registrados (activos e históricos) | Más de 200 |
| Instructores y profesionales | Alrededor de 12 |
| Franjas horarias | Mañana (07:00–12:00) y tarde-noche (16:00–22:00) |
| Servicio complementario | Consultorio nutricional (martes de 15:00 a 18:00) |

---

## 2. Problema a resolver

Actualmente **toda la gestión administrativa de Vitalis se realiza mediante planillas de cálculo** (Microsoft Excel): una planilla para el registro de alumnos y el seguimiento de cuotas, y otra para la planificación de actividades y la asignación de instructores.

Durante el relevamiento se identificaron las siguientes problemáticas:

| # | Problemática | Impacto |
|---|---|---|
| P1 | **Modalidades de cobro variables** (mensual fija, por clase, combinada). Cada alumno puede tener una condición distinta. | Dificulta el control automático de morosidad y la proyección de ingresos. |
| P2 | **Alta rotación de alumnos**: un porcentaje significativo de los registros corresponde a personas dadas de baja. | Riesgo de pérdida de historial si se eliminan registros; imposibilidad de reactivar alumnos con su información previa. |
| P3 | **Planificación por temporada**: la grilla de clases e instructores se reorganiza periódicamente (se relevaron al menos dos versiones distintas). | Las versiones anteriores se pisan o se duplican en archivos sueltos, sin trazabilidad. |
| P4 | **Ausencia de acceso a la información según el rol**: solo quien tiene la planilla puede consultar datos. | Instructores y alumnos dependen de terceros para cualquier consulta; el control de asistencia es informal o inexistente. |
| P5 | **Datos sensibles sin resguardo**: el seguimiento nutricional se lleva en registros físicos. | Riesgo de pérdida y de exposición de información de salud. |
| P6 | **Errores de carga y duplicación de registros** al no existir validaciones. | Padrón inconsistente; un alta duplicada contamina pagos, asistencias e historial. |

---

## 3. Objetivo del sistema

> Diseñar un sistema de información web que **centralice y digitalice los procesos administrativos de Vitalis**, eliminando la dependencia de las planillas manuales y brindando a cada actor —propietaria, recepcionista, instructores, nutricionista y alumnos— una herramienta **confiable, accesible y escalable**, con vistas y permisos diferenciados según su rol.

### Objetivos específicos

| ID | Objetivo |
|---|---|
| OE1 | Unificar el padrón de alumnos en un único repositorio digital, con validación de duplicados y baja lógica. |
| OE2 | Registrar los pagos de cuotas contemplando las tres modalidades de cobro vigentes y detectar la morosidad de forma automática. |
| OE3 | Gestionar la planificación de disciplinas, horarios e instructores admitiendo múltiples versiones de grilla. |
| OE4 | Digitalizar el control de asistencia para que cada instructor lo registre desde el salón. |
| OE5 | Aislar el seguimiento nutricional en un módulo de acceso restringido. |
| OE6 | Proveer reportes de gestión (morosidad, asistencia, ocupación) a la dirección. |
| OE7 | Implementar control de acceso por roles sobre todas las funcionalidades del sistema. |

---

## 4. Alcance

### Dentro del alcance

- Gestión de alumnos: altas, modificaciones, bajas lógicas y reactivaciones.
- Gestión de cuotas y pagos: registro de pagos con modalidades variables, seguimiento de morosidad y reportes.
- Planificación de actividades: gestión de disciplinas, horarios, turnos y versiones de grilla.
- Asignación de instructores a clases.
- Inscripción de alumnos a clases.
- Control de asistencia por clase y por alumno.
- Seguimiento nutricional: registro de consultas y evolución de parámetros.
- Gestión de instructores: altas, asignaciones y visualización de horarios propios.
- Reportes de gestión, con foco en morosidad.
- Gestión de usuarios y control de acceso por roles.

### Fuera del alcance

- Gestión contable general del negocio (libro diario, balances, liquidación de sueldos).
- Integración con medios de pago electrónicos externos (pasarelas, débito automático, QR).
- Comunicación masiva con alumnos (campañas, newsletters, notificaciones push).
- Facturación electrónica y su integración con organismos fiscales.
- Control de acceso físico al establecimiento (molinetes, tarjetas, biometría).
- Aplicación móvil nativa. La solución se plantea como web responsiva.

> Estos puntos quedan fuera de la **primera versión**. El sistema debe diseñarse de forma modular (RNF12) para poder incorporarlos más adelante sin refactorizar lo existente.

---

## 5. Actores principales

| Actor | Tipo | Rol en el sistema | Nivel de impacto |
|---|---|---|---|
| Propietaria / Directora | Interno | Administrador | Alto |
| Recepcionista / Personal administrativo | Interno — usuario operativo | Recepcionista | Alto |
| Instructores / Profesores | Interno — usuario operativo | Instructor | Alto |
| Nutricionista | Interno — usuario operativo | Nutricionista | Medio |
| Alumnos | Externo — beneficiario final | Alumno (consulta) | Alto |
| Familiares de alumnos menores | Externo — representante legal | Sin acceso directo en v1.0 | Medio |

El detalle completo de necesidades, expectativas, información requerida y riesgos asociados a cada actor se encuentra en [`docs/stakeholders.md`](docs/stakeholders.md).

---

## 6. Módulos del sistema

| # | Módulo | Descripción | Requisitos asociados |
|---|---|---|---|
| M1 | **Gestión de alumnos** | Alta, modificación, baja lógica y reactivación de alumnos. Padrón único con validación de DNI. | RF01 – RF05 |
| M2 | **Gestión de cuotas y pagos** | Registro de pagos, modalidades de cobro variables, detección de morosidad e historial. | RF06 – RF10 |
| M3 | **Planificación de actividades** | Disciplinas, clases, turnos, versiones de grilla, asignación de instructores e inscripción de alumnos. | RF11 – RF14 |
| M4 | **Control de asistencia** | Listado de inscriptos por clase, registro de asistencia e historial. | RF15 – RF18 |
| M5 | **Seguimiento nutricional** | Registro de consultas, evolución de parámetros y acceso restringido. | RF19 – RF21 |
| M6 | **Gestión de instructores** | Alta y baja de instructores, asignaciones y agenda propia. | Requisitos adicionales (ver `docs/requisitos.md`) |
| M7 | **Seguridad, usuarios y roles** | Autenticación, perfiles de usuario, permisos y auditoría. | Requisitos adicionales + RNF03 – RNF06 |
| M8 | **Reportes** | Morosidad, ingresos, asistencia y ocupación de clases. | RF09 + requisitos adicionales |

> **Nota de trazabilidad:** los módulos M6, M7 y M8 aparecen en el alcance del documento original pero no contaban con requisitos funcionales propios. Los requisitos que los cubren fueron incorporados por el equipo como **requisitos adicionales** y están identificados como tales en [`docs/requisitos.md`](docs/requisitos.md).

---

## 7. Tecnologías y herramientas previstas

La etapa actual del trabajo es de **análisis y diseño funcional**, por lo que todavía no hay implementación. La siguiente propuesta técnica es **preliminar y sujeta a confirmación** con la cátedra antes de comenzar el desarrollo.

| Capa | Propuesta | Motivo |
|---|---|---|
| Front-end | HTML5, CSS3, JavaScript y Bootstrap 5 | Interfaz responsiva exigida por RNF07 (uso en tablet desde el salón). |
| Back-end | Node.js con Express (alternativa evaluada: PHP 8) | Concentra toda la lógica de negocio y las reglas del sistema. |
| Base de datos | PostgreSQL | Modelo relacional acorde al ER documentado. |
| Automatización e integración | n8n | Ejecuta procesos periódicos y automáticos que no requieren intervención de un usuario. No reemplaza al back-end. |
| Reportes PDF | Librería de generación de PDF del lado del servidor | Exigido por RF09 (exportación del reporte de morosidad). |
| Modelado UML | PlantUML | Diagramas versionables en texto plano dentro del repositorio. |
| Wireframes | Figma / Balsamiq | Prototipado de baja fidelidad. |
| Documentación | Markdown | Legible en GitHub y versionable. |
| Control de versiones | Git + GitHub | Trabajo colaborativo con ramas y Pull Requests. |
| Gestión ágil | Tablero Kanban (GitHub Projects) | Seguimiento de historias de usuario y slices. |

### Arquitectura conceptual

```text
   ┌──────────────┐        ┌──────────────┐        ┌──────────────┐
   │   Web App    │ ─────► │   Back-end   │ ─────► │  PostgreSQL  │
   │  (navegador  │        │  (lógica de  │        │   (datos)    │
   │   y tablet)  │ ◄───── │   negocio)   │ ◄───── │              │
   └──────────────┘        └──────┬───────┘        └──────┬───────┘
                                  │                       │
                                  │  consulta / dispara   │
                                  ▼                       ▼
                           ┌──────────────────────────────────┐
                           │              n8n                 │
                           │  Automatizaciones programadas    │
                           │  - Detección de morosidad        │
                           │  - Reportes periódicos           │
                           │  - Notificaciones (extensión)    │
                           └──────────────────────────────────┘
```

**Sobre el rol de n8n.** La Web App y el back-end siguen siendo responsables de toda la funcionalidad del sistema: las altas, los cobros, la asistencia y las consultas se resuelven ahí. n8n se incorpora para lo que ocurre **sin que nadie lo pida**: tareas que se disparan por tiempo o por un evento y que hoy alguien tiene que acordarse de hacer.

| Automatización prevista | Qué resuelve | Estado |
|---|---|---|
| Detección y seguimiento de morosidad | Ejecuta a diario la clasificación de alumnos morosos (RF08) y de inactivos (RN25), y avisa a la dirección cuando aparecen casos nuevos. | Prevista para la implementación |
| Reportes periódicos a la dirección | Genera y envía el reporte de morosidad e ingresos con la frecuencia que la dirección defina, sin que tenga que entrar al sistema a pedirlo. | Prevista para la implementación |
| Notificaciones automáticas por email | Avisos de vencimiento de cuota o de cambios de grilla. | Extensión futura. La comunicación masiva con alumnos está fuera del alcance de la versión 1.0. |

El cálculo de la morosidad y la generación de los reportes se resuelven en el back-end. n8n solo decide **cuándo** ejecutarlos y **a quién** entregar el resultado.

---

## 8. Estructura del repositorio

```text
/
├── README.md                        # Portada del proyecto (este archivo)
├── integrantes.md                   # Equipo, roles y responsabilidades
├── RECURSOS.md                      # Guía de Git, GitHub, PlantUML y UML
├── DoR.md                           # Definition of Ready del equipo
├── slicing.md                       # Épica → historias → slices verticales
│
├── docs/
│   ├── requisitos.md                # Contexto, alcance, RF, RNF y reglas de negocio
│   ├── historias-de-usuario.md      # Historias con criterios de aceptación e INVEST
│   ├── casos-de-uso.md              # Casos de uso desarrollados
│   ├── er-modelo.md                 # Modelo entidad-relación y decisiones de diseño
│   ├── diseño-ui.md                 # Documentación funcional de pantallas
│   └── stakeholders.md              # Análisis y matriz de stakeholders
│
├── diagramas/
│   ├── arquitectura-general.dot     # Diagrama general de arquitectura (Graphviz)
│   ├── arquitectura-general.png     # Imagen generada del diagrama general
│   ├── arquitectura-general.svg     # Versión vectorial del diagrama general
│   ├── casos-de-uso.puml            # Diagrama UML de casos de uso (PlantUML)
│   ├── er.puml                      # Modelo entidad-relación (PlantUML)
│   └── wireframes/                  # Bocetos de las pantallas principales
│
└── cuestionario/                    # Relevamiento: preguntas y respuestas
```

---

## 9. Documentación disponible

| Documento | Contenido | Estado |
|---|---|---|
| [`docs/requisitos.md`](docs/requisitos.md) | Contexto, problema, objetivos, alcance, RF01–RF21, RNF01–RNF13, reglas de negocio, restricciones, roles y permisos, supuestos y dependencias. | Pendiente |
| [`docs/historias-de-usuario.md`](docs/historias-de-usuario.md) | Historias de usuario con rol, módulo, requisitos relacionados, criterios de aceptación y evaluación INVEST. | Pendiente |
| [`docs/casos-de-uso.md`](docs/casos-de-uso.md) | Casos de uso con actores, precondiciones, postcondiciones, flujo normal, alternativas, excepciones y reglas de negocio. | Pendiente |
| [`docs/er-modelo.md`](docs/er-modelo.md) | Entidades, atributos, cardinalidades, claves y justificación de las decisiones de modelado. | Pendiente |
| [`docs/diseño-ui.md`](docs/diseño-ui.md) | Definición funcional de las pantallas del sistema: objetivo, acceso por rol, elementos, acciones, validaciones y navegación. | Pendiente |
| [`docs/stakeholders.md`](docs/stakeholders.md) | Fichas de cada parte interesada y matriz de impacto/interés. | Pendiente |
| [`DoR.md`](DoR.md) | Condiciones que debe cumplir una historia para entrar a desarrollo, checklist y autoevaluación del equipo. | Pendiente |
| [`slicing.md`](slicing.md) | Descomposición de épicas en historias y slices verticales entregables. | Pendiente |
| [`RECURSOS.md`](RECURSOS.md) | Guía práctica de trabajo con Git, GitHub, PlantUML y material de consulta. | Pendiente |
| [`cuestionario/`](cuestionario/) | Cuestionario de relevamiento aplicado a los stakeholders y sus respuestas. | Pendiente |

---

## 10. Diagramas

Todos los diagramas se versionan como código fuente dentro de `diagramas/`, de modo que cualquier cambio quede registrado en el historial de Git y no dependa de un archivo binario.

| Archivo | Diagrama | Contenido |
|---|---|---|
| [`diagramas/arquitectura-general.dot`](diagramas/arquitectura-general.dot) | Arquitectura general | Vista completa del sistema en una sola lámina: roles, puestos de acceso, capa de presentación, back-end, base de datos y automatización. |
| [`diagramas/casos-de-uso.puml`](diagramas/casos-de-uso.puml) | Casos de uso (UML) | Actores del sistema, casos de uso por módulo y relaciones `include` / `extend`. |
| [`diagramas/er.puml`](diagramas/er.puml) | Entidad-relación | Entidades, atributos, claves primarias y foráneas, y cardinalidades. |
| [`diagramas/wireframes/`](diagramas/wireframes/) | Wireframes | Bocetos de baja fidelidad de las pantallas principales. |

### 10.1 — Diagrama general de arquitectura

Es la vista de conjunto del sistema. Integra en una sola lectura lo que el resto de la documentación desarrolla por separado: los cinco roles de `docs/requisitos.md`, las diecinueve pantallas de `docs/diseño-ui.md`, los ocho módulos con sus treinta y un requisitos funcionales, las dieciocho entidades de `docs/er-modelo.md` y la automatización prevista en la restricción RE11.

![Diagrama general de arquitectura del Sistema de Gestión Integral de Vitalis](diagramas/arquitectura-general.png)

Se lee de arriba hacia abajo, en seis capas:

| Capa | Qué muestra |
|---|---|
| 1 · Usuarios y roles | Los cinco roles definidos en `docs/requisitos.md` 10.1, con el alcance de cada uno. |
| 2 · Puestos de acceso | Desde qué equipo trabaja cada rol. La tablet del salón corresponde a la restricción RE05. |
| 3 · Capa de presentación | Las diecinueve pantallas agrupadas por área funcional. |
| 4 · Capa de aplicación | Los servicios transversales y los ocho módulos funcionales. Acá vive toda la lógica de negocio. |
| 5 · Capa de datos | Las dieciocho entidades de PostgreSQL, agrupadas por el módulo que las usa. |
| 6 · Automatización | n8n y los dos procesos programados previstos. No contiene lógica de negocio. |

Dos aclaraciones que el diagrama deja explícitas, porque son las que suelen malinterpretarse:

- **La interfaz no valida ni decide.** Las reglas RN01 a RN29 y el control de acceso por rol se resuelven en el back-end. La matriz de permisos de `docs/requisitos.md` 10.2 define qué puede hacer cada rol sobre cada funcionalidad.
- **n8n no reemplaza al back-end.** Define *cuándo* se ejecuta un proceso y *a quién* se le entrega el resultado. Ningún requisito funcional depende de n8n para cumplirse.

### 10.2 — Cómo visualizar y regenerar los diagramas

**Diagramas en PlantUML** (`.puml`): abrir el archivo, copiar su contenido y pegarlo en el servidor oficial <https://www.plantuml.com/plantuml/uml/>. También pueden previsualizarse desde Visual Studio Code con la extensión **PlantUML** (`jebbs.plantuml`). En [`RECURSOS.md`](RECURSOS.md) está el paso a paso de instalación.

**Diagrama en Graphviz** (`.dot`): el diagrama general usa Graphviz en lugar de PlantUML porque necesita control explícito del apilado por capas, que PlantUML no garantiza. Los diagramas UML del repositorio siguen en PlantUML. Para regenerar la imagen después de editar el `.dot`:

```bash
dot -Tpng -Gdpi=110 diagramas/arquitectura-general.dot -o diagramas/arquitectura-general.png
dot -Tsvg              diagramas/arquitectura-general.dot -o diagramas/arquitectura-general.svg
```

Sin instalar nada, puede pegarse el contenido del `.dot` en <https://dreampuf.github.io/GraphvizOnline/>.

> Si se modifica el `.dot`, hay que volver a generar el `.png` y el `.svg` en el mismo commit. La imagen del README se rompe visualmente si queda desactualizada respecto del fuente.

---

## 11. Estado del proyecto

**Etapa actual:** análisis funcional y diseño de la solución. **No hay desarrollo de software iniciado.**

| Entregable | Estado | Observaciones |
|---|---|---|
| Relevamiento y contexto | Completo | Basado en las planillas provistas y entrevistas con la dirección. |
| Identificación de stakeholders | Completo | Seis stakeholders identificados y caracterizados. |
| Requisitos funcionales (RF01–RF21) | Completo | Se agregan requisitos adicionales para cubrir M6, M7 y M8. |
| Requisitos no funcionales (RNF01–RNF13) | Completo | Cinco categorías: rendimiento, seguridad, usabilidad, disponibilidad y mantenibilidad. |
| Historias de usuario | En ampliación | Cinco historias base (una por módulo); se amplía la cobertura. |
| Casos de uso | En ampliación | Tres casos desarrollados; se suman los faltantes. |
| Modelo entidad-relación | En revisión | Diez entidades base; se evalúan ajustes por trazabilidad. |
| Diseño de interfaz | Pendiente | Documentación funcional de pantallas y wireframes. |
| Slicing y DoR | Pendiente | Artefactos ágiles del equipo. |
| Implementación | No iniciada | Fuera del alcance de la entrega actual. |

**Versión de la documentación:** v1.1 — repositorio en construcción
**Base:** Presentación Preliminar v1.0 (mayo de 2026)

---

## 12. Integrantes

| Integrante | Rol en el equipo |
|---|---|
| **Duran, Berenice** | Análisis de requisitos y documentación |
| **Gómez, Felipe** | Modelado de datos y diagramas UML |
| **Rodriguez, Lautaro** | Casos de uso y control de consistencia |
| **Verduna, Valentino** | Historias de usuario, diseño UI y coordinación del repositorio |

**Grupo:** Grupo 02
**Docente:** Pedernera, Pablo
**Institución:** Escuela Superior de Comercio N° 49 "Justo José de Urquiza" — Rosario, Santa Fe
**Materia:** Desarrollo Web — Analista Funcional de Sistemas
**Ciclo lectivo:** 2026

El detalle de responsabilidades y la forma de trabajo del equipo están en [`integrantes.md`](integrantes.md).

---

## 13. Convenciones de trabajo

### Ramas

| Rama | Uso |
|---|---|
| `main` | Documentación estable y revisada. |
| `docs/<tema>` | Redacción o modificación de documentación. |
| `diagram/<tema>` | Creación o ajuste de diagramas. |
| `fix/<tema>` | Corrección de errores o inconsistencias. |

### Mensajes de commit

Se utiliza una convención basada en *Conventional Commits*:

```text
docs:     agregar o modificar documentación
feat:     incorporar un nuevo artefacto o funcionalidad
fix:      corregir un error o una inconsistencia
refactor: reorganizar contenido sin cambiar su significado
diagram:  crear o actualizar un diagrama
```

Ejemplo:

```bash
git commit -m "docs: agregar README del proyecto Vitalis"
```

Todo cambio sobre `main` se integra mediante **Pull Request** con revisión de al menos un integrante. El procedimiento completo está en [`RECURSOS.md`](RECURSOS.md).

---

<sub>Escuela Superior de Comercio N° 49 "Justo José de Urquiza" — Desarrollo Web / Analista Funcional de Sistemas — 2026.</sub>
