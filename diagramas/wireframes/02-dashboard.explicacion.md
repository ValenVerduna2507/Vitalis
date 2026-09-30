# 02 — Dashboard

Explicación del wireframe [`02-dashboard.puml`](../02-dashboard.puml).

| | |
|---|---|
| **Pantalla** | P02 — Dashboard |
| **Rol** | Recepcionista (el dashboard cambia según el rol) |
| **Historia** | Transversal |
| **Caso de uso** | — |
| **Requisitos** | RNF03, RNF07 |
| **Detalle funcional** | [`docs/diseño-ui.md`](../../../docs/diseño-ui.md), pantalla P02 |

![Wireframe P02 — Dashboard](../02-dashboard.png)

---

## Qué representa

Es la primera pantalla que se ve después de iniciar sesión. El boceto muestra la versión del rol **Recepcionista**: el dashboard es una sola pantalla que cambia de contenido según quién entre. El Administrador ve monto adeudado e ingresos del mes; el Instructor, sus clases del día; la Nutricionista, sus pacientes. Este boceto es también el que muestra la estructura común que comparten todas las pantallas del sistema salvo el login.

## Recorrido del boceto

| Zona del boceto | Qué es | Por qué está |
|---|---|---|
| Encabezado | Barra fija | Nombre del sistema, usuario conectado con su rol entre paréntesis, y salida. Se repite igual en todas las pantallas. |
| Menú lateral | Navegación | Las secciones habilitadas para el rol. Una función no permitida no aparece en el menú y tampoco es accesible escribiendo la dirección (RNF03). |
| Tres indicadores numéricos | Datos calculados en el momento | Alumnos activos, cobros de hoy y alumnos morosos: las tres cifras que la recepcionista necesita al abrir el mostrador. |
| Accesos rápidos | Botones | Buscar alumno, Nuevo alumno y Registrar pago. Son las tres operaciones más frecuentes del puesto. |
| Bloque de avisos | Alerta con acción | Muestra los alumnos que quedaron sin clase tras un cambio de grilla. No es un dato decorativo: es un problema operativo concreto detectado en el relevamiento, y el botón lleva al listado para resolverlo. |

## Decisiones que el boceto hace visibles

- **El dashboard no es un tablero de estadísticas.** Cada indicador existe porque alguien lo necesita para trabajar, no porque quede bien. La recepcionista no ve ingresos del mes ni monto adeudado total: eso es información de dirección.
- **El acceso rápido a Registrar pago acorta el camino frecuente.** El requisito RNF08 limita el cobro a cuatro pasos desde la búsqueda; este botón permite arrancar el flujo sin pasar por el menú.
- **El aviso trae la acción incorporada.** No informa un problema y deja al usuario buscando dónde resolverlo: el botón Ver listado está al lado.

## Lo que este boceto no define

Un wireframe define qué elementos tiene la pantalla y cómo se organizan. No define colores, tipografías, iconos ni espaciados exactos: eso corresponde a la etapa de implementación. Tampoco muestra los estados alternativos de la pantalla —errores, listas vacías, cargas en curso—, que están descriptos en `docs/diseño-ui.md`. Tampoco muestra las otras cuatro versiones del dashboard, una por cada rol restante.

## Observación para revisar

El menú lateral del boceto tiene cuatro entradas: Inicio, Alumnos, Pagos y Clases. La tabla de menú por rol de `docs/diseño-ui.md` asigna al rol Recepcionista una entrada más, **Reportes**, con acceso de solo consulta al listado de alumnos morosos (P08). Conviene agregarla al boceto o dejar constancia de por qué no está.

---

<sub>Escuela Superior de Comercio N° 49 "Justo José de Urquiza" — Desarrollo Web / Analista Funcional de Sistemas — Grupo 02 — 2026.</sub>
