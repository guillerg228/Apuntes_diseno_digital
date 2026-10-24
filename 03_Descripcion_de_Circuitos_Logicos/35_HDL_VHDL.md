
**Indice: [[30_Indice_y_Resumen]]**
**Seccion anterior: [[34_Simbolos_Alternos_Activo_Bajo_IEEE]]**
# HDL y VHDL

## 3-17 HDL vs. lenguaje de programación

**HDL (Hardware Description Language):** describe la **configuración de un circuito**, en texto que una computadora puede procesar. Permite **documentar, simular y sintetizar** circuitos.

**VHDL** (VHSIC Hardware Description Language): lo desarrolló el Departamento de Defensa de EE. UU. a inicios de los 80 para el proyecto de circuitos integrados de muy alta velocidad (VHSIC). Está **estandarizado por el IEEE** y es universal (sirve para cualquier fabricante y dispositivo).

|              | Lenguaje de programación (C, Java…)                 | HDL (VHDL)                                              |
| ------------ | --------------------------------------------------- | ------------------------------------------------------- |
| Qué describe | **Secuencia de instrucciones** para una computadora | **Hardware** (compuertas y conexiones)                  |
| Ejecución    | **Secuencial**: una instrucción a la vez            | **Concurrente**: todo opera al mismo tiempo             |
| Velocidad    | Depende de cuánto tarda cada instrucción            | Solo del retardo del circuito (~10 ns en una compuerta) |

**Analogía del pit stop:** un solo mecánico haciendo todas las tareas una tras otra = **computadora**; un equipo de mecánicos trabajando a la vez = **circuito digital**.

**Ejemplo 3-25 (y = AB):** el circuito es una AND y responde en ~10 ns; una computadora necesita 2–3 instrucciones (~20 ns c/u → 40–60 ns): ¿A en ALTO? ¿B en ALTO? → poner y en ALTO o BAJO → repetir.

![[03_Descripcion_de_Circuitos_Logicos/img/43.png|530]]

> **Idea clave:** en VHDL **no "ejecutas" código**, **describes hardware**. Cada línea es un pedazo de circuito que existe en paralelo con los demás.

## 3-18 Implementación con PLDs

- **PLD** (dispositivo lógico programable): **no ejecuta un programa**; sus circuitos internos se **"alambran" electrónicamente** para formar el circuito lógico.
- El alambrado es una **matriz de conexiones programables**: cada cruce fila–columna está **conectado (1)** o **no conectado (0)**.
- Flujo de trabajo:
  1. Describes el circuito en **HDL**.
  2. El **compilador** lo traduce a un **patrón de 1s y 0s** (la matriz de conexiones).
  3. Ese patrón se **carga en el PLD**.


![[03_Descripcion_de_Circuitos_Logicos/img/44.png|485]]

## 3-19 Formato y sintaxis

Toda descripción de hardware, en cualquier HDL, tiene **dos partes**:

1. **Definición de E/S:** nombre del circuito y de sus puertos. Cada puerto tiene:
   - **Modo:** entrada, salida o entrada/salida.
   - **Tipo:** número de bits y cómo se interpretan (1 bit = 2 valores; 4 bits = 16 valores).
2. **Descripción funcional:** cómo responden las salidas a las entradas.

Además: **encabezado** (proyecto, autor, fecha) y **comentarios**.

![[03_Descripcion_de_Circuitos_Logicos/img/45.png|591]]

![[03_Descripcion_de_Circuitos_Logicos/img/46.png|256]]


### Código 1: compuerta AND

```vhdl
ENTITY compuerta_and IS
    PORT ( a, b :IN BIT;
           y    :OUT BIT);
END compuerta_and;

ARCHITECTURE ckt OF compuerta_and IS
BEGIN
    y <= a AND b;
END ckt;
```

| Elemento | Qué hace |
|---|---|
| `ENTITY nombre IS ... END nombre;` | Declara el **bloque** (la "caja negra" con sus terminales) |
| `PORT ( ... );` | Lista las **entradas y salidas** |
| `a, b :IN BIT` | **Modo** (`IN`, `OUT`) y **tipo** (`BIT` = 1 bit, valores `'0'` y `'1'`) |
| `ARCHITECTURE ckt OF compuerta_and IS` | Describe la **operación interna** de la ENTITY. Toda ENTITY necesita al menos una; el nombre (`ckt`) lo eliges tú |
| `BEGIN ... END ckt;` | Cuerpo de la arquitectura |
| `y <= a AND b;` | **Asignación concurrente** de señal (`<=`), equivale a conectar la salida de la AND a `y` |

**Puntos de sintaxis:**
- Cada declaración termina en **`;`** (la última línea de un `PORT` cierra con `);`).
- **Comentarios:** `--` hasta el final de la línea.
- Estilo recomendado: **PALABRAS CLAVE en mayúsculas**, nombres propios en minúsculas (el compilador no lo exige, pero mejora la lectura).
- Las instrucciones entre `BEGIN` y `END` son **concurrentes**: **el orden en que las escribas no importa**.
- Por convención, el archivo se guarda con el mismo nombre que la ENTITY.

## 3-20 Señales intermedias (nodos ocultos / señales locales)

Son puntos **internos** del circuito que no son entradas ni salidas (como los nodos o puntos de prueba de un esquemático). Solo existen **dentro del bloque**. Ejemplo: `y = m + c` donde `m = a·b`.

![[03_Descripcion_de_Circuitos_Logicos/img/49.png]]
### Código 2: señal intermedia

```vhdl
-- Variables intermedias en VHDL (Figura 3-49)
-- Sistemas digitales 10a ed
-- NS Widmer
-- MAYO 23, 2005

ENTITY fig3_51 IS
    PORT( a, b, c :IN BIT;    -- define las entradas del bloque
          y       :OUT BIT);  -- define la salida del bloque
END fig3_51;

ARCHITECTURE ckt OF fig3_51 IS

    SIGNAL m :BIT;            -- nombra una señal intermedia

BEGIN
    m <= a AND b;             -- genera término de producto oculto
    y <= m OR c;              -- genera suma en la salida
END ckt;
```

**Lo nuevo respecto al código 1:**
- **`SIGNAL m :BIT;`** declara la señal local. Va **dentro de `ARCHITECTURE`, antes de `BEGIN`**.
- Se declara con **nombre + `:` + tipo**, igual que los puertos, pero **sin modo** (no es ni `IN` ni `OUT`).
- La señal intermedia se **asigna con `<=`** y luego se **usa** como entrada en otra asignación.
- No importa si escribes primero `m` o `y`; se recomienda definir primero las intermedias porque es más legible.
- Aparece el **encabezado** de comentarios (proyecto, autor, fecha).

## Operadores y reglas útiles de VHDL

| Operación | VHDL   | Ejemplo          |
| --------- | ------ | ---------------- |
| NOT       | `NOT`  | `y <= NOT a;`    |
| AND       | `AND`  | `y <= a AND b;`  |
| OR        | `OR`   | `y <= a OR b;`   |
| NAND      | `NAND` | `y <= a NAND b;` |
| NOR       | `NOR`  | `y <= a NOR b;`  |
| XOR       | `XOR`  | `y <= a XOR b;`  |

**Regla importante: VHDL no tiene precedencia entre AND y OR.** Para el compilador, `a AND b OR c` es un **error de sintaxis**. Hay que usar **paréntesis siempre** al mezclar operadores:

```vhdl
y <= (a AND b) OR c;      -- correcto: y = ab + c
y <= a AND b OR c;        -- ERROR de compilación
y <= NOT (a OR b);        -- NOR escrito con NOT: y = (a + b)'
```

(Esto es distinto de la regla del álgebra booleana que vimos en 3-6, donde AND se evaluaba antes que OR.)

**Otras reglas básicas de nombres:**
- VHDL **no distingue mayúsculas de minúsculas**.
- Los identificadores llevan letras, números y `_`, **no pueden empezar con número ni llevar guion `-`**. Por eso `ROM-A` del libro se escribiría `rom_a`.
- Para señales activas en BAJO es común usar el sufijo `_n` (ej. `mem_n`, `rd_n`).

## Términos importantes
ARCHITECTURE · asignación concurrente (`<=`) · BIT · compilador · concurrente · ENTITY · HDL · IEEE (VHDL estandarizado) · modo (`IN`/`OUT`) · PLD · PORT · SIGNAL · sintaxis · tipo · VHDL



**Siguiete capitulo: [[40_Indice_y_Resumen]]**
