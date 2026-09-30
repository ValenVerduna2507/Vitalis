# 03 — Gestión de alumnos

Explicación del wireframe [`03-gestion-alumnos.puml`](../03-gestion-alumnos.puml).

| | |
|---|---|
| **Pantalla** | P03 — Gestión de alumnos |
| **Rol** | Administrador y Recepcionista (Instructor, solo consulta) |
| **Historia** | HU-09 |
| **Caso de uso** | — |
| **Requisitos** | RF24, RNF09 |
| **Detalle funcional** | [`docs/diseño-ui.md`](../../../docs/diseño-ui.md), pantalla P03 |

![Wireframe P03 — Gestión de alumnos](../03-gestion-alumnos.png)

---

## Qué representa

Es la pantalla de búsqueda del padrón. El problema que resuelve es concreto: encontrar a una persona entre más de doscientos registros, con alguien esperando del otro lado del mostrador. Todo el diseño está orientado a eso.

## Recorrido del boceto

| Zona del boceto | Qué es | Por qué está |
|---|---|---|
| Campo Buscar | Un solo campo de texto | Acepta apellido, DNI o legajo indistintamente. El sistema resuelve qué se escribió; el usuario no elige el tipo de búsqueda. |
| Filtros Estado, Actividad y Turno | Selectores | Acotan el resultado. El filtro de estado permite incluir o excluir a los alumnos dados de baja. |
| Contador "12 alumnos encontrados" | Texto dinámico | Le dice al usuario cuánto abarca el resultado antes de leerlo. |
| Tabla de resultados | Listado | Legajo, apellido y nombre, DNI, actividad, estado y situación de cuenta. |
| Columna Cuenta | Dato derivado | Muestra "Al día" o "Moroso" con los días de atraso. Es la columna que evita abrir la ficha para responder la pregunta más frecuente del mostrador. |
| Fila con estado Baja | Caso de ejemplo | Un alumno dado de baja aparece con la cuenta en guiones: no se le calcula situación de cuenta. Refleja la baja lógica de la regla RN02: el registro no se elimina. |
| Nota al pie | Aclaración de comportamiento | Seleccionar una fila abre la ficha del alumno (P05). El boceto lo aclara porque la tabla no tiene un botón visible de apertura. |

## Decisiones que el boceto hace visibles

- **Un campo de búsqueda en lugar de tres.** Viene del criterio 1 de la historia HU-09: la recepcionista escribe lo que tiene a mano y el sistema se arregla. Tres campos separados obligarían a decidir antes de buscar.
- **La situación de cuenta está en el listado.** Responde al requisito RNF08 y a la operación real: la pregunta más habitual en el mostrador es si un alumno está al día.
- **El ejemplo usa cuatro apellidos iguales a propósito.** Cuatro Gómez muestran por qué el listado necesita DNI y legajo: con el apellido solo no alcanza para identificar a nadie.

## Lo que este boceto no define

Un wireframe define qué elementos tiene la pantalla y cómo se organizan. No define colores, tipografías, iconos ni espaciados exactos: eso corresponde a la etapa de implementación. Tampoco muestra los estados alternativos de la pantalla —errores, listas vacías, cargas en curso—, que están descriptos en `docs/diseño-ui.md`. Tampoco muestra el comportamiento de la búsqueda sin resultados, que según el principio 6 del documento de diseño no se trata como un error.

---

<sub>Escuela Superior de Comercio N° 49 "Justo José de Urquiza" — Desarrollo Web / Analista Funcional de Sistemas — Grupo 02 — 2026.</sub>
