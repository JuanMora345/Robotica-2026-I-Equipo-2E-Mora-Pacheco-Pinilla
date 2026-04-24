<div align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=0:5f7adb,50:7d5fff,100:a18cd1&height=180&section=header&text=Robótica%20Industrial%20•%20Lab%2001&fontSize=36&desc=Trayectorias,%20E/S%20Digitales%20y%20Calibración%20•%20ABB%20IRB140&descSize=14&animation=fadeIn" width="100%" />
</div>

---

# 🤖 Proyecto: Decoración Automatizada de Tortas — ABB IRB140

> **Resumen:** Práctica de laboratorio desarrollada en la **Universidad Nacional de Colombia**. Consiste en la automatización del proceso de decoración de una "torta virtual" mediante el uso de un robot industrial **ABB IRB140**, integrando diseño de herramientas (TCP), gestión de WorkObjects sobre bandas transportadoras y lógica de control mediante señales digitales (E/S).

---

## 📌 Contenido del repositorio

- `CAD-Design/` — Modelos de la herramienta (Marker Holder) en formato STL/SAT.
- `RobotStudio-Project/` — Estación de RobotStudio con la configuración del sistema.
- `RAPID-Code/` — Módulos `.mod` con la lógica de programación en RAPID.
- `Documentation/` — Diagramas de flujo, planos de planta y guía del laboratorio.
- `Media/` — Videos de simulación y ejecución real, además de capturas de resultados.

---

## 🗂 Estructura del Proyecto

```text
Robotica-UNAL-Lab01/
├─ CAD-Design/          # Diseño de herramienta y marcador
├─ Documentation/
│  ├─ Flowchart.png     # Lógica de operación
│  └─ Layout_Planta.pdf # Ubicación de elementos en el banco
├─ RAPID-Code/          # Código fuente para el controlador IRC5
├─ Simulation/          # Archivos .rspag de RobotStudio
├─ Media/               # GIFs y videos demostrativos
└─ README.md
