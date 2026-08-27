# Definition of Ready (DoR)

Sistema de Gestión Integral — Vitalis Centro de Entrenamiento
Equipo: Grupo 02
Versión: 1.1

---

## Índice

- [1. Qué es la Definition of Ready](#1-qué-es-la-definition-of-ready)
- [2. Por qué la necesitamos en este proyecto](#2-por-qué-la-necesitamos-en-este-proyecto)
- [3. Alcance de aplicación](#3-alcance-de-aplicación)
- [4. Criterios de la DoR](#4-criterios-de-la-dor)
- [5. Checklist operativo](#5-checklist-operativo)
- [6. Cómo se evalúa una historia](#6-cómo-se-evalúa-una-historia)
- [7. Aplicación práctica: evaluación de historias propias](#7-aplicación-práctica-evaluación-de-historias-propias)
- [8. Autoevaluación del equipo](#8-autoevaluación-del-equipo)
- [9. Autoevaluación individual](#9-autoevaluación-individual)
- [10. Anti-patrones detectados](#10-anti-patrones-detectados)
- [11. Relación con la Definition of Done](#11-relación-con-la-definition-of-done)
- [12. Acuerdos sobre esta DoR](#12-acuerdos-sobre-esta-dor)

---

## 1. Qué es la Definition of Ready

La **Definition of Ready** es el acuerdo del equipo sobre las condiciones que debe cumplir una historia de usuario **antes** de entrar a desarrollo.

No es un trámite burocrático: es un filtro. Si una historia entra al sprint sin estar lista, lo que pasa en la práctica es que el desarrollo se frena a mitad de camino para preguntarle algo al cliente, y esa historia queda a medias al cierre de la iteración.

La DoR se aplica en el **refinamiento del backlog**, antes de la planificación del sprint. Es el "portero" que decide qué puede entrar.

```text
   Idea / pedido        Historia escrita      Historia refinada       En desarrollo
        │                      │                      │                     │
        ▼                      ▼                      ▼                     ▼
   ┌─────────┐          ┌─────────────┐        ┌─────────────┐       ┌───────────┐
   │ Backlog │ ───────► │  Redacción  │ ─────► │ Refinamiento│ ────► │  Sprint   │
   │  bruto  │          │  + criterios│        │             │       │           │
   └─────────┘          └─────────────┘        └──────┬──────┘       └───────────┘
                                                      │                     ▲
                                                      │      ┌──────────────┘
                                                      ▼      │
                                               ┌─────────────┴──┐
                                               │  ¿Cumple DoR?  │
                                               │  Sí → entra    │
                                               │  No → vuelve   │
                                               └────────────────┘
```

---

## 2. Por qué la necesitamos en este proyecto

Durante el relevamiento en Vitalis nos encontramos con situaciones concretas que justifican tener una DoR explícita:

| Situación real detectada | Qué pasaría sin DoR | Qué exige nuestra DoR |
|---|---|---|
| El umbral de morosidad "son como 30 días, más o menos" | Se programa un valor fijo y a los dos meses la dirección lo quiere cambiar. | Todo valor de negocio debe estar **cuantificado** y definido como fijo o parametrizable (C4). |
| Las modalidades de cobro son tres y se combinan entre sí | Se desarrolla el alta de pago pensando solo en la cuota mensual y no contempla el resto. | Toda historia que toque pagos debe indicar **cuál de las tres modalidades** contempla (C5). |
| El módulo nutricional maneja datos de salud | Se implementa igual que el resto y queda accesible para recepción. | Toda historia que toque datos sensibles debe declarar **quién accede y quién no** (C7). |
| Hay dos versiones de grilla conviviendo en las planillas | Se desarrolla la planificación sin versionado y se pierde el histórico. | Toda historia de planificación debe aclarar su relación con la **grilla activa** (C6). |
| La recepcionista es quien más va a usar el sistema | Se diseña una pantalla que técnicamente funciona pero le suma pasos respecto de Excel. | Toda historia operativa debe indicar el **rol que la ejecuta** y respetar los límites de usabilidad definidos (C3, C9). |

En síntesis: el mayor riesgo de este proyecto no es técnico, es de **ambigüedad en las reglas de negocio**. La DoR está construida para atacar eso.

---

## 3. Alcance de aplicación

| Se aplica a | No se aplica a |
|---|---|
| Historias de usuario del backlog. | Tareas técnicas internas del equipo (configurar el repositorio, armar el entorno). |
| Slices verticales derivados de una épica. | Corrección de errores de tipeo en la documentación. |
| Historias que vuelven al backlog tras ser rechazadas. | Spikes de investigación (tienen su propio criterio: caja de tiempo y pregunta a responder). |

**Quién evalúa:** los cuatro integrantes, en la reunión de refinamiento. La decisión se toma por consenso. Si hay desacuerdo sobre si una historia está lista, **no está lista**.

---

## 4. Criterios de la DoR

Los criterios están agrupados en cuatro bloques. Una historia debe cumplir **todos** los criterios obligatorios de los cuatro bloques.

### Bloque A — Comprensión del negocio

| ID | Criterio | Obligatorio |
|---|---|---|
| **C1** | La historia está escrita en formato `Como <rol>, quiero <funcionalidad>, para <beneficio>`, y el beneficio es un valor real para el negocio de Vitalis, no una repetición de la funcionalidad. | Sí |
| **C2** | La historia tiene un **ID único** (`HU-nn`) y está vinculada a **al menos un requisito funcional** (`RFnn`) documentado en `docs/requisitos.md`. | Sí |
| **C3** | Está identificado el **rol que ejecuta** la funcionalidad (Administrador, Recepcionista, Instructor, Nutricionista o Alumno) y se verificó que ese rol tenga el permiso correspondiente en la matriz de roles. | Sí |
| **C4** | Todo **valor de negocio** que aparezca en la historia está cuantificado y se aclara si es fijo o parametrizable. No se aceptan expresiones como "bastante tiempo", "muchos alumnos" o "un monto razonable". | Sí |
| **C5** | Si la historia toca el **módulo de cuotas**, indica explícitamente qué modalidades de cobro contempla (mensual fija, por clase, combinada) y qué hace con las que no. | Cuando aplica |
| **C6** | Si la historia toca la **planificación de actividades**, aclara su comportamiento respecto de la grilla activa y de las versiones anteriores. | Cuando aplica |
| **C7** | Si la historia toca **datos sensibles** (módulo nutricional), declara qué roles acceden, qué roles quedan explícitamente excluidos y qué se registra en el log de auditoría. | Cuando aplica |

### Bloque B — Especificación funcional

| ID | Criterio | Obligatorio |
|---|---|---|
| **C8** | La historia tiene **criterios de aceptación** escritos en lenguaje natural, en forma de lista, verificables uno por uno. Mínimo tres. | Sí |
| **C9** | Los criterios de aceptación contemplan **al menos un camino alternativo o de error** (qué pasa si el dato ya existe, si el alumno está de baja, si no hay resultados). | Sí |
| **C10** | Están definidos los **campos de entrada** con sus validaciones: cuáles son obligatorios, qué formato tienen y qué mensaje se muestra si fallan. | Sí |
| **C11** | Está definida la **información que el sistema devuelve** al usuario: qué se muestra en pantalla y qué confirmación recibe. | Sí |
| **C12** | Existe un **boceto o descripción de la pantalla** asociada en `docs/diseño-ui.md`, aunque sea preliminar. | Sí |

### Bloque C — Consistencia con el resto del análisis

| ID | Criterio | Obligatorio |
|---|---|---|
| **C13** | Las **entidades y atributos** que la historia necesita existen en el modelo ER, o el cambio necesario está identificado y acordado. | Sí |
| **C14** | Si la historia se corresponde con un **caso de uso**, la referencia cruzada está registrada en ambos documentos. | Cuando aplica |
| **C15** | La historia **no contradice** ninguna decisión registrada en la tabla de decisiones de `integrantes.md`. Si la contradice, la decisión se revisó y se actualizó primero. | Sí |
| **C16** | Las **dependencias** con otras historias están identificadas. Si depende de una historia que todavía no se desarrolló, se indica cuál y por qué. | Sí |

### Bloque D — Viabilidad para el sprint

| ID | Criterio | Obligatorio |
|---|---|---|
| **C17** | La historia cumple los **seis criterios INVEST** y esa evaluación está documentada. | Sí |
| **C18** | La historia está **estimada** por el equipo y su tamaño **entra en una sola iteración**. Si no entra, se divide antes de aceptarla. | Sí |
| **C19** | Los cuatro integrantes **entendemos la historia** y podemos explicarla con nuestras palabras sin volver a leerla. | Sí |
| **C20** | No quedan **preguntas abiertas** dirigidas al cliente. Si quedan, la historia vuelve al backlog hasta obtener la respuesta. | Sí |

---

## 5. Checklist operativo

Esta es la versión corta que usamos en la reunión de refinamiento. Se copia como comentario en el issue de cada historia.

```markdown
## Checklist DoR — HU-__

### A. Negocio
- [ ] C1  — Formato "Como... quiero... para..." con beneficio real
- [ ] C2  — ID asignado y vinculada a RF documentado
- [ ] C3  — Rol ejecutor identificado y con permiso en la matriz
- [ ] C4  — Valores de negocio cuantificados (sin "más o menos")
- [ ] C5  — Modalidades de cobro contempladas        (N/A si no aplica)
- [ ] C6  — Comportamiento frente a la grilla activa (N/A si no aplica)
- [ ] C7  — Acceso a datos sensibles declarado       (N/A si no aplica)

### B. Especificación
- [ ] C8  — Mínimo 3 criterios de aceptación verificables
- [ ] C9  — Al menos un camino alternativo o de error
- [ ] C10 — Campos, obligatoriedad y validaciones definidos
- [ ] C11 — Salida e información al usuario definida
- [ ] C12 — Pantalla descripta en docs/diseño-ui.md

### C. Consistencia
- [ ] C13 — Entidades y atributos existen en el modelo ER
- [ ] C14 — Referencia cruzada con caso de uso        (N/A si no aplica)
- [ ] C15 — No contradice decisiones registradas
- [ ] C16 — Dependencias identificadas

### D. Viabilidad
- [ ] C17 — INVEST evaluado y documentado
- [ ] C18 — Estimada y entra en una iteración
- [ ] C19 — Los cuatro la entendemos
- [ ] C20 — Sin preguntas abiertas al cliente

**Resultado:** [ ] Ready   [ ] Ready con observaciones   [ ] No Ready
**Evaluada el:** __/__/2026
**Observaciones:**
```

---

## 6. Cómo se evalúa una historia

| Resultado | Condición | Qué pasa |
|---|---|---|
| **Ready** | Cumple los 20 criterios (contando como cumplidos los N/A justificados). | Entra al sprint. |
| **Ready con observaciones** | Cumple todos los obligatorios, pero hay un punto menor a resolver que no bloquea el desarrollo. | Entra al sprint con la observación anotada y un responsable de resolverla. |
| **No Ready** | Falla al menos un criterio obligatorio. | Vuelve al backlog. Se anota qué falta y quién lo resuelve. **No se negocia el ingreso.** |

**Regla acordada:** una historia rechazada dos veces seguidas por el mismo motivo se lleva a la reunión con el docente o con la dirección de Vitalis, según de quién dependa la respuesta. No la volvemos a evaluar hasta tener esa definición.

---

## 7. Aplicación práctica: evaluación de historias propias

Aplicamos la DoR a tres historias que escribimos nosotros mismos. La idea de este ejercicio es probar que la DoR sirve de verdad: si todas las historias pasaran, el filtro no estaría filtrando nada.

### 7.1 — HU-01: Registrar un nuevo alumno

> Como recepcionista, quiero registrar un nuevo alumno con sus datos personales y la actividad elegida, para tener un padrón digital centralizado que evite duplicados y errores.

| ID | Criterio | Estado | Justificación |
|---|---|:---:|---|
| C1 | Formato y beneficio | Cumple | El beneficio ("padrón centralizado que evite duplicados") es un problema real relevado (P6). |
| C2 | ID y vínculo con RF | Cumple | HU-01 → RF01, RF02. |
| C3 | Rol ejecutor | Cumple | Recepcionista. También lo puede hacer el Administrador. |
| C4 | Valores cuantificados | Cumple | No hay valores numéricos de negocio en juego. |
| C5 | Modalidades de cobro | Cumple | Contempla las tres: se selecciona la modalidad en el alta. |
| C6 | Grilla | N/A | N/A. |
| C7 | Datos sensibles | N/A | N/A. Datos personales comunes, no de salud. |
| C8 | Criterios de aceptación | Cumple | Cinco criterios verificables. |
| C9 | Camino alternativo | Cumple | DNI duplicado y campos obligatorios vacíos. |
| C10 | Campos y validaciones | Cumple | Nombre, apellido, DNI, teléfono, email, actividad. DNI único. |
| C11 | Salida al usuario | Cumple | Número de legajo asignado y mensaje de confirmación. |
| C12 | Pantalla | Cumple | "Alta de alumno", documentada en `docs/diseño-ui.md`. |
| C13 | Entidades | Cumple | `Alumno` y `ModalidadCobro` existen en el modelo. |
| C14 | Caso de uso | Cumple | CU-01. |
| C15 | Decisiones | Cumple | No contradice ninguna. |
| C16 | Dependencias | Cumple | Ninguna. Es el punto de entrada del sistema. |
| C17 | INVEST | Cumple | Los seis criterios se cumplen. |
| C18 | Estimación | Cumple | Estimada como historia chica. Entra holgada en una iteración. |
| C19 | Comprensión | Cumple | Los cuatro la explicamos sin releerla. |
| C20 | Preguntas abiertas | Cumple | Ninguna. |

**Resultado: READY**

**Comentario:** es la historia de referencia del proyecto. Cuando dudamos de si otra historia está bien escrita, la comparamos con esta.

---

### 7.2 — HU-03: Consultar alumnos morosos

> Como directora del gimnasio, quiero ver el listado de alumnos con cuota vencida filtrado por actividad, para tomar acciones de cobranza a tiempo y conocer el impacto financiero de la mora.

| ID | Criterio | Estado | Justificación |
|---|---|:---:|---|
| C1 | Formato y beneficio | Cumple | Beneficio claro: acción de cobranza e impacto financiero. |
| C2 | ID y vínculo con RF | Cumple | HU-03 → RF08, RF09. |
| C3 | Rol ejecutor | Cumple | Administrador (directora). |
| C4 | Valores cuantificados | Cumple | 30 días de mora, definido como parámetro configurable (decisión D6). |
| C5 | Modalidades de cobro | Observación | **Observación:** el cálculo de mora para la modalidad "por clase" no está definido. Un alumno que paga por clase y no viene hace 40 días, ¿es moroso o simplemente no asistió? |
| C6 | Grilla | N/A | N/A. |
| C7 | Datos sensibles | N/A | N/A. |
| C8 | Criterios de aceptación | Cumple | Cinco criterios. |
| C9 | Camino alternativo | Cumple | Caso "sin alumnos morosos" contemplado explícitamente. |
| C10 | Campos y validaciones | Cumple | Filtros por actividad y turno. |
| C11 | Salida al usuario | Cumple | Nombre, actividad, días de mora, monto adeudado. Exportable a PDF. |
| C12 | Pantalla | Cumple | "Alumnos morosos", documentada en `docs/diseño-ui.md`. |
| C13 | Entidades | Cumple | `Alumno`, `Cuota` y `ModalidadCobro`. |
| C14 | Caso de uso | Cumple | CU-03. |
| C15 | Decisiones | Cumple | Consistente con D6. |
| C16 | Dependencias | Cumple | Depende de HU-02 (registrar pago): sin pagos cargados no hay mora que calcular. Está declarado. |
| C17 | INVEST | Cumple | Documentado. |
| C18 | Estimación | Cumple | Historia mediana. Entra en una iteración. |
| C19 | Comprensión | Cumple | Sí. |
| C20 | Preguntas abiertas | Observación | La duda de C5 es una pregunta para la dirección de Vitalis. |

**Resultado: READY CON OBSERVACIONES**

**Qué falta:** definir con la dirección cómo se calcula la mora para la modalidad "por clase". Es una regla de negocio, no una decisión técnica.

**Por qué la dejamos pasar igual:** el grueso de los alumnos está en modalidad mensual, así que la historia se puede desarrollar contemplando ese caso y dejando el otro parametrizado. La observación queda asignada y con fecha límite: si no hay respuesta antes del cierre del refinamiento siguiente, la historia se divide.

---

### 7.3 — HU-05: Registrar consulta nutricional

> Como nutricionista, quiero registrar los datos de la consulta de cada paciente (peso, medidas, observaciones) y consultar su evolución histórica, para llevar un seguimiento digital sin usar registros físicos.

| ID | Criterio | Estado | Justificación |
|---|---|:---:|---|
| C1 | Formato y beneficio | Cumple | Beneficio claro. |
| C2 | ID y vínculo con RF | Cumple | HU-05 → RF19, RF20, RF21. |
| C3 | Rol ejecutor | Cumple | Nutricionista. |
| C4 | Valores cuantificados | No cumple | **No cumple.** "Medidas" no está definido. ¿Cuáles? ¿Cintura, cadera, brazo? ¿Todas obligatorias? ¿En qué unidad? Sin esto no se puede diseñar el formulario. |
| C5 | Modalidades de cobro | N/A | N/A. |
| C6 | Grilla | N/A | N/A. |
| C7 | Datos sensibles | Observación | Declara que recepcionistas e instructores no acceden, pero **no define** si el alumno-paciente puede ver su propio historial, ni qué pasa con el consentimiento para registrar datos de salud de un menor. |
| C8 | Criterios de aceptación | Cumple | Cinco criterios. |
| C9 | Camino alternativo | Cumple | Acceso denegado para otros roles. |
| C10 | Campos y validaciones | No cumple | **No cumple.** Derivado de C4: no se pueden definir validaciones de campos que no están definidos. |
| C11 | Salida al usuario | Cumple | Historial completo del paciente. |
| C12 | Pantalla | Cumple | "Módulo nutricional", documentada en `docs/diseño-ui.md`. |
| C13 | Entidades | Observación | `SeguimientoNutricional` tiene `medidas : varchar`, que es un campo genérico. Si las medidas se definen como campos separados, el modelo cambia. |
| C14 | Caso de uso | No cumple | **No cumple.** No hay un caso de uso desarrollado para el módulo nutricional. |
| C15 | Decisiones | Cumple | No contradice ninguna. |
| C16 | Dependencias | Cumple | Depende del módulo de usuarios y roles. Declarado. |
| C17 | INVEST | Observación | La propia evaluación INVEST marca "Pequeña: Parcial" — la historia agrupa registrar y consultar, que son dos cosas distintas. |
| C18 | Estimación | No cumple | **No cumple.** No se puede estimar sin saber cuántos campos tiene el formulario. |
| C19 | Comprensión | Cumple | La entendemos, pero entendemos también que está incompleta. |
| C20 | Preguntas abiertas | No cumple | **No cumple.** Quedan tres preguntas abiertas para la nutricionista. |

**Resultado: NO READY**

**Qué hay que hacer antes de volver a evaluarla:**

| # | Acción | Depende de |
|---|---|---|
| 1 | Entrevistar a la nutricionista para definir exactamente qué parámetros registra en cada consulta. | Vitalis |
| 2 | Definir si el alumno accede a su propio historial nutricional. | Vitalis / equipo |
| 3 | Definir el tratamiento del consentimiento para menores de edad. | Vitalis |
| 4 | Ajustar `SeguimientoNutricional` en el modelo ER según lo que surja del punto 1. | Equipo |
| 5 | Desarrollar el caso de uso correspondiente. | Equipo |
| 6 | **Dividir la historia** en HU-05a (registrar consulta) y HU-05b (consultar historial). | Equipo |

**Comentario del equipo:** esta historia es el mejor ejemplo de por qué escribimos la DoR. Leída rápido parece completa —tiene formato correcto, criterios de aceptación y evaluación INVEST— pero al pasarla por el checklist aparecen seis cosas sin resolver, cuatro de ellas bloqueantes. Si hubiera entrado a un sprint, se habría frenado el primer día.

**Actualización posterior.** Las seis acciones correctivas se completaron. Los parámetros de la consulta quedaron definidos en la regla RN26 (decisión D11), lo que resolvió C4, C10, C13 y habilitó la estimación exigida por C18. El acceso del alumno a su historial se definió en RN28 (D13) y el consentimiento de menores en RN27 (D14), cerrando C7 y C20. La historia se dividió en HU-05a y HU-05b, con lo que C17 pasa a cumplirse plenamente. Ambas historias fueron reevaluadas con resultado **READY** e ingresaron al plan de entrega. El registro original de esta evaluación se conserva sin modificar, porque documenta el estado real de la historia en el momento del refinamiento.

---

### 7.4 — Resumen del ejercicio

| Historia | Resultado | Criterios incumplidos |
|---|---|---|
| HU-01 — Registrar alumno | Ready | 0 |
| HU-03 — Consultar morosos | Ready con observaciones | 0 bloqueantes, 2 observaciones |
| HU-05 — Registrar consulta nutricional | No Ready | 5 bloqueantes, 3 observaciones |

**Tasa de aprobación: 1 de 3 sin observaciones.** Es un resultado esperable para un backlog en etapa temprana y confirma que el filtro funciona.

**Estado tras las acciones correctivas:**

| Historia | Resultado inicial | Acciones aplicadas | Resultado tras la corrección |
|---|---|---|---|
| HU-01 | Ready | Ninguna | Ready |
| HU-03 | Ready con observaciones | Definición de la mora en modalidad por clase (RN25, decisión D12). | Ready |
| HU-05 | No Ready | División en HU-05a y HU-05b. Definición de parámetros (RN26), acceso del administrador (RN29), acceso del alumno (RN28) y consentimiento de menores (RN27). | Ready, en sus dos historias derivadas |

Las cinco preguntas abiertas que la DoR detectó se resolvieron antes de que alguna de las historias afectadas entrara a una iteración. Ninguna llegó a frenar el desarrollo, que es exactamente el resultado que el filtro busca producir.

---

## 8. Autoevaluación del equipo

Evaluación colectiva realizada al cierre de la primera iteración documental.

### 8.1 — Qué hicimos bien

| Aspecto | Evidencia |
|---|---|
| Relevamiento apoyado en fuentes reales | Trabajamos sobre las dos planillas provistas por Vitalis, no sobre supuestos. De ahí salieron las problemáticas P1 a P6. |
| Trazabilidad desde el inicio | Cada historia declara sus requisitos desde la primera versión. No tuvimos que reconstruir las relaciones después. |
| Requisitos no funcionales cuantificados | RNF01 dice "3 segundos", no "rápido". RNF08 dice "4 pasos", no "pocos pasos". |
| Honestidad en la evaluación INVEST | En HU-05 pusimos "Pequeña: Parcial" en lugar de forzar un "Sí". Esa honestidad es la que después detectó el problema. |
| Reparto equitativo del trabajo | Los cuatro redactamos entre 3 y 4 documentos y revisamos una cantidad equivalente. |

### 8.2 — Qué nos costó

| Dificultad | Cómo la enfrentamos |
|---|---|
| **Confundir historia de usuario con caso de uso.** Al principio escribíamos historias con flujos paso a paso. | Acordamos que la historia responde *qué y para qué*, y el caso de uso *cómo*. Reescribimos las cinco historias. |
| **Requisitos con dos cosas adentro.** Varios borradores decían "el sistema debe registrar el pago y actualizar el estado de cuenta". | Regla del equipo: si un requisito tiene un "y", se revisa si son dos requisitos. |
| **Modelar sin decidir.** Discutimos el modelo ER antes de tener resueltas las reglas de negocio de cobro. | Invertimos el orden: primero requisitos y reglas, después modelo. |
| **Numeración inconsistente.** Convivían `RF01` y `RF-01`. | Decisión D1: formato único sin guion, aplicado a todos los documentos. |
| **Suponer en vez de preguntar.** Completamos huecos con lo que nos parecía razonable sin marcarlos. | Ahora todo supuesto se registra explícitamente en `docs/requisitos.md`. |

### 8.3 — Qué vamos a cambiar

| Compromiso | Cómo lo medimos |
|---|---|
| Ninguna historia entra al sprint sin pasar el checklist completo. | El checklist queda como comentario en el issue de cada historia. |
| El refinamiento se hace con los cuatro presentes. | Si falta alguien, se reprograma. |
| Toda pregunta al cliente se anota con responsable y fecha. | Lista de preguntas abiertas en el issue correspondiente. |
| Antes de cerrar una entrega, revisamos la matriz de trazabilidad completa. | Tarea fija en la checklist de cierre de iteración. |
| Cuando una historia se divide, las partes conservan la trazabilidad al RF original. | Se verifica en la revisión del PR. |

### 8.4 — Cómo nos calificamos

| Dimensión | Nivel | Fundamento |
|---|---|---|
| Comprensión del negocio del cliente | Alto | Identificamos seis problemáticas concretas y sus implicancias en el diseño. |
| Calidad de la especificación de requisitos | Alto | 21 RF y 13 RNF, todos verificables y priorizados. |
| Cobertura del backlog | Medio | Cinco historias para 21 requisitos: falta ampliar. |
| Consistencia entre documentos | Medio-Alto | Detectamos y resolvimos seis inconsistencias, pero las encontramos tarde. |
| Aplicación de prácticas ágiles | Medio | Sabemos aplicar INVEST y DoR, pero es la primera vez que lo hacemos en un proyecto propio. |
| Uso de Git y trabajo colaborativo | Medio | El flujo de ramas y PR funciona, aunque al principio hubo commits directos a `main`. |

---

## 9. Autoevaluación individual

Cada integrante evalúa su propio aporte, qué le costó y qué se compromete a mejorar.

### Duran, Berenice

| | |
|---|---|
| **Qué aporté** | Redacté `docs/requisitos.md`, `docs/stakeholders.md`, `RECURSOS.md` y el cuestionario de relevamiento. Fui quien insistió en cuantificar los requisitos no funcionales en lugar de dejarlos como adjetivos. |
| **Qué aprendí** | Que un requisito no funcional sin número no sirve para nada. "El sistema debe ser rápido" no se puede verificar ni discutir con el cliente; "debe responder en menos de 3 segundos" sí. |
| **Qué me costó** | Separar el requisito de la solución. En los primeros borradores escribía cosas como "el sistema debe tener un botón para exportar", que ya está diciendo cómo se implementa. |
| **Mi compromiso** | Revisar cada requisito que escriba preguntándome: ¿esto se puede verificar con una prueba concreta? |

### Gómez, Felipe

| | |
|---|---|
| **Qué aporté** | Redacté `docs/er-modelo.md`, los dos diagramas PlantUML y esta Definition of Ready. Propuse separar `ModalidadCobro` como entidad y versionar la grilla con el atributo `activa`. |
| **Qué aprendí** | Que el modelo de datos no se diseña primero. Cuando intenté modelar antes de tener las reglas de cobro definidas, tuve que rehacer la entidad `Cuota` dos veces. |
| **Qué me costó** | Las cardinalidades entre `Inscripción` y `Asistencia`. Recién cuando leí la historia del "asistente ocasional" me di cuenta de que el modelo que había hecho no la soportaba (de ahí salió la decisión D7). |
| **Mi compromiso** | Antes de tocar el modelo, releer las historias que dependen de las entidades involucradas. |

### Rodriguez, Lautaro

| | |
|---|---|
| **Qué aporté** | Redacté `docs/casos-de-uso.md`, `docs/historias-de-usuario.md`, `slicing.md` e `integrantes.md`. Detecté las inconsistencias de numeración entre los requisitos y las historias. |
| **Qué aprendí** | Que los flujos de excepción son la parte más valiosa de un caso de uso. El flujo normal lo escribe cualquiera; lo difícil es pensar qué pasa cuando algo sale mal. En CU-03 apareció un requisito que no estaba en ningún RF: que el sistema avise explícitamente cuando no hay resultados. |
| **Qué me costó** | No convertir cada historia en un caso de uso. Al principio escribía las historias con demasiado detalle procedimental y perdían el foco en el valor. |
| **Mi compromiso** | Mantener la historia en el "qué y para qué" y dejar el "cómo" para el caso de uso. |

### Verduna, Valentino

| | |
|---|---|
| **Qué aporté** | Redacté el `README.md`, `docs/diseño-ui.md` y los wireframes. Armé la estructura del repositorio y las convenciones de commits y ramas. |
| **Qué aprendí** | Que el diseño de interfaz es donde se descubren los agujeros del análisis. Al intentar dibujar la pantalla de registro de pago me di cuenta de que no estaba definido qué monto sugiere el sistema cuando el alumno tiene modalidad combinada. |
| **Qué me costó** | Documentar pantallas sin adelantarme a decisiones visuales. Tuve que reescribir varias fichas que hablaban de colores y disposición en lugar de función. |
| **Mi compromiso** | Documentar cada pantalla en términos de objetivo, acciones y validaciones, y dejar el diseño gráfico para la etapa de implementación. |

### Síntesis común

Los cuatro coincidimos en el mismo aprendizaje: **el problema de este proyecto no era técnico**. Ninguna de las funcionalidades de Vitalis es difícil de programar. Lo difícil fue descubrir que "la cuota" no es una sola cosa, que la grilla cambia por temporada y que los datos de la nutricionista no pueden estar en el mismo lugar que los de recepción. Todo eso salió de leer con atención dos planillas de Excel y de preguntar.

---

## 10. Anti-patrones detectados

Situaciones que nos pasaron a nosotros y que la DoR está diseñada para evitar.

| Anti-patrón | Cómo se ve | Criterio que lo bloquea |
|---|---|---|
| **Historia camuflada** | Parece completa porque tiene formato correcto, pero cuando la mirás en detalle no se puede estimar. (Caso HU-05.) | C18, C20 |
| **Requisito con "y"** | "El sistema debe registrar el pago y actualizar el estado de cuenta." Son dos requisitos. | C2 |
| **Valor no cuantificado** | "Más de 30 días, más o menos." | C4 |
| **Rol implícito** | La historia no dice quién la ejecuta y se asume que es la recepcionista. | C3 |
| **Épica disfrazada** | "Gestionar alumnos" presentado como una sola historia. | C18 |
| **Criterio de aceptación vago** | "El sistema debe funcionar correctamente." | C8 |
| **Historia sin camino de error** | Solo se describe el caso feliz. | C9 |
| **Contradicción silenciosa** | Una historia asume un umbral de mora distinto al registrado en las decisiones. | C15 |

---

## 11. Relación con la Definition of Done

La DoR y la DoD son dos puertas distintas del mismo proceso.

```text
       ┌──── DoR ────┐                         ┌──── DoD ────┐
       │             │                         │             │
Backlog│    filtro   │  Sprint / desarrollo    │   filtro    │  Entregado
───────┼────────────►├────────────────────────►┼────────────►
       │  ¿está      │                         │  ¿está      │
       │  lista para │                         │  realmente  │
       │  empezar?   │                         │  terminada? │
       └─────────────┘                         └─────────────┘
```

| | Definition of Ready | Definition of Done |
|---|---|---|
| Cuándo se aplica | Antes de empezar. | Antes de dar por terminado. |
| Qué pregunta | ¿Está lo suficientemente clara para empezar? | ¿Está lo suficientemente completa para entregar? |
| Quién la usa | El equipo en el refinamiento. | El equipo en la revisión del sprint. |
| Ejemplo en este proyecto | La historia tiene criterios de aceptación verificables. | El documento está aprobado en un PR y las referencias cruzadas están actualizadas. |

Nuestra Definition of Done para esta etapa documental es:

- [ ] El documento está completo según lo que pide la consigna.
- [ ] Las referencias a IDs de requisitos, historias y casos de uso existen y son correctas.
- [ ] Los enlaces internos funcionan.
- [ ] Las tablas se ven bien renderizadas en GitHub.
- [ ] El documento fue revisado por un integrante que no lo escribió.
- [ ] El PR fue aprobado y mergeado a `main`.
- [ ] Si el cambio afectó a otro documento, ese documento se actualizó en el mismo PR.
- [ ] La tabla de decisiones de `integrantes.md` está al día.

---

## 12. Acuerdos sobre esta DoR

| # | Acuerdo |
|---|---|
| 1 | La DoR se revisa al cierre de cada iteración, en la retrospectiva. |
| 2 | Cualquier integrante puede proponer agregar, modificar o eliminar un criterio. Se decide por consenso de los cuatro. |
| 3 | Un criterio que nunca detectó nada en tres iteraciones se elimina: si no filtra, sobra. |
| 4 | La DoR no se relaja por presión de fecha de entrega. Si no llegamos, entregamos menos historias, no historias peores. |
| 5 | El resultado de cada evaluación queda registrado en el issue de la historia, incluso cuando es "No Ready". |
| 6 | Los criterios marcados "Cuando aplica" requieren justificar el N/A. No alcanza con dejarlo en blanco. |

### Historial de versiones

| Versión | Fecha | Cambio |
|---|---|---|
| 1.0 | 05/2026 | Versión inicial. 20 criterios en cuatro bloques, aplicados a HU-01, HU-03 y HU-05. |
| 1.1 | 05/2026 | Registro del cierre de las acciones correctivas de HU-03 y HU-05. Las evaluaciones originales se conservan sin modificar. |

---

<sub>Escuela Superior de Comercio N° 49 "Justo José de Urquiza" — Desarrollo Web / Analista Funcional de Sistemas — 2026.</sub>
