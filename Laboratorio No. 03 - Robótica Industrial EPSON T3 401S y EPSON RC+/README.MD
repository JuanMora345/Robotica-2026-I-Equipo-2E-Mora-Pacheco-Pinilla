<div align="center">

<!-- ═══════════════════════  COSMIC HEADER  ═══════════════════════ -->

  <img src="https://capsule-render.vercel.app/api?type=waving&color=0:89f7fe,100:66a6ff&height=180&section=header&text=Robótica%20UNAL%20•%20Lab%2001&fontSize=36&desc=Trayectorias,%20E/S%20Digitales%20y%20Calibración%20•%20ABB%20IRB140&descSize=14&animation=twinkle" width="100%" />


<!-- ═══════════════════════  BADGE ROW  ════════════════════════════ -->

<a href="https://new.abb.com/products/robotics/robotstudio"><img src="https://img.shields.io/badge/RobotStudio-%E2%89%A55.0-00F5FF?style=for-the-badge&amp;logo=abb&amp;logoColor=white&amp;labelColor=0a0a1a" /></a>
<a href="https://new.abb.com/products/robotics/robots/articulated-robots/irb-140"><img src="https://img.shields.io/badge/Robot-ABB%20IRB140-7B2FBE?style=for-the-badge&amp;logoColor=white&amp;labelColor=0a0a1a" /></a>
<a href="https://library.e.abb.com/public/688894b98123f87bc1257cc50044e809/Technical%20reference%20manual_RAPID_3HAC16581-1_revJ_en.pdf"><img src="https://img.shields.io/badge/Language-RAPID-FF006E?style=for-the-badge&amp;logoColor=white&amp;labelColor=0a0a1a" /></a>
<a href="https://www.autodesk.com/products/fusion-360"><img src="https://img.shields.io/badge/CAD-Fusion%20360-FFD700?style=for-the-badge&amp;logo=autodesk&amp;logoColor=black&amp;labelColor=0a0a1a" /></a>
<a href="./LICENSE"><img src="https://img.shields.io/badge/License-MIT-00F5FF?style=for-the-badge&amp;labelColor=0a0a1a" /></a>

</div>

---

<div align="center">

```
╔══════════════════════════════════════════════════════════════════╗
║  🌌  Automatización de decorado de tortas — ABB IRB 140          ║
║  Trayectorias  ·  Herramienta  ·  WorkObjects  ·  I/O Digital   ║
╚══════════════════════════════════════════════════════════════════╝
```

</div>

> **Resumen del proyecto:** Práctica de laboratorio del curso *Robótica Industrial 2026-I* donde se diseña, simula y ejecuta con el robot **ABB IRB140** un sistema de decorado automático de tortas. El robot traza los nombres de los integrantes y una decoración personalizada sobre una superficie plana transportada por conveyor, integrando calibración de herramienta, manejo de WorkObjects, programación RAPID intermedia y control de señales digitales de entrada/salida.

---

## 📋 Tabla de contenidos

| # | Sección |
|---|---------|
| 1 | [🗂 Estructura del repositorio](#-estructura-del-repositorio) |
| 2 | [📘 Descripción detallada de la solución](#-descripción-detallada-de-la-solución) |
| 3 | [🔁 Diagrama de flujo y plano de planta](#-diagrama-de-flujo-y-plano-de-planta) |
| 4 | [⚙️ Funciones RAPID utilizadas](#️-funciones-rapid-utilizadas) |
| 5 | [🛠️ Diseño de la herramienta — *Marker*](#️-diseño-de-la-herramienta--marker) |
| 6 | [🎯 WorkObjects y calibración](#-workobjects-y-calibración) |
| 7 | [🔌 Entradas y Salidas Digitales](#-entradas-y-salidas-digitales) |
| 8 | [🚌 Control del conveyor](#-control-del-conveyor) |
| 9 | [✏️ Diseño de trayectorias](#️-diseño-de-trayectorias) |
| 10 | [🎥 Video de simulación e implementación](#-video-de-simulación-e-implementación) |
| 11 | [🖼 Previews](#-previews) |
| 12 | [🧾 Autores](#-autores) |

---

## 🗂 Estructura del repositorio

```
robotic-ABB/
│
├─ 📁 ABB-Calibration/          ← Archivos de calibración de herramienta (TCP real vs RobotStudio)
│
├─ 📁 Design/                   ← Modelos CAD y planos de la herramienta Marker
│   ├─ Marker_Base_Plane.pdf
│   ├─ Assembly_Plane.pdf
│   ├─ marker_tool.f3d           (Fusion 360)
│   └─ marker_tool.stl           (para impresión 3D)
│
├─ 📁 Diagrams/                 ← Diagramas de flujo y plano de planta
│   ├─ Flowchart_lab1.svg
│   └─ Plant_Plan.pdf
│
├─ 📁 RS/                       ← Proyecto RobotStudio
│   └─ robotic-ABB-Cake/
│       └─ Controller Data/IRB140_6_81/RAPID/TASK1/PROGMOD/
│           └─ Module1.mod       (código RAPID principal)
│
├─ 📁 Sources/                  ← Multimedia: GIFs, imágenes de resultados
│   ├─ Simulation.gif
│   ├─ Real.gif
│   └─ Results.png
│
├─ 📁 Zips/                     ← Paquetes exportados de RobotStudio (.rspag / .zip)
│
├─ Laboratorio_No_1.pdf         ← Guía oficial del laboratorio
├─ README.md
└─ LICENSE
```

---

## 📘 Descripción detallada de la solución

El objetivo es **automatizar el trazado de nombres y decoración** sobre una torta virtual (superficie plana) transportada por una banda conveyor, simulando procesos industriales de decoración en la industria alimentaria.

### Componentes del sistema

| Componente | Descripción |
|---|---|
| **Robot** | ABB IRB140 — Movimientos articulares (MoveJ), lineales (MoveL) y circulares (MoveC) |
| **Herramienta** | *Marker* — Portamarcador diseñado e impreso en 3D, calibrado mediante TCP |
| **WorkObjects** | `WObj_Torta_A` (cuadrante x+, y+) y `WObj_Torta_B` (cuadrante x+, y−) |
| **Conveyor** | Banda transportadora controlada por salida digital; modelo GrabCAD |
| **Panel I/O** | Módulo DSQC 652 del IRC5: 2 entradas digitales, 2+ salidas digitales |

### Flujo general de operación

1. El operador coloca la torta sobre el conveyor y activa la **Entrada Digital 1**.
2. El robot abandona `Home` y ejecuta las trayectorias de decorado sobre `WObj_Torta_A`.
3. Al terminar, activa la **Salida Digital conveyor** para transportar la torta y regresa a `Home`.
4. La **Entrada Digital 2** sitúa el robot en posición de mantenimiento para recarga/inspección de herramienta.

---

## 🔁 Diagrama de flujo y plano de planta

```mermaid
flowchart TD
    A([Inicio del programa]) --> B[Limpiar FlexPendant con TPErase]
    B --> C[Mostrar mensaje: SISTEMA INICIADO]
    C --> D[Mover robot a Home_ABS]
    D --> E[Apagar DO_01, DO_02, FWD_Conveyor y BWD_Conveyor]
    E --> F{{Ciclo principal WHILE TRUE}}

    F --> G{¿DI_03 = 1?}
    G -- Sí --> H[Activar BWD_Conveyor]
    H --> I[Esperar 4.275 s]
    I --> J[Apagar FWD_Conveyor y BWD_Conveyor]
    J --> K{Evaluar entradas DI_01 y DI_02}
    G -- No --> K

    K --> L{¿DI_01 = 1 y DI_02 = 0?}
    L -- Sí --> M[Mostrar: MOVIENDO CAJA]
    M --> N[Activar FWD_Conveyor]
    N --> O[Activar DO_01]
    O --> P[Esperar 4.275 s]
    P --> Q[Apagar FWD_Conveyor]
    Q --> R[Mostrar: CAJA EN POSICION - INICIANDO GRABADO]
    R --> S[Ejecutar Path_aprox]
    S --> T[Ejecutar Path_duvan]
    T --> U[Ejecutar Path_mora]
    U --> V[Ejecutar Path_gusta]
    V --> W[Ejecutar Path_robot]
    W --> X[Ejecutar Path_220]
    X --> Y[Ejecutar Path_home]
    Y --> Z[Apagar DO_01]
    Z --> F

    K --> AA{¿DI_01 = 0 y DI_02 = 1?}
    AA -- Sí --> AB[Activar DO_02]
    AB --> AC[Mostrar: RUTINA CAMBIO DE HERRAMIENTA]
    AC --> AD[Mover robot a Mantenimiento_Point]
    AD --> AE[Mostrar advertencia de operario cerca a herramienta]
    AE --> AF[Esperar DI_01 = 1]
    AF --> AG[Esperar DI_02 = 1]
    AG --> AH[Apagar DO_02]
    AH --> AI[Mostrar: REANUDANDO PROGRAMA]
    AI --> AJ[Esperar 3 s]
    AJ --> AK[Mover robot a Home_ABS]
    AK --> F

    AA --> AL{¿DI_01 = 0 y DI_02 = 0?}
    AL -- Sí --> AM[Mostrar: ESPERANDO INSTRUCCIONES]
    AM --> F

    AL --> AN{¿DI_01 = 1 y DI_02 = 1?}
    AN -- Sí --> AO[Mostrar ERROR: no presionar ambas entradas al tiempo]
    AO --> F
```

Los archivos finales estarán disponibles en:
- `Diagrams/Flowchart_lab1.svg` — Diagrama de flujo de acciones del robot
- `Diagrams/Plant_Plan.pdf` — Plano de planta con ubicación de todos los elementos

---

## ⚙️ Funciones RAPID utilizadas

> Las siguientes instrucciones y procedimientos describen la lógica de programación desde el punto de vista de su **aplicación práctica en banco**, sin exponer el código fuente directamente.

### Primitivas de movimiento

<details>
<summary><b>MoveJ — Movimiento articular</b></summary>

- **Qué hace:** mueve el robot actuando sobre sus ejes en el espacio articular, sin garantizar trayectoria cartesiana recta.
- **Cómo se aplica:** desplazamientos rápidos hacia posiciones seguras (`Home`, `Maintenance`) y transiciones entre zonas de trabajo. No se usa durante el trazado.
- **Parámetros clave:** velocidad (`v500`–`v1000`), zona de aproximación (`z10`–`z100`).

</details>

<details>
<summary><b>MoveL — Movimiento lineal</b></summary>

- **Qué hace:** desplaza la herramienta en línea recta manteniendo orientación constante del TCP.
- **Cómo se aplica:** trazado de segmentos rectos en letras y contornos, posicionamiento preciso sobre el `WorkObject`. Velocidades bajas (`v100`–`v300`) y zona `z10` para máxima precisión.
- **Restricción del laboratorio:** velocidad entre 100 y 1000; zona máxima `z10`.

</details>

<details>
<summary><b>MoveC — Movimiento circular</b></summary>

- **Qué hace:** genera trayectorias en arco circular entre tres puntos (inicio, vía, destino).
- **Cómo se aplica:** ejecución de curvas y decoraciones circulares, manteniendo continuidad de trayectoria con los `MoveL` adyacentes.

</details>

### Control de I/O y temporización

| Instrucción | Función | Aplicación en la práctica |
|---|---|---|
| `SetDO` | Activa/desactiva salida digital | Enciende/apaga conveyor y luces de estado |
| `WaitDI` | Espera hasta que una entrada digital cambie de estado | Aguarda señal del operador para iniciar decorado |
| `WaitTime` | Pausa la ejecución por intervalo definido | Sincroniza arranque del conveyor con posición de pieza |
| `Set` / `Reset` | Forma alternativa de controlar salidas | Control de indicadores luminosos en el panel |

### Procedimientos / bloques funcionales

| Procedimiento | Rol | Descripción práctica |
|---|---|---|
| `main()` | Supervisor y coordinador | Máquina de estados: lee entradas, dispara rutinas de decorado o mantenimiento |
| `P_Home()` | Posición de referencia global | Todos los ángulos articulares en 0°; inicio y fin de cada ciclo |
| `P_Maintenance()` | Posición de intervención | Robot en pose accesible para instalar/desinstalar herramienta |
| `MARK_A()` | Decorado en `WObj_Torta_A` | Ejecuta trayectorias de nombre + decoración en cuadrante x+, y+ |
| `MARK_B()` | Decorado en `WObj_Torta_B` | Reutiliza trayectorias reeferenciando el segundo WorkObject |
| `P_Names()` | Trazado de nombres | Subtrayectorias modulares para cada nombre del equipo |
| `P_Decoration()` | Trazado de decoración | Figura/ícono personalizado del equipo |

### Ruta del módulo RAPID

```
RS/robotic-ABB-Cake/Controller Data/IRB140_6_81/RAPID/TASK1/PROGMOD/Module1.mod
```

---

## 🛠️ Diseño de la herramienta — *Marker*

> 📂 Archivos disponibles en [`Design/`](./Design/): planos PDF, modelos Fusion 360 (`.f3d`), exportaciones STL/SAT y ensambles completos.

### Descripción general

El *Marker* es un portamarcador diseñado para ser **robusto, recargable y de fácil mantenimiento**. Se optó por un marcador recargable con cartucho reemplazable para **eliminar mecanismos con resortes**, reduciendo puntos de fallo.

### Funcionamiento de la punta

La punta opera por **presión directa**: al posicionar el robot sobre la superficie, la punta se sumerge ligeramente en su alojamiento garantizando flujo de tinta y contacto. Sin resortes ni actuadores adicionales.

### Especificaciones de fabricación 3D

| Parámetro | Valor recomendado |
|---|---|
| **Material** | PLA (prototipo) / PETG-ABS (mayor resistencia) |
| **Infill** | 25% — grid o gyroid |
| **Perímetros (walls)** | 2–3 |
| **Altura de capa** | 0.15–0.25 mm |
| **Ensamble** | Press-fit — sin tornillos |
| **Tolerancia press-fit** | ≈ 0.1–0.3 mm según impresora |

### Proceso de calibración TCP

La calibración del **Tool Center Point** se realizó con la técnica de los **4 puntos** (método estático): el robot registra cuatro orientaciones distintas apuntando al mismo punto físico de referencia fijo. RobotStudio calcula automáticamente la posición del TCP relativa al flanche.

Se compararon los resultados obtenidos:
- **Con el robot real** (IRC5 — 4 puntos físicos)
- **En RobotStudio** (importando el modelo CAD de la herramienta)

---

## 🎯 WorkObjects y calibración

La práctica exige dos WorkObjects para reutilizar las mismas trayectorias en dos cuadrantes distintos sobre el conveyor:

| WorkObject | Cuadrante | Propósito |
|---|---|---|
| `WObj_Torta_A` | x(+), y(+) | Decorado principal — posición inicial de la torta |
| `WObj_Torta_B` | x(+), y(−) | Réplica del decorado — posición final tras desplazamiento |

Todas las trayectorias de marcado se referencian al `WorkObject` activo, lo que permite recalibrar la posición de la torta sin reprogramar los paths.

---

## 🔌 Entradas y Salidas Digitales

Módulo utilizado: **DSQC 652** del controlador IRC5.

### Entradas digitales configuradas

| Señal | Acción disparada |
|---|---|
| `DI_01` — Inicio de decorado | Inicia rutina completa de marking; enciende luz de indicación `DO_01` |
| `DI_02` — Mantenimiento | Mueve robot a `P_Maintenance()`; apaga luz de indicación `DO_01` |

### Salidas digitales configuradas

| Señal | Función |
|---|---|
| `DO_01` — Luz de indicación | ON durante proceso de decorado; OFF en reposo/mantenimiento |
| `DO_02` — Control conveyor | Activa/desactiva la banda transportadora al final del decorado |

---

## 🚌 Control del conveyor

Para controlar el encendido/apagado de la banda transportadora, una salida digital del sistema (`DO_02`) se conecta a una entrada del **variador de frecuencia** que regula el motor.

**Secuencia de operación:**
1. La torta se coloca sobre el conveyor **antes** del inicio del ciclo.
2. El robot ejecuta el decorado completo.
3. Al finalizar, `SetDO DO_02, 1` activa el variador → la banda transporta la torta al extremo opuesto.
4. `SetDO DO_02, 0` detiene el conveyor.

> **Referencia del modelo de conveyor usado en simulación:**  
> *Modular Belt Conveyor 1.2m* — https://grabcad.com/library/modular-belt-conveyor-1-2m-1

---

## ✏️ Diseño de trayectorias

Las trayectorias se diseñaron en **Fusion 360** combinando texto vectorizado y geometrías personalizadas:

- **Nombres del equipo:** generados con la herramienta *Sketch Text*, vectorizados y exportados como contornos DXF para importar a RobotStudio.
- **Decoración personalizada:** vectorización de imagen 2D con posterior extrusión y conversión a waypoints en RobotStudio.
- **Restricciones aplicadas:** velocidad entre 100–1000 mm/s; zona máxima `z10`; trazo continuo desde `Home` hasta `Home`.

> 📷 Vista previa de las trayectorias diseñadas:

![Trayectorias](Sources/Trayectory%20Design.png)

---

## 🎥 Video de simulación e implementación

<div align="center">

<a href="https://youtu.be/V4895KkYYIQ"><img src="https://img.shields.io/badge/%E2%96%B6%20Ver%20video-YouTube-FF0000?style=for-the-badge&amp;logo=youtube&amp;logoColor=white&amp;labelColor=0a0a1a" /></a>

> El video debe iniciar con la **introducción oficial del laboratorio LabSIR** e incluir tanto la simulación en RobotStudio como la implementación con el robot real.

</div>

---

## 🖼 Previews

**Simulación en RobotStudio**

![Simulación](Sources/Simulation.gif)

**Ejecución real en banco**

![Real](Sources/Real.gif)

**Resultados sobre la pieza**

![Resultados](Sources/Results.png)

---

## 🧾 Autores

<div align="center">

| Integrante | GitHub |
|---|---|
| **Samuel David Sanchez Cardenas** | <a href="https://github.com/samsanchezcar"><img src="https://img.shields.io/badge/GitHub-%40samsanchezcar-00F5FF?style=for-the-badge&amp;logo=github&amp;logoColor=white&amp;labelColor=0a0a1a" /></a> |
| **Santiago Ávila** | <a href="https://github.com/search?q=Santiago+Avila"><img src="https://img.shields.io/badge/GitHub-Santiago%20%C3%81vila-7B2FBE?style=for-the-badge&amp;logo=github&amp;logoColor=white&amp;labelColor=0a0a1a" /></a> |

</div>

---

## 📜 Licencia

Este proyecto está bajo la licencia indicada en [`LICENSE`].

---

<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&amp;color=0:0d003b,30:1a0533,60:0d0d2b,100:0a0a1a&amp;height=120&amp;section=footer&amp;text=LabSIR%20%E2%80%A2%20Rob%C3%B3tica%20Industrial%202026-I&amp;fontSize=18&amp;fontColor=7B2FBE&amp;animation=twinkling" width="100%" />

<img src="https://img.shields.io/badge/Last%20updated-2026-FF006E?style=flat-square&amp;labelColor=0a0a1a" />
<img src="https://img.shields.io/badge/Universidad%20Nacional%20de%20Colombia-FFD700?style=flat-square&amp;labelColor=0a0a1a" />

</div>
