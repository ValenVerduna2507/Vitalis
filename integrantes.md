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
| Metodología de trabajo | Ágil, con iteraciones cortas y revisión por pares |
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

1. El integrante que redacta crea una rama a partir de `main` siguiendo la convención `docs/<tema>`, `diagram/<tema>` o `fix/<tema>`.
2. Trabaja sobre esa rama con commits pequeños y descriptivos.
3. Abre un Pull Request hacia `main` describiendo qué cambió y qué documentos se ven afectados.
4. El revisor que le corresponde según la rotación lee el PR y deja sus comentarios.
5. Si el cambio afecta a más de un documento, el PR requiere la aprobación de **dos** integrantes.
6. Una vez aprobado, se hace merge y se elimina la rama.
7. Ningún integrante hace push directo sobre `main`, incluido el autor del documento.

El detalle de los comandos está en [`RECURSOS.md`](RECURSOS.md).

### Criterios de calidad acordados

- Todo requisito debe tener un ID único y estable. Los IDs nunca se reutilizan ni se renumeran.
- Toda historia de usuario debe indicar a qué requisitos responde.
- Todo caso de uso debe indicar a qué historia y a qué requisitos corresponde.
- Toda entidad del modelo debe justificarse por al menos un requisito.
- Si un cambio en un documento obliga a modificar otro, ambos cambios viajan en el mismo Pull Request.
- Antes de cerrar una entrega se revisa la matriz de trazabilidad completa entre los cuatro integrantes.

---

## 5. Canales de comunicación

| Canal | Uso |
|---|---|
| Grupo de mensajería del equipo | Coordinación diaria, dudas rápidas, avisos. |
| Issues de GitHub | Registro de tareas pendientes, inconsistencias detectadas y decisiones a tomar. |
| Pull Requests | Discusión sobre el contenido de cada documento. |
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

Las alternativas de modelado que se evaluaron y se descartaron están documentadas en [`docs/er-modelo.md`](docs/er-modelo.md), sección 6.3.

Toda decisión nueva se agrega a esta tabla mediante un Pull Request que también actualice los documentos afectados.

---

<sub>Escuela Superior de Comercio N° 49 "Justo José de Urquiza" — Desarrollo Web / Analista Funcional de Sistemas — 2026.</sub>
