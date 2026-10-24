
**Indice: [[30_Indice_y_Resumen]]**
# Algebra booleana, tablas de verdad, OR, AND, NOT

## Introducción: qué es el álgebra booleana
- **George Boole (1854)**, *Una investigación sobre las leyes del pensamiento*: decisiones lógicas con base en circunstancias verdaderas/falsas.
- **Lógica booleana** = los métodos; **álgebra booleana** = símbolos y operadores para describirlos.
- Propósito: describir la relación **salida (decisión) ↔ entradas (condiciones)** de un circuito lógico.
- **Análisis** = obtener la expresión de un circuito dado. **Síntesis** = crear el circuito a partir de la relación entrada/salida deseada.

- Herramientas para describir circuitos:
  - Álgebra booleana → herramienta **matemática**
  - Tablas de verdad → **organización de datos**
  - Símbolos esquemáticos → **representación gráfica**
  - Diagramas de tiempos → **graficar la respuesta**
  - Lenguaje (HDL) → **descripción universal**

## 3-1 Constantes y variables booleanas
- Solo pueden valer **0 o 1** (no son números reales, son **niveles lógicos**).
- Ejemplo de asignación: 0 = 0–0.8 V, 1 = 2–5 V (0.8–2 V: indefinido).
- No hay fracciones, negativos, raíces, logaritmos… Solo **3 operaciones básicas: OR, AND, NOT**.

| 0 lógico | 1 lógico |
|---|---|
| Falso | Verdadero |
| Apagado | Encendido |
| Bajo | Alto |
| No | Sí |
| Interruptor abierto | Interruptor cerrado |

Las **compuertas lógicas** son circuitos (diodos, transistores, resistencias) que realizan estas operaciones.

## 3-2 Tablas de verdad
Lista **todas las combinaciones** de entradas y la salida resultante.
- Con **N entradas → 2^N combinaciones** (2 entradas: 4; 3: 8; 4: 16; 5: 32).
- Se listan en **orden de conteo binario** para no omitir ninguna.
![[03_Descripcion_de_Circuitos_Logicos/img/1.png|581]]

## 3-3 Operación OR y compuerta OR

- Expresión: **x = A + B** (el `+` es OR, no suma: **1 + 1 = 1**).  ** enlace a suma binaria
- La salida es **1 si una o más entradas son 1**; solo es 0 cuando **todas** son 0.
- Con 3 entradas: x = A + B + C. Cualquier cantidad de entradas funciona igual.
- Ejemplo cotidiano: luz del horno = interruptor encendido **OR** puerta abierta.

| A   | B   | x = A + B |
| --- | --- | --------- |
| 0   | 0   | 0         |
| 0   | 1   | 1         |
| 1   | 0   | 1         |
| 1   | 1   | 1         |
![[03_Descripcion_de_Circuitos_Logicos/img/2.png|484]]

**Resumen OR:** (1) salida 1 si cualquier entrada es 1; (2) la compuerta OR realiza la operación; (3) se lee "x es igual a A OR B".

**Diagramas de tiempos con OR (ejemplos 3-2, 3-3):**
- Salida = ALTO siempre que **al menos una** entrada esté en ALTO.
- Si dos entradas cambian casi a la vez (una baja, otra sube), puede aparecer una **deformación (glitch)** en la salida por la región indefinida en la transición.
- Si otra entrada se mantiene en ALTO, **no aparece el glitch** (esa entrada manda).

![[03_Descripcion_de_Circuitos_Logicos/img/5.png]]

## 3-4 Operación AND y compuerta AND

- Expresión: **x = A · B = AB** (el `·` es AND; se opera como multiplicación ordinaria de 1s y 0s).     ** multiplicacion binaria
- La salida es **1 solo si todas las entradas son 1**.
- Ejemplo cotidiano: secadora de ropa funciona si temporizador > 0 **AND** puerta cerrada.

| A | B | x = AB |
|---|---|---|
| 0 | 0 | 0 |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 1 | 1 | 1 |

Con 3 entradas: x = ABC (1 solo en 111). Con 4: x = ABCD.

![[03_Descripcion_de_Circuitos_Logicos/img/7.png|450]]

**Resumen AND:** (1) opera como multiplicación; (2) la compuerta AND realiza la operación; (3) salida 1 solo si todas las entradas son 1; (4) x = AB se lee "x es igual a A AND B".

### AND como habilitador / inhibidor (ejemplo 3-5)
- Con **B = 1 (habilitación)**, la salida **sigue a A**.
- Con **B = 0 (inhibición)**, la salida es **0** sin importar A.
- B funciona como entrada de **control**; esta aplicación se verá con detalle en el cap. 4. (De aquí viene el nombre "compuerta".)

## 3-5 Operación NOT (inversor)

- **x = A'** → "NOT A", "inverso de A" o "complemento de A". Opera con **una sola entrada**.
- 0' = 1 y 1' = 0. También se llama **inversión** o **complementación**. El apóstrofo (`'`) equivale a la barra superior.
- El **círculo (burbuja)** en un símbolo siempre denota inversión.

| A | x = A' |
|---|---|
| 0 | 1 |
| 1 | 0 |
![[03_Descripcion_de_Circuitos_Logicos/img/11.png|476]]

**Aplicación 3-1 (botón):** un botón cableado da 1 al oprimirse; un inversor da una señal verdadera cuando **no** está oprimido.

### Resumen de las 3 operaciones

| OR | AND | NOT |
|---|---|---|
| 0+0=0 | 0·0=0 | 0'=1 |
| 0+1=1 | 0·1=0 | 1'=0 |
| 1+0=1 | 1·0=0 | |
| 1+1=1 | 1·1=1 | |

**Siguiente seccion: [[32_Evaluacion_de_Expresiones_NOR_NAND]]**