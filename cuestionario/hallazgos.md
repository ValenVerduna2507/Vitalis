# Hallazgos del relevamiento

Sistema de Gestión Integral — Vitalis Centro de Entrenamiento
Equipo: Grupo 02
Versión: 1.0

Este documento cierra el ciclo del relevamiento: toma lo que se preguntó en [`cuestionario-relevamiento.md`](cuestionario-relevamiento.md), lo que se respondió en [`respuestas-relevamiento.md`](respuestas-relevamiento.md), y muestra en qué se convirtió cada dato dentro del análisis.

---

## Índice

- [1. Cómo leer este documento](#1-cómo-leer-este-documento)
- [2. Hallazgos y su derivación](#2-hallazgos-y-su-derivación)
- [3. De los hallazgos a los problemas](#3-de-los-hallazgos-a-los-problemas)
- [4. Frases que definieron decisiones](#4-frases-que-definieron-decisiones)
- [5. Contradicciones entre fuentes](#5-contradicciones-entre-fuentes)
- [6. Preguntas que quedaron sin respuesta](#6-preguntas-que-quedaron-sin-respuesta)
- [7. Lo que el relevamiento descartó](#7-lo-que-el-relevamiento-descartó)
- [8. Evaluación del relevamiento](#8-evaluación-del-relevamiento)

---

## 1. Cómo leer este documento

Cada hallazgo tiene un identificador `Hnn` y se rastrea hasta el artefacto donde terminó: un requisito funcional, una regla de negocio, una decisión del equipo o una restricción.

La regla que el equipo se impuso fue: **ningún requisito puede existir sin un hallazgo que lo respalde**. Si al escribir un requisito no se podía señalar de dónde salía, el requisito se marcaba como supuesto y se llevaba a la segunda entrevista.

| Fuente | Código |
|---|---|
| Entrevista | E1 a E6 |
| Encuesta a alumnos | A |
| Análisis documental | P |
| Observación directa | O |

Dentro de cada entrevista, las preguntas se numeran con una letra que identifica el bloque —`G` para el bloque general, `D` para la entrevista a la dirección, `R` para recepción, `I` para instructores y `N` para la nutricionista— seguida del número de pregunta. Una referencia como `E1 D22` se lee entonces como "entrevista 1, pregunta D22".

> Esa `D` identifica preguntas del relevamiento y **no tiene relación con las decisiones del equipo `D1` a `D15`** registradas en [`integrantes.md`](../integrantes.md), que se numeran en un espacio propio.

---

## 2. Hallazgos y su derivación

### 2.1 — Sobre el padrón de alumnos

| # | Hallazgo | Fuente | Derivó en |
|---|---|---|---|
| H01 | El DNI está vacío en 41 de 292 registros y hay 3 pares con el mismo DNI. | P3, P4 | RF02, RN01 |
| H02 | La recepcionista deja el DNI para después "y después me olvida". | E2 R3 | RF01 con DNI obligatorio, P04 |
| H03 | 78 de 292 registros están dados de baja. El 27 por ciento del padrón. | P1 | Problema P2, RF04 |
| H04 | La baja se escribe a mano en observaciones, con tres variantes de escritura. | P5 | RF04 con campo de estado propio, RN02 |
| H05 | No se registra el motivo de la baja. | E2 R22 | CU-05 con motivo obligatorio |
| H06 | El 41 por ciento de los alumnos encuestados se fue y volvió. | A2 | RF05, RN04 |
| H07 | Cuando alguien vuelve, se carga como registro nuevo porque buscar el anterior "es más trabajo". | E2 R24 | RF05, HU-08, CU-06 |
| H08 | La directora quiere conservar los registros de baja: "si vuelve, quiero saber que ya estuvo". | E6 C2 | RN02, baja lógica en todo el modelo |
| H09 | Buscar a un alumno es lo que más tiempo le lleva a la recepcionista. Un cobro tarda entre 40 segundos y 3 minutos, y la variable es encontrar la fila. | E2 G4, O2 | RF24, HU-09, P03 |
| H10 | La búsqueda falla con acentos: "si me dicen Gómez y está cargado con acento, no me lo encuentra". | E2 R18 | HU-09 criterio 2 |
| H11 | Los datos del tutor de un menor se anotan en la columna de observaciones. | E2 R6 | RN05, entidad `Tutor` |

### 2.2 — Sobre el cobro

| # | Hallazgo | Fuente | Derivó en |
|---|---|---|---|
| H12 | Existen tres modalidades de cobro con reglas de cálculo distintas. | E1 D7 | Problema P1, RF07 |
| H13 | La modalidad combinada figura escrita de cinco formas distintas en la planilla. | P6 | `ModalidadCobro` como entidad, RF07 |
| H14 | La tarifa combinada no es la suma de las individuales: tiene un valor propio. | E1 D8 | RN06, RN07 |
| H15 | Los precios se actualizan varias veces al año y lo decide la directora sola. | E1 D10 | RE04, RF30, HU-11 |
| H16 | "Lo que se pagó, se pagó." Un aumento no modifica los cobros anteriores. | E1 D11 | RN08, decisión D8, doble referencia a `ModalidadCobro` |
| H17 | Solo se conoce la fecha exacta en 61 de 847 pagos. En el resto, apenas el mes. | P8 | RF06 con fecha obligatoria |
| H18 | En la celda del mes se escribe el monto, una tilde o la fecha, sin criterio uniforme. | P7 | `Cuota` como entidad con campos tipados |
| H19 | La directora detecta deudores recorriendo la planilla fila por fila, y le lleva una hora. | E1 D15 | Problema P1, RF08, RF09 |
| H20 | El umbral de mora es difuso: "un mes, un mes y monedas". Ante la repregunta: treinta días. | E1 D13 | RF08, RN09, criterio C4 de la DoR |
| H21 | El plazo no es igual para todos: con alumnos antiguos es más flexible. | E1 D14 | RF30, `ParametroSistema` |
| H22 | "Ponelo en treinta pero dejame cambiarlo." | E6 C8 | RF30, decisión D6 |
| H23 | El alumno que paga por clase no genera deuda: "paga y entrena". | E1 D9, E6 C6 | RN25, decisión D12 |
| H24 | Aun así, la directora quiere detectar al que dejó de venir, "no como deudor, como alguien que se está yendo". | E6 C7 | RN25, HU-23 criterio 7 |
| H25 | Hubo un reclamo de cuota a una alumna que ya había pagado, por un registro en la fila equivocada. | E1 G6 | RNF05, columna "registrado por" en P07 |
| H26 | Un error de carga se corrige sobrescribiendo, sin dejar rastro. | E2 R12, O6 | RNF05, `LogAuditoria` |
| H27 | El 18 por ciento de los alumnos encuestados sufrió una confusión con un pago. | A5 | RF10, P19 |
| H28 | Existe un cuaderno paralelo con los pagos por clase del día, que se pasa a la planilla "cuando puede". | E2 G5, O4 | RF06, RNF08 |
| H29 | Los medios de pago son solo efectivo y transferencia. | E1 D12 | RN11 |
| H30 | El pico de cobros es del 1 al 10 de cada mes, con más de treinta operaciones diarias. | E2 R13, R14 | RNF02, RNF08 |

### 2.3 — Sobre la planificación

| # | Hallazgo | Fuente | Derivó en |
|---|---|---|---|
| H31 | La grilla se reorganiza dos o tres veces al año. | E1 D4 | Problema P3, RF13 |
| H32 | Conviven dos hojas de grilla sin indicación de vigencia. | P11, P12 | RF13, entidad `Grilla` con atributo `activa` |
| H33 | Al reorganizar, la directora hace una copia del archivo y termina con cuatro versiones. | E6 C4 | HU-14, CU-10 |
| H34 | En la hoja de agosto, el mismo instructor figura en dos disciplinas a la misma hora. | P15 | RN14, validación de CU-09 |
| H35 | "Si el sistema me lo frena, mejor." | E6 C13 | CU-09 excepción E1 |
| H36 | Un instructor puede dictar una disciplina que no es su especialidad, en un reemplazo. | E6 C14 | CU-15 flujo A3, especialidades informativas |
| H37 | Los instructores figuran por iniciales en la grilla. | P14 | Entidad `Instructor` con nombre completo |
| H38 | Las disciplinas infantiles se dictan solo en el turno tarde. | E1 D2 | RN15 |
| H39 | No hay ningún registro de inscripciones de alumnos a clases. | P16 | RF14, entidad `Inscripcion` |
| H40 | Ante un cambio de grilla: "avisame quiénes son y los llamo. No los saques vos sin avisar". | E6 C5 | CU-10 excepción E1 |
| H41 | El nombre de las actividades está escrito con y sin acento, en mayúsculas y minúsculas. | P10 | Entidad `Disciplina` |

### 2.4 — Sobre la asistencia

| # | Hallazgo | Fuente | Derivó en |
|---|---|---|---|
| H42 | No se toma lista en ninguna clase. No existe ningún registro de asistencia. | E3, E4 I4, P17 | Problema P4, RF15, RF16 |
| H43 | Los instructores no saben quién está anotado en su clase. | E3, E4 I3 | RF15 |
| H44 | Una o dos personas por clase vienen de otro horario y nadie lo registra. | E3, E4 I8, O11 | RF27, decisión D7 |
| H45 | El único momento disponible del instructor es la entrada en calor, de cinco minutos. | O7 | RNF07, diseño de P13 |
| H46 | Durante el resto de la clase el instructor tiene las manos ocupadas corrigiendo posturas. | O8 | P13 con marcado masivo |
| H47 | "Si son dos toques, sí. Si tengo que ir uno por uno buscando, no." | E3 I9 | P13, botón Marcar todos presentes |
| H48 | El wifi del salón se cortó dos veces durante una hora de clase. | O12, E3 I10 | RE05, P13 excepción de conexión, confirmación única |
| H49 | Hay una repisa donde apoyar un dispositivo. | O9 | Viabilidad del uso de tablet |
| H50 | Los instructores se enteran de los cambios de horario por WhatsApp, a veces el mismo día. | E3, E4 I2 | RF26, HU-19 |
| H51 | "De plata no queremos saber nada." | E3, E4 I12 | Matriz de permisos: el instructor no accede a pagos |

### 2.5 — Sobre el módulo nutricional

| # | Hallazgo | Fuente | Derivó en |
|---|---|---|---|
| H52 | Los registros son fichas de cartulina en una caja. Se perdieron dos. | E5 N8, N9 | Problema P5, RF19 |
| H53 | Los parámetros registrados son: fecha, peso, altura, perímetro de cintura, perímetro de cadera, masa grasa, objetivo y observaciones. | E5 N4 | RN26, decisión D11 |
| H54 | Obligatorios: fecha, peso y objetivo. La altura solo en la primera consulta. El resto, opcional. | E5 N5 | RN26, HU-05a criterio 4 |
| H55 | El índice de masa corporal se calcula a mano con la calculadora del celular. | E5 N7 | IMC como dato derivado, HU-05a criterio 5 |
| H56 | La comparación entre consultas se hace "a ojo", mirando la ficha anterior. | E5 N10 | RF20, HU-05b criterio 4 |
| H57 | "Eso es secreto profesional, no es un dato del gimnasio." | E5 N15 | RF21, RNF06, RN20 |
| H58 | La nutricionista aceptó que la directora administre el módulo sin ver el contenido. | E5 N16 | RN29, decisión D10 |
| H59 | "Yo necesito poder darle el usuario y sacar copia de seguridad, nada más." | E6 C10 | RN29, decisión D10 |
| H60 | Atiende menores y la autorización de los padres es verbal, sin registro. | E5 N17, N18, N19 | RN27, decisión D14 |
| H61 | Los resultados se muestran interpretados: "el número solo no dice nada y a veces desanima". | E5 N12 | RN28, decisión D13 |

### 2.6 — Sobre accesos y seguridad

| # | Hallazgo | Fuente | Derivó en |
|---|---|---|---|
| H62 | La planilla la ven la directora y las dos recepcionistas. Nadie más tiene acceso a nada. | E1 D22 | Problema P4, RNF03 |
| H63 | Apareció un pago cargado que nadie recordaba haber hecho. | E1 D26 | RNF05, `LogAuditoria` |
| H64 | El alta y la baja de empleados se resuelve compartiendo el archivo. Nunca se pensó como problema. | E1 D25 | RF23, HU-21 |
| H65 | "Eso es salud, no puede andar dando vueltas." | E1 D23 | RNF06 |
| H66 | Los alumnos consultan todo en recepción: 31 de 34 encuestados. | A4 | Problema P4, RF10, P19 |
| H67 | El 65 por ciento de los alumnos usaría una consulta desde el celular. | A6 | P19, decisión D5 |
| H68 | "Para ver si están al día y los horarios, sí. Para tocar algo, no." | E6 C12 | Decisión D5, rol Alumno de solo consulta |

### 2.7 — Sobre el alcance y las restricciones

| # | Hallazgo | Fuente | Derivó en |
|---|---|---|---|
| H69 | La facturación y la contabilidad las lleva un estudio externo. | E1 D27, D28 | Fuera de alcance |
| H70 | El pago online interesa pero no ahora: "primero ordenemos esto". | E1 D29 | Fuera de alcance |
| H71 | Los avisos masivos no son prioridad. | E1 D30 | Fuera de alcance |
| H72 | "Si Julieta necesita un curso para usarlo, no sirve." | E1 D31 | RE02, RNF09 |
| H73 | "Sigo con la planilla y después cargo todo junto. O no lo cargo." | E2 R28 | RNF08, riesgo R1 de stakeholders |
| H74 | El centro tiene una sola sede, sin planes concretos de abrir otra. | E1 D6 | Supuesto S05 |
| H75 | La planilla no se cierra en todo el turno. | O1 | RNF10, disponibilidad en horario de atención |
| H76 | Una vez se perdió una mañana entera de cobros por un cierre sin guardar. | E2 R27 | Justificación general del sistema |

**Total: 76 hallazgos registrados.**

---

## 3. De los hallazgos a los problemas

Los seis problemas documentados en `docs/requisitos.md` no se inventaron: cada uno agrupa un conjunto de hallazgos.

| Problema | Hallazgos que lo sustentan | Evidencia más fuerte |
|---|---|---|
| **P1** Modalidades de cobro variables | H12, H13, H14, H15, H19, H23 | La directora tarda una hora en detectar deudores recorriendo la planilla. |
| **P2** Alta rotación de alumnos | H03, H06, H07, H08 | 78 de 292 registros dados de baja. El 41 por ciento de los encuestados volvió. |
| **P3** Planificación por temporada | H31, H32, H33, H34 | Dos hojas de grilla conviviendo sin indicación de vigencia. |
| **P4** Ausencia de acceso por rol | H42, H43, H50, H62, H66 | 31 de 34 alumnos responden "pregunto en recepción". |
| **P5** Datos sensibles sin resguardo | H52, H57, H60, H65 | Fichas de cartulina en una caja. Dos se perdieron. |
| **P6** Errores de carga y duplicación | H01, H02, H18, H25, H26, H41 | 41 registros sin DNI y 3 pares duplicados. |

---

## 4. Frases que definieron decisiones

Algunas respuestas textuales tuvieron consecuencias directas sobre el diseño. Se registran porque explican mejor que cualquier resumen por qué el sistema es como es.

| Frase | Quién | Qué definió |
|---|---|---|
| "Sigo con la planilla y después cargo todo junto. O no lo cargo." | Recepcionista | Es el riesgo más grave del proyecto. De ahí sale RNF08 y el principio de que el sistema tiene que ser más rápido que Excel. |
| "Si son dos toques, sí. Si tengo que ir uno por uno buscando, no." | Instructor | Definió el diseño completo de la pantalla de asistencia: botones grandes, marcado masivo, una sola confirmación. |
| "Eso es secreto profesional, no es un dato del gimnasio." | Nutricionista | Fundamenta RNF06 y la separación entre administración y contenido clínico. |
| "Lo que se pagó, se pagó." | Directora | Es el origen de la decisión D8: cada cuota conserva su modalidad y su monto. |
| "Avisame quiénes son y los llamo. No los saques vos sin avisar." | Directora | El sistema no desinscribe en silencio al cambiar de grilla: informa para que recepción actúe. |
| "Ponelo en treinta pero dejame cambiarlo." | Directora | Origen de RF30 y de la entidad `ParametroSistema`. |
| "Ese lo quiero saber igual, pero no como deudor. Como alguien que se está yendo." | Directora | Resolvió el punto abierto A3 y dio origen a la categoría de alumno inactivo. |
| "Si Julieta necesita un curso para usarlo, no sirve." | Directora | Restricción RE02. Condiciona todo el diseño de interfaz. |
| "A veces no me acuerdo si pagué y me da vergüenza preguntar." | Alumno encuestado | Justifica el portal del alumno mejor que cualquier argumento técnico. |

---

## 5. Contradicciones entre fuentes

El relevamiento produjo cuatro contradicciones. Ninguna fue mala fe: son el resultado esperable de contrastar lo que la gente dice con lo que muestran los datos.

| # | Contradicción | Cómo se resolvió |
|---|---|---|
| **X1** | La recepcionista dijo que las bajas "no son tantas". La planilla mostró 78 sobre 292. | Se tomó el dato de la planilla. La percepción de la recepcionista es válida sobre su turno; el archivo cubre todo el historial. |
| **X2** | La directora dijo que el DNI se pide siempre. La planilla lo tiene vacío en 41 registros. La recepcionista admitió que "a veces lo dejo para después". | Se tomó la práctica real. El requisito RF01 lo define obligatorio justamente porque hoy no se cumple. |
| **X3** | La directora dijo que la grilla vigente es la "regular", pero la hoja "GRILLA AGOSTO" tenía modificaciones más recientes. | Se preguntó en la segunda entrevista (E6 C3). Era una prueba del año anterior que quedó sin borrar. Confirma el problema P3. |
| **X4** | Los instructores dijeron que "vienen los que vienen" y que no pasa casi nunca que aparezca alguien de otro horario. En la observación de una sola clase apareció un caso. | Se tomó la observación. El hecho de que no lo registren explica por qué lo perciben como poco frecuente. Dio origen a RF27. |

---

## 6. Preguntas que quedaron sin respuesta

Cinco cuestiones no se resolvieron durante el relevamiento y se registraron como puntos abiertos en `docs/requisitos.md`. Todas se cerraron después, en la segunda entrevista o por decisión del equipo.

| Punto | Pregunta pendiente | Cómo se cerró |
|---|---|---|
| **A1** | Qué parámetros exactos registra la nutricionista y cuáles son obligatorios. | Entrevista E5 (N4, N5, N6). Formalizado en RN26, decisión D11. |
| **A2** | Quién accede al módulo nutricional: contradicción entre RF21 y RNF06. | Entrevistas E5 N16 y E6 C10. Resuelto con RN29, decisión D10. |
| **A3** | Cómo se calcula la mora en la modalidad por clase. | Entrevista E6 C6 y C7. Resuelto con RN25, decisión D12. |
| **A4** | Si el alumno puede ver su historial nutricional. | Entrevistas E5 N12 y E6 C11. Resuelto con RN28, decisión D13. |
| **A5** | Cómo se documenta el consentimiento para menores. | Entrevista E5 N17 a N19. Resuelto con RN27, decisión D14. |

**Observación del equipo.** Los cinco puntos abiertos correspondían al módulo nutricional y a las reglas de cobro. Ninguno surgió del módulo de alumnos ni del de planificación, que fueron los mejor cubiertos por el relevamiento inicial. La causa es que la primera entrevista se concentró en la operación diaria y dejó los temas más específicos para el final, cuando ya quedaba poco tiempo. Es el aprendizaje metodológico principal de esta etapa.

---

## 7. Lo que el relevamiento descartó

Tan importante como lo que se incorporó es lo que se dejó afuera con fundamento.

| Idea descartada | Por qué |
|---|---|
| Módulo de facturación electrónica | La facturación la lleva un estudio contable externo (H69). |
| Pago online desde el sistema | La directora lo quiere a futuro pero no ahora (H70). |
| Comunicaciones masivas a alumnos | No es prioridad para la dirección (H71). |
| Cupo máximo por clase | Nadie lo mencionó en ninguna entrevista. Se registró como observación O2 del modelo por si aparece más adelante. |
| Que el instructor cobre cuotas | Los dos instructores lo rechazaron explícitamente (H51). |
| Control de acceso físico al establecimiento | No surgió como necesidad en ninguna instancia. |
| Sistema multisede | Una sola sede, sin planes concretos (H74). Registrado como supuesto S05. |

---

## 8. Evaluación del relevamiento

### 8.1 — Qué funcionó

| Aspecto | Evidencia |
|---|---|
| Combinar cuatro técnicas | Cada una detectó algo que las otras no. El caso más claro es la contradicción X1: sin la planilla, el problema P2 se hubiera subestimado. |
| Contrastar el dicho con el dato | Las cuatro contradicciones aparecieron por comparar fuentes, no por desconfiar de los entrevistados. |
| La observación directa | Aportó los hallazgos más útiles para el diseño de interfaz: los cinco minutos de entrada en calor, las manos ocupadas, la repisa y el wifi que se corta. |
| La segunda entrevista | Las catorce consultas de E6 cerraron los cinco puntos abiertos. Sin esa instancia, la mitad del módulo nutricional seguiría sin definir. |
| Registrar las frases textuales | Varias decisiones de diseño se explican mejor con la frase original que con un resumen. |

### 8.2 — Qué haríamos distinto

| Problema | Qué haríamos |
|---|---|
| La primera entrevista se quedó sin tiempo para los temas específicos, y de ahí salieron los cinco puntos abiertos. | Dividirla en dos desde el inicio: una sobre la operación y otra sobre reglas de negocio. |
| Entrevistamos a la recepcionista del turno tarde, no a la de la mañana. | Entrevistar a las dos. El turno mañana tiene otro perfil de alumnos y probablemente otra operatoria. |
| La encuesta a alumnos tuvo 34 respuestas sobre 214, un 16 por ciento. | Entregarla en el momento del cobro, cuando el alumno ya está en el mostrador. |
| No entrevistamos a ningún familiar de alumno menor. | Hacerlo. Sus necesidades se dedujeron de lo que dijeron otros, y eso es más débil. |
| El análisis documental se hizo después de la primera entrevista. | Hacerlo antes. Habría permitido llevar las contradicciones a la entrevista en lugar de descubrirlas después. |

### 8.3 — Cobertura

| Control | Resultado |
|---|---|
| Stakeholders entrevistados | 4 de 6 de forma directa. Alumnos por encuesta. Familiares y tutores, no entrevistados. |
| Hallazgos registrados | 76 |
| Hallazgos con derivación a un artefacto | 76 de 76 |
| Problemas del relevamiento sustentados en hallazgos | 6 de 6 |
| Contradicciones detectadas y resueltas | 4 de 4 |
| Puntos abiertos generados | 5 |
| Puntos abiertos cerrados | 5 de 5 |
| Requisitos funcionales sin hallazgo que los respalde | Ninguno |

---

## Historial de versiones

| Versión | Fecha | Cambio |
|---|---|---|
| 1.0 | 04/2026 | Consolidación de 76 hallazgos, su derivación a requisitos y decisiones, las contradicciones detectadas, los puntos abiertos y la evaluación metodológica del relevamiento. |

---

<sub>Escuela Superior de Comercio N° 49 "Justo José de Urquiza" — Desarrollo Web / Analista Funcional de Sistemas — 2026.</sub>
