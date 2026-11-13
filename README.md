# Proyecto Final de Comunicaciones II - Grupo 8

**Integrantes:** 
* Camilo Andrés Valcárcel Vargas (2185607)
* Kevin Santiago Contreras Araque (2205535)
* **Universidad Industrial de Santander**

📺 **Video de Socialización:** [Ver video](./Pantallazos/Video.mp4)
📄 **Informe Final (IEEE):** [Descargar PDF](./Informe_Fase4_Grupo8.pdf)

---

## 📖 Resumen del Proyecto y Evolución del Diseño
El proyecto inició con una Decisión Técnica Ciega planteando un enlace **BPSK** para el escenario S6 (Selectividad frente a emisiones cercanas), con parámetros de \(Rs=32\) ksym/s, \(SPS=8\) y filtro RRC (\(\beta=0.35\)). 

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

```text
ComunicacionesII_Grupo8/
│
├── README.md
├── Informe_Fase4_Grupo8.pdf              # Informe final en formato IEEE
├── Decisión Técnica Ciega...pdf          # Ficha del diseño inicial (BPSK)
│
├── grc/                                  # Archivos ejecutables de GNU Radio
│   ├── LAB_TX_16QAM_HackRF_MEJORADO.grc  # Flujograma transmisor
│   └── LAB_RX_16QAM_HackRF_MEJORADO.grc  # Flujograma receptor
│
├── Pantallazos/                          # Evidencias visuales y video
│   ├── 1.jpeg (Montaje físico)
│   ├── 2.jpeg (Log de BER y CRC)
│   ├── RX.png (Espectro)
│   ├── RX_normal.png (Constelación recuperada)
│   ├── curva_ber.png (Curva de desempeño)
│   ├── flujograma1.jpeg (Captura del GRC Rx)
│   ├── flujograma2.jpeg (Captura del GRC Tx)
│   └── Video.mp4                         # Video de socialización
│
└── Carpeta de archivos necesarios.../    # Instrumentación suministrada
    ├── BER_SDR_INSTRUMENTACION_BASE.grc
    ├── e_EbN0_Sweep.py
    └── e_BER_Curve.py
