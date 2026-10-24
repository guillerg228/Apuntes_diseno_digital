
**Indice: [[40_Indice_y_Resumen]]**
**Seccion anterior: [[43_XOR_XNOR_Paridad_Habilitacion]]**
# CIs digitales y diagnóstico de fallas

## 4-9 Características básicas de los CIs digitales

### Qué es un CI digital
Resistencias, diodos y transistores fabricados en una sola pieza de **silicio (sustrato = chip)**, dentro de un encapsulado protector del que salen las terminales.

### Encapsulados
- **DIP (dual in-line package):** dos filas paralelas de terminales; se numeran **en sentido contrario a las manecillas del reloj**, mirando desde arriba, empezando desde la **muesca o punto** del encapsulado. DIP de 14 terminales: 0.75 × 0.25 pulg (también de 16, 20, 24, 28, 40, 64).
- El chip de silicio es mucho más pequeño que el DIP (~0.05 pulg²) y se conecta con alambres de ~1 mil de diámetro.
- **PLCC:** terminales en todo el perímetro; para CIs grandes que se pueden sustituir. **La terminal 1 está en el centro de la parte superior, no en la esquina.** Ejemplo: PLD **EPM7128SLC84** de Altera.

![[04_Circuitos_Logicos_Combinacionales/img/29.png]]

### Complejidad de integración (Tabla 4-5)

| Nivel | Compuertas por chip |
|---|---|
| **SSI** (pequeña escala) | menos de 12 |
| **MSI** (mediana escala) | 12 a 99 |
| **LSI** (gran escala) | 100 a 9,999 |
| **VLSI** (muy grande escala) | 10,000 a 99,999 |
| **ULSI** (ultra gran escala) | 100,000 a 999,999 |
| **GSI** (giga escala) | 1,000,000 o más |

- Los chips de los caps. 3 y 4 son **SSI**. Los SSI siguen usándose como **"pegamento"** entre chips complejos y en laboratorios.
- Hoy la industria usa **PLDs** para sistemas de cualquier tamaño.

### Bipolares vs. unipolares
| | **Bipolares** | **Unipolares** |
|---|---|---|
| Elemento principal | Transistor de unión bipolar (NPN/PNP) | MOSFET (canal P y N) |
| Familia principal | **TTL** (transistor/transistor) | **CMOS** |

- **CMOS** usa **menos componentes** que TTL (mayor simplicidad, menor tamaño) y domina hoy los CIs de gran escala; TTL se sigue usando mucho en laboratorio por su **resistencia**.

![[04_Circuitos_Logicos_Combinacionales/img/30.png]]

### Series TTL (mismo pinout y lógica; cambian velocidad y consumo)
| Serie | Prefijo | Ejemplo (inversor hex) |
|---|---|---|
| TTL estándar | 74 | 7404 |
| Schottky | 74S | 74S04 |
| Schottky de bajo consumo | **74LS** | 74LS04 |
| Schottky avanzado | 74AS | 74AS04 |
| Schottky avanzado de bajo consumo | 74ALS | 74ALS04 |

> Todos los 7404, 74S04, 74LS04, 74AS04 y 74ALS04 son **seis inversores en un chip**. Las series solo difieren en **disipación de potencia y velocidad**.

### Series CMOS
| Serie | Compatibilidad |
|---|---|
| **4000** (ej. 4001 = 4 NOR) | La más antigua; **no** compatible en terminales con TTL |
| **74C** | Compatible en terminales con TTL |
| **74HC** | Alta velocidad; compatible en terminales con TTL |
| **74HCT** | Compatible en terminales **y eléctricamente** con TTL (se conecta directo) |
| **74AC** | Alto desempeño; **no** compatible en terminales ni eléctricamente con TTL |
| **74ACT** | Alto desempeño; **no** compatible en terminales, **sí** eléctricamente con TTL |

- Ejemplo: 74C02, 74HC02 y 74HCT02 tienen el mismo pinout que 7402 y 74LS02.

### Alimentación y tierra
- Sin VCC/GND conectados, **las compuertas no responden bien**. Se conectan **por chip, no por compuerta**.
- **VCC** (TTL) y **VDD** (CMOS); muchos CMOS compatibles con TTL también usan VCC.
- **TTL:** VCC = **+5 V**. **CMOS:** VDD de **+3 a +18 V** (lo común: +5 V cuando comparte tarjeta con TTL).

### Niveles de voltaje de entrada

| | **0 lógico** | **Indeterminado** | **1 lógico** |
|---|---|---|---|
| **TTL** | 0 – 0.8 V | 0.8 – 2.0 V | 2 – 5 V |
| **CMOS** (VDD = 5 V) | 0 – 1.5 V | 1.5 – 3.5 V | 3.5 – 5 V |

![[04_Circuitos_Logicos_Combinacionales/img/31.png|429]]

### Entradas desconectadas (flotantes)
- **TTL:** una entrada flotante actúa como **1 lógico** (suele medir 1.4–1.8 V, en el intervalo indeterminado). Útil al diagnosticar, pero **mala práctica** en diseño final (capta ruido).
- **CMOS:** una entrada flotante es **peligrosa**: no actúa ni como 0 ni como 1, **fluctúa con el ruido**, la salida es impredecible (puede oscilar) y el CI puede **sobrecalentarse y dañarse**. **Todas las entradas CMOS deben ir a ALTO, BAJO o a la salida de otro CI.** Algunos CMOS complejos tienen protección interna, pero conviene conectar siempre lo no usado.

### Diagramas de conexiones
Muestran **todas las conexiones eléctricas**: números de terminal, número de CI, valores de componentes, nombres de señales y alimentación. Puntos clave de la Fig. 4-32:
1. Cada chip lleva una designación (**Z1, Z2…**, también IC1 o U1…) y cada compuerta indica a qué chip pertenece (ej. 2 inversores de un 74HC04 = Z1; 2 NAND de un 74HC00 = Z2).
2. Se anota el **número de terminal** de cada entrada/salida (ej. "terminal 2 de Z1").
3. VCC y tierra se muestran **por CI** (ej. terminal 14 a +5 V y terminal 7 a tierra en un chip de 14 pines).
4. Entradas a la izquierda, salidas a la derecha. Una **barra sobre el nombre** y **burbujas** indican señales **activas en BAJO**.

![[04_Circuitos_Logicos_Combinacionales/img/32.png|547]]

Con **captura de diagramas esquemáticos** (ej. MAX+PLUS de Altera, archivos `.gdf`) los símbolos no llevan terminales ni chips, porque el circuito se **programa en un PLD** en lugar de cablearse con SSI/MSI.

## 4-10 Diagnóstico de fallas en sistemas digitales

### Tres pasos básicos
1. **Detección de fallas:** observar la operación y compararla con la correcta.
2. **Aislamiento de fallas:** pruebas y mediciones para ubicar la falla.
3. **Corrección de fallas:** reemplazar el componente, reparar la conexión o eliminar el corto.

- Se aprende **en el laboratorio**, con práctica real. El libro desarrolla las **habilidades analíticas** (razonar a partir de lo observado).
- Herramientas básicas: **sonda lógica, osciloscopio y generador de pulsos lógico**. La más importante es **el técnico**.

### Sonda lógica
Punta metálica que se toca en un punto del circuito; sus **LEDs** indican el nivel:

| Condición | Indicación |
|---|---|
| ALTO | LED rojo |
| BAJO | LED verde |
| Indeterminado / **abierto o flotante** | **Ningún LED** |
| Pulsos (transiciones) | LED **amarillo** destellando (~3 Hz) |

Con rojo/verde + amarillo se puede saber si la señal está **casi siempre en ALTO, en BAJO, o pulsando**.


![[34.png]]

## 4-11 Fallas internas en los CIs digitales

Las cuatro más comunes:
1. **Fallas en circuitos internos** (un componente falla o sale de especificación). Salidas impredecibles. **La menos común.**
2. **Entrada o salida en corto a tierra o a VCC.**
3. **Entrada o salida abierta** (se rompe el alambre fino interno).
4. **Corto entre dos terminales** (que no sean tierra ni VCC).

### Entrada en corto a tierra / VCC (Fig. a, b)
La terminal queda **atascada en BAJO (o ALTO)**; además **afecta a la señal que la excita** (la fuerza a tierra o a +5 V).

### Salida en corto a tierra / VCC (Fig. c, d)
La salida queda **atascada en BAJO (o ALTO)** y **no responde** a las entradas. **No afecta** las señales en las entradas del CI.

![[04_Circuitos_Logicos_Combinacionales/img/35.png|400]]

**Ejemplo 4-24 (Fig. 4-36):** la salida de un inversor (Z1-4) debería pulsar porque su entrada pulsa, pero está **atascada en BAJO**. Causas posibles:
1. Falla de un componente interno en el inversor.
2. Z1-4 en corto a tierra internamente.
3. La entrada Z2-1 (a donde conecta) en corto a tierra internamente.
4. Un corto externo a tierra en la ruta entre ambas terminales.

### Entrada o salida abierta (Fig. 4-37)
- **Entrada abierta:** no responde a las señales; queda **flotante** (TTL → se comporta como **1**; CMOS → errático, riesgo de daño).
- **Salida abierta:** no llega señal a la terminal; el voltaje ahí es **impredecible/flotante** (la sonda no enciende LEDs).

**Ejemplo 4-25 (Fig. 4-37):** ¿qué indica la sonda en las terminales 13 (entrada abierta) y 6 (salida abierta)?
- **Terminal 13:** el nivel de la **señal externa** conectada a ella (el abierto está *dentro* del chip, después de la terminal).
- **Terminal 6:** **ningún LED** (nivel indeterminado), porque el nivel de salida de la NAND nunca llega a esa terminal.

**Ejemplo 4-26 (Fig. 4-38, TTL):** el inversor funciona, pero la NAND tiene salida incorrecta: debería estar en ALTO porque su entrada 1 está en BAJO (inhibe), y sin embargo sigue los pulsos de la otra entrada. **Causa probable: circuito abierto interno en la entrada 1 de la NAND** (el BAJO no llega a la compuerta; TTL lo interpreta como ALTO).
- Si el abierto fuera **externo** al chip, el voltaje en esa terminal estaría en ~1.4–1.8 V (indeterminado); como es **interno**, el voltaje sí llega a la terminal y la sonda ve el BAJO.

### Corto entre dos terminales (Fig. 4-39)
Las dos señales quedan **idénticas**. Si dos señales que deberían ser distintas muestran las mismas variaciones, sospecha de **corto**.
- **Colisión de señales (contención):** una salida intenta ALTO y la otra BAJO "luchando". El nivel resultante depende del CI: en **TTL** suele quedar cerca de **0.8 V** (extremo alto del 0, o indeterminado); en **CMOS**, en el intervalo **indeterminado**.
- **Señal con tres niveles distintos** → sospecha de dos salidas en corto.

![[04_Circuitos_Logicos_Combinacionales/img/39.png|564]]

## 4-12 Fallas externas

### Líneas de señal abiertas (discontinuidad)
Causas: alambre roto o con mal aislamiento · **soldadura defectuosa** o alambre enrollado flojo · **grieta en la pista** del PCB (a veces del grosor de un cabello) · **terminal doblada o rota** · **zócalo defectuoso** (mal contacto).
- Se detecta con **inspección visual** y se verifica **con la alimentación apagada** midiendo **continuidad con un óhmetro**.
- **Ejemplo 4-27 (Fig. 4-40, CMOS):** el BAJO de Z1-6 no llega a Z2-2 → **circuito abierto en la ruta** entre ambos. Se ubica rastreando con la sonda desde Z1-6 hasta donde el nivel se vuelve indeterminado.

### Líneas de señal en corto
Fuerzan dos señales a ser iguales, o una a tierra/VCC. Causas: **alambrado mal instalado** (demasiado aislante pelado), **puentes de soldadura** entre pines cercanos, **desbaste incompleto** del cobre en PCB. Se detectan con inspección visual y se comprueban con **óhmetro**.

### Fuente de alimentación defectuosa
- Voltajes mal regulados por falla interna o **sobrecarga** (un chip consume demasiada corriente).
- Síntoma típico: **uno o más chips operan mal o no operan**.
- Buenas prácticas: medir **VCC/VDD y tierra en cada CI** que parezca fallar; ver con **osciloscopio** si hay **rizo de CA** y que el voltaje se mantenga regulado en operación.

### Carga de la señal de salida (loading)
Si una salida alimenta **demasiadas entradas**, se excede su **corriente nominal** y su voltaje puede caer al **intervalo indeterminado**. Normalmente es un error de diseño o conexión.


## 4-13 Ejemplo práctico de diagnóstico (ejemplo 4-28, Fig. 4-41)

Circuito con compuertas NAND y una XOR (Z1). La salida `Y` debería ser ALTO cuando (A = 1, B = 0, sin importar C) o (A = 0, B = 1, C = 1). El técnico observa que **Y sube cuando A o C están en ALTO, sin importar B**.

Con A = B = 0 y C = 1, la sonda muestra que las NAND son correctas, pero la **XOR (Z1-3) se queda en ALTO** cuando debería ser BAJO (dos entradas iguales en BAJO).

**Causas posibles:**
1. Falla de un componente interno de Z1.
2. Corto externo a VCC en cualquier punto del nodo X.
3. Z1-3 en corto interno a VCC.
4. Z2-5 en corto interno a VCC.
5. Z2-13 en corto interno a VCC.

**Procedimiento para aislar la falla:**
1. Verificar **VCC y tierra** en Z1 (poco probable que sea la causa, pero siempre se revisa en un CI con salida incorrecta).
2. **Apagar el circuito** y medir con óhmetro si hay **corto (< 1 Ω) entre el nodo X y cualquier punto de VCC** (ej. Z1-14, Z2-14). Si **no** hay corto → lo más probable es **falla interna de Z1** (reemplazarlo).
3. Si hay corto, **inspección visual** detallada (puentes de soldadura, cobre sin desbastar, alambres sin aislante). Lugar probable: entre las terminales **13 y 14 de Z2** (VCC y nodo X).
4. Si no se ve corto externo, quedan tres cortos internos (Z1-3, Z2-13, Z2-5): **desconectar cada terminal sospechosa del nodo X, una a una**, y volver a medir contra VCC. Al desconectar la culpable, el corto desaparece.
   - Con CIs en **zócalo**: sacar el CI, doblar la terminal y reinsertarlo. Si está **soldado**: cortar la pista y repararla después.

![[04_Circuitos_Logicos_Combinacionales/img/41.png]]

> **Idea general:** compara **lo observado con lo esperado**, lista **todas las causas posibles** y diseña pruebas que vayan **descartando** hipótesis, de la más barata y probable a la más invasiva.








**Siguiente seccion: [[45_PLD_y_HDL_VHDL]]**