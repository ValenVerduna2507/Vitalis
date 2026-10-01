# Prototipo funcional

Implementación navegable del Sistema de Gestión Integral para Vitalis Centro de
Entrenamiento, construida a partir del análisis funcional contenido en este
repositorio.

## Dirección

https://vitalis-prototipo.vercel.app

Accesible desde cualquier navegador, sin instalación previa (RE06). El acceso a
cada rol se solicita a los integrantes del grupo.

## Alcance implementado

| Pantalla | Descripción | Requisitos |
|---|---|---|
| P01 | Acceso al sistema con control por rol | RF22, RNF03, RNF04 |
| P02 | Panel principal, con indicadores diferenciados por rol | RNF03, RNF07 |
| P03 | Padrón de alumnos con búsqueda y filtros | RF24 |
| P04 | Alta de alumno con validación de DNI, tutor para menores y reactivación | RF01, RF02, RF03, RF05 |
| P05 | Ficha del alumno | RF03, RF10, RF18 |
| P06 | Registro de pago de cuota | RF06, RF07, RNF08 |
| P07 | Historial de pagos | RF10 |
| P08 | Reporte de alumnos morosos, con umbral configurable y exportación | RF08, RF09, RF30 |
| P12 | Agenda del instructor | RF26 |
| P13 | Control de asistencia | RF15, RF16, RF17, RF27 |

Las secciones del menú que todavía no tienen pantalla propia se indican como no
disponibles dentro del sistema.

## Reglas de negocio implementadas

El control de acceso por rol se resuelve en el servidor: ocultar una opción del
menú no constituye control de acceso (CU-00, excepción E4).

Entre las reglas verificables en el prototipo:

- **RN01** — El DNI identifica unívocamente al alumno, incluidos los dados de baja.
- **RN02** — La baja es lógica: ningún registro se elimina.
- **RN04** — El legajo se asigna de forma automática y correlativa, y no se reutiliza.
- **RN05** — Los alumnos menores de 18 años exigen familiar o tutor responsable.
- **RN09** — La morosidad se calcula contra un umbral configurable, no almacenado.
- **RN18** — Un alumno activo puede registrarse como asistente ocasional.
- **RN19** — Un único registro de asistencia por alumno, clase y fecha.
- **RN22** — Cada usuario tiene un único rol asignado.
- **RN24** — Un usuario desactivado no puede iniciar sesión.
- **RN25** — La modalidad por clase no genera morosidad.

El estado de cuenta, los días de mora y el monto adeudado son datos derivados:
se calculan en el momento de la consulta, conforme a la sección 8 de
`docs/er-modelo.md`.

## Arquitectura

Respeta la separación definida en la restricción RE11 y detallada en la sección
9.4 de `docs/requisitos.md`:

```text
   Navegador  ──►  Servidor  ──►  PostgreSQL
```

| Componente | Responsabilidad |
|---|---|
| Navegador | Las pantallas documentadas en `docs/diseño-ui.md`. |
| Servidor | Validaciones, reglas de negocio y control de acceso por rol. |
| PostgreSQL | Las dieciocho entidades de `docs/er-modelo.md`. |

La automatización con n8n, prevista en RE11, no forma parte de esta versión.
Ningún requisito funcional depende de ella para cumplirse.

## Herramientas

| Herramienta | Uso |
|---|---|
| Next.js y React, en TypeScript | Interfaz y lógica del servidor |
| Tailwind CSS | Diseño de la interfaz |
| Prisma | Acceso a datos y migraciones, derivadas del modelo ER |
| PostgreSQL sobre Neon | Persistencia |
| Vercel | Publicación |
| Git y GitHub | Versionado |

Todas de código abierto o en plan gratuito: el costo de construcción y
publicación es cero, conforme a la restricción RE07.

## Relación con este repositorio

Este repositorio contiene el análisis funcional, que es el entregable de la
materia. El prototipo es material de apoyo construido sobre ese análisis y se
versiona por separado. Ante cualquier diferencia entre ambos, la documentación
de `docs/` es la referencia válida.

---

<sub>Escuela Superior de Comercio N° 49 "Justo José de Urquiza" — Desarrollo Web / Analista Funcional de Sistemas — 2026.</sub>
