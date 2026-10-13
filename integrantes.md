# Integrantes del equipo

Sistema de Gestión Integral — Vitalis Centro de Entrenamiento
Escuela Superior de Comercio N° 49 "Justo José de Urquiza" — Rosario, Santa Fe
Materia: Desarrollo Web — Analista Funcional de Sistemas
Ciclo lectivo: 2026

---

## 1. Datos del equipo

| Campo | Detalle |
|---|---|
| Nombre del grupo | Grupo 02 |
| Cantidad de integrantes | 4 |
| Cliente del proyecto | Vitalis Centro de Entrenamiento — Pueblo Esther, Santa Fe |
| Docente a cargo | Pedernera, Pablo |
| Metodología de trabajo | Ágil, con iteraciones cortas y revisión por pares en la reunión de cierre |
| Organización del equipo | Horizontal, sin jefe de grupo. Las decisiones se toman por consenso. |
| Repositorio | `vitalis-sistema-gestion` |
| Rama principal | `main` |

---

## 2. Integrantes

El equipo trabaja de forma **horizontal** (decisión [D9](#6-registro-de-decisiones-del-equipo)): los cuatro integrantes participan de todas las actividades del análisis —relevamiento, requisitos, historias, casos de uso, modelado y diseño— y ninguno concentra la decisión final sobre el proyecto. Lo que sí se reparte es la **redacción de cada artefacto**, para que el trabajo avance en paralelo y cada documento tenga un responsable claro de su escritura.

| Integrante | Artefactos que redacta | Artefactos que revisa |
|---|---|---|
| **Duran, Berenice** | `docs/requisitos.md`<br>`docs/stakeholders.md`<br>`RECURSOS.md`<br>`cuestionario/` | Los artefactos de Gómez |
| **Gómez, Felipe** | `docs/er-modelo.md`<br>`diagramas/er.puml`<br>`diagramas/casos-de-uso.puml`<br>`DoR.md` | Los artefactos de Rodriguez |
| **Rodriguez, Lautaro** | `docs/casos-de-uso.md`<br>`docs/historias-de-usuario.md`<br>`slicing.md`<br>`integrantes.md` | Los artefactos de Verduna |
| **Verduna, Valentino** | `README.md`<br>`docs/diseño-ui.md`<br>`diagramas/wireframes/` | Los artefactos de Duran |

La revisión rota en círculo, de modo que cada integrante revisa el trabajo de otro y es revisado por un tercero. Ningún documento se integra a `main` sin haber sido leído por alguien que no lo escribió.

---

## 3. Reparto de la carga de trabajo

| Actividad | Duran | Gómez | Rodriguez | Verduna |
|---|:---:|:---:|:---:|:---:|
| Relevamiento y entrevistas | Sí | Sí | Sí | Sí |
| Definición de requisitos | Sí | Sí | Sí | Sí |
| Redacción de historias de usuario | Sí | Sí | Sí | Sí |
| Desarrollo de casos de uso | Sí | Sí | Sí | Sí |
| Modelado de datos | Sí | Sí | Sí | Sí |
| Diagramas UML | Sí | Sí | Sí | Sí |
| Diseño de interfaz y wireframes | Sí | Sí | Sí | Sí |
| Control de consistencia | Sí | Sí | Sí | Sí |
| Documentos redactados | 4 | 4 | 4 | 3 |
| Documentos revisados | 4 | 4 | 3 | 4 |

Todas las actividades se realizan de manera conjunta: se discuten en reunión, se acuerda el criterio y recién después el integrante que tiene asignado el documento lo redacta. La carga de redacción quedó repartida en partes iguales (tres o cuatro documentos por integrante), y la de revisión también.

---

## 4. Forma de trabajo

### Ritmo de trabajo

| Instancia | Frecuencia | Objetivo |
|---|---|---|
| Reunión de planificación | Al inicio de cada iteración | Definir qué documentos se trabajan y repartirlos entre los cuatro. |
| Sincronización rápida | Dos veces por semana (mensajería) | Estado de cada tarea, bloqueos y dudas de interpretación. |
| Revisión de entregable | Al cierre de cada iteración | Revisar el trabajo terminado contra la Definition of Ready. |
| Retrospectiva breve | Al cierre de cada iteración | Qué funcionó, qué no y qué se ajusta para la siguiente. |

En cada iteración un integrante distinto **modera** la reunión y lleva el registro de decisiones. El rol rota siguiendo el orden alfabético del apellido, de manera que a lo largo del cuatrimestre los cuatro cumplen esa función la misma cantidad de veces.

### Flujo de trabajo en el repositorio

El equipo trabaja **directamente sobre `main`**. Cada integrante sube sus archivos desde la interfaz web de GitHub, sin ramas intermedias y sin Pull Requests (decisión D17).

**Por qué funciona en este proyecto.** Los artefactos están repartidos: la tabla de la sección 2 asigna a cada integrante sus documentos, de modo que dos personas rara vez editan el mismo archivo el mismo día. Además esto es documentación y no código: no hay compilación que romper ni pruebas que fallen, y una redacción incorrecta se detecta leyendo, no ejecutando.

**Qué controla la calidad, entonces.** No es la revisión del Pull Request, porque no existe. Son tres cosas:

| Control | Cuándo ocurre |
|---|---|
| Reparto por artefacto | Permanente. Cada documento tiene un autor y un revisor asignados en la sección 2. |
| Revisión contra la Definition of Ready | En la reunión de cierre de cada iteración, sobre el trabajo ya subido. |
| Acuerdo previo de las decisiones que cruzan documentos | Antes de escribir. Se toman en reunión y quedan registradas en la sección 6. |

**Qué perdemos, dicho con todas las letras.** Un error llega a `main` sin ningún filtro previo. La revisión existe pero no deja rastro escrito en el repositorio: ocurre en la reunión, no en el historial, así que mirando los commits no se puede saber quién revisó qué. Y si dos integrantes editan el mismo archivo desde la web con pocos minutos de diferencia, el segundo pisa al primero sin aviso.

**Lo que sí sostenemos.** Mensajes de commit descriptivos, que digan qué cambió y no "actualización". Un cambio por commit cuando es posible. Y si un cambio obliga a modificar otro documento, los dos se suben el mismo día, nunca uno hoy y el otro la semana que viene.

> **Sobre la versión anterior de este apartado.** Hasta el 13/10/2026 esta sección describía un flujo de ramas y Pull Requests con revisión cruzada obligatoria. Ese flujo nunca se aplicó: el historial del repositorio muestra commits directos sobre `main` desde el primer día. El docente lo señaló en la devolución del 13/10 y el apartado se reescribió para que describa cómo se trabajó realmente. El flujo con ramas y Pull Requests sigue documentado en [`RECURSOS.md`](RECURSOS.md), ahora como material de referencia y no como el procedimiento vigente del equipo.

### Criterios de calidad acordados

- Todo requisito debe tener un ID único y estable. Los IDs nunca se reutilizan ni se renumeran.
- Toda historia de usuario debe indicar a qué requisitos responde.
- Todo caso de uso debe indicar a qué historia y a qué requisitos corresponde.
- Toda entidad del modelo debe justificarse por al menos un requisito.
- Si un cambio en un documento obliga a modificar otro, los dos se suben el mismo día y el mensaje de commit nombra los dos documentos.
- Antes de cerrar una entrega se revisa la matriz de trazabilidad completa entre los cuatro integrantes.

---

## 5. Canales de comunicación

| Canal | Uso |
|---|---|
| Grupo de mensajería del equipo | Coordinación diaria, dudas rápidas, avisos. Es también donde se anotan las tareas pendientes y las inconsistencias detectadas: el equipo no usa los Issues de GitHub. |
| Historial de commits | Registro de qué cambió y cuándo. El mensaje de cada commit es la única traza escrita del cambio, de modo que se redacta pensando en quien lo lea después. |
| Reuniones presenciales / videollamada | Planificación, revisión y retrospectiva. |
| Consultas al docente | Dudas de metodología, alcance del trabajo y criterios de evaluación. |

---

## 6. Registro de decisiones del equipo

Las decisiones que afectan a más de un documento se toman en reunión, por consenso de los cuatro integrantes, y se registran acá para mantener la consistencia del repositorio.

| # | Fecha | Decisión | Motivo | Impacto |
|---|---|---|---|---|
| D1 | 05/2026 | Los requisitos funcionales se numeran `RF01`…`RFnn` y los no funcionales `RNF01`…`RNFnn`, sin guion intermedio. | En los primeros documentos convivían `RF01` y `RF-01`. La inconsistencia rompía las referencias cruzadas entre artefactos y obligaba a buscar dos veces cada identificador. | Todos los documentos. |
| D2 | 05/2026 | La sede documentada es Pueblo Esther, Santa Fe. | La única referencia a Rosario en el proyecto corresponde a la institución educativa, no al domicilio del centro. Mantener las dos ciudades sin distinguirlas inducía a error sobre dónde opera Vitalis. | `README.md`, `docs/requisitos.md`. |
| D3 | 05/2026 | Pilates se incorpora a la oferta de disciplinas documentada. | La modalidad de cobro combinada relevada la contempla de forma explícita: la disciplina ya existía en la operación aunque no figuraba en el listado inicial. | `README.md`, `docs/requisitos.md`, `docs/er-modelo.md`. |
| D4 | 05/2026 | Se incorporan los módulos M6 (instructores), M7 (seguridad y roles) y M8 (reportes), con requisitos identificados como adicionales. | Los tres estaban dentro del alcance del relevamiento pero no tenían requisitos funcionales propios. Sin ellos, ningún requisito definía cómo se inicia sesión, cómo se crean los usuarios ni cómo se consulta el log que los RNF ya exigían. | `docs/requisitos.md` y derivados. |
| D5 | 05/2026 | El alumno es un actor del sistema con permisos de solo consulta. | 31 de 34 alumnos encuestados respondieron que consultan todo en recepción (hallazgo H66). La consulta propia descarga el mostrador; habilitarle escritura expondría el padrón sin resolver ninguna necesidad relevada. | `docs/casos-de-uso.md`, `docs/diseño-ui.md`. |
| D6 | 05/2026 | El umbral de morosidad es de 30 días y se define como parámetro configurable por la dirección. | La dirección describió el umbral como difuso —"un mes, un mes y monedas"— y recién ante la repregunta lo fijó en 30 días (hallazgo H20). La restricción RE04 advierte además que modifica sus criterios sin aviso al equipo técnico, de modo que el valor no puede quedar escrito en el código. | `docs/requisitos.md`, `docs/casos-de-uso.md`. |
| D7 | 05/2026 | El registro de asistencia admite alumnos no inscriptos en la clase (asistente ocasional). | El criterio de aceptación de HU-04 exige registrar a un alumno activo que se presenta sin estar inscripto. En el modelo preliminar toda asistencia derivaba de una inscripción, de modo que ese caso era imposible de representar. | `docs/er-modelo.md`, `diagramas/er.puml`. |
| D8 | 05/2026 | La modalidad de cobro se guarda tanto en el alumno (vigente) como en cada cuota (aplicada). | Un cambio de precio o de modalidad no debe reescribir la historia de pagos ya cobrados. Guardándola solo en el alumno, el histórico cambiaría de forma retroactiva cada vez que la dirección ajusta una tarifa. | `docs/er-modelo.md`, `diagramas/er.puml`. |
| D9 | 05/2026 | El equipo trabaja de forma horizontal: no hay jefe de grupo y toda decisión de alcance o modelado se toma por consenso. | Los cuatro integrantes participan de todas las actividades del análisis en igual proporción; lo que se reparte es la redacción de cada artefacto, no la decisión. Designar un responsable único no reflejaría cómo trabaja el equipo y concentraría en una sola persona criterios que afectan a todos los documentos. | `integrantes.md`, `DoR.md`. |
| D10 | 05/2026 | El acceso al módulo nutricional se separa en dos planos: el contenido clínico es exclusivo de la nutricionista (RF21) y la administración del módulo corresponde al Administrador, sin ver consultas (RNF06). Resuelve el punto abierto A2. | Leídos de forma literal, RF21 y RNF06 se contradicen: uno reserva el módulo a la nutricionista y el otro se lo concede también al Administrador. Separar el contenido clínico de la administración cumple los dos sin modificar el enunciado de ninguno, y respeta la posición que la profesional planteó durante el relevamiento. | `docs/requisitos.md`, `docs/casos-de-uso.md`, `docs/diseño-ui.md`. |
| D11 | 05/2026 | La consulta nutricional registra peso, altura, perímetro de cintura, perímetro de cadera, porcentaje de masa grasa, objetivo y observaciones, con IMC calculado. Reemplaza el campo genérico `medidas`. Resuelve A1. | El atributo genérico `medidas` como texto libre no permitía validar los valores, compararlos entre consultas ni graficar la evolución, que es la finalidad del seguimiento nutricional. | `docs/requisitos.md`, `docs/er-modelo.md`, `diagramas/er.puml`, `docs/diseño-ui.md`. |
| D12 | 05/2026 | La modalidad de cobro por clase no genera morosidad. Estos alumnos se clasifican como inactivos tras 60 días sin pagos ni asistencias. Resuelve A3. | El pago es anticipado, de modo que no existe deuda posible: aplicarles el umbral de mora los marcaría como deudores sin que deban nada. La inactividad es el indicador que sí aporta información sobre este grupo. | `docs/requisitos.md`, `docs/casos-de-uso.md`. |
| D13 | 05/2026 | El rol Alumno no accede a su historial nutricional en la versión 1.0. La devolución se realiza en consultorio. Resuelve A4. | La devolución de resultados se hace en el consultorio y a criterio de la profesional, según la regla RN28. Habilitar una vista de consulta para el paciente exige un acuerdo previo sobre el tratamiento de datos de salud que no se alcanzó durante el relevamiento. | `docs/requisitos.md`, `docs/diseño-ui.md`. |
| D14 | 05/2026 | Para registrar como paciente nutricional a un alumno menor de 18 años, el sistema exige el tutor responsable y la fecha de autorización. Resuelve A5. | Las reglas RN05 y RN27 obligan a registrar un tutor responsable para los menores de 18 años, y el centro dicta dos disciplinas infantiles. Sin el tutor y la fecha de autorización, la consulta de un menor quedaría registrada sin respaldo. | `docs/requisitos.md`, `docs/er-modelo.md`, `docs/casos-de-uso.md`. |
| D15 | 05/2026 | La arquitectura prevista es Web App, back-end y PostgreSQL, con **n8n** como motor de automatización e integración. n8n no contiene lógica de negocio ni reemplaza al back-end: ejecuta procesos periódicos y distribuye resultados. Ningún requisito funcional depende de él. | La restricción RE06 descarta las aplicaciones de escritorio y RE07 acota el presupuesto, lo que lleva a una solución web sobre tecnologías de código abierto. n8n cubre los procesos que se disparan por tiempo y no por acción de un usuario; mantener las reglas de negocio fuera de él las deja bajo control de versiones y dentro del alcance de las pruebas que exige RNF13. | `README.md`, `RECURSOS.md`, `docs/requisitos.md` (RE11), `docs/diseño-ui.md`. |
| D16 | 10/2026 | La accesibilidad se incorpora al sistema como requisito verificable: se adopta WCAG 2.1 nivel AA sobre los criterios de contraste, color, escalado de texto, operación por teclado y tamaño de los objetivos táctiles. Se documenta en la sección 6 de `docs/diseño-ui.md` y se formaliza como **RNF14** en el apartado 7.6 de `docs/requisitos.md`. | El relevamiento no planteó la accesibilidad, pero las condiciones de uso relevadas la exigen: la recepcionista cobra de pie con las manos ocupadas, el instructor opera la tablet con una mano en el salón (restricción RE05), la dirección trabaja el listado de morosos impreso en blanco y negro, y el portal del alumno se abre desde el teléfono de un padrón que va de los chicos de Aeróbica Infantil a los adultos mayores de Pilates. Dejarla solo como criterio de diseño la mantenía fuera de toda verificación; escrita como requisito, se verifica igual que el resto. | `docs/requisitos.md` (RNF14, RE02), `docs/diseño-ui.md`, `DoR.md` (criterio C7). |
| D17 | 10/2026 | El equipo trabaja directamente sobre `main`, sin ramas ni Pull Requests. La calidad se controla con el reparto por artefacto, la revisión contra la Definition of Ready en la reunión de cierre y el acuerdo previo de toda decisión que cruce documentos. | El apartado 4 declaraba un flujo de ramas y Pull Requests que el equipo nunca aplicó, y el historial del repositorio lo desmiente desde el primer commit. Ante la alternativa de adoptar el flujo declarado o declarar el flujo real, el equipo eligió lo segundo: un procedimiento que el repositorio contradice no es un procedimiento, es una intención. Queda registrado también qué se pierde al trabajar así, para que la elección sea explícita y no por omisión. | `integrantes.md` (apartados 1, 4 y 5), `README.md` (apartados 11 y 12), `RECURSOS.md`, `DoR.md` (autoevaluación). |

Las alternativas de modelado que se evaluaron y se descartaron están documentadas en [`docs/er-modelo.md`](docs/er-modelo.md), sección 6.3.

Toda decisión nueva se agrega a esta tabla en la misma tanda de cambios que actualiza los documentos afectados, y el mensaje de commit la nombra por su identificador.

---

<sub>Escuela Superior de Comercio N° 49 "Justo José de Urquiza" — Desarrollo Web / Analista Funcional de Sistemas — 2026.</sub>
