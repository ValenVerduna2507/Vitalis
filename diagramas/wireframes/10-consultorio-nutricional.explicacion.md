# 10 — Consultorio nutricional

Explicación del wireframe [`10-consultorio-nutricional.puml`](../10-consultorio-nutricional.puml).

| | |
|---|---|
| **Pantalla** | P14 — Consultorio nutricional |
| **Rol** | Exclusivo de la Nutricionista |
| **Historia** | HU-05a |
| **Caso de uso** | CU-13 |
| **Requisitos** | RF19, RF21, RNF06 |
| **Detalle funcional** | [`docs/diseño-ui.md`](../../../docs/diseño-ui.md), pantalla P14 |

![Wireframe P14 — Consultorio nutricional](../10-consultorio-nutricional.png)

---

## Qué representa

Es el único módulo del sistema con acceso exclusivo de un solo rol. Ni la recepcionista ni los instructores lo ven en el menú, y el Administrador puede asignarle pacientes a la nutricionista pero no entra acá ni ve el contenido de las consultas. El boceto está dividido en dos columnas: a la izquierda a quién se atiende, a la derecha qué se registra.

## Recorrido del boceto

| Zona del boceto | Qué es | Por qué está |
|---|---|---|
| Columna Mis pacientes | Listado | Solo los pacientes asignados a esa profesional, con la fecha de la última consulta. |
| Encabezado de la consulta | Contexto | Paciente y edad. La edad importa porque condiciona la lectura de los parámetros. |
| Campos de medición | Formulario | Peso, altura, perímetro de cintura, perímetro de cadera y porcentaje de masa grasa, cada uno en su propio campo. |
| Campo Objetivo | Selector | Clasifica el tratamiento y permite comparar la evolución contra una meta. |
| Campo Observaciones | Texto libre | Lo único que no está estructurado, porque no todo lo clínico se puede tabular. |
| Franja de IMC y consulta anterior | Datos calculados | El IMC se calcula en pantalla y se muestra junto al peso y al IMC de la consulta previa. |
| Nota de acceso exclusivo | Aclaración | Deja constancia en el propio boceto de que la pantalla pertenece a un solo rol. |

## Decisiones que el boceto hace visibles

- **Cada medida en su propio campo.** El modelo preliminar tenía un único campo de texto llamado "medidas". Así no se puede validar, ni comparar entre consultas, ni graficar una evolución. La decisión D11 lo reemplazó por campos separados.
- **El IMC se calcula, no se guarda.** Es un valor derivado del peso y la altura. Almacenarlo abriría la posibilidad de que quede desactualizado si se corrige alguno de los dos.
- **La consulta anterior está a la vista mientras se carga la nueva.** El seguimiento nutricional es comparativo: un peso aislado no dice nada.
- **El acceso exclusivo es una decisión de diseño, no una omisión.** Responde a la posición que la nutricionista planteó en el relevamiento sobre el secreto profesional, y quedó formalizada en la regla RN29 y en la decisión D10.

## Lo que este boceto no define

Un wireframe define qué elementos tiene la pantalla y cómo se organizan. No define colores, tipografías, iconos ni espaciados exactos: eso corresponde a la etapa de implementación. Tampoco muestra los estados alternativos de la pantalla —errores, listas vacías, cargas en curso—, que están descriptos en `docs/diseño-ui.md`. No está dibujada la validación de rango razonable de los valores, que se incorporó a pedido de la nutricionista porque un error de tipeo en el peso distorsiona toda la curva de evolución del paciente.

---

<sub>Escuela Superior de Comercio N° 49 "Justo José de Urquiza" — Desarrollo Web / Analista Funcional de Sistemas — Grupo 02 — 2026.</sub>
