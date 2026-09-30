# 07 — Alumnos morosos

Explicación del wireframe [`07-alumnos-morosos.puml`](../07-alumnos-morosos.puml).

| | |
|---|---|
| **Pantalla** | P08 — Alumnos morosos |
| **Rol** | Administrador (Recepcionista, solo consulta) |
| **Historia** | HU-03 |
| **Caso de uso** | CU-03 |
| **Requisitos** | RF08, RF09, RF30 |
| **Detalle funcional** | [`docs/diseño-ui.md`](../../../docs/diseño-ui.md), pantalla P08 |

![Wireframe P08 — Alumnos morosos](../07-alumnos-morosos.png)

---

## Qué representa

Es el reporte que reemplaza la tarea que hoy le lleva alrededor de una hora por mes a la dirección: recorrer la planilla fila por fila para saber quién debe. El boceto está pensado para que el listado no se lea, se use.

## Recorrido del boceto

| Zona del boceto | Qué es | Por qué está |
|---|---|---|
| Franja de totales | Resumen | Cantidad de morosos y monto adeudado total. Son las dos cifras que la dirección mira primero. |
| Filtros de actividad y turno | Selectores | Permiten reclamar por disciplina, que es como la dirección organiza la cobranza. |
| Campo "Mora mayor a … días" | Numérico editable | El umbral es un parámetro configurable, no un valor escrito en el código. Corresponde al requisito RF30 y a la decisión D6. |
| Tabla de morosos | Listado | Legajo, nombre, actividad, turno, último pago, días de mora, monto adeudado y teléfono. |
| Columna Teléfono | Dato de contacto | Está en el listado porque el uso real del reporte es llamar a cada alumno. Sin ella habría que abrir una ficha por persona. |
| Botón Exportar PDF | Acción | Permite imprimir el listado filtrado y trabajarlo fuera del sistema. |
| Nota sobre la modalidad por clase | Aclaración de alcance | Los alumnos que pagan por clase no aparecen en este reporte. |

## Decisiones que el boceto hace visibles

- **El umbral es editable desde la propia pantalla.** La dirección describió la mora como algo difuso —"un mes, un mes y monedas"— y modifica sus criterios sin avisar. Por eso el valor es un parámetro y no una constante.
- **La columna de teléfono responde al uso, no al dato.** El reporte no termina cuando se muestra: termina cuando alguien llamó.
- **Los alumnos con modalidad por clase quedan fuera.** Su pago es anticipado, de modo que no pueden adeudar nada. Incluirlos los marcaría como deudores sin que deban. Regla RN25 y decisión D12.

## Lo que este boceto no define

Un wireframe define qué elementos tiene la pantalla y cómo se organizan. No define colores, tipografías, iconos ni espaciados exactos: eso corresponde a la etapa de implementación. Tampoco muestra los estados alternativos de la pantalla —errores, listas vacías, cargas en curso—, que están descriptos en `docs/diseño-ui.md`. No está dibujado el caso sin resultados, que según el principio 6 se informa como una situación normal y no como un error.

---

<sub>Escuela Superior de Comercio N° 49 "Justo José de Urquiza" — Desarrollo Web / Analista Funcional de Sistemas — Grupo 02 — 2026.</sub>
