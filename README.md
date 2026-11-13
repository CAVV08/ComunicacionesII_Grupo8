# Enlace digital 16-QAM con HackRF One en GNU Radio

**Proyecto Final · Comunicaciones II (27145) · Universidad Industrial de Santander**
**Escenario S6: selectividad frente a una emisión cercana · Grupo 8**

| | |
|---|---|
| **Video de socialización** | [youtu.be/N-nEPbQD5SE](https://youtu.be/N-nEPbQD5SE) |
| **Informe IEEE** | [KContreras-CValcarcel_Proyecto-Laboratorio_Com2.pdf](./KContreras-CValcarcel_Proyecto-Laboratorio_Com2.pdf) |

**Integrantes:** Camilo Andrés Valcárcel Vargas (2185607) · Kevin Santiago Contreras Araque (2205535)

---

## Resumen

Este proyecto implementa y caracteriza un enlace digital punto a punto con dos radios definidos por software HackRF One en GNU Radio. El diseño inicial para el escenario S6 planteaba BPSK; por disponibilidad de equipos, el enlace se implementó sobre un flujo 16-QAM compartido, a 960 MHz y por cable coaxial con atenuadores. El receptor recupera la señal con FLL Band-Edge, Polyphase Clock Sync y Constellation Receiver, y adapta la tasa con un Rational Resampler. Se midió un EVM de 19.3 %, una BER de 2.1 % y verificación CRC = OK. Ese EVM equivale a un E_b/N_0 de unos 8.3 dB, donde la teoría en canal AWGN predice 0.77 %: la pérdida de implementación es de unos 1.7 dB. No se realizó el barrido de E_b/N_0 sobre el hardware ni el estudio con interferente.

## Conclusión principal

El hardware de bajo costo degrada la señal (EVM de 19.3 %), pero los lazos de sincronización de frecuencia, portadora y reloj recuperan las tramas sin errores de CRC. La BER medida es 2.7 veces la teórica para el E_b/N_0 estimado, una pérdida de implementación cercana a 1.7 dB atribuible al desbalance I/Q, al ruido de fase y al offset de portadora residual.

---

## Diseño: de la ficha inicial a la implementación

| Parámetro | Ficha inicial (S6) | Implementado en hardware |
|---|---|---|
| Modulación | BPSK | 16-QAM (M = 16, k = 4) |
| Tasa de símbolos R_s | 32 ksímbolos/s | 8 ksímbolos/s |
| Muestras por símbolo (SPS) | 8 | 8 |
| Tasa de muestreo en banda base | 256 kS/s | 64 kS/s |
| Filtro de conformación | RRC, β = 0.35 | RRC, β = 0.5 |
| Tasa de bits R_b | 32 kb/s | 32 kb/s |
| Ancho de banda ≈ (1 + β)·R_s | 43.2 kHz | 12 kHz |
| Perturbación | Interferente con f_int variable | Ninguna adicional (solo imperfecciones del enlace) |

Ambos diseños tienen la misma tasa de bits, pero el 16-QAM ocupa menos ancho de banda.

## Parámetros del enlace implementado

| Parámetro | Valor |
|---|---|
| Hardware | 2 × HackRF One, enlace por cable coaxial con atenuadores |
| Frecuencia central | 960 MHz |
| Tasa de muestreo del SDR (`radio_rate`) | 2 MS/s |
| Tasa en banda base (`bb_rate`) | 64 kS/s (Rational Resampler 4/125 en recepción) |
| R_s · SPS | 8 k × 8 = 64 kS/s |
| Desplazamiento de sintonía | 200 kHz (compensado con Frequency Xlating FIR Filter) |
| Filtro | RRC, β = 0.5 |

## Pruebas y resultados

| Prueba | Condición | EVM | BER | CRC |
|---|---|---|---|---|
| Enlace por cable | Referencia, sin interferente | 19.3 % | 2.1 % | OK |

Comparación con la teoría (16-QAM con mapeo Gray en AWGN):

| Magnitud | Valor |
|---|---|
| SNR estimada a partir del EVM | ≈ 14.3 dB |
| E_b/N_0 estimado | ≈ 8.3 dB |
| BER teórica en ese E_b/N_0 | 0.77 % |
| BER medida | 2.1 % (2.7 veces la teórica) |
| E_b/N_0 teórico que daría BER = 2.1 % | ≈ 6.6 dB (pérdida ≈ 1.7 dB) |

---

## Estructura del repositorio

```
.
├── KContreras-CValcarcel_Proyecto-Laboratorio_Com2.pdf   Informe final (IEEE)
├── Pantallazos/                  Capturas de GNU Radio y del montaje, y el video (Video.mp4)
├── Carpeta de archivos necesarios.../   Instrumentación de la Fase 4 (e_EbN0_Sweep.py, e_BER_Curve.py, ...)
└── grc/                          Flujogramas del transmisor y del receptor (.grc)
```

Archivo principal del receptor: `LAB_RX_16QAM_HackRF_MEJORADO.grc` (el del transmisor está junto a él).

## Cómo ejecutarlo

1. Instalar GNU Radio Companion 3.10 o superior, con `gr-osmosdr` y los controladores del HackRF One.
2. Abrir el flujograma del transmisor en un computador y el del receptor en otro (o en el mismo, con cada HackRF por separado, `hackrf=0` y `hackrf=1`).
3. Conectar los dos HackRF One con cable coaxial y atenuadores, para evitar la saturación del receptor. No transmitir por aire sin autorización.
4. Configurar la frecuencia central en 960 MHz y ajustar las ganancias de transmisión y de recepción.
5. Ejecutar primero el receptor y verificar la constelación y las métricas de EVM, BER y CRC.

## Limitaciones

- No se ejecutó el barrido automático de E_b/N_0 (`e_EbN0_Sweep` y `e_BER_Curve`) sobre el enlace físico. Solo hay un punto de operación medido.
- La gráfica de BER del informe es **teórica** (16-QAM y BPSK en AWGN), con el punto medido ubicado a partir del EVM. No es una curva experimental.
- No se estudió el interferente del escenario S6 ni se realizaron las pruebas suave, media y fuerte.
- No se aplicó calibración I/Q ni ecualización.

## Uso de inteligencia artificial

Se usó un asistente de IA (Claude, de Anthropic) para estructurar el informe en LaTeX, calcular y graficar las curvas teóricas de BER, redactar parte del texto y organizar este repositorio. El EVM, la BER, el CRC, los flujogramas y las capturas provienen de las ejecuciones reales del equipo, y los resultados fueron revisados por los integrantes.

## Referencias

La lista completa está en el [informe](./KContreras-CValcarcel_Proyecto-Laboratorio_Com2.pdf).

---

*Última actualización: 12 de noviembre de 2026*
