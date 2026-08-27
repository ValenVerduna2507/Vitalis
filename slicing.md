# Épicas y slicing de historias de usuario

Sistema de Gestión Integral — Vitalis Centro de Entrenamiento
Equipo: Grupo 02
Versión: 1.1

---

## Índice

- [1. Conceptos que usamos](#1-conceptos-que-usamos)
- [2. Por qué el corte vertical importa en este proyecto](#2-por-qué-el-corte-vertical-importa-en-este-proyecto)
- [3. Mapa de épicas del sistema](#3-mapa-de-épicas-del-sistema)
- [4. Épica E1 — Gestionar alumnos](#4-épica-e1--gestionar-alumnos)
- [5. Épica E2 — Gestionar cuotas y pagos](#5-épica-e2--gestionar-cuotas-y-pagos)
- [6. Épica E3 — Planificar actividades](#6-épica-e3--planificar-actividades)
- [7. Slicing del resto de las épicas](#7-slicing-del-resto-de-las-épicas)
- [8. Patrones de división aplicados](#8-patrones-de-división-aplicados)
- [9. Orden de entrega propuesto](#9-orden-de-entrega-propuesto)
- [10. Criterios de tamaño y anti-patrones](#10-criterios-de-tamaño-y-anti-patrones)
- [11. Trazabilidad épica → historia → requisito](#11-trazabilidad-épica--historia--requisito)

---

## 1. Conceptos que usamos

### 1.1 — La jerarquía

```text
                    ÉPICA
        Funcionalidad grande de negocio.
        No entra en una iteración.
        Ejemplo: "Gestionar alumnos"
                      │
                      │  slicing (nivel 1)
                      ▼
              HISTORIA DE USUARIO
        Unidad de valor para un rol concreto.
        Entra en una iteración.
        Ejemplo: HU-01 "Registrar un nuevo alumno"
                      │
                      │  slicing (nivel 2, solo si sigue siendo grande)
                      ▼
                SLICE VERTICAL
        Recorte entregable que atraviesa toda la aplicación.
        Ejemplo: S2.1 "Registrar pago de cuota mensual fija"
```

| Nivel | Qué es | Tamaño esperado | Se entrega |
|---|---|---|---|
| Épica | Un proceso de negocio completo. | Varias iteraciones. | No directamente. |
| Historia de usuario | Un objetivo concreto de un rol. | Una iteración. | Sí. |
| Slice vertical | Un recorte de una historia que sigue siendo usable de punta a punta. | Días. | Sí. |

Un slice **no** es una tarea técnica. "Crear la tabla `alumno`" no es un slice: es un paso interno. Un slice tiene que dejar algo que la recepcionista pueda abrir y usar.

### 1.2 — Vertical contra horizontal

Un slice **vertical** atraviesa todas las capas de la aplicación para una porción acotada de funcionalidad. Un slice **horizontal** hace una capa completa para toda la funcionalidad.

```text
        CORTE HORIZONTAL (incorrecto)         CORTE VERTICAL (correcto)

    ┌───────────────────────────────┐     ┌─────┬─────┬─────┬─────┬─────┐
    │  Interfaz de todo el sistema  │  4  │ In  │ In  │ In  │ In  │ In  │
    ├───────────────────────────────┤     │ ter │ ter │ ter │ ter │ ter │
    │  Lógica de todo el sistema    │  3  │ ─── │ ─── │ ─── │ ─── │ ─── │
    ├───────────────────────────────┤     │ Ló  │ Ló  │ Ló  │ Ló  │ Ló  │
    │  Acceso a datos               │  2  │ gi  │ gi  │ gi  │ gi  │ gi  │
    ├───────────────────────────────┤     │ ─── │ ─── │ ─── │ ─── │ ─── │
    │  Base de datos completa       │  1  │ BD  │ BD  │ BD  │ BD  │ BD  │
    └───────────────────────────────┘     └─────┴─────┴─────┴─────┴─────┘
                                             S1    S2    S3    S4    S5
    Recién en la etapa 4 el usuario         Cada slice se puede usar
    puede tocar algo.                        apenas termina.
```

La diferencia práctica: con corte horizontal, si el proyecto se corta a mitad de camino no hay nada usable. Con corte vertical, hay cinco funcionalidades funcionando y el resto sin empezar.

---

## 2. Por qué el corte vertical importa en este proyecto

En Vitalis esto no es una preferencia metodológica: es una restricción del negocio.

**El centro no puede dejar de operar mientras se construye el sistema.** Las cuotas se cobran todos los meses, los alumnos se anotan todas las semanas y las clases se dictan todos los días. La migración desde Excel no puede ser un salto de todo o nada: tiene que ser gradual, y para eso cada entrega parcial tiene que servir para algo por sí sola.

### 2.1 — El error que estuvimos por cometer

En la primera planificación del backlog habíamos ordenado el trabajo así:

| Iteración | Contenido planificado |
|---|---|
| 1 | Diseñar y crear todas las tablas de la base de datos. |
| 2 | Programar toda la capa de acceso a datos. |
| 3 | Programar todas las pantallas. |
| 4 | Integrar y probar. |

Es un corte horizontal puro. El problema salió al preguntarnos qué le mostrábamos a la directora de Vitalis al terminar la iteración 1. La respuesta era: un diagrama de base de datos. Nada que pueda usar, nada sobre lo que pueda opinar, y ninguna forma de detectar que entendimos mal una regla de negocio hasta la iteración 4.

Lo reordenamos en vertical:

| Iteración | Contenido | Qué puede hacer Vitalis al terminar |
|---|---|---|
| 1 | Login + alta de alumno + listado. | Cargar el padrón real y dejar de usar la planilla de alumnos. |
| 2 | Registro de pago mensual + estado de cuenta. | Cobrar la cuota del mes en el sistema. |
| 3 | Detección de mora + reporte. | Dejar de revisar la planilla fila por fila. |

Al final de la iteración 1 la recepcionista ya está cargando alumnos de verdad. Si algo del alta está mal pensado, nos enteramos en la semana 2 y no en el mes 3.

### 2.2 — La segunda razón: las reglas de cobro

El módulo de cuotas tiene tres modalidades (mensual fija, por clase, combinada) y cada una calcula distinto la deuda y la mora. Si intentáramos hacer "el módulo de pagos" en un solo bloque, tendríamos que resolver las tres reglas antes de mostrar nada.

Cortado en vertical, podemos entregar primero la modalidad mensual —que es la que tiene la mayoría de los alumnos— y validar con la recepcionista que el flujo de cobro funciona, antes de meternos con las otras dos. Esto es exactamente lo que pasó con HU-03: la Definition of Ready dejó abierta la regla de mora para la modalidad "por clase" y esa duda tardó varias semanas en resolverse. Con corte vertical no bloqueó nada; con corte horizontal habría frenado el módulo entero hasta que el cliente respondiera.

---

## 3. Mapa de épicas del sistema

Las épicas coinciden con los módulos definidos en el README, de manera que la trazabilidad se mantiene entre documentos.

| ID | Épica | Módulo | Historias | Prioridad de negocio |
|---|---|---|---|---|
| **E1** | Gestionar alumnos | M1 | HU-01, HU-06, HU-07, HU-08, HU-09 | Muy alta |
| **E2** | Gestionar cuotas y pagos | M2 | HU-02, HU-03, HU-10, HU-11 | Muy alta |
| **E3** | Planificar actividades | M3 | HU-12, HU-13, HU-14, HU-15 | Alta |
| **E4** | Controlar asistencia | M4 | HU-04, HU-16, HU-17 | Media |
| **E5** | Seguimiento nutricional | M5 | HU-05a, HU-05b | Media |
| **E6** | Gestionar instructores | M6 | HU-18, HU-19 | Media |
| **E7** | Seguridad, usuarios y roles | M7 | HU-20, HU-21 | Muy alta |
| **E8** | Reportes de gestión | M8 | HU-22, HU-23 | Baja |

Formato de las épicas: *"Como <área del negocio>, necesitamos <capacidad>, para <resultado de negocio>."*

> **Nota de consistencia:** las historias HU-06 a HU-23 se enuncian en este documento y se desarrollan en detalle en `docs/historias-de-usuario.md`. Los requisitos RF22 a RF31 son los requisitos adicionales que el equipo incorporó para cubrir los módulos M6, M7 y M8, y se formalizan en `docs/requisitos.md`.

---

## 4. Épica E1 — Gestionar alumnos

> **E1** — Como centro de entrenamiento, necesitamos administrar el padrón de alumnos de forma digital y centralizada, para eliminar la planilla de Excel y dejar de perder historial cuando un alumno se da de baja.

| Campo | Detalle |
|---|---|
| Módulo | M1 — Gestión de alumnos |
| Requisitos que cubre | RF01, RF02, RF03, RF04, RF05, RF24 |
| Roles involucrados | Recepcionista, Administrador |
| Entidades | `Alumno`, `ModalidadCobro` |
| Por qué es una épica | Abarca todo el ciclo de vida del alumno: entra, cambia sus datos, se va, vuelve. Son cinco operaciones distintas con reglas propias. No entra en una iteración. |

### 4.1 — Slicing nivel 1: por operación del ciclo de vida

El eje de corte elegido es el **ciclo de vida del alumno**. Cada operación es una historia independiente porque tiene un disparador distinto, un momento distinto y, en algunos casos, un rol distinto.

| ID | Historia | Rol | RF | Valor propio |
|---|---|---|---|---|
| **HU-01** | Registrar un nuevo alumno | Recepcionista | RF01, RF02 | Sin esto no hay padrón. Es la puerta de entrada de todo el sistema. |
| **HU-09** | Buscar y listar alumnos | Recepcionista | RF24 | Convierte los datos cargados en información consultable. Sin búsqueda, un padrón de 200 alumnos es inservible. |
| **HU-06** | Modificar los datos de un alumno | Recepcionista | RF03 | La gente cambia de teléfono y de actividad. Sin esto, el padrón se desactualiza en semanas. |
| **HU-07** | Dar de baja a un alumno | Recepcionista | RF04 | Resuelve el problema P2: el alumno sale del padrón activo pero su historial se conserva. |
| **HU-08** | Reactivar un alumno dado de baja | Recepcionista | RF05 | Vitalis tiene alta rotación estacional. Reactivar en dos clics evita recargar todos los datos. |

### 4.2 — Por qué cada historia es independiente

| Historia | Se puede desarrollar sola porque... | Se puede entregar sola porque... |
|---|---|---|
| HU-01 | No depende de ninguna otra funcionalidad. Solo necesita la tabla `Alumno`. | La recepcionista puede empezar a cargar el padrón real ese mismo día. |
| HU-09 | Consulta la tabla que HU-01 ya llena. No modifica nada. | Permite verificar si alguien está anotado sin abrir Excel. |
| HU-06 | Reutiliza el mismo formulario de HU-01 en modo edición. | Corrige errores de carga sin esperar al resto del módulo. |
| HU-07 | Solo cambia un campo de estado y agrega fecha y motivo. | La directora deja de ver bajas mezcladas con activos en el listado. |
| HU-08 | Es la operación inversa de HU-07 sobre el mismo campo. | Un alumno que vuelve en septiembre se reincorpora sin volver a cargar sus datos. |

**Dependencia real, y la única:** HU-06, HU-07 y HU-08 necesitan que exista al menos un alumno cargado, es decir, HU-01. Pero eso es una dependencia de *datos*, no de *código*: se pueden desarrollar en paralelo y probar con registros de prueba.

### 4.3 — Slicing nivel 2: cuando una historia todavía es grande

Al estimar HU-01 vimos que el formulario completo (datos personales + contacto + actividad + modalidad de cobro + legajo automático) era más grande de lo que queríamos para una primera entrega. La dividimos en tres slices verticales.

| ID | Slice | Qué incluye | Qué deja usable |
|---|---|---|---|
| **S1.1** | Alta mínima | Formulario con apellido, nombre y DNI. Guardado en base. Listado simple. | La recepcionista ya puede cargar el padrón completo con los datos esenciales. Es el reemplazo directo de las tres primeras columnas de la planilla. |
| **S1.2** | Validación de DNI único | Verificación antes de confirmar, mensaje de error y bloqueo del alta. | Resuelve el problema P6 (duplicados). El padrón deja de ensuciarse. |
| **S1.3** | Datos completos y modalidad | Teléfono, email, actividad, modalidad de cobro y asignación automática de legajo. | La ficha del alumno queda completa y habilita el módulo de cuotas. |

Cada uno de estos tres slices toca interfaz, lógica y base de datos. Ninguno es "la pantalla" o "la base": los tres son la aplicación entera, más angosta.

**Por qué este orden:** S1.1 primero porque permite empezar a migrar datos desde el día uno. S1.2 segundo porque cuanto antes esté, menos duplicados hay que limpiar después. S1.3 último porque la modalidad de cobro recién hace falta cuando llega el módulo de pagos.

**Un detalle que descubrimos al dividir:** si S1.1 se entrega sin S1.2, durante ese período se pueden cargar DNI repetidos. Acordamos que la migración de datos reales arranca recién con S1.2 entregado; hasta entonces se usa con datos de prueba.

---

## 5. Épica E2 — Gestionar cuotas y pagos

> **E2** — Como centro de entrenamiento, necesitamos registrar los pagos y detectar automáticamente a los alumnos con cuota vencida, para dejar de revisar la planilla fila por fila y poder reclamar la deuda a tiempo.

| Campo | Detalle |
|---|---|
| Módulo | M2 — Gestión de cuotas y pagos |
| Requisitos que cubre | RF06, RF07, RF08, RF09, RF10, RF30 |
| Roles involucrados | Recepcionista, Administrador |
| Entidades | `Cuota`, `ModalidadCobro`, `Alumno` |
| Por qué es una épica | Tres modalidades de cobro con reglas distintas, más la lógica de mora y la exportación de reportes. Es la parte más compleja del sistema en reglas de negocio. |

### 5.1 — Slicing nivel 1: por función

| ID | Historia | Rol | RF | Valor propio |
|---|---|---|---|---|
| **HU-11** | Gestionar las modalidades de cobro | Administrador | RF07, RF30 | La directora actualiza precios sin depender del equipo de desarrollo. |
| **HU-02** | Registrar el pago de una cuota | Recepcionista | RF06, RF07 | Es la operación más frecuente del centro. Reemplaza la planilla de cuotas. |
| **HU-10** | Consultar el historial de pagos de un alumno | Recepcionista | RF10 | Responde la pregunta más habitual del mostrador: "¿yo pagué el mes pasado?". |
| **HU-03** | Consultar alumnos morosos | Administrador | RF08, RF09 | Convierte los pagos registrados en una acción de cobranza concreta. |

### 5.2 — Slicing nivel 2 de HU-02: por modalidad de cobro

Este es el corte más específico de Vitalis y el que mejor muestra por qué el slicing vertical sirve. HU-02 no se divide por pantalla ni por capa: se divide **por regla de negocio**.

| ID | Slice | Regla que implementa | Alumnos cubiertos | Valor entregado |
|---|---|---|---|---|
| **S2.1** | Pago con modalidad mensual fija | Monto fijo mensual. Vencimiento a los 30 días del último pago. | La mayoría del padrón. | Cubre el grueso del cobro. Con este slice solo, la recepcionista ya puede cobrar la cuota de casi todos los alumnos. |
| **S2.2** | Pago con modalidad por clase | Monto por clase asistida. No hay período mensual. | Alumnos ocasionales. | Permite cobrar a quien viene salteado, algo que hoy se anota a mano en un cuaderno. |
| **S2.3** | Pago con modalidad combinada | Tarifa combinada según las actividades del alumno (por ejemplo, Pilates dos días más Entrenamiento tres días). | Alumnos con más de una disciplina. | Cierra el caso más complejo y elimina el último cálculo manual. |

**Por qué este orden y no otro:** el criterio fue cobertura de alumnos. S2.1 cubre a la mayoría del padrón con la regla más simple. Si el proyecto se frenara después de S2.1, Vitalis igual tendría el cobro digitalizado para casi todos sus alumnos, y solo un puñado seguiría en planilla.

**Cómo se relaciona con la Definition of Ready:** la evaluación de HU-03 dejó abierta la pregunta de cómo se calcula la mora para la modalidad "por clase". Mientras estuvo sin responder, esa duda bloqueó S2.2 pero no S2.1 ni el reporte de morosos para alumnos mensuales. Sin este corte, una única duda del cliente habría frenado el módulo entero. La cuestión se resolvió con la decisión D12: la modalidad por clase no genera morosidad porque el pago es anticipado, y estos alumnos se clasifican como inactivos tras 60 días sin pagos ni asistencias (regla RN25). S2.2 queda desbloqueado.

### 5.3 — Slicing nivel 2 de HU-03: por profundidad de la funcionalidad

| ID | Slice | Qué incluye | Valor entregado |
|---|---|---|---|
| **S3.1** | Listado de morosos en pantalla | Consulta con el umbral de 30 días. Muestra nombre, actividad, días de mora y monto adeudado. | La directora ve la mora del mes sin abrir Excel. Es el 80 por ciento del valor de la historia. |
| **S3.2** | Filtros por actividad y turno | Filtrado del listado ya existente. | Permite reclamar por disciplina, que es como la directora organiza la cobranza. |
| **S3.3** | Exportación a PDF | Generación y descarga del listado filtrado. | Permite imprimir el listado para trabajarlo fuera del sistema. |
| **S3.4** | Umbral de mora configurable | Parámetro editable por el Administrador, con 30 días por defecto. | Cumple la decisión D6: la dirección puede cambiar el criterio sin tocar código. |

**Observación del equipo:** S3.1 es lo que resuelve el problema real. S3.3 es lo que casi ponemos primero porque el requisito RF09 menciona explícitamente la exportación en PDF. Al dividir nos dimos cuenta de que exportar un listado que todavía no se puede ver en pantalla no tiene sentido. El orden del requisito no es necesariamente el orden de entrega.

---

## 6. Épica E3 — Planificar actividades

> **E3** — Como centro de entrenamiento, necesitamos administrar las disciplinas, los horarios y la asignación de instructores con soporte de múltiples versiones de grilla, para poder reorganizar la temporada sin perder la planificación anterior.

| Campo | Detalle |
|---|---|
| Módulo | M3 — Planificación de actividades |
| Requisitos que cubre | RF11, RF12, RF13, RF14 |
| Roles involucrados | Administrador, Recepcionista |
| Entidades | `Disciplina`, `Clase`, `Grilla`, `Instructor`, `Inscripcion` |

### 6.1 — Slicing nivel 1

| ID | Historia | Rol | RF | Valor propio |
|---|---|---|---|---|
| **HU-12** | Gestionar disciplinas | Administrador | RF11 | Define el catálogo de actividades. Es el prerrequisito de todo lo demás. |
| **HU-13** | Crear una clase y asignar instructor | Administrador | RF12 | Arma la grilla horaria concreta. |
| **HU-15** | Inscribir un alumno a una clase | Recepcionista | RF14 | Vincula el padrón con la grilla. Habilita el control de asistencia. |
| **HU-14** | Gestionar versiones de grilla | Administrador | RF13 | Resuelve el problema P3: convivencia de grilla regular y grilla de temporada sin pisar la anterior. |

### 6.2 — Slicing nivel 2 de HU-14: el caso del versionado

HU-14 es la historia más riesgosa de esta épica, porque el versionado de grilla afecta a todas las clases ya cargadas. La dividimos así:

| ID | Slice | Qué incluye | Valor entregado |
|---|---|---|---|
| **S4.1** | Grilla única activa | Todas las clases pertenecen a una grilla implícita. Se puede ver y editar. | La planificación deja de estar en Excel. Cubre la operación normal del centro. |
| **S4.2** | Crear una grilla nueva copiando la activa | Duplicación de la grilla vigente como punto de partida de la próxima temporada. | La directora arma la grilla de la temporada siguiente sin cargar todo de cero ni perder la actual. |
| **S4.3** | Activar una grilla y consultar las anteriores | Cambio de grilla activa, con las anteriores en modo lectura. | Cierra el problema P3 por completo: se puede volver a mirar cómo estaba organizada la temporada pasada. |

**Por qué S4.1 primero:** el 90 por ciento del tiempo Vitalis opera con una sola grilla. El versionado hace falta dos o tres veces al año. Entregar primero el caso frecuente y después el excepcional es lo que permite que el sistema sea útil desde temprano.

**Riesgo identificado:** si S4.1 se implementa sin prever que después va a haber varias grillas, S4.2 obliga a rehacer la estructura de datos. Por eso la entidad `Grilla` existe en el modelo ER desde el principio, aunque en S4.1 haya un solo registro. Dividir en slices no significa ignorar lo que viene: significa no construirlo todavía.

---

## 7. Slicing del resto de las épicas

### E4 — Controlar asistencia

| ID | Historia | Rol | RF | Valor propio |
|---|---|---|---|---|
| **HU-04** | Registrar la asistencia de una clase | Instructor | RF15, RF16, RF17 | Reemplaza la lista en papel. El instructor lo hace desde la tablet en el salón. |
| **HU-17** | Agregar un asistente ocasional | Instructor | RF27 | Contempla al alumno que aparece en una clase donde no está inscripto (decisión D7). |
| **HU-16** | Consultar el historial de asistencia de un alumno | Recepcionista, Administrador | RF18 | Permite detectar ausencias reiteradas y anticipar bajas. |

**Slices de HU-04:**

| ID | Slice | Valor entregado |
|---|---|---|
| **S5.1** | Ver el listado de inscriptos de la clase del día | El instructor deja de imprimir la lista. Solo lectura, sin marcar nada. |
| **S5.2** | Marcar Presente y Ausente | Registro digital de asistencia con los dos estados más usados. |
| **S5.3** | Agregar el estado Justificado y el sello de fecha y hora | Completa el requisito RF16 y RF17. |

### E5 — Seguimiento nutricional

| ID | Historia | Rol | RF | Valor propio |
|---|---|---|---|---|
| **HU-05a** | Registrar una consulta nutricional | Nutricionista | RF19, RF21 | Elimina el registro físico de la consulta. |
| **HU-05b** | Consultar la evolución de un paciente | Nutricionista | RF20, RF21 | Permite ver el progreso entre consultas, que es el objetivo del seguimiento. |

**Estado:** desbloqueada. La Definition of Ready marcó HU-05 como *No Ready* por cinco criterios incumplidos. Las cuatro causas de fondo quedaron resueltas: los parámetros de la consulta están definidos en la regla RN26 (decisión D11), el acceso del Administrador al módulo está delimitado por RN29 (decisión D10), el acceso del alumno a su propio historial está definido por RN28 (decisión D13) y el consentimiento de menores por RN27 (decisión D14). La división en HU-05a y HU-05b se mantiene como acción correctiva del criterio de tamaño. La épica se planifica en la iteración 9.

### E6 — Gestionar instructores

| ID | Historia | Rol | RF | Valor propio |
|---|---|---|---|---|
| **HU-18** | Registrar y dar de baja instructores | Administrador | RF25 | Habilita la asignación de instructores a clases de HU-13. |
| **HU-19** | Consultar la agenda propia | Instructor | RF26 | El instructor ve sus clases sin preguntar en recepción. |

### E7 — Seguridad, usuarios y roles

| ID | Historia | Rol | RF | Valor propio |
|---|---|---|---|---|
| **HU-20** | Iniciar sesión en el sistema | Todos | RF22 | Sin esto no hay control de acceso posible. Es transversal a todo el sistema. |
| **HU-21** | Gestionar usuarios y asignar roles | Administrador | RF23 | Permite dar de alta a cada empleado con sus permisos. |

**Slices de HU-20:**

| ID | Slice | Valor entregado |
|---|---|---|
| **S6.1** | Login con usuario y contraseña | Acceso controlado al sistema, con un único perfil. |
| **S6.2** | Diferenciación de menú según rol | Cada rol ve solo las opciones que le corresponden. Cumple RNF03. |
| **S6.3** | Restricción efectiva de acceso por rol | Bloqueo real de las funciones no permitidas, incluso por URL directa. Cumple RNF06 para el módulo nutricional. |

**Nota importante sobre el orden:** S6.1 es prerrequisito de toda la primera iteración, porque sin login no se puede identificar quién carga cada dato. Pero S6.2 y S6.3 pueden esperar: durante las primeras semanas el sistema puede operar con un solo perfil administrativo mientras se migra el padrón.

### E8 — Reportes de gestión

| ID | Historia | Rol | RF | Valor propio |
|---|---|---|---|---|
| **HU-22** | Consultar el reporte de ingresos por período | Administrador | RF28 | Da a la dirección la proyección de ingresos que hoy no existe. |
| **HU-23** | Consultar la ocupación de las clases | Administrador | RF29 | Permite decidir qué horarios conviene abrir o cerrar. |

Esta épica es la de menor prioridad porque depende de que haya datos cargados: un reporte de ingresos sin pagos registrados no muestra nada.

---

## 8. Patrones de división aplicados

Estos son los criterios que usamos para cortar, con el caso concreto de Vitalis en el que se aplicó cada uno.

| Patrón | En qué consiste | Dónde lo aplicamos |
|---|---|---|
| **Por operación CRUD** | Separar alta, consulta, modificación y baja. | E1: HU-01, HU-09, HU-06, HU-07, HU-08. |
| **Por regla de negocio** | Una historia por variante de la regla. | HU-02: S2.1 mensual, S2.2 por clase, S2.3 combinada. |
| **Por camino feliz y excepciones** | Primero el flujo principal, después los casos borde. | HU-04: S5.2 Presente/Ausente antes que S5.3 Justificado. |
| **Por profundidad de funcionalidad** | Versión simple usable primero, refinamientos después. | HU-03: S3.1 listado antes que S3.2 filtros y S3.3 exportación. |
| **Por caso frecuente contra caso excepcional** | Lo que pasa todos los días antes de lo que pasa dos veces al año. | HU-14: S4.1 grilla única antes que S4.2 y S4.3 versionado. |
| **Por rol de usuario** | Separar la funcionalidad según quién la usa. | E7: S6.1 acceso general, S6.2 y S6.3 diferenciación por rol. |
| **Por operación manual contra automática** | Primero que el usuario lo cargue, después que el sistema lo calcule. | HU-11 permite cargar la modalidad antes de que HU-02 la aplique automáticamente. |

### Patrones que descartamos

| Patrón | Por qué no lo usamos |
|---|---|
| Por capa técnica | Es corte horizontal. No entrega valor hasta el final. |
| Por pantalla | Una pantalla no es una unidad de valor. La ficha del alumno sirve a varias historias distintas. |
| Por entidad de la base de datos | Lleva a construir tablas que nadie usa todavía. |

---

## 9. Orden de entrega propuesto

El orden combina tres criterios: qué habilita otra cosa, cuánto valor entrega y qué tan frecuente es la operación.

| Iteración | Slices e historias | Estado al terminar |
|---|---|---|
| **1** | S6.1 (login), S1.1 (alta mínima), S1.2 (DNI único), HU-09 (búsqueda) | Vitalis puede migrar el padrón real desde la planilla y consultarlo. La planilla de alumnos deja de usarse. |
| **2** | S1.3 (datos completos y modalidad), HU-11 (modalidades de cobro), S2.1 (pago mensual fijo) | Se cobra la cuota mensual en el sistema. La mayoría del padrón queda cubierta. |
| **3** | HU-10 (historial de pagos), S3.1 (listado de morosos), S3.4 (umbral configurable) | La dirección ve la mora en pantalla sin revisar Excel. |
| **4** | S2.3 (modalidad combinada), S3.2 (filtros), S3.3 (exportación PDF), HU-06, HU-07, HU-08 | Módulo de cuotas completo salvo modalidad por clase. Ciclo de vida del alumno cerrado. |
| **5** | HU-12 (disciplinas), HU-18 (instructores), HU-13 (clases), S4.1 (grilla única) | La planificación sale de Excel. |
| **6** | HU-15 (inscripción), S5.1 y S5.2 (asistencia), HU-19 (agenda del instructor) | Los instructores toman asistencia digital desde el salón. |
| **7** | S6.2 y S6.3 (roles), HU-21 (usuarios), S5.3, HU-17, HU-16 | Control de acceso completo. Asistencia completa. |
| **8** | S4.2 y S4.3 (versionado de grilla), HU-22, HU-23 | Versionado de temporada y reportes de gestión. |
| **9** | S2.2 (modalidad por clase), HU-05a, HU-05b | Módulo de cuotas completo con las tres modalidades. Consultorio nutricional digitalizado. |

### Por qué el login va primero

Es la única excepción a la regla de "primero lo que más valor entrega". El login por sí solo no le sirve a nadie: no permite hacer nada nuevo. Va primero por una razón práctica: sin identificar al usuario no se puede registrar quién cargó cada dato, y agregarlo después obliga a revisar todas las operaciones ya construidas. Es una dependencia técnica genuina, no un corte horizontal disfrazado.

### Por qué la iteración 9 va al final

S2.2 y las historias del módulo nutricional estuvieron fuera del plan durante toda la primera planificación, porque dependían de definiciones que el cliente todavía no había dado. Ese es justamente el comportamiento que buscábamos: el corte vertical permitió seguir entregando valor durante todo ese tiempo sin que las dudas pendientes frenaran el resto del sistema.

Con los puntos abiertos A1 a A5 ya resueltos (decisiones D10 a D14), las tres unidades ingresan al plan. Se ubican al final por prioridad de negocio y no por bloqueo: la modalidad por clase alcanza a una minoría del padrón y el consultorio nutricional atiende un día por semana, de modo que ninguna de las dos compite en urgencia con el cobro de la cuota mensual o el control de asistencia.

El resultado del ejercicio quedó registrado: de las cinco preguntas abiertas que la Definition of Ready detectó, ninguna llegó a frenar una iteración en curso.

---

## 10. Criterios de tamaño y anti-patrones

### 10.1 — Cuándo hay que dividir

Dividimos una historia si se cumple alguna de estas condiciones:

| Señal | Ejemplo del proyecto |
|---|---|
| El equipo no se pone de acuerdo en la estimación. | HU-05 no se pudo estimar porque no estaban definidos los campos. |
| La historia tiene un "y" en el título. | "Registrar consulta **y** consultar historial" fueron dos historias distintas. |
| Contempla más de una regla de negocio para el mismo objetivo. | HU-02 tenía tres modalidades de cobro adentro. |
| Los criterios de aceptación pasan de siete u ocho. | Es señal de que hay más de un objetivo mezclado. |
| No entra en una iteración. | HU-14 con versionado completo. |
| Una parte está bloqueada por una definición del cliente y el resto no. | HU-02: la modalidad por clase bloqueada, las otras dos no. |

### 10.2 — Cuándo no hay que dividir

| Señal | Motivo |
|---|---|
| El slice resultante no sirve para nada por sí solo. | "Crear la tabla de cuotas" no es un slice. |
| La división genera más trabajo de integración que el que ahorra. | Separar el formulario del guardado obliga a hacer dos veces lo mismo. |
| La historia ya entra holgada en una iteración. | HU-07 (baja lógica) es un cambio de estado. Dividirla sería absurdo. |

### 10.3 — Anti-patrones de slicing que detectamos

| Anti-patrón | Cómo se ve | Caso en el que casi caemos |
|---|---|---|
| **Slice de capa** | "Hacer la base de datos de pagos". | Era la iteración 1 de nuestra primera planificación. |
| **Slice sin usuario** | Un recorte que no deja nada que alguien pueda abrir. | "Implementar el cálculo de mora" sin pantalla donde verlo. |
| **Slice por pantalla** | "Hacer la ficha del alumno". | La ficha sirve a HU-01, HU-06, HU-10 y HU-16 a la vez. No es una unidad de valor. |
| **Slice guiado por el orden del requisito** | Implementar en el orden en que están numerados los RF. | RF09 menciona la exportación PDF antes de que exista el listado en pantalla. |
| **División infinita** | Cortar hasta que los slices son tareas de una hora. | Se pierde de vista el valor y se multiplica la coordinación. |

---

## 11. Trazabilidad épica → historia → requisito

| Épica | Historia | Slices | Requisitos | Caso de uso |
|---|---|---|---|---|
| E1 | HU-01 | S1.1, S1.2, S1.3 | RF01, RF02 | CU-01 |
| E1 | HU-06 | — | RF03 | CU-04 |
| E1 | HU-07 | — | RF04 | CU-05 |
| E1 | HU-08 | — | RF05 | CU-06 |
| E1 | HU-09 | — | RF24 | — |
| E2 | HU-02 | S2.1, S2.2, S2.3 | RF06, RF07 | CU-02 |
| E2 | HU-03 | S3.1, S3.2, S3.3, S3.4 | RF08, RF09, RF30 | CU-03 |
| E2 | HU-10 | — | RF10 | — |
| E2 | HU-11 | — | RF07 | CU-07 |
| E3 | HU-12 | — | RF11 | CU-08 |
| E3 | HU-13 | — | RF12 | CU-09 |
| E3 | HU-14 | S4.1, S4.2, S4.3 | RF13 | CU-10 |
| E3 | HU-15 | — | RF14 | CU-11 |
| E4 | HU-04 | S5.1, S5.2, S5.3 | RF15, RF16, RF17 | CU-12 |
| E4 | HU-16 | — | RF18 | — |
| E4 | HU-17 | — | RF27 | — |
| E5 | HU-05a | — | RF19, RF21 | CU-13 |
| E5 | HU-05b | — | RF20, RF21 | CU-14 |
| E6 | HU-18 | — | RF25 | CU-15 |
| E6 | HU-19 | — | RF26 | — |
| E7 | HU-20 | S6.1, S6.2, S6.3 | RF22 | CU-00 |
| E7 | HU-21 | — | RF23 | CU-16 |
| E8 | HU-22 | — | RF28 | — |
| E8 | HU-23 | — | RF29 | — |

**Cobertura:** las 24 historias cubren los 21 requisitos funcionales originales (RF01 a RF21) más los 10 adicionales (RF22 a RF31). Ningún requisito queda sin al menos una historia asociada, y ninguna historia existe sin un requisito que la justifique.

> Los identificadores de casos de uso listados en la última columna son los que se desarrollan en `docs/casos-de-uso.md`. Las historias sin caso de uso asociado son consultas simples que no justifican un desarrollo completo de flujos.

---

## Historial de versiones

| Versión | Fecha | Cambio |
|---|---|---|
| 1.0 | 05/2026 | Versión inicial. Ocho épicas, veinticuatro historias y veinte slices verticales. |
| 1.1 | 05/2026 | Incorporación de la iteración 9 tras la resolución de los puntos abiertos A1 a A5 (decisiones D10 a D14). S2.2, HU-05a y HU-05b pasan de sin planificar a planificadas. |

---

<sub>Escuela Superior de Comercio N° 49 "Justo José de Urquiza" — Desarrollo Web / Analista Funcional de Sistemas — 2026.</sub>
