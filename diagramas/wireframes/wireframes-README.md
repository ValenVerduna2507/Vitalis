# Wireframes

Bocetos de baja fidelidad de las pantallas principales del **Sistema de Gestión Integral — Vitalis Centro de Entrenamiento**.

Equipo: Grupo 02

---

## Qué son y qué no son

Un wireframe define **qué elementos tiene una pantalla y cómo se organizan**. No define colores, tipografías, iconos ni estilo visual: eso corresponde a la etapa de implementación.

| Sí definen | No definen |
|---|---|
| Qué información se muestra y dónde. | Paleta de colores. |
| Qué acciones están disponibles. | Tipografía y tamaños. |
| Cómo se agrupan y jerarquizan los datos. | Iconografía. |
| Qué campos tiene cada formulario. | Espaciados exactos. |

La descripción funcional completa de cada pantalla —objetivo, acceso por rol, validaciones, mensajes y navegación— está en [`docs/diseño-ui.md`](../../docs/diseño-ui.md). Estos bocetos son su complemento visual.

---

## Cómo están hechos

Los wireframes se escriben en **PlantUML Salt**, la extensión de PlantUML para maquetar interfaces. Se eligió por las mismas razones que el resto de los diagramas del proyecto:

| Motivo | Detalle |
|---|---|
| Versionable | Al ser texto plano, Git muestra exactamente qué cambió entre versiones. |
| Consistente | Misma herramienta que el diagrama de casos de uso y el modelo entidad-relación. |
| Sin dependencias | No requiere una cuenta ni una licencia de una herramienta de diseño. |
| Colaborativo | Dos integrantes pueden editar el mismo boceto sin pisarse. |

Cada boceto tiene tres archivos en esta misma carpeta: el `.puml` es la fuente, el `.png` es su render —incluido en el repositorio para que pueda verse directamente desde GitHub sin necesidad de compilar— y el `.explicacion.md` desarrolla en texto qué representa el boceto y por qué está cada elemento.

---

## Cómo visualizarlos o modificarlos

**Opción A — Servidor web**

1. Abrir <https://www.plantuml.com/plantuml/uml/>
2. Copiar el contenido del archivo `.puml`.
3. Pegarlo y presionar **Submit**.

**Opción B — Visual Studio Code**

1. Instalar la extensión **PlantUML** (`jebbs.plantuml`).
2. Abrir el archivo `.puml`.
3. Presionar `Alt + D`.

**Opción C — Línea de comandos**

```bash
plantuml -tpng 01-login.puml
```

Si se modifica un `.puml`, hay que regenerar su `.png` y subir ambos en el mismo commit.

---

## Listado de wireframes

| Archivo | Pantalla | Rol | Historias | Casos de uso |
|---|---|---|---|---|
| `01-login.puml` | P01 — Login | Todos | HU-20 | CU-00 |
| `02-dashboard.puml` | P02 — Dashboard | Recepcionista | Transversal | — |
| `03-gestion-alumnos.puml` | P03 — Gestión de alumnos | Recepcionista | HU-09 | — |
| `04-alta-alumno.puml` | P04 — Alta de alumno | Recepcionista | HU-01, HU-06 | CU-01, CU-04 |
| `05-ficha-alumno.puml` | P05 — Ficha del alumno | Recepcionista | HU-06, HU-07, HU-08, HU-16 | CU-04, CU-05, CU-06 |
| `06-registro-pago.puml` | P06 — Registro de pago | Recepcionista | HU-02 | CU-02 |
| `07-alumnos-morosos.puml` | P08 — Alumnos morosos | Administrador | HU-03 | CU-03 |
| `08-gestion-clases.puml` | P09 — Gestión de clases y grillas | Administrador | HU-12, HU-13, HU-14 | CU-08, CU-09, CU-10 |
| `09-control-asistencia.puml` | P13 — Control de asistencia | Instructor | HU-04, HU-17 | CU-12 |
| `10-consultorio-nutricional.puml` | P14 — Consultorio nutricional | Nutricionista | HU-05a | CU-13 |

**Total: 10 wireframes.**

---

## Explicaciones

Cada wireframe tiene al lado un archivo `NN-<pantalla>.explicacion.md`. Un boceto muestra qué hay en la pantalla, pero no dice por qué: la explicación es la que registra el criterio.

Cada una desarrolla cuatro cosas:

| Apartado | Qué responde |
|---|---|
| Qué representa | Qué pantalla es, a qué rol corresponde y qué lugar ocupa en el sistema. |
| Recorrido del boceto | Zona por zona, qué es cada elemento y por qué está. |
| Decisiones que el boceto hace visibles | Qué criterio de análisis se ve en el dibujo, con el requisito o la regla que lo respalda. |
| Lo que este boceto no define | Qué queda deliberadamente afuera, para que no se lea como una omisión. |

Están en esta misma carpeta y no en una subcarpeta, de modo que el `.puml`, el `.png` y su explicación queden juntos y se vean de un vistazo.

---

## Criterio de selección

De las diecinueve pantallas documentadas se bocetaron diez. El criterio fue cubrir:

| Criterio | Pantallas |
|---|---|
| Al menos una pantalla por cada rol del sistema. | P01 todos, P03 recepcionista, P08 administrador, P13 instructor, P14 nutricionista. |
| Las operaciones más frecuentes del centro. | P03 búsqueda, P06 cobro, P13 asistencia. |
| Las pantallas con mayor riesgo de diseño. | P06 por el límite de cuatro pasos, P13 por el uso en tablet. |
| Las pantallas que concentran información. | P05 ficha del alumno, P02 dashboard. |

Las nueve pantallas restantes (P07, P10, P11, P12, P15, P16, P17, P18, P19) siguen la estructura común descripta en `docs/diseño-ui.md` y reutilizan los mismos patrones de listado, formulario y ficha que los bocetos ya realizados.

---

## Decisiones de diseño reflejadas en los bocetos

| Wireframe | Decisión visible | Origen |
|---|---|---|
| `01-login` | El mensaje de error no revela si falló el usuario o la contraseña. | CU-00, E1 |
| `03-gestion-alumnos` | Un único campo de búsqueda que acepta apellido, DNI o legajo. | HU-09, criterio 1 |
| `03-gestion-alumnos` | Columna de situación de cuenta en el listado, para no tener que abrir la ficha. | RNF08 |
| `04-alta-alumno` | Solo cuatro campos obligatorios. La sección de tutor aparece si el alumno es menor. | RN05, conflicto CF2 |
| `05-ficha-alumno` | Barra de acciones fija arriba: la ficha es el centro operativo del sistema. | `docs/diseño-ui.md`, sección 4 |
| `06-registro-pago` | Monto y período precargados: el caso frecuente se confirma sin tipear nada. | RNF08 |
| `07-alumnos-morosos` | Columna de teléfono, porque el uso real del reporte es llamar a cada alumno. | HU-03 |
| `08-gestion-clases` | Vista de calendario semanal, no listado plano. | HU-13 |
| `09-control-asistencia` | Botones grandes, marcado masivo y una sola confirmación para toda la clase. | RE05, RNF07 |
| `10-consultorio-nutricional` | IMC calculado en pantalla y comparación con la consulta anterior. | RN26, decisión D11 |

---

## Datos de ejemplo

Los bocetos usan el mismo conjunto de datos que el resto del repositorio, para que las pantallas se lean como un sistema coherente y no como capturas sueltas.

| Rol | Nombre |
|---|---|
| Directora | Andrea Sosa |
| Recepcionista | Julieta Ferreyra |
| Instructores | Martín López, Sofía Martínez, Lucas Fernández, Carla Giménez, Diego Ríos |
| Nutricionista | Paula Ibarra |
| Alumna de ejemplo | Laura Benítez, legajo 218 |

---

<sub>Escuela Superior de Comercio N° 49 "Justo José de Urquiza" — Desarrollo Web / Analista Funcional de Sistemas — 2026.</sub>
