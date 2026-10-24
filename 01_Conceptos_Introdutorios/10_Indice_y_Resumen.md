# Cap. 1 – Conceptos introductorios: índice y resumen

> Es el capítulo de **arranque**: define qué es lo digital, por qué se usa el binario y cuáles son las piezas de una computadora. No hay mucha mecánica de cálculo, pero **todo el vocabulario** (bit, MSB/LSB, ADC/DAC, serie/paralelo, memoria, CPU) se usa en el resto del libro.
> **Notación:** MSB/LSB = bit más/menos significativo; N = número de bits.

## Mapa de archivos

| Archivo                                           | Secciones | Contenido                                                                                                                    |
| ------------------------------------------------- | --------- | ---------------------------------------------------------------------------------------------------------------------------- |
| [[11_Analogico_y_Digital]]                        | 1-1, 1-2  | Representación analógica vs. digital, ventajas y limitaciones de lo digital, los 4 pasos con sensores/ADC/DAC                |
| [[12_Sistemas_Numericos]]                         | 1-3       | Decimal y binario, valor posicional, conversión binario→decimal, conteo binario, fórmulas con N bits                         |
| [[13_Senales_Transmision_Memoria_y_Computadoras]] | 1-4 a 1-8 | Niveles de voltaje y diagramas de tiempos, circuitos lógicos, transmisión serie/paralelo, memoria, partes de una computadora |

> Estos tres archivos están ahora dentro de `01_Conceptos_Introdutorios/` junto con este índice.

## Hoja de repaso rápida

### Analógico vs. digital
| | Analógico | Digital |
|---|---|---|
| Valores | **Continuos** | **Discretos** |
| Ejemplos | Velocímetro de aguja, mercurio, voltaje de un micrófono | Reloj digital, interruptor de 10 posiciones |

**Ventajas de lo digital:** más fácil de diseñar · fácil de almacenar información · **mayor precisión/exactitud** (no se degrada al procesarse) · **programable** · **resistente al ruido** · **más integración** en CI.
**Limitaciones:** (1) **el mundo real es analógico** y (2) **procesar lleva tiempo**.

### Los 4 pasos para procesar el mundo real
**Sensor → ADC → procesar (digital) → DAC.** Tendencia: digitalizar **lo más pronto** posible y volver a analógico **lo más tarde** posible.

### Binario en 30 segundos
- Binario = base 2 (0 y 1), **posicional**: pesos 2⁰, 2¹, 2², … a la izquierda del punto; 2⁻¹, 2⁻², … a la derecha.
- **Binario → decimal:** suma de (bit × peso). Ej.: 1011.101₂ = 8 + 2 + 1 + 0.5 + 0.125 = **11.625**.
- **Con N bits:** 2^N combinaciones; valores de 0 a **2^N − 1**.

| N | 4 | 8 | 10 | 12 | 14 | 16 |
|---|---|---|---|---|---|---|
| Máximo | 15 | 255 | 1023 | 4095 | 16383 | 65535 |

### Niveles lógicos y diagramas de tiempos
| Intervalo (ejemplo, 5 V) | Nivel |
|---|---|
| 0 – 0.8 V | **0 (BAJO)** |
| 0.8 – 2 V | **inválido** (solo en transiciones) |
| 2 – 5 V | **1 (ALTO)** |

- En digital **el valor exacto del voltaje no importa**; en analógico sí.
- **Diagrama de tiempos:** voltaje vs. tiempo; muestra cómo cambian una o varias señales. Clave para **probar y diagnosticar fallas**.
- Un **circuito digital** responde al **nivel** (0 o 1), no al voltaje exacto → también se le llama **circuito lógico**.

### Paralelo vs. serie
| | Paralela | Serie |
|---|---|---|
| Bits | **todos a la vez** | **uno a la vez** |
| Líneas | **N** | **1** |
| Velocidad | **más rápida** | más lenta |

### Memoria
Un circuito **con memoria** conserva su salida aunque desaparezca la entrada que la provocó (latches y flip-flops; se estudian en el cap. 5).

### Computadora digital
**5 unidades:** entrada · memoria · control · aritmético-lógica (ALU) · salida. **CPU = control + ALU.**

| Término | Qué es |
|---|---|
| **Microprocesador** | CPU completa en **un solo CI** |
| **Microcomputadora** | computadora con microprocesador como CPU; **propósito general** |
| **Microcontrolador** | microcomputadora **dedicada** (CPU + memoria + E/S en un chip); **controlador embebido** |

## Resumen oficial del capítulo (versión corta)
1. Dos formas de representar cantidades: **analógica (continua)** y **digital (discreta)**.
2. Lo real es analógico, pero lo digital suele ser superior.
3. **Binario (0 y 1)** es el sistema base de la tecnología digital.
4. Los **circuitos lógicos** operan con intervalos de voltaje que representan 0 o 1.
5. Transferir información: **paralela** (todos los bits a la vez) o **serie** (uno a la vez).
6. Partes de toda computadora: **entrada, control, memoria, ALU y salida**.
7. **CPU** = ALU + unidad de control.
8. **Microprocesador** = CPU en un chip.
9. **Microcontrolador** = microcomputadora para control dedicado.


**Siguiente capitulo: [[20_Indice_y_Resumen]]**
