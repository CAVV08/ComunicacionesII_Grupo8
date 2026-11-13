# Selectividad frente a una emisión cercana: enlace digital sobre HackRF One

**Proyecto final de Comunicaciones II (27145) · Escuela E3T, Universidad Industrial de Santander · Grupo 8**
Camilo Andrés Valcárcel Vargas (2185607) · Kevin Santiago Contreras Araque (2205535)

| | |
|---|---|
| **Video de socialización** | [youtu.be/N-nEPbQD5SE](https://youtu.be/N-nEPbQD5SE) |
| **Informe IEEE** | [KContreras-CValcarcel_Proyecto-Laboratorio_Com2.pdf](./KContreras-CValcarcel_Proyecto-Laboratorio_Com2.pdf) |

## Resumen

Este proyecto parte del escenario S6 (selectividad del receptor frente a una emisión cercana), diseñado inicialmente con BPSK, y lo lleva a hardware real con dos radios HackRF One en GNU Radio. El enlace, a 960 MHz y por cable coaxial con atenuadores, se implementó con un flujo 16-QAM. El receptor recupera las tramas con FLL Band-Edge, Polyphase Clock Sync y Constellation Receiver, y adapta la tasa con un Rational Resampler. Se obtuvo la constelación de 16 nubes, un EVM de 19.3 %, una BER de 2.1 % y verificación CRC = OK. A partir del EVM se estima un E_b/N_0 de 8.3 dB, y el resultado se contrasta con la BER teórica de 16-QAM y de BPSK en canal AWGN.

## Conclusión principal

Los lazos de sincronización de frecuencia, portadora y reloj recuperan las tramas con CRC válido pese a las imperfecciones del hardware de bajo costo (EVM de 19.3 %). La BER medida (2.1 %) queda a unos 1.7 dB de la teoría para el E_b/N_0 estimado, y a esa BER el 16-QAM exige unos 3.4 dB más que BPSK, lo que lo convierte en una prueba más exigente para la cadena de recepción.

---

## Ficha de decisión técnica (Grupo 8)

| Campo | Decisión |
|---|---|
| Escenario | S6 · Selectividad del receptor frente a una emisión cercana |
| Problema | Una emisión cercana se aproxima a la banda útil y pone a prueba la selectividad |
| Modulación | BPSK, 1 bit/símbolo |
| GRC base | `Modulaciones_basadas_en_constelaciones.grc` |
| Parámetros | R_s = 32 ksímbolos/s, SPS = 8, f_s = 256 kS/s |
| Pulso | RRC β = 0.35 en Tx y filtro acoplado RRC en Rx |
| Perturbación y variable | Interferente cercano con frecuencia f_int variable |
| Pruebas | Referencia, suave, media y fuerte |
| Observables | PSD, constelación, diagrama de ojo y BER automática |
| Hipótesis | Al acercar el interferente a la banda útil, la PSD mostrará menor separación espectral |

## Parámetros del sistema

| | Ficha de decisión (S6) | Hardware y medición |
|---|---|---|
| Modulación | BPSK | 16-QAM (M = 16, k = 4, mapeo Gray) |
| R_s / R_b | 32 ksímbolos/s / 32 kb/s | 8 ksímbolos/s / 32 kb/s |
| SPS / f_s | 8 / 256 kS/s | 8 / 64 kS/s (`bb_rate`) |
| Pulso | RRC, β = 0.35 | RRC, β = 0.5 |
| Ancho de banda ≈ (1 + β)·R_s | 43.2 kHz | 12 kHz |
| Radio | | HackRF One, 2 MS/s (`radio_rate`, Rational Resampler 4/125), f_c = 960 MHz, LO desplazado 200 kHz |
| Enlace | | Cable coaxial con atenuadores |

Ambos diseños mantienen la misma tasa de bits (32 kb/s); el 16-QAM la logra ocupando 12 kHz en lugar de 43.2 kHz.

## Resultados

| Magnitud | Valor |
|---|---|
| Constelación recuperada | 16 nubes agrupadas en sus regiones de decisión |
| EVM | 19.3 % |
| BER | 2.1 % |
| CRC | OK |
| SNR estimada a partir del EVM | ≈ 14.3 dB |
| E_b/N_0 estimado | ≈ 8.3 dB |
| BER teórica 16-QAM en ese E_b/N_0 | 0.77 % |
| E_b/N_0 que exigen BPSK y 16-QAM para BER = 2.1 % | ≈ 3.1 dB y ≈ 6.6 dB |

La curva teórica de BER (16-QAM y BPSK, canal AWGN) con el punto medido marcado está en el informe, figura 6.

## Arquitectura del receptor

```
osmocom Source (2 MS/s) → Xlating FIR (−200 kHz) → Rational Resampler (4/125) → filtro RRC
→ FLL Band-Edge → Polyphase Clock Sync → Constellation Receiver → RX tramas (BER, EVM, CRC)
```

Esquema simplificado; el flujograma completo está en el informe (figura 2) y en la carpeta `grc/`.

## Uso de herramientas de inteligencia artificial

Usamos un asistente de IA (Claude, de Anthropic) como apoyo para estructurar el informe en LaTeX, calcular y graficar las curvas teóricas de BER, redactar parte del texto y organizar este repositorio.


---

*Última actualización: 12 de noviembre de 2026*
