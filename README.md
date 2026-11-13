# Proyecto Final de Comunicaciones II - Grupo 8

**Integrantes:**
* Camilo Andrés Valcárcel Vargas (2185607)
* Kevin Santiago Contreras Araque (2205535)
* **Universidad Industrial de Santander**

📺 **Video de Socialización:** [Ver en YouTube](https://youtu.be/N-nEPbQD5SE)
📄 **Informe Final (IEEE):** [Descargar PDF](./KContreras-CValcarcel_Proyecto-Laboratorio_Com2.pdf)

---

## 📖 Resumen del Proyecto y Evolución del Diseño

El proyecto inició con una Decisión Técnica Ciega planteando un enlace **BPSK** para el escenario S6 (Selectividad frente a emisiones cercanas), con parámetros de \(R_s=32\) ksym/s, \(SPS=8\) y filtro RRC (\(\beta=0.35\)).

Sin embargo, debido a la disponibilidad compartida de los equipos SDR en el laboratorio, el equipo tomó la decisión de ingeniería de realizar un montaje colaborativo empleando la plataforma **HackRF One** con una modulación **16-QAM** a 960 MHz. Esta adaptación permitió validar los objetivos de las fases 3 y 4 sobre un esquema de modulación de mayor complejidad.

Los resultados experimentales evidencian una constelación recuperada con un Error Vector Magnitude (EVM) del **19.3%** y una Tasa de Error de Bit (BER) del **2.1%**, validando la integridad de las tramas mediante verificación de redundancia cíclica (**CRC=OK**). Se demostró que los conceptos de adaptación de tasa, sincronización de portadora y recuperación de reloj son transversales a cualquier modulación.

---

## 🛠️ Parámetros Principales de la Implementación

| Parámetro | Valor |
| :--- | :--- |
| **Modulación Final** | 16-QAM (M=16, k=4) |
| **Tasa de Símbolos (Rs)** | 8 ksym/s |
| **Tasa de Bits (Rb)** | 32 kbps |
| **Muestras por Símbolo (Sps)** | 8 |
| **Tasa de Muestreo del SDR** | 2 Msps (radio_rate) |
| **Frecuencia Central (fc)** | 960 MHz |
| **Hardware** | 2x HackRF One |
| **Filtro de conformación** | RRC (Roll-off = 0.5) |
| **BER Medida** | ~2.1% |
| **EVM Medido** | ~19.3% |

---

## 📂 Estructura del Repositorio

* `KContreras-CValcarcel_Proyecto-Laboratorio_Com2.pdf`: Documento final en formato IEEE.
* `Pantallazos/`: Carpeta con las capturas de GNU Radio, el montaje físico y el video de socialización (`Video.mp4`).
* `Carpeta de archivos necesarios.../`: Instrumentación suministrada en la Fase 4 (`e_EbN0_Sweep.py`, `e_BER_Curve.py`, etc.).
* *(Nota: Los flujogramas `.grc` deben estar subidos en la raíz o en la carpeta `grc/`)*.

---

## 🚀 Cómo ejecutar el proyecto
1. Instalar **GNU Radio Companion** (versión 3.10 en adelante).
2. Abrir el flujograma del receptor (`LAB_RX_16QAM_HackRF_MEJORADO.grc`) y el del transmisor.
3. Conectar ambos HackRF One mediante cable coaxial con atenuadores (evitar saturación).
4. Configurar la frecuencia central en **960 MHz**, ajustar ganancias (IF y BB) y verificar la constelación en el receptor.

---

## ⚠️ Limitaciones y Uso de IA

**Limitaciones:** Debido a la disponibilidad del hardware, se realizó un montaje colaborativo con 16-QAM. No fue posible ejecutar el barrido dinámico automatizado de \(E_b/N_0\) (bloque `e_EbN0_Sweep`) sobre el enlace físico en tiempo real. La curva presentada se extrapoló a partir del punto de operación medido estáticamente (BER = 2.1%), calculando una pérdida de implementación de ~1.7 dB respecto a la teoría.

**Uso de IA:** Se utilizó un asistente de Inteligencia Artificial como herramienta de apoyo para la estructuración del informe en LaTeX, la generación de la gráfica teórica de BER y la organización del repositorio. Los datos numéricos, los flujogramas y las capturas provienen de las ejecuciones reales del equipo.
