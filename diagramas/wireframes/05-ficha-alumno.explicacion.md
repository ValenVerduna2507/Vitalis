# 05 — Ficha del alumno

Explicación del wireframe [`05-ficha-alumno.puml`](../05-ficha-alumno.puml).

| | |
|---|---|
| **Pantalla** | P05 — Ficha del alumno |
| **Rol** | Administrador y Recepcionista |
| **Historias** | HU-06, HU-07, HU-08, HU-16 |
| **Casos de uso** | CU-04, CU-05, CU-06 |
| **Requisitos** | RF03, RF04, RF05, RF10, RF18 |
| **Detalle funcional** | [`docs/diseño-ui.md`](../../../docs/diseño-ui.md), pantalla P05 |

![Wireframe P05 — Ficha del alumno](../05-ficha-alumno.png)

---

## Qué representa

Es el centro operativo del sistema. Desde esta pantalla se resuelve casi toda la operación diaria de la recepcionista sin volver al menú. El boceto está organizado en cuatro franjas horizontales que van de lo identificatorio a lo histórico.

## Recorrido del boceto

| Zona del boceto | Qué es | Por qué está |
|---|---|---|
| Franja de identificación | Encabezado del alumno | Apellido y nombre, legajo, DNI, estado y situación de cuenta destacada: "MOROSO - 41 días - $56.000". Es lo primero que hay que saber al abrir la ficha. |
| Barra de acciones | Cuatro botones fijos | Editar, Registrar pago, Inscribir a clase y Dar de baja. Está arriba y siempre visible: son las operaciones que nacen desde acá. |
| Columna Datos personales | Bloque de consulta | Nacimiento, teléfono, email, fecha de alta y modalidad vigente. |
| Columna Clases inscriptas | Tabla | Disciplina, día, horario e instructor de cada clase en la que participa. |
| Bloque Últimos pagos | Tabla resumida con enlace | Muestra los dos últimos y ofrece Ver historial. La ficha resume; el detalle vive en P07. |
| Bloque Últimas asistencias | Tabla resumida con enlace | Mismo criterio que los pagos. Incluye el estado de cada asistencia. |

## Decisiones que el boceto hace visibles

- **La situación de cuenta se destaca en el encabezado, no se esconde en una tabla.** Si el alumno debe, tiene que verse antes de cualquier otra cosa: es el dato que cambia la conversación en el mostrador.
- **La barra de acciones va arriba y fija.** La ficha es el punto de partida de casi todas las operaciones sobre un alumno; obligar a bajar hasta el pie para encontrar los botones sería un costo diario.
- **Los botones no disponibles no se ocultan: se deshabilitan.** Ocultarlos generaría la sensación de que la función no existe. Está definido en la sección 4 de `docs/diseño-ui.md`.
- **Los bloques históricos muestran solo dos filas.** La ficha resume y deriva. Cargar la ficha con el historial completo la volvería ilegible para el caso frecuente.

## Lo que este boceto no define

Un wireframe define qué elementos tiene la pantalla y cómo se organizan. No define colores, tipografías, iconos ni espaciados exactos: eso corresponde a la etapa de implementación. Tampoco muestra los estados alternativos de la pantalla —errores, listas vacías, cargas en curso—, que están descriptos en `docs/diseño-ui.md`. No están dibujados el estado de la ficha de un alumno dado de baja, ni el diálogo de confirmación de la baja, que según el principio 5 del documento de diseño debe explicar su efecto antes de ejecutarse.

---

<sub>Escuela Superior de Comercio N° 49 "Justo José de Urquiza" — Desarrollo Web / Analista Funcional de Sistemas — Grupo 02 — 2026.</sub>
