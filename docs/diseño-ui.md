# Diseño de interfaz

Sistema de Gestión Integral — Vitalis Centro de Entrenamiento
Equipo: Grupo 02
Versión: 1.3

---

## Índice

- [1. Alcance de este documento](#1-alcance-de-este-documento)
- [2. Principios de diseño](#2-principios-de-diseño)
- [3. Estructura común de las pantallas](#3-estructura-común-de-las-pantallas)
- [4. Mapa de navegación](#4-mapa-de-navegación)
- [5. Convenciones de mensajes y validaciones](#5-convenciones-de-mensajes-y-validaciones)
- [6. Accesibilidad](#6-accesibilidad)
- [7. Listado de pantallas](#7-listado-de-pantallas)
- [8. Pantallas transversales](#8-pantallas-transversales)
- [9. Pantallas de gestión de alumnos](#9-pantallas-de-gestión-de-alumnos)
- [10. Pantallas de cuotas y pagos](#10-pantallas-de-cuotas-y-pagos)
- [11. Pantallas de planificación](#11-pantallas-de-planificación)
- [12. Pantallas de instructores](#12-pantallas-de-instructores)
- [13. Pantallas de asistencia](#13-pantallas-de-asistencia)
- [14. Pantallas del módulo nutricional](#14-pantallas-del-módulo-nutricional)
- [15. Pantallas de gestión y configuración](#15-pantallas-de-gestión-y-configuración)
- [16. Pantalla del alumno](#16-pantalla-del-alumno)
- [17. Verificación de cobertura](#17-verificación-de-cobertura)

---

## 1. Alcance de este documento

Este documento define **qué hace cada pantalla del sistema**, no cómo se ve. Es documentación funcional: describe objetivo, acceso, elementos, acciones, validaciones, navegación e información mostrada.

Las decisiones visuales (paleta de colores, tipografía, iconografía) corresponden a la etapa de implementación y no se definen acá. Los bocetos de baja fidelidad se encuentran en [`diagramas/wireframes/`](../diagramas/wireframes/).

### Qué no incluye

| No incluye | Motivo |
|---|---|
| Paleta de colores y tipografía | Decisión de implementación. |
| Código HTML o CSS | El alcance de esta etapa es el análisis funcional. |
| Micro-interacciones y animaciones | No condicionan el análisis. |
| Pantallas de error del servidor | Son genéricas y comunes a todo el sistema. |

### Procesos que no tienen pantalla

No todo lo que hace el sistema ocurre porque alguien lo pide. Algunos procesos se ejecutan solos, de forma periódica, y su resultado llega al usuario sin que este haya abierto ninguna pantalla. Se documentan acá porque forman parte de la experiencia de uso, aunque no tengan interfaz propia.

| Proceso automático | Cómo llega al usuario | Pantalla equivalente |
|---|---|---|
| Clasificación diaria de alumnos morosos e inactivos. | El dashboard y el reporte de morosos ya muestran el resultado actualizado al abrirlos. | P02, P08 |
| Envío periódico del reporte de morosidad e ingresos a la dirección. | Llega por email, sin necesidad de entrar al sistema. | P16 |
| Notificaciones automáticas a alumnos. | Extensión futura, fuera del alcance de la versión 1.0. | — |

Estos procesos están orquestados por **n8n**, la herramienta de automatización prevista en la arquitectura (ver `docs/requisitos.md`, sección 9.4). Desde el punto de vista funcional lo relevante es que **ninguna pantalla depende de ellos**: si la automatización no se ejecuta, la información sigue estando disponible consultándola manualmente. La automatización ahorra el paso de pedirla, no la genera.

---

## 2. Principios de diseño

Los principios surgen de los requisitos no funcionales y de las restricciones relevadas. No son preferencias estéticas.

| # | Principio | Origen | Consecuencia práctica |
|---|---|---|---|
| 1 | **Cada rol ve solo lo suyo** | RNF03, RN22 | El menú se arma según el rol. Una función no permitida no aparece, y tampoco es accesible por dirección directa. |
| 2 | **El camino frecuente es el más corto** | RNF08 | Registrar un pago no supera los cuatro pasos desde la búsqueda hasta la confirmación. |
| 3 | **Funciona en tablet** | RNF07, RE05 | La toma de asistencia se opera con el dedo, de pie y con poco tiempo. Los controles son grandes y no requieren precisión. |
| 4 | **Los mensajes dicen qué hacer** | RNF09 | Ningún mensaje se limita a informar un error: siempre indica la acción siguiente. |
| 5 | **Nada se borra sin avisar** | RN02 | Toda operación destructiva o irreversible pide confirmación explícita y explica su efecto. |
| 6 | **Sin resultados no es un error** | CU-03, E1 | Cuando una búsqueda o un reporte no devuelve datos, el sistema lo informa como situación normal y ofrece la acción siguiente. |
| 7 | **La aplicación es más simple que la planilla** | RE02 | Si una operación requiere más pasos que en Excel, el diseño está mal. |
| 8 | **Lo que puede llegar solo, llega solo** | RE11 | Si la dirección necesita un reporte todos los meses, no debería tener que acordarse de entrar a pedirlo. Ver los procesos sin pantalla de la sección 1. |
| 9 | **La interfaz no depende del color ni del mouse** | Sección 6 | Ningún estado se comunica únicamente con color, y toda operación se puede completar con el teclado. |

---

## 3. Estructura común de las pantallas

Todas las pantallas del sistema, salvo el login, comparten la misma estructura.

```text
┌──────────────────────────────────────────────────────────────────────┐
│  VITALIS                    Andrea Sosa (Administrador)   Cerrar     │  Encabezado
├────────────────┬─────────────────────────────────────────────────────┤
│                │  Inicio > Alumnos > Ficha de alumno                 │  Ruta
│  Inicio        ├─────────────────────────────────────────────────────┤
│  Alumnos       │                                                     │
│  Pagos         │                                                     │
│  Clases        │              Área de contenido                      │
│  Asistencia    │                                                     │
│  Instructores  │                                                     │
│  Reportes      │                                                     │
│  Configuración │                                                     │
│                │                                                     │
├────────────────┴─────────────────────────────────────────────────────┤
│  Vitalis Centro de Entrenamiento — Pueblo Esther, Santa Fe           │  Pie
└──────────────────────────────────────────────────────────────────────┘
```

| Zona | Contenido | Comportamiento |
|---|---|---|
| Encabezado | Nombre del sistema, usuario conectado con su rol, opción de cerrar sesión. | Fijo en todas las pantallas. |
| Menú lateral | Secciones habilitadas para el rol del usuario. | Se arma según el rol. En tablet se contrae a un menú desplegable. |
| Ruta de navegación | Camino desde el inicio hasta la pantalla actual. | Cada nivel es un enlace de retorno. |
| Área de contenido | La pantalla propiamente dicha. | Variable. |
| Pie | Identificación del centro. | Fijo. |

### Menú por rol

| Sección del menú | Administrador | Recepcionista | Instructor | Nutricionista | Alumno |
|---|:---:|:---:|:---:|:---:|:---:|
| Inicio | Sí | Sí | Sí | Sí | Sí |
| Alumnos | Sí | Sí | No | No | No |
| Pagos | Sí | Sí | No | No | No |
| Clases | Sí | Sí | No | No | No |
| Mi agenda | No | No | Sí | No | No |
| Asistencia | Sí | No | Sí | No | No |
| Instructores | Sí | No | No | No | No |
| Consultorio | No | No | No | Sí | No |
| Reportes | Sí | Sí | No | No | No |
| Configuración | Sí | No | No | No | No |
| Mi cuenta | No | No | No | No | Sí |

> **Sobre la sección Reportes.** La Recepcionista accede a ella únicamente para
> consultar el reporte de alumnos morosos (P08), conforme a la matriz de
> permisos de `docs/requisitos.md` y al resumen de acceso por rol de la sección
> 16 de este documento. No accede a P16 Reportes de gestión, ni puede modificar
> el umbral de morosidad, que sigue siendo atribución del Administrador (RF30).

---

## 4. Mapa de navegación

```text
                            P01 Login
                                │
                                ▼
                        P02 Dashboard (por rol)
                                │
    ┌───────────┬───────────┬───┴────┬──────────┬───────────┬──────────┐
    ▼           ▼           ▼        ▼          ▼           ▼          ▼
P03 Alumnos  P08 Morosos  P09     P11        P13         P16       P19 Portal
    │                    Clases  Instructores Asistencia  Reportes   del alumno
    ├──► P04 Alta                  │             │
    │                              ▼             ▼
    └──► P05 Ficha              P12 Agenda   (asistente
          │                     del instructor  ocasional)
          ├──► P06 Registro de pago
          ├──► P07 Historial de pagos
          ├──► P10 Inscripción a clases
          └──► P05b Historial de asistencia

                        P14 Consultorio (Nutricionista)
                                │
                                ├──► P14b Nueva consulta
                                └──► P15 Historial de evolución

                        P17 Usuarios y roles      (Administrador)
                        P18 Parámetros            (Administrador)
```

La ficha del alumno (P05) es el centro operativo del sistema: desde ahí se llega a pagos, historial, inscripciones y asistencia sin volver al menú.

---

## 5. Convenciones de mensajes y validaciones

### 5.1 — Tipos de mensaje

| Tipo | Cuándo se usa | Ejemplo |
|---|---|---|
| Confirmación | Una operación se completó. | "El alumno se registró correctamente con el legajo 218." |
| Advertencia | La operación puede continuar, pero hay algo que el usuario debe saber. | "Este alumno tiene 2 cuotas impagas por 56.000 pesos. Podés continuar con la baja igualmente." |
| Bloqueo | La operación no puede completarse. | "El DNI 30.145.876 ya está registrado a nombre de Laura Benítez. Revisá los datos o buscá al alumno en el padrón." |
| Sin resultados | Una consulta no devolvió datos. | "Sin alumnos morosos al día de hoy." |
| Confirmación previa | Antes de una operación irreversible o de impacto amplio. | "Vas a activar la grilla de agosto. La grilla regular pasará a solo lectura. ¿Confirmás?" |

### 5.2 — Reglas de redacción

Según el requisito RNF09, todo mensaje debe cumplir tres condiciones: estar escrito sin tecnicismos, explicar qué pasó e indicar qué hacer.

| Correcto | Incorrecto |
|---|---|
| "El DNI ya está registrado a nombre de Laura Benítez. Revisá los datos o buscá al alumno en el padrón." | "Error: violación de restricción única." |
| "No se pudo guardar porque se perdió la conexión. Los datos siguen cargados, probá de nuevo." | "Error 500." |
| "Falta completar el motivo de la baja." | "Campo obligatorio." |

### 5.3 — Momento de la validación

| Tipo de validación | Cuándo se ejecuta |
|---|---|
| Formato de un campo (DNI, email, fecha) | Al salir del campo. |
| Campo obligatorio vacío | Al intentar confirmar. |
| Unicidad (DNI, nombre de usuario) | Al intentar confirmar, contra la base de datos. |
| Regla de negocio (superposición horaria, alumno de baja) | Al intentar confirmar. |

---

## 6. Accesibilidad

Los criterios de este apartado son transversales: valen para las diecinueve pantallas y se verifican sobre cada una antes de darla por terminada.

No son una formalidad. Salen de cómo y dónde se usa este sistema:

| Quién | En qué condiciones | Qué exige |
|---|---|---|
| Recepcionista | De pie en el mostrador, con alguien esperando y las manos repartidas entre el efectivo y el teclado. | Que el cobro completo se pueda hacer sin pasar al mouse. |
| Instructor | En el salón, con la tablet en una mano y pocos minutos entre una clase y la siguiente. | Controles grandes y separados, operables con el dedo y sin precisión. |
| Dirección | Revisando el listado de morosos para llamar por teléfono, muchas veces impreso en blanco y negro. | Que la situación de cada alumno se entienda sin depender del color. |
| Alumnos | Desde su propio teléfono. Las edades van de los chicos de Kids Fit and Fun a los adultos mayores de Pilates. | Texto que se pueda agrandar y contraste suficiente para leer con luz de día. |

**Criterio adoptado.** El equipo toma como referencia las pautas **WCAG 2.1, nivel AA**. Este documento define lo que se puede decidir en la etapa de diseño funcional; lo que depende del marcado y de la paleta definitiva queda enunciado acá y se verifica en la implementación, según el apartado 6.6.

> **Origen de estos criterios.** No derivan de un requisito no funcional: el relevamiento no planteó la accesibilidad y `docs/requisitos.md` no la cubre. El equipo los incorpora por decisión propia y propone sumarlos como **RNF14** en una próxima versión del documento de requisitos, para que queden sujetos a verificación como el resto.

### 6.1 — Color y contraste

| Criterio | Definición |
|---|---|
| Contraste de texto | Relación mínima de **4,5:1** entre el texto y su fondo. Para texto grande —18 puntos, o 14 en negrita— el mínimo baja a **3:1**. |
| Contraste de controles | Los bordes de campos, botones y el indicador de foco mantienen al menos **3:1** contra el fondo adyacente. |
| El color nunca es el único portador de información | Todo estado que hoy se piensa con color lleva además texto, y en los casos críticos también una diferencia de forma. |

La regla del color se aplica así en las pantallas donde aparece:

| Dónde | Qué se comunica | Refuerzo además del color |
|---|---|---|
| P03, P05 | Situación de cuenta del alumno | La palabra **Al día** o **Moroso**, y en la ficha también los días de atraso y el monto adeudado. |
| P03 | Estado del alumno | Columna de texto con **Activo** o **Baja**. La cuenta de un alumno dado de baja se muestra con guiones, no en blanco. |
| P08 | Gravedad de la mora | La columna de días de mora con el número. El orden del listado no se infiere de un color. |
| P09 | Grilla vigente | La etiqueta **(ACTIVA)** escrita en el selector. |
| P13 | Estado de asistencia | Las palabras **Presente**, **Ausente** y **Justificado** en cada botón. El botón seleccionado se distingue además por un cambio de borde, no solo de relleno. |
| P04, P06, P14 | Campos obligatorios | El asterisco, y el mensaje de error que nombra el campo que falta. |

**Prueba de control:** cada pantalla debe seguir siendo comprensible vista en escala de grises. Si al quitarle el color deja de entenderse un estado, el diseño está mal.

### 6.2 — Texto y tamaño

| Criterio | Definición |
|---|---|
| Tamaño base | 16 píxeles para el texto corriente. Ningún texto de la interfaz baja de 12. |
| Escalado | La interfaz debe poder ampliarse hasta el **200 %** sin que se pierda contenido ni funcionalidad, y sin que aparezca desplazamiento horizontal. Implica medidas relativas, no fijas. |
| Texto en imágenes | No se usa. Todo texto es texto seleccionable, de modo que el navegador pueda ampliarlo y un lector de pantalla leerlo. |
| Longitud de línea | Los bloques de texto no superan los 80 caracteres por línea, para no obligar a rastrear el renglón. |

El caso que gobierna este criterio es **P19 Portal del alumno**: es la única pantalla que se abre fuera del centro, en el teléfono de cualquiera, y el padrón incluye tanto a los tutores de los alumnos de Aeróbica Infantil como a los adultos mayores de Pilates y Yoga.

### 6.3 — Operación por teclado

| Criterio | Definición |
|---|---|
| Cobertura | Toda acción que se puede hacer con el mouse se puede hacer con el teclado. No existe ninguna función que dependa del puntero. |
| Orden de tabulación | Sigue el orden visual de la pantalla: de arriba hacia abajo y de izquierda a derecha. |
| Foco visible | El elemento con foco se distingue siempre, con un indicador que no se apoya únicamente en el color. |
| Sin trampas de foco | Ningún diálogo retiene el foco sin ofrecer una salida con teclado. |
| Atajos | No se definen combinaciones propias que puedan pisar las del navegador. |

El recorrido que hay que poder hacer completo sin tocar el mouse es el del cobro, porque es la operación más frecuente del mostrador y la que se hace con gente esperando:

| Paso | Pantalla | Teclado |
|---|---|---|
| 1 | P03 | Escribir en el campo de búsqueda y confirmar con Enter. |
| 2 | P03 | Recorrer las filas del resultado con las flechas y abrir la ficha con Enter. |
| 3 | P05 | Llegar a **Registrar pago** con Tab y activarlo con Enter. |
| 4 | P06 | Confirmar con Enter, con los campos ya precargados. |

Esto es coherente con lo ya definido en **P01**, donde presionar Enter en el campo de contraseña equivale a pulsar Ingresar.

### 6.4 — Controles táctiles

| Criterio | Definición |
|---|---|
| Tamaño del objetivo | En **P13 Control de asistencia**, los botones de estado miden al menos **44 × 44 píxeles**. Es un valor por encima del mínimo recomendado, y se adopta porque la pantalla se opera de pie y con la tablet en una mano. |
| Separación | Al menos 8 píxeles entre controles contiguos, para que marcar un estado no implique riesgo de tocar el de al lado. |
| Acciones destructivas | **Dar de baja** en P05 no queda contigua a una acción frecuente, y pide confirmación explicando su efecto, conforme al principio 5. |

Este apartado desarrolla el principio 3 y la restricción RE05, que ya condicionaban el diseño de P13.

### 6.5 — Formularios y mensajes

| Criterio | Definición |
|---|---|
| Etiquetas | Cada campo tiene una etiqueta visible y asociada al campo. El texto de ejemplo dentro del campo no reemplaza a la etiqueta: desaparece al escribir. |
| Agrupación | Los bloques de un formulario largo llevan título, como ya ocurre en P04 con *Datos personales*, *Contacto* y *Actividad y cobro*. |
| Ubicación del error | El mensaje aparece junto al campo que lo origina, además de en el resumen superior si lo hubiera. |
| Redacción del error | El mensaje nombra el campo y dice qué hacer, conforme al principio 4 y a la convención de la sección 5. |
| Confirmaciones | El resultado de una acción se informa con texto, no solo con un cambio de color o la desaparición de un elemento. |

### 6.6 — Cómo se verifica

| Criterio | Cómo se verifica | Cuándo |
|---|---|---|
| Contraste de texto y de controles | Medición de cada par de colores con un verificador de contraste, sobre la paleta definitiva. | Al definir la paleta. |
| El color no es el único portador | Revisión de cada pantalla en escala de grises. | Por pantalla, antes de darla por terminada. |
| Escalado al 200 % | Prueba manual en el navegador, verificando que no aparezca desplazamiento horizontal. | Por pantalla. |
| Operación por teclado | Recorrido completo del cobro, P03 → P05 → P06, sin usar el mouse. | Por iteración. |
| Tamaño de los objetivos táctiles | Medición sobre la tablet que efectivamente se usa en el salón. | Al entregar P13. |
| Etiquetas asociadas y orden de foco | Inspección del marcado. | En la implementación. |

**Lo que queda fuera de este documento.** La prueba con lectores de pantalla y la validación automática del marcado requieren la aplicación construida: no pueden resolverse en la etapa de diseño funcional. Quedan registradas como verificación pendiente para la etapa de implementación, junto con la incorporación del RNF14 propuesto al comienzo de esta sección.

---

## 7. Listado de pantallas

| ID | Pantalla | Acceso | Historias | Casos de uso |
|---|---|---|---|---|
| P01 | Login | Todos | HU-20 | CU-00 |
| P02 | Dashboard | Todos | — | — |
| P03 | Gestión de alumnos | Administrador, Recepcionista | HU-09 | — |
| P04 | Alta de alumno | Administrador, Recepcionista | HU-01, HU-06 | CU-01, CU-04, CU-06 |
| P05 | Ficha del alumno | Administrador, Recepcionista | HU-06, HU-07, HU-08, HU-16 | CU-04, CU-05, CU-06 |
| P06 | Registro de pago | Administrador, Recepcionista | HU-02 | CU-02 |
| P07 | Historial de pagos | Administrador, Recepcionista, Alumno | HU-10 | — |
| P08 | Alumnos morosos | Administrador | HU-03 | CU-03 |
| P09 | Gestión de clases y grillas | Administrador | HU-12, HU-13, HU-14 | CU-08, CU-09, CU-10 |
| P10 | Inscripción a clases | Administrador, Recepcionista | HU-15 | CU-11 |
| P11 | Gestión de instructores | Administrador | HU-18 | CU-15 |
| P12 | Agenda del instructor | Instructor | HU-19 | — |
| P13 | Control de asistencia | Instructor, Administrador | HU-04, HU-17 | CU-12 |
| P14 | Consultorio nutricional | Nutricionista | HU-05a | CU-13 |
| P15 | Historial de evolución | Nutricionista | HU-05b | CU-14 |
| P16 | Reportes | Administrador | HU-22, HU-23 | — |
| P17 | Gestión de usuarios y roles | Administrador | HU-21 | CU-16 |
| P18 | Configuración de parámetros | Administrador | HU-03 | CU-03 |
| P19 | Portal del alumno | Alumno | HU-10, HU-16 | — |

**Total: 19 pantallas.**

---

## 8. Pantallas transversales

### P01 — Login

| Campo | Detalle |
|---|---|
| Objetivo | Permitir que un usuario se identifique y acceda al sistema con los permisos de su rol. |
| Acceso | Todos los roles. Es la única pantalla accesible sin sesión iniciada. |
| Historias | HU-20 |
| Casos de uso | CU-00 |
| Requisitos | RF22, RNF03, RNF04, RNF09 |

**Elementos principales**

| Elemento | Tipo | Descripción |
|---|---|---|
| Identificación del centro | Estático | Nombre del sistema y del centro. |
| Nombre de usuario | Campo de texto | Obligatorio. |
| Contraseña | Campo de texto oculto | Obligatorio. |
| Botón Ingresar | Acción principal | Envía las credenciales. |
| Área de mensajes | Dinámico | Muestra el resultado de un intento fallido. |

**Información que se muestra**

- Nombre del sistema y del centro.
- Mensaje de error, si el intento anterior falló.

**Acciones disponibles**

| Acción | Resultado |
|---|---|
| Ingresar | Valida credenciales y redirige al dashboard correspondiente al rol. |
| Presionar Enter en el campo contraseña | Equivale a Ingresar. |

**Validaciones**

| Situación | Comportamiento | Mensaje |
|---|---|---|
| Algún campo vacío | Bloquea el envío. | "Completá tu usuario y tu contraseña para ingresar." |
| Credenciales incorrectas | Bloquea el acceso. No indica cuál de los dos campos falló. | "Usuario o contraseña incorrectos. Verificá los datos e intentá de nuevo." |
| Usuario desactivado | Bloquea el acceso. | "Tu cuenta no está habilitada. Contactá a la administración del centro." |
| Servicio no disponible | Bloquea el acceso. | "No pudimos conectarnos al sistema. Intentá de nuevo en unos minutos." |

**Navegación**

| Desde | Hacia |
|---|---|
| Entrada al sistema, o cierre de sesión desde cualquier pantalla. | P02 Dashboard, con el contenido correspondiente al rol. |

**Observaciones**

- El mensaje de credenciales incorrectas es deliberadamente genérico: distinguir entre usuario inexistente y contraseña errónea permitiría averiguar qué nombres de usuario son válidos.
- No se ofrece registro de cuenta. Los usuarios los crea el Administrador desde P17.

---

### P02 — Dashboard

| Campo | Detalle |
|---|---|
| Objetivo | Presentar a cada usuario la información y los accesos que necesita al comenzar su jornada, según su rol. |
| Acceso | Todos los roles, con contenido diferenciado. |
| Historias | Transversal |
| Casos de uso | — |
| Requisitos | RNF03, RNF07 |

**Elementos principales por rol**

| Rol | Indicadores | Accesos rápidos |
|---|---|---|
| Administrador | Alumnos activos, alumnos morosos, monto adeudado total, ingresos del mes, clases de la grilla activa. | Reporte de morosos, alta de alumno, reportes. |
| Recepcionista | Alumnos activos, cobros registrados hoy, alumnos morosos. | Buscar alumno, alta de alumno, registrar pago. |
| Instructor | Clases asignadas hoy, cantidad de alumnos inscriptos en ellas. | Toma de asistencia de cada clase del día. |
| Nutricionista | Pacientes asignados, consultas registradas en el mes. | Listado de pacientes, nueva consulta. |
| Alumno | Estado de cuenta, próxima clase, asistencias del mes. | Historial de pagos, horarios. |

**Información que se muestra**

- Indicadores numéricos correspondientes al rol, calculados en el momento.
- Listado de accesos rápidos a las operaciones más frecuentes de ese rol.
- Avisos pendientes, cuando corresponda: alumnos con inscripciones en clases inexistentes tras un cambio de grilla, o parámetros sin configurar.

**Acciones disponibles**

| Acción | Rol | Resultado |
|---|---|---|
| Seleccionar un indicador | Administrador, Recepcionista | Abre la pantalla correspondiente con el filtro ya aplicado. |
| Seleccionar una clase del día | Instructor | Abre P13 Control de asistencia para esa clase. |
| Seleccionar un acceso rápido | Todos | Abre la pantalla correspondiente. |

**Validaciones**

No aplica. Es una pantalla de solo lectura.

**Navegación**

| Desde | Hacia |
|---|---|
| P01 tras el ingreso, u opción Inicio del menú. | Cualquier pantalla habilitada para el rol. |

**Observaciones**

- El indicador de morosos del dashboard del Administrador es el mismo dato de P08. Seleccionarlo abre esa pantalla con el filtro predeterminado.
- Para el Instructor, el dashboard es prácticamente su pantalla de trabajo: entra, ve sus clases del día y toca la que va a dictar.

---

## 9. Pantallas de gestión de alumnos

### P03 — Gestión de alumnos

| Campo | Detalle |
|---|---|
| Objetivo | Encontrar rápidamente a un alumno dentro de un padrón de más de doscientos registros y acceder a su ficha. |
| Acceso | Administrador y Recepcionista con acceso total. Instructor con acceso de solo consulta. |
| Historias | HU-09 |
| Casos de uso | — |
| Requisitos | RF24, RNF09 |

**Elementos principales**

| Elemento | Tipo | Descripción |
|---|---|---|
| Campo de búsqueda | Texto | Único campo. Acepta apellido, DNI o número de legajo. |
| Filtro de estado | Selector | Activos, Baja o Todos. Predeterminado: Activos. |
| Filtro de actividad | Selector | Disciplinas activas. |
| Filtro de turno | Selector | Mañana, Tarde-noche o Todos. |
| Tabla de resultados | Listado | Una fila por alumno. |
| Botón Nuevo alumno | Acción principal | Abre P04. |
| Contador de resultados | Estático | Cantidad de alumnos que cumplen los filtros. |

**Información que se muestra**

| Columna | Contenido |
|---|---|
| Legajo | Número correlativo del alumno. |
| Apellido y nombre | |
| DNI | |
| Actividad | Disciplinas en las que participa. |
| Estado | Activo o Baja, con la fecha de baja si corresponde. |
| Situación de cuenta | Al día, Moroso con días de mora, o Sin pagos registrados. |

**Acciones disponibles**

| Acción | Rol | Resultado |
|---|---|---|
| Buscar | Todos los habilitados | Filtra la tabla según el texto ingresado. |
| Aplicar filtros | Todos los habilitados | Actualiza la tabla. |
| Seleccionar una fila | Todos los habilitados | Abre P05 Ficha del alumno. |
| Nuevo alumno | Administrador, Recepcionista | Abre P04 Alta de alumno. |
| Ordenar por columna | Todos los habilitados | Reordena la tabla sin volver a consultar. |

**Validaciones**

| Situación | Comportamiento | Mensaje |
|---|---|---|
| La búsqueda no arroja resultados | Muestra la tabla vacía con un mensaje y un acceso al alta. | "No encontramos ningún alumno con ese dato. Revisá lo que escribiste o registrá un alumno nuevo." |
| Búsqueda con acentos o mayúsculas distintas | No es un error: la búsqueda los ignora. | — |
| El padrón está vacío | Muestra un mensaje inicial. | "Todavía no hay alumnos registrados. Empezá registrando el primero." |

**Navegación**

| Desde | Hacia |
|---|---|
| Menú lateral, opción Alumnos. Dashboard, indicador de alumnos activos. | P04 Alta de alumno, P05 Ficha del alumno. |

**Observaciones**

- Un único campo de búsqueda en lugar de tres separados. La recepcionista escribe lo que tiene a mano y el sistema resuelve si es apellido, DNI o legajo.
- La columna de situación de cuenta evita tener que abrir la ficha para responder la pregunta más frecuente del mostrador.

---

### P04 — Alta de alumno

| Campo | Detalle |
|---|---|
| Objetivo | Registrar un alumno nuevo en el padrón, o editar los datos de uno existente. |
| Acceso | Administrador y Recepcionista. |
| Historias | HU-01, HU-06 |
| Casos de uso | CU-01, CU-04, CU-06 |
| Requisitos | RF01, RF02, RF03, RNF09 |

**Elementos principales**

| Sección | Campos |
|---|---|
| Datos personales | Apellido, nombre, DNI, fecha de nacimiento. |
| Contacto | Teléfono, email. |
| Actividad | Disciplinas en las que participa, modalidad de cobro. |
| Tutor responsable | Apellido, nombre, DNI, teléfono, parentesco. Se habilita solo si el alumno es menor de 18 años. |
| Botones | Guardar, Cancelar. |

**Información que se muestra**

- Formulario vacío en el alta, o con los datos actuales en la edición.
- Monto vigente de la modalidad de cobro seleccionada, a modo informativo.
- En la edición, la fecha de alta y el número de legajo en modo de solo lectura.

**Acciones disponibles**

| Acción | Rol | Resultado |
|---|---|---|
| Guardar | Administrador, Recepcionista | Valida y registra el alumno. Muestra el legajo asignado. |
| Cancelar | Administrador, Recepcionista | Descarta los datos y vuelve a P03. |
| Reactivar | Administrador, Recepcionista | Aparece cuando el DNI corresponde a un alumno dado de baja. Abre el flujo de reactivación. |
| Editar el DNI | Solo Administrador | El campo se muestra deshabilitado para el rol Recepcionista. |

**Validaciones**

| Campo o situación | Regla | Mensaje |
|---|---|---|
| Apellido, nombre, DNI, fecha de nacimiento | Obligatorios. | "Falta completar los datos personales del alumno." |
| Actividad | Al menos una seleccionada. | "Elegí al menos una actividad." |
| Modalidad de cobro | Obligatoria. | "Elegí una modalidad de cobro." |
| DNI | Formato numérico, entre 7 y 8 dígitos. | "El DNI debe tener 7 u 8 números, sin puntos." |
| DNI duplicado en alumno activo | Bloquea el alta. | "El DNI 30.145.876 ya está registrado a nombre de Laura Benítez. Revisá los datos o buscá al alumno en el padrón." |
| DNI de un alumno dado de baja | No bloquea. Ofrece reactivar. | "Este DNI corresponde a Laura Benítez, dada de baja el 12/03/2026. ¿Querés reactivarla en lugar de crear un registro nuevo?" |
| Alumno menor de 18 años | Habilita y exige los datos del tutor. | "Como el alumno es menor de edad, necesitamos los datos del familiar o tutor responsable." |
| Email | Formato válido si se completa. | "Revisá el formato del email." |
| Fecha de nacimiento | No puede ser futura. | "La fecha de nacimiento no puede ser posterior a hoy." |

**Navegación**

| Desde | Hacia |
|---|---|
| P03, botón Nuevo alumno. P05, opción Editar. | P05 Ficha del alumno tras guardar. P03 si se cancela. |

**Observaciones**

- La sección del tutor aparece automáticamente al ingresar una fecha de nacimiento que indique menos de 18 años. No es una casilla que el usuario deba marcar.
- El mensaje de DNI duplicado nombra al alumno existente. Es más útil que un error genérico: muchas veces el alumno ya está cargado con otro apellido.

---

### P05 — Ficha del alumno

| Campo | Detalle |
|---|---|
| Objetivo | Concentrar toda la información de un alumno y servir de punto de acceso a las operaciones que lo involucran. |
| Acceso | Administrador y Recepcionista. |
| Historias | HU-06, HU-07, HU-08, HU-16 |
| Casos de uso | CU-04, CU-05, CU-06 |
| Requisitos | RF03, RF04, RF05, RF10, RF18 |

**Elementos principales**

| Sección | Contenido |
|---|---|
| Encabezado | Apellido y nombre, legajo, DNI, estado y situación de cuenta. |
| Datos personales | Fecha de nacimiento, teléfono, email, fecha de alta. |
| Tutor responsable | Datos del tutor, si el alumno es menor. |
| Actividad y cobro | Disciplinas, modalidad de cobro vigente y monto. |
| Clases inscriptas | Listado de clases con día, horario e instructor. |
| Últimos pagos | Las tres cuotas más recientes. |
| Últimas asistencias | Los cinco registros más recientes. |
| Barra de acciones | Botones de las operaciones disponibles. |

**Información que se muestra**

- Datos completos del alumno.
- Situación de cuenta calculada: al día, o moroso con días de mora y monto adeudado.
- Resumen de clases, pagos y asistencias, con acceso al detalle completo.
- Si el alumno está dado de baja, la fecha y el motivo de la baja.

**Acciones disponibles**

| Acción | Condición | Resultado |
|---|---|---|
| Editar | Alumno activo o de baja | Abre P04 en modo edición. |
| Registrar pago | Solo alumno activo | Abre P06. |
| Ver historial de pagos | Siempre | Abre P07. |
| Inscribir a clase | Solo alumno activo | Abre P10. |
| Ver historial de asistencia | Siempre | Abre el detalle de asistencias. |
| Ver historial de cambios | Siempre | Muestra las modificaciones registradas en el log de auditoría. |
| Dar de baja | Solo alumno activo | Abre el diálogo de baja. |
| Reactivar | Solo alumno de baja | Abre el diálogo de reactivación. |

**Validaciones**

| Situación | Comportamiento | Mensaje |
|---|---|---|
| Baja de un alumno con cuotas impagas | Advierte, no bloquea. | "Este alumno tiene 2 cuotas impagas por 56.000 pesos. Podés continuar con la baja igualmente." |
| Baja sin motivo seleccionado | Bloquea. | "Elegí el motivo de la baja." |
| Fecha de baja futura | Bloquea. | "La fecha de baja no puede ser posterior a hoy." |
| Confirmación de baja | Pide confirmación explícita. | "Vas a dar de baja a Laura Benítez. Sus datos e historial se conservan y podés reactivarla más adelante. ¿Confirmás?" |
| Intento de registrar pago sobre alumno de baja | El botón no está disponible. | Al pasar sobre él: "Para registrar un pago, primero reactivá al alumno." |

**Navegación**

| Desde | Hacia |
|---|---|
| P03, al seleccionar una fila. P04, tras guardar. | P04, P06, P07, P10, y el detalle de asistencias. |

**Observaciones**

- Esta pantalla es el centro operativo del sistema. Desde acá se resuelve casi toda la operación diaria de la recepcionista sin volver al menú.
- Los botones de acciones no disponibles no se ocultan: se muestran deshabilitados con la explicación al pasar por encima. Ocultarlos generaría la sensación de que la función no existe.

---

## 10. Pantallas de cuotas y pagos

### P06 — Registro de pago

| Campo | Detalle |
|---|---|
| Objetivo | Registrar el pago de la cuota de un alumno en la menor cantidad de pasos posible. |
| Acceso | Administrador y Recepcionista. |
| Historias | HU-02 |
| Casos de uso | CU-02 |
| Requisitos | RF06, RF07, RNF08 |

**Elementos principales**

| Elemento | Tipo | Descripción |
|---|---|---|
| Encabezado del alumno | Estático | Apellido, nombre, legajo, modalidad vigente y situación de cuenta. |
| Monto | Numérico | Precargado con el monto de la modalidad del alumno. Editable. |
| Período | Selector de mes y año | Precargado con el mes en curso. No se muestra en la modalidad por clase. |
| Cantidad de clases | Numérico | Solo en la modalidad por clase. |
| Fecha de pago | Fecha | Precargada con el día de hoy. |
| Medio de pago | Selector | Efectivo o Transferencia. |
| Botones | Acción | Confirmar pago, Cancelar. |

**Información que se muestra**

- Datos del alumno y su modalidad de cobro vigente.
- Situación de cuenta antes del pago.
- Monto sugerido según la modalidad.
- Si el alumno adeuda períodos anteriores, el listado de los períodos pendientes.

**Acciones disponibles**

| Acción | Resultado |
|---|---|
| Confirmar pago | Registra la cuota, actualiza la situación de cuenta y muestra confirmación. |
| Modificar el monto sugerido | Se acepta cualquier valor mayor a cero. |
| Seleccionar el período a imputar | Cuando hay deuda de períodos anteriores. |
| Cancelar | Vuelve a P05 sin registrar nada. |

**Validaciones**

| Situación | Comportamiento | Mensaje |
|---|---|---|
| Monto cero o negativo | Bloquea. | "El monto tiene que ser mayor a cero." |
| Alumno dado de baja | La pantalla no es accesible. | "Para registrar un pago, primero reactivá al alumno." |
| Ya existe un pago para ese período | Advierte, permite continuar. | "Ya hay un pago registrado para agosto de 2026 por 28.000 pesos. ¿Querés registrar otro pago para el mismo período?" |
| Fecha de pago futura | Bloquea. | "La fecha del pago no puede ser posterior a hoy." |
| Modalidad por clase sin cantidad | Bloquea. | "Indicá cuántas clases se abonan." |
| Pérdida de conexión al confirmar | Conserva los datos. | "No pudimos guardar el pago porque se perdió la conexión. Los datos siguen cargados, probá de nuevo." |

**Navegación**

| Desde | Hacia |
|---|---|
| P05, botón Registrar pago. Dashboard de Recepcionista, acceso rápido. | P05 con la situación de cuenta actualizada, o P07 Historial de pagos. |

**Cumplimiento de RNF08**

El requisito limita el flujo a cuatro pasos desde la búsqueda del alumno hasta la confirmación. El recorrido es:

| Paso | Acción |
|---|---|
| 1 | Buscar al alumno en P03. |
| 2 | Seleccionarlo y abrir P05. |
| 3 | Presionar Registrar pago y abrir P06. |
| 4 | Confirmar, con los campos precargados. |

En el caso más frecuente, alumno con modalidad mensual que paga el mes en curso, el paso 4 no requiere modificar ningún campo.

---

### P07 — Historial de pagos

| Campo | Detalle |
|---|---|
| Objetivo | Consultar todos los pagos registrados de un alumno y su situación de cuenta. |
| Acceso | Administrador y Recepcionista sobre cualquier alumno. Alumno sobre sus propios pagos. |
| Historias | HU-10 |
| Casos de uso | — |
| Requisitos | RF10 |

**Elementos principales**

| Elemento | Tipo | Descripción |
|---|---|---|
| Encabezado del alumno | Estático | Apellido, nombre, legajo y situación de cuenta. |
| Filtro por año | Selector | Predeterminado: año en curso. |
| Tabla de pagos | Listado | Un registro por cuota. |
| Totalizador | Estático | Suma de lo abonado en el período consultado. |

**Información que se muestra**

| Columna | Contenido |
|---|---|
| Fecha de pago | |
| Período | Mes y año abonado, o cantidad de clases. |
| Monto | Monto efectivamente cobrado. |
| Modalidad | Modalidad aplicada en ese cobro. |
| Medio de pago | Efectivo o Transferencia. |
| Registrado por | Usuario que cargó el pago. |

**Acciones disponibles**

| Acción | Rol | Resultado |
|---|---|---|
| Filtrar por año | Todos los habilitados | Actualiza la tabla y el totalizador. |
| Registrar pago | Administrador, Recepcionista | Abre P06. |
| Ordenar por fecha o por monto | Todos los habilitados | Reordena la tabla. |

**Validaciones**

| Situación | Comportamiento | Mensaje |
|---|---|---|
| El alumno no registra pagos | Muestra la tabla vacía con contexto. | "Este alumno todavía no registra pagos. Está dado de alta desde el 04/03/2026." |
| No hay pagos en el año filtrado | Muestra la tabla vacía. | "No hay pagos registrados en 2025 para este alumno." |
| El rol Alumno intenta ver otro historial | No es posible: solo accede al propio. | — |

**Navegación**

| Desde | Hacia |
|---|---|
| P05, opción Ver historial de pagos. P19 para el rol Alumno. | P06 Registro de pago. |

**Observaciones**

- La columna "Registrado por" cumple con la trazabilidad que exige RNF05 y resuelve consultas del tipo "quién cobró esto".
- La modalidad se muestra por cuota y no del alumno, porque una cuota conserva la modalidad con la que fue cobrada. Un alumno que cambió de mensual a combinada verá ambas en su historial.

---

### P08 — Alumnos morosos

| Campo | Detalle |
|---|---|
| Objetivo | Dar a la dirección el listado de alumnos con cuota vencida para poder accionar la cobranza. |
| Acceso | Administrador con acceso total. Recepcionista con acceso de solo consulta. |
| Historias | HU-03 |
| Casos de uso | CU-03 |
| Requisitos | RF08, RF09, RF30 |

**Elementos principales**

| Elemento | Tipo | Descripción |
|---|---|---|
| Encabezado de resumen | Estático | Cantidad de morosos y monto total adeudado. |
| Filtro de actividad | Selector | Disciplinas activas. |
| Filtro de turno | Selector | Mañana, Tarde-noche o Todos. |
| Umbral de días | Numérico | Muestra el valor configurado. Editable solo por el Administrador. |
| Tabla de morosos | Listado | Un registro por alumno. |
| Botón Exportar PDF | Acción | Genera el archivo con el listado filtrado. |

**Información que se muestra**

| Columna | Contenido |
|---|---|
| Legajo | |
| Apellido y nombre | |
| Actividad | |
| Turno | |
| Último pago | Fecha de la última cuota registrada. |
| Días de mora | Calculado a la fecha de consulta. |
| Monto adeudado | Períodos vencidos por el monto de la modalidad vigente. |
| Teléfono | Para facilitar el contacto. |

**Acciones disponibles**

| Acción | Rol | Resultado |
|---|---|---|
| Aplicar filtros | Administrador, Recepcionista | Actualiza el listado y el resumen. |
| Ordenar por días de mora o monto | Administrador, Recepcionista | Reordena el listado. |
| Seleccionar un alumno | Administrador, Recepcionista | Abre P05 Ficha del alumno. |
| Exportar PDF | Administrador | Genera el archivo con los filtros aplicados. |
| Modificar el umbral de días | Solo Administrador | Recalcula el listado y guarda el nuevo valor predeterminado. |

**Validaciones**

| Situación | Comportamiento | Mensaje |
|---|---|---|
| No hay morosos con los filtros aplicados | Muestra el mensaje y desactiva la exportación. | "Sin alumnos morosos al día de hoy." |
| Falla la generación del PDF | El listado sigue disponible en pantalla. | "No pudimos generar el PDF. El listado sigue disponible acá, probá exportarlo de nuevo." |
| Umbral cero o negativo | Bloquea. | "El umbral tiene que ser un número mayor a cero." |

**Navegación**

| Desde | Hacia |
|---|---|
| Menú lateral, sección Reportes, para Administrador y Recepcionista. Dashboard del Administrador, indicador de morosos. | P05 Ficha del alumno, P18 Configuración de parámetros. |

**Observaciones**

- Los alumnos con modalidad de cobro por clase no figuran en este listado, según la regla RN25 y la decisión D12. Su seguimiento se realiza desde el reporte de ocupación de P16, bajo la categoría de inactivos.
- Los alumnos dados de baja no figuran, aun cuando registren deuda.
- La columna de teléfono está presente porque el uso real del reporte es llamar a cada alumno. Sin ella, la directora tendría que abrir la ficha de cada uno.

---

## 11. Pantallas de planificación

### P09 — Gestión de clases y grillas

| Campo | Detalle |
|---|---|
| Objetivo | Armar y mantener la planificación de actividades del centro, incluyendo la gestión de versiones de grilla y de disciplinas. |
| Acceso | Administrador con acceso total. Recepcionista con acceso de solo consulta. |
| Historias | HU-12, HU-13, HU-14 |
| Casos de uso | CU-08, CU-09, CU-10 |
| Requisitos | RF11, RF12, RF13 |

**Elementos principales**

| Elemento | Tipo | Descripción |
|---|---|---|
| Selector de grilla | Selector | Grilla activa por defecto. Permite consultar otras versiones. |
| Indicador de estado de la grilla | Estático | Activa o Solo lectura. |
| Vista de calendario semanal | Tabla | Filas por franja horaria, columnas por día. Cada celda es una clase. |
| Filtro de turno | Selector | Mañana, Tarde-noche o Ambos. |
| Botón Nueva clase | Acción | Abre el formulario de clase. |
| Botón Nueva grilla | Acción | Crea una grilla copiando la activa o vacía. |
| Botón Activar grilla | Acción | Disponible solo sobre grillas no activas. |
| Acceso a Disciplinas | Enlace | Abre el mantenimiento del catálogo de disciplinas. |

**Información que se muestra**

- Grilla completa organizada por día y franja horaria.
- En cada celda: disciplina, horario, instructor y cantidad de inscriptos.
- Nombre y vigencia de la grilla consultada.
- Cantidad total de clases de la grilla.

**Acciones disponibles**

| Acción | Rol | Resultado |
|---|---|---|
| Crear una clase | Administrador | Abre el formulario con disciplina, día, horario, turno e instructor. |
| Editar una clase | Administrador | Abre el formulario con los datos cargados. |
| Desactivar una clase | Administrador | Pide confirmación e informa cuántos inscriptos quedan afectados. |
| Crear una grilla nueva | Administrador | Solicita nombre y vigencia. Copia las clases de la activa o crea vacía. |
| Activar una grilla | Administrador | Advierte los efectos y solicita confirmación. |
| Consultar una grilla anterior | Administrador, Recepcionista | Muestra la grilla en modo de solo lectura. |
| Gestionar disciplinas | Administrador | Abre el catálogo de disciplinas. |

**Validaciones**

| Situación | Comportamiento | Mensaje |
|---|---|---|
| Instructor con clase superpuesta | Bloquea. | "Martín López ya tiene Funcional los lunes de 18:00 a 19:00. Elegí otro horario o asigná otro instructor." |
| Hora de fin anterior o igual a la de inicio | Bloquea. | "La hora de fin tiene que ser posterior a la de inicio." |
| Disciplina infantil en turno mañana | Bloquea. | "Las disciplinas para niños se dictan solo en el turno tarde." |
| Desactivar una clase con inscriptos | Advierte, permite continuar. | "Esta clase tiene 12 alumnos inscriptos. Si la desactivás, van a quedar sin esa clase asignada. ¿Confirmás?" |
| Activar una grilla con inscripciones huérfanas | Advierte, permite continuar y lista los casos. | "Al activar la grilla de agosto, 7 alumnos quedan inscriptos en clases que no existen en la nueva grilla. Vas a poder verlos en un listado para reasignarlos. ¿Confirmás?" |
| Editar una grilla vencida | Bloquea. | "Esta grilla ya venció y es de solo lectura." |
| Eliminar una disciplina con clases | Bloquea, ofrece desactivar. | "Esta disciplina se usa en 6 clases. No se puede eliminar, pero podés desactivarla para que no aparezca al crear clases nuevas." |

**Navegación**

| Desde | Hacia |
|---|---|
| Menú lateral, sección Clases. | Formulario de clase, catálogo de disciplinas, P10 Inscripción a clases. |

**Observaciones**

- La vista de calendario semanal reproduce la forma en que la dirección piensa la grilla. Un listado plano de clases obligaría a reconstruir mentalmente la organización horaria.
- El selector de grilla permite comparar la temporada anterior con la actual sin salir de la pantalla, que es la necesidad que hoy resuelven con dos archivos de Excel abiertos.

---

### P10 — Inscripción a clases

| Campo | Detalle |
|---|---|
| Objetivo | Inscribir a un alumno activo en las clases que va a cursar. |
| Acceso | Administrador y Recepcionista. |
| Historias | HU-15 |
| Casos de uso | CU-11 |
| Requisitos | RF14 |

**Elementos principales**

| Elemento | Tipo | Descripción |
|---|---|---|
| Encabezado del alumno | Estático | Apellido, nombre, legajo, estado y actividades declaradas. |
| Clases actuales | Listado | Clases en las que ya está inscripto, con opción de dar de baja. |
| Selector de clases | Tabla con casillas | Clases de la grilla activa, agrupadas por día y turno. |
| Filtro de disciplina | Selector | Facilita encontrar la clase buscada. |
| Botones | Acción | Confirmar inscripción, Cancelar. |

**Información que se muestra**

- Clases disponibles de la grilla activa: disciplina, día, horario, instructor e inscriptos actuales.
- Clases en las que el alumno ya participa.
- Indicación visual de las clases que se superponen con otra ya seleccionada.

**Acciones disponibles**

| Acción | Resultado |
|---|---|
| Seleccionar una o varias clases | Marca las clases a inscribir. |
| Confirmar inscripción | Registra las inscripciones y vuelve a P05. |
| Dar de baja una inscripción | Pide confirmación y desactiva la inscripción, conservando las asistencias. |
| Filtrar por disciplina | Reduce el listado de clases disponibles. |

**Validaciones**

| Situación | Comportamiento | Mensaje |
|---|---|---|
| Alumno dado de baja | La pantalla no es accesible. | "Para inscribir a este alumno, primero reactivalo." |
| Alumno ya inscripto en la clase | La casilla aparece marcada y deshabilitada. | "Ya está inscripto en esta clase." |
| Clases superpuestas | Advierte, permite continuar. | "Yoga y Pilates de los martes se superponen de 10:00 a 11:00. Podés inscribirlo en las dos igualmente." |
| Ninguna clase seleccionada | Bloquea la confirmación. | "Elegí al menos una clase para inscribir." |
| Baja de una inscripción | Pide confirmación. | "Vas a dar de baja la inscripción a Funcional de los lunes. Las asistencias ya registradas se conservan. ¿Confirmás?" |

**Navegación**

| Desde | Hacia |
|---|---|
| P05, opción Inscribir a clase. P09, desde una clase. | P05 Ficha del alumno tras confirmar. |

**Observaciones**

- Solo se ofrecen clases de la grilla activa, conforme a la regla RN13. Inscribir en una grilla futura dejaría al alumno anotado en un horario que todavía no rige.
- La superposición se advierte pero no se bloquea. La dirección aclaró que hay alumnos que se anotan en dos clases del mismo horario para elegir según el día.

---

## 12. Pantallas de instructores

### P11 — Gestión de instructores

| Campo | Detalle |
|---|---|
| Objetivo | Administrar el plantel de instructores del centro y sus especialidades. |
| Acceso | Administrador. |
| Historias | HU-18 |
| Casos de uso | CU-15 |
| Requisitos | RF25 |

**Elementos principales**

| Elemento | Tipo | Descripción |
|---|---|---|
| Filtro de estado | Selector | Activos, Inactivos o Todos. |
| Tabla de instructores | Listado | Un registro por instructor. |
| Botón Nuevo instructor | Acción | Abre el formulario de alta. |
| Formulario de instructor | Formulario | Apellido, nombre, DNI, teléfono, email y especialidades. |

**Información que se muestra**

| Columna | Contenido |
|---|---|
| Apellido y nombre | |
| DNI | |
| Especialidades | Disciplinas que dicta. |
| Clases asignadas | Cantidad en la grilla activa. |
| Estado | Activo o Inactivo. |
| Teléfono | |

**Acciones disponibles**

| Acción | Resultado |
|---|---|
| Crear instructor | Registra el instructor y lo habilita para asignarse a clases. |
| Editar instructor | Actualiza sus datos y especialidades. |
| Dar de baja | Cambia el estado a Inactivo, previa verificación de clases asignadas. |
| Reactivar | Devuelve el instructor a estado Activo. |
| Ver clases asignadas | Muestra la agenda del instructor en la grilla activa. |

**Validaciones**

| Situación | Comportamiento | Mensaje |
|---|---|---|
| DNI duplicado entre instructores | Bloquea. | "Ese DNI ya está registrado a nombre de Lucas Fernández." |
| Baja con clases asignadas en la grilla activa | Bloquea hasta reasignar. | "Martín López tiene 5 clases asignadas en la grilla activa. Reasignalas a otro instructor antes de darlo de baja." |
| Campos obligatorios vacíos | Bloquea. | "Falta completar apellido, nombre y DNI." |
| Ninguna especialidad seleccionada | Advierte, permite continuar. | "No cargaste ninguna especialidad. Vas a poder asignarlo a clases igualmente." |

**Navegación**

| Desde | Hacia |
|---|---|
| Menú lateral, sección Instructores. | Formulario de instructor, agenda del instructor. |

**Observaciones**

- La validación de baja es más estricta que la de alumnos: dar de baja a un instructor deja clases sin dictar, así que el sistema bloquea hasta que se resuelva la reasignación. Es la diferencia entre P11 y P05.
- Las especialidades son informativas y no restringen la asignación a clases. Se acordó así para no bloquear reemplazos ocasionales entre profesores.

---

### P12 — Agenda del instructor

| Campo | Detalle |
|---|---|
| Objetivo | Que cada instructor consulte sus clases sin depender de recepción, y acceda desde ahí a la toma de asistencia. |
| Acceso | Instructor sobre sus propias clases. Administrador sobre cualquier instructor. |
| Historias | HU-19 |
| Casos de uso | — |
| Requisitos | RF26 |

**Elementos principales**

| Elemento | Tipo | Descripción |
|---|---|---|
| Encabezado | Estático | Nombre del instructor y grilla vigente. |
| Vista semanal | Tabla | Clases organizadas por día y horario. |
| Destacado del día | Visual | Las clases del día en curso aparecen resaltadas. |
| Botón Tomar asistencia | Acción | Disponible solo en las clases del día en curso. |

**Información que se muestra**

| Dato | Contenido |
|---|---|
| Disciplina | |
| Día y horario | |
| Turno | |
| Alumnos inscriptos | Cantidad. |
| Estado de la asistencia | Registrada o Pendiente, para las clases del día. |

**Acciones disponibles**

| Acción | Condición | Resultado |
|---|---|---|
| Tomar asistencia | Solo clases del día en curso | Abre P13 con la clase seleccionada. |
| Ver el listado de inscriptos | Cualquier clase propia | Muestra los alumnos inscriptos. |
| Cambiar de semana | Siempre | Navega la agenda hacia adelante o hacia atrás. |

**Validaciones**

| Situación | Comportamiento | Mensaje |
|---|---|---|
| El instructor no tiene clases en la grilla activa | Muestra la agenda vacía. | "No tenés clases asignadas en la grilla vigente. Consultá con la administración del centro." |
| No hay clases el día consultado | Muestra el día vacío. | "No tenés clases programadas para este día." |
| Intento de acceder a la clase de otro instructor | Deniega el acceso y lo registra. | "No tenés permiso para acceder a esta clase." |

**Navegación**

| Desde | Hacia |
|---|---|
| Dashboard del Instructor. Menú lateral, opción Mi agenda. | P13 Control de asistencia. |

**Observaciones**

- Esta pantalla es la entrada natural del instructor al sistema: abre la aplicación, ve sus clases del día y toca la que va a dictar.
- El estado de asistencia Registrada o Pendiente evita la duda de si ya se cargó la lista, situación frecuente cuando dos clases se dictan seguidas.

---

## 13. Pantallas de asistencia

### P13 — Control de asistencia

| Campo | Detalle |
|---|---|
| Objetivo | Que el instructor registre la asistencia de su clase desde el salón, con la menor cantidad de toques posible. |
| Acceso | Instructor sobre sus propias clases del día. Administrador sobre cualquier clase. |
| Historias | HU-04, HU-17 |
| Casos de uso | CU-12 |
| Requisitos | RF15, RF16, RF17, RF27, RNF07 |

**Elementos principales**

| Elemento | Tipo | Descripción |
|---|---|---|
| Encabezado de la clase | Estático | Disciplina, día, horario y fecha. |
| Contador | Estático | Presentes, ausentes y justificados marcados hasta el momento. |
| Listado de alumnos | Tabla táctil | Un alumno por fila, con tres botones de estado. |
| Botón Marcar todos presentes | Acción | Aplica el estado Presente a todo el listado. |
| Buscador de alumno | Campo de texto | Para agregar un asistente ocasional. |
| Botón Confirmar | Acción principal | Guarda la asistencia de toda la clase. |

**Información que se muestra**

- Datos de la clase y fecha del registro.
- Listado alfabético de alumnos inscriptos, con su estado sin definir al abrir.
- Indicación visual de los alumnos agregados como ocasionales.
- Contador en vivo de presentes, ausentes y justificados.

**Acciones disponibles**

| Acción | Resultado |
|---|---|
| Marcar Presente, Ausente o Justificado | Aplica el estado al alumno en la vista, sin guardar todavía. |
| Marcar todos presentes | Aplica Presente a todo el listado. El instructor corrige las excepciones. |
| Agregar asistente ocasional | Busca un alumno activo y lo incorpora al listado, marcado como ocasional. |
| Confirmar | Guarda la asistencia de toda la clase con fecha y hora. |
| Corregir un registro ya guardado | Vuelve a abrir la clase con los valores cargados y permite modificarlos. |

**Validaciones**

| Situación | Comportamiento | Mensaje |
|---|---|---|
| El instructor no tiene clases hoy | No muestra listado. | "No tenés clases programadas para hoy." |
| La clase no tiene inscriptos | Muestra el listado vacío con la opción de agregar ocasionales. | "Esta clase todavía no tiene alumnos inscriptos. Podés agregar asistentes ocasionales." |
| Alumno buscado como ocasional está de baja | Bloquea. | "Laura Benítez está dada de baja. Consultá en recepción antes de agregarla." |
| Asistencia ya registrada para esa clase y fecha | Muestra los valores cargados. | "La asistencia de esta clase ya fue registrada a las 18:42. Podés corregirla si hace falta." |
| Confirmar con alumnos sin estado asignado | Advierte, permite continuar. | "Quedaron 3 alumnos sin marcar. Si confirmás, van a registrarse como ausentes." |
| Pérdida de conexión al confirmar | Conserva las marcas en pantalla. | "No pudimos guardar por un problema de conexión. Tus marcas siguen acá, probá de nuevo." |

**Navegación**

| Desde | Hacia |
|---|---|
| P12 Agenda del instructor. Dashboard del Instructor. | P12 tras confirmar. |

**Observaciones**

- Es la pantalla más condicionada por su contexto de uso. La restricción RE05 establece que se opera desde una tablet, de pie y con poco tiempo. De ahí los botones grandes, el marcado masivo y el contador visible.
- El botón Marcar todos presentes responde al caso más frecuente: en una clase de doce alumnos suele faltar uno o dos. Marcar todos y corregir las excepciones son dos toques en lugar de doce.
- La confirmación es una sola operación para toda la clase, no un guardado por alumno. Con conectividad inestable, doce guardados individuales multiplican las probabilidades de falla.

---

## 14. Pantallas del módulo nutricional

### P14 — Consultorio nutricional

| Campo | Detalle |
|---|---|
| Objetivo | Que la nutricionista acceda a sus pacientes y registre las consultas del día. |
| Acceso | Exclusivo del rol Nutricionista. |
| Historias | HU-05a |
| Casos de uso | CU-13 |
| Requisitos | RF19, RF21, RNF06 |

**Elementos principales**

| Elemento | Tipo | Descripción |
|---|---|---|
| Listado de pacientes | Tabla | Pacientes asignados, con la fecha de su última consulta. |
| Buscador | Campo de texto | Búsqueda por apellido dentro de sus propios pacientes. |
| Ficha del paciente | Panel | Datos básicos y última consulta registrada. |
| Formulario de consulta | Formulario | Fecha, peso, altura, perímetros, masa grasa, objetivo y observaciones. |
| Indicador de IMC | Calculado | Se actualiza al ingresar peso y altura. |
| Botones | Acción | Guardar consulta, Ver historial, Cancelar. |

**Información que se muestra**

- Listado de pacientes asignados con fecha de última consulta y días transcurridos.
- Al abrir un paciente: nombre, edad, fecha de alta como paciente y valores de la consulta anterior como referencia.
- Índice de masa corporal calculado en el momento.
- Si el paciente es menor de edad: datos del tutor y fecha de autorización.

**Acciones disponibles**

| Acción | Resultado |
|---|---|
| Seleccionar un paciente | Abre su ficha con la última consulta registrada. |
| Registrar nueva consulta | Abre el formulario con la fecha del día y la altura precargada. |
| Guardar consulta | Registra la consulta e incorpora el registro al historial. |
| Ver historial de evolución | Abre P15. |
| Buscar paciente | Filtra el listado. |

**Validaciones**

| Situación | Comportamiento | Mensaje |
|---|---|---|
| Fecha, peso u objetivo sin completar | Bloquea. | "Falta completar la fecha, el peso o el objetivo de la consulta." |
| Primera consulta sin altura | Bloquea. | "Necesitamos la altura para calcular el índice de masa corporal." |
| Peso o altura fuera de rango razonable | Pide confirmación. | "Cargaste 152 kg. ¿Es correcto o hubo un error de tipeo?" |
| Paciente menor sin autorización del tutor | Bloquea la primera consulta. | "Para registrar la consulta de un paciente menor de edad necesitamos la autorización del tutor. Solicitala en administración." |
| Alumno dado de baja | Bloquea. | "Este alumno está dado de baja en el centro. No se pueden registrar consultas nuevas." |
| Otro rol intenta acceder | Deniega el acceso y lo registra en el log. | "No tenés permiso para acceder a esta sección." |

**Navegación**

| Desde | Hacia |
|---|---|
| Dashboard de la Nutricionista. Menú lateral, sección Consultorio. | P15 Historial de evolución. |

**Observaciones**

- Es el único módulo con acceso exclusivo de un rol. Ni la recepcionista ni los instructores lo ven en el menú, y el intento de acceso por dirección directa queda registrado en el log de auditoría.
- El Administrador puede asignar pacientes a la nutricionista desde P17, pero no accede a esta pantalla ni al contenido de las consultas, según la regla RN29.
- La validación de rango razonable se incorporó a pedido de la nutricionista: un error de tipeo en el peso distorsiona toda la curva de evolución del paciente.

---

### P15 — Historial de evolución

| Campo | Detalle |
|---|---|
| Objetivo | Comparar los valores de un paciente entre consultas para ajustar el plan alimentario. |
| Acceso | Exclusivo del rol Nutricionista. |
| Historias | HU-05b |
| Casos de uso | CU-14 |
| Requisitos | RF20, RF21, RNF06 |

**Elementos principales**

| Elemento | Tipo | Descripción |
|---|---|---|
| Encabezado del paciente | Estático | Nombre, edad y cantidad de consultas registradas. |
| Filtro por período | Selector de fechas | Rango de consultas a mostrar. |
| Tabla de consultas | Listado | Un registro por consulta, del más reciente al más antiguo. |
| Columna de variación | Calculado | Diferencia respecto de la consulta anterior y de la primera. |
| Resumen del período | Estático | Variación total de peso y de IMC. |

**Información que se muestra**

| Columna | Contenido |
|---|---|
| Fecha | |
| Peso | En kilogramos. |
| IMC | Calculado. |
| Variación respecto de la anterior | Diferencia de peso e IMC. |
| Variación respecto de la primera | Diferencia acumulada. |
| Perímetros | Cintura y cadera, si fueron cargados. |
| Masa grasa | Porcentaje, si fue cargado. |
| Objetivo | Objetivo declarado en esa consulta. |
| Observaciones | |

**Acciones disponibles**

| Acción | Resultado |
|---|---|
| Filtrar por período | Actualiza la tabla y recalcula las variaciones sobre el rango. |
| Registrar nueva consulta | Abre el formulario de P14. |
| Volver al listado de pacientes | Regresa a P14. |

**Validaciones**

| Situación | Comportamiento | Mensaje |
|---|---|---|
| El paciente tiene una sola consulta | Oculta las columnas de variación. | "Esta es la primera consulta registrada. Cuando cargues la siguiente vas a poder ver la evolución." |
| El paciente no tiene consultas | Ofrece registrar la primera. | "Este paciente todavía no tiene consultas registradas." |
| No hay consultas en el período filtrado | Muestra la tabla vacía. | "No hay consultas registradas en el período seleccionado." |
| El rol Alumno intenta acceder | La opción no figura en su menú. | — |

**Navegación**

| Desde | Hacia |
|---|---|
| P14, opción Ver historial. | P14 Consultorio nutricional. |

**Observaciones**

- El alumno no accede a esta pantalla, según la regla RN28 y la decisión D13. La devolución de resultados se realiza en el consultorio, a criterio de la nutricionista.
- El equipo evaluó incluir un gráfico de evolución y lo dejó fuera de la primera versión. La tabla con las variaciones cubre la necesidad clínica; el gráfico es una mejora posterior.

---

## 15. Pantallas de gestión y configuración

### P16 — Reportes

| Campo | Detalle |
|---|---|
| Objetivo | Dar a la dirección información agregada sobre ingresos y ocupación para tomar decisiones comerciales. |
| Acceso | Administrador. |
| Historias | HU-22, HU-23 |
| Casos de uso | — |
| Requisitos | RF28, RF29 |

**Elementos principales**

| Elemento | Tipo | Descripción |
|---|---|---|
| Selector de reporte | Pestañas | Morosidad, Ingresos, Ocupación de clases. |
| Selector de período | Fechas | Desde y hasta. Predeterminado: mes en curso. |
| Área de resultados | Tabla | Contenido según el reporte seleccionado. |
| Comparación con el período anterior | Estático | Solo en el reporte de ingresos. |
| Botón Exportar PDF | Acción | Genera el archivo del reporte consultado. |

**Información que se muestra**

*Reporte de ingresos*

| Dato | Contenido |
|---|---|
| Total recaudado | Suma del período. |
| Cantidad de pagos | Registros del período. |
| Detalle por modalidad | Mensual fija, por clase y combinada. |
| Detalle por disciplina | Recaudación imputada a cada actividad. |
| Comparación | Variación respecto del período anterior de igual duración. |

*Reporte de ocupación*

| Columna | Contenido |
|---|---|
| Clase | Disciplina, día y horario. |
| Instructor | |
| Inscriptos | Cantidad de inscripciones activas. |
| Asistencia promedio | Porcentaje del período. |
| Ocasionales | Cantidad de asistencias de alumnos no inscriptos. |

Se incluye además un listado de alumnos inactivos: aquellos con modalidad por clase sin pagos ni asistencias en los últimos sesenta días.

**Acciones disponibles**

| Acción | Resultado |
|---|---|
| Cambiar de reporte | Actualiza el área de resultados. |
| Seleccionar período | Recalcula el reporte. |
| Ordenar por columna | Reordena el resultado. |
| Exportar PDF | Genera el archivo con los filtros aplicados. |
| Seleccionar una clase | Abre el detalle de esa clase en P09. |

**Validaciones**

| Situación | Comportamiento | Mensaje |
|---|---|---|
| Fecha desde posterior a fecha hasta | Bloquea. | "La fecha inicial tiene que ser anterior a la final." |
| Sin pagos en el período | Muestra el reporte vacío y desactiva la exportación. | "No hay pagos registrados entre el 01/07/2026 y el 31/07/2026." |
| Grilla activa sin clases | Muestra el reporte vacío. | "La grilla vigente todavía no tiene clases cargadas." |
| Falla la generación del PDF | El reporte sigue en pantalla. | "No pudimos generar el PDF. El reporte sigue disponible acá, probá de nuevo." |

**Navegación**

| Desde | Hacia |
|---|---|
| Menú lateral, sección Reportes. Dashboard del Administrador. | P05 Ficha del alumno, P09 Gestión de clases. |

**Observaciones**

- El reporte de ingresos se calcula sobre el monto efectivamente cobrado en cada cuota, no sobre el monto vigente de la modalidad. Un aumento de precios no debe distorsionar retroactivamente los períodos anteriores.
- El listado de alumnos inactivos es la contrapartida de la decisión D12: como los alumnos con modalidad por clase no aparecen en el reporte de morosos, este reporte es el que los hace visibles para la dirección.
- La columna de ocasionales del reporte de ocupación permite detectar clases con demanda real superior a la cantidad de inscriptos.

---

### P17 — Gestión de usuarios y roles

| Campo | Detalle |
|---|---|
| Objetivo | Administrar las cuentas de acceso al sistema, sus roles y consultar el registro de auditoría. |
| Acceso | Administrador. |
| Historias | HU-21 |
| Casos de uso | CU-16 |
| Requisitos | RF23, RF31, RNF04, RNF05 |

**Elementos principales**

| Elemento | Tipo | Descripción |
|---|---|---|
| Pestañas | Navegación | Usuarios, Asignación de pacientes, Auditoría. |
| Tabla de usuarios | Listado | Un registro por cuenta. |
| Formulario de usuario | Formulario | Nombre, nombre de usuario, rol, contraseña inicial, persona vinculada. |
| Filtros de auditoría | Selectores | Usuario, tipo de operación, rango de fechas. |
| Tabla de auditoría | Listado | Registro de operaciones sensibles. |

**Información que se muestra**

*Pestaña Usuarios*

| Columna | Contenido |
|---|---|
| Nombre de usuario | |
| Persona vinculada | Alumno, instructor o nutricionista asociado, si corresponde. |
| Rol | |
| Estado | Activo o Inactivo. |
| Último acceso | Fecha y hora. |

*Pestaña Auditoría*

| Columna | Contenido |
|---|---|
| Fecha y hora | |
| Usuario | Quien ejecutó la operación. |
| Operación | Alta, modificación, baja, reactivación, pago o acceso denegado. |
| Entidad afectada | |
| Detalle | Valor anterior y valor nuevo, en las modificaciones. |

**Acciones disponibles**

| Acción | Resultado |
|---|---|
| Crear usuario | Registra la cuenta con su rol y contraseña inicial. |
| Modificar rol | Cambia los permisos del usuario y registra el cambio. |
| Desactivar usuario | Impide el inicio de sesión y conserva su historial. |
| Restablecer contraseña | Genera una contraseña nueva que el usuario debe cambiar en su próximo acceso. |
| Asignar pacientes a la nutricionista | Vincula alumnos al consultorio, sin acceder al contenido de las consultas. |
| Consultar auditoría | Filtra el registro de operaciones. |

**Validaciones**

| Situación | Comportamiento | Mensaje |
|---|---|---|
| Nombre de usuario duplicado | Bloquea. | "Ese nombre de usuario ya existe. Elegí otro." |
| Desactivar al último Administrador | Bloquea. | "Tiene que quedar al menos un administrador activo en el sistema." |
| Rol Instructor sin instructor vinculado | Advierte. | "Este usuario tiene rol Instructor pero no está vinculado a ningún instructor. No va a poder ver su agenda." |
| Contraseña demasiado corta | Bloquea. | "La contraseña tiene que tener al menos 8 caracteres." |
| Intento de modificar un registro de auditoría | La acción no existe en la interfaz. | — |
| Asignar como paciente a un alumno de baja | Bloquea. | "Solo se pueden asignar al consultorio alumnos activos." |

**Navegación**

| Desde | Hacia |
|---|---|
| Menú lateral, sección Configuración. | Formulario de usuario, P18 Configuración de parámetros. |

**Observaciones**

- La contraseña nunca se muestra, ni siquiera al Administrador que la genera. Se comunica al usuario por fuera del sistema y debe cambiarse en el primer acceso.
- La pestaña de asignación de pacientes materializa la decisión D10: el Administrador vincula alumnos al consultorio, pero no ve ninguna consulta.
- El registro de auditoría es de solo lectura. La interfaz no ofrece ninguna acción de edición ni de borrado, conforme a la regla RN23.

---

### P18 — Configuración de parámetros

| Campo | Detalle |
|---|---|
| Objetivo | Permitir a la dirección ajustar los valores de negocio del sistema sin intervención técnica. |
| Acceso | Administrador. |
| Historias | HU-03 |
| Casos de uso | CU-03 |
| Requisitos | RF30 |

**Elementos principales**

| Elemento | Tipo | Descripción |
|---|---|---|
| Tabla de parámetros | Listado editable | Un registro por parámetro configurable. |
| Descripción de cada parámetro | Estático | Explica en lenguaje claro qué afecta cada valor. |
| Botón Guardar | Acción | Aplica los cambios. |
| Historial de cambios | Listado | Últimas modificaciones de cada parámetro. |

**Información que se muestra**

| Parámetro | Valor inicial | Qué afecta |
|---|---|---|
| Días para considerar moroso | 30 | Reporte de morosos y situación de cuenta. |
| Días para considerar inactivo | 60 | Listado de inactivos del reporte de ocupación. |
| Mínimo de inscriptos por clase | 3 | Clases destacadas en el reporte de ocupación. |
| Semanas sin asistir para alertar | 2 | Alerta en el historial de asistencia. |
| Longitud mínima de contraseña | 8 | Validación de P17. |

Cada parámetro muestra además quién lo modificó por última vez y cuándo.

**Acciones disponibles**

| Acción | Resultado |
|---|---|
| Modificar un valor | Habilita el guardado. |
| Guardar | Aplica el nuevo valor y registra el cambio en el log de auditoría. |
| Restaurar valor inicial | Devuelve el parámetro a su valor por defecto. |
| Ver historial de cambios | Muestra las modificaciones anteriores. |

**Validaciones**

| Situación | Comportamiento | Mensaje |
|---|---|---|
| Valor cero o negativo | Bloquea. | "El valor tiene que ser un número mayor a cero." |
| Valor no numérico | Bloquea. | "Ingresá un número." |
| Cambio del umbral de mora | Advierte el impacto antes de guardar. | "Si bajás el umbral a 15 días, van a pasar a figurar como morosos 24 alumnos más. ¿Confirmás?" |
| Días de inactividad menores que los de mora | Advierte, permite continuar. | "El plazo de inactividad es menor que el de mora. Revisá si es lo que querés." |

**Navegación**

| Desde | Hacia |
|---|---|
| Menú lateral, sección Configuración. P08, al modificar el umbral. | P08 Alumnos morosos con el nuevo criterio aplicado. |

**Observaciones**

- Cada parámetro se acompaña de una explicación en lenguaje claro. La directora no tiene por qué saber a qué se refiere una clave interna del sistema.
- La advertencia de impacto antes de guardar responde al principio 5: ningún cambio de alcance amplio se aplica sin que el usuario sepa qué va a provocar.

---

## 16. Pantalla del alumno

### P19 — Portal del alumno

| Campo | Detalle |
|---|---|
| Objetivo | Que el alumno consulte su propia información sin depender de recepción. |
| Acceso | Exclusivo del rol Alumno, limitado a sus propios datos. |
| Historias | HU-10, HU-16 |
| Casos de uso | — |
| Requisitos | RF10, RF18, RNF03 |

**Elementos principales**

| Elemento | Tipo | Descripción |
|---|---|---|
| Estado de cuenta | Panel destacado | Al día o con deuda, con el monto y los días de mora. |
| Mis clases | Listado | Clases en las que está inscripto, con día, horario e instructor. |
| Próxima clase | Estático | La clase más cercana en el calendario. |
| Mis pagos | Tabla | Historial de cuotas abonadas. |
| Mi asistencia | Tabla | Registro de asistencias del mes en curso. |

**Información que se muestra**

- Situación de cuenta calculada al momento de la consulta.
- Clases inscriptas de la grilla activa.
- Historial completo de pagos propios.
- Asistencias propias con su estado.

**Acciones disponibles**

| Acción | Resultado |
|---|---|
| Consultar historial de pagos | Abre P07 limitado a sus propios registros. |
| Consultar asistencias | Muestra el detalle del período seleccionado. |
| Cambiar contraseña | Actualiza su propia contraseña. |

**Validaciones**

| Situación | Comportamiento | Mensaje |
|---|---|---|
| Alumno sin pagos registrados | Muestra el panel con contexto. | "Todavía no tenés pagos registrados. Estás dado de alta desde el 04/03/2026." |
| Alumno sin inscripciones | Muestra el listado vacío. | "Todavía no estás inscripto en ninguna clase. Consultá en recepción." |
| Intento de acceder a datos de otro alumno | Deniega el acceso y lo registra. | "No tenés permiso para acceder a esta información." |
| Intento de acceder al módulo nutricional | La opción no figura en su menú. | — |

**Navegación**

| Desde | Hacia |
|---|---|
| P01 Login con rol Alumno. | P07 Historial de pagos. |

**Observaciones**

- El rol Alumno es exclusivamente de consulta. No puede modificar ningún dato propio salvo su contraseña. Corresponde a la decisión D5.
- El historial nutricional no forma parte de este portal, según la regla RN28 y la decisión D13.
- Esta pantalla ataca directamente el problema P4 del relevamiento: hoy el alumno depende de recepción para cualquier consulta sobre su cuenta o sus horarios.

---

## 17. Verificación de cobertura

### 17.1 — Pantallas por historia de usuario

| Historia | Pantallas |
|---|---|
| HU-01 Registrar alumno | P04 |
| HU-02 Registrar pago | P06 |
| HU-03 Consultar morosos | P08, P18 |
| HU-04 Registrar asistencia | P13 |
| HU-05a Registrar consulta nutricional | P14 |
| HU-05b Consultar evolución | P15 |
| HU-06 Modificar alumno | P04, P05 |
| HU-07 Dar de baja alumno | P05 |
| HU-08 Reactivar alumno | P04, P05 |
| HU-09 Buscar y listar alumnos | P03 |
| HU-10 Historial de pagos | P07, P19 |
| HU-11 Gestionar modalidades | P18 y catálogo asociado |
| HU-12 Gestionar disciplinas | P09 |
| HU-13 Crear clase | P09 |
| HU-14 Versiones de grilla | P09 |
| HU-15 Inscribir a clase | P10 |
| HU-16 Historial de asistencia | P05, P19 |
| HU-17 Asistente ocasional | P13 |
| HU-18 Gestionar instructores | P11 |
| HU-19 Agenda del instructor | P12 |
| HU-20 Iniciar sesión | P01 |
| HU-21 Gestionar usuarios | P17 |
| HU-22 Reporte de ingresos | P16 |
| HU-23 Ocupación de clases | P16 |

### 17.2 — Controles

| Control | Resultado |
|---|---|
| Historias sin pantalla asociada | Ninguna. Las 24 están cubiertas. |
| Pantallas sin historia que las justifique | Ninguna. P02 es transversal a todos los roles. |
| Pantallas con acceso definido por rol | 19 de 19. |
| Pantallas con validaciones y mensajes redactados | 19 de 19. |
| Cumplimiento de RNF08 en el registro de pago | Verificado: 4 pasos. |
| Cumplimiento de RNF09 en los mensajes | Todos los mensajes indican la acción a seguir. |

### 17.3 — Pantallas por rol

| Rol | Pantallas accesibles |
|---|---|
| Administrador | P01, P02, P03, P04, P05, P06, P07, P08, P09, P10, P11, P12, P13, P16, P17, P18 |
| Recepcionista | P01, P02, P03, P04, P05, P06, P07, P08 (consulta), P09 (consulta), P10 |
| Instructor | P01, P02, P03 (consulta), P12, P13 |
| Nutricionista | P01, P02, P14, P15 |
| Alumno | P01, P19, P07 (propio) |

---

## Historial de versiones

| Versión | Fecha | Cambio |
|---|---|---|
| 1.0 | 05/2026 | Versión inicial. Diecinueve pantallas documentadas con objetivo, acceso, elementos, acciones, validaciones, navegación e información mostrada. |
| 1.1 | 05/2026 | Incorporación del apartado de procesos automáticos sin pantalla y del principio de diseño 8, derivados de la arquitectura definida en la restricción RE11. Ninguna pantalla fue modificada. |
| 1.2 | 09/2026 | Corrección de una inconsistencia interna: la tabla del menú por rol de la sección 3 negaba a la Recepcionista la sección Reportes, mientras que la matriz de permisos de `docs/requisitos.md` y el resumen de acceso por rol de la sección 17 de este documento le otorgan consulta sobre P08. Se habilita la sección en el menú, limitada a ese reporte. Ninguna pantalla fue modificada en su definición. |
| 1.3 | 10/2026 | Incorporación de la sección 6, Accesibilidad, con los criterios de color y contraste, tamaño de texto, operación por teclado, controles táctiles y formularios, y su forma de verificación. Se agrega el principio de diseño 9 y se propone el requisito RNF14. Las secciones 6 a 16 pasan a numerarse 7 a 17. Ninguna pantalla fue modificada en su definición. |

---

<sub>Escuela Superior de Comercio N° 49 "Justo José de Urquiza" — Desarrollo Web / Analista Funcional de Sistemas — 2026.</sub>
