
**Indice: [[40_Indice_y_Resumen]]**
**Seccion anterior: [[44_CIS_Digitales_y_Diagnostico_Fallas]]**
# PLDs y HDL (VHDL)

## 4-14 Dispositivos lógicos programables (PLDs)

### Idea y ventaja
- Un PLD pone **muchas compuertas lógicas en un solo CI** y **controla su interconexión por medios electrónicos**.
- Automatiza los pasos tediosos del diseño clásico (tabla de verdad → expresión → simplificar → elegir CIs → cablear), que son los más propensos a errores.
- El trabajo del diseñador: identificar **entradas y salidas**, especificar la **relación lógica** de la forma más conveniente y elegir el PLD más barato que la pueda implementar.

### Hardware: matriz de conmutación / arreglo programable
- Rejilla de **filas y columnas**; las entradas van a las columnas, las salidas a las filas. En cada cruce hay un **interruptor electrónico** (fusible, relevador, transistor…) que **conecta (1) o no (0)**.
- Al decidir qué cruces se conectan se "programa" cómo las entradas llegan a las salidas.
- Ejemplo (Fig. 4-42): un arreglo programable que **selecciona las entradas de cada compuerta AND**, para producir cualquier **producto** de las variables. Otro arreglo similar puede conectar las salidas de las AND a **compuertas OR** (→ suma de productos). La arquitectura se estudia en el cap. 13.

![[04_Circuitos_Logicos_Combinacionales/img/42.png|513]]

### Cómo se programa un PLD
| Método | Descripción |
|---|---|
| **Con programador** | Se **extrae el chip** de la tarjeta y se coloca en un **programador** (en un **zócalo ZIF**, de cero esfuerzo de inserción) conectado a una PC con software que conoce muchos dispositivos. |
| **ISP** (programación en sistema) | El chip **no se saca** del circuito. Usa la interfaz **JTAG** (4 terminales del CI como "portal" de datos) conectada a la PC con un cable. |

- **JEDEC:** organismo que estandarizó terminales y el formato de archivo de programación (**archivo JEDEC**, independiente del fabricante). Cualquier programador compatible puede cargarlo.
- **JTAG:** estándar para **probar CIs** sin conectar equipo a todas sus terminales; también permite programarlos. Lo incluyen PLDs y microcontroladores.

![[04_Circuitos_Logicos_Combinacionales/img/43.png|468]]

### Software de desarrollo (Altera, en el libro)
- Convierte cualquier descripción (esquemático, tabla de verdad, ecuaciones, **HDL**) en la **matriz de 1s y 0s** para el PLD.
- Admite entrada por **diagramas esquemáticos (.gdf)**, AHDL, **VHDL**, Verilog y **diagramas de tiempos**.
- Los bloques hechos con métodos distintos se pueden **interconectar** (Fig. 4-44).

![[04_Circuitos_Logicos_Combinacionales/img/44.png|535]]

### Diseño jerárquico y flujo de desarrollo
- **Diseño jerárquico:** circuitos pequeños y útiles se describen del modo más conveniente y se **combinan** para formar un sistema (ejemplo: reproductor de CDs con sus secciones de audio, rastreo láser, eje, controles de usuario…).
- Método **arriba-abajo (top-down):** se parte del **sistema completo**, se divide en subsecciones y luego en circuitos individuales, cada uno con **entradas, salidas y comportamiento definidos**. Luego se **construye de abajo hacia arriba** y se prueba cada nivel por separado.

![[04_Circuitos_Logicos_Combinacionales/img/45.png]]

![[04_Circuitos_Logicos_Combinacionales/img/46.png]]

**Ciclo de desarrollo de cada bloque:**
1. **Crear el archivo de diseño** (HDL, esquemático…).
2. **Compilar:** detecta **errores de sintaxis** (no traduce hasta que la sintaxis sea correcta).
3. **Simular:** un **simulador** calcula las salidas correctas a partir de la descripción y de **vectores de prueba** (entradas hipotéticas con sus salidas esperadas).
4. **Crear el archivo de salida y programar** el PLD (el compilador puede **asignar terminales**, o las asignas tú respetando la arquitectura del chip).
5. **Prueba en circuito** y **diagnóstico de fallas** si algo falla.
- Después se prueban las subsecciones y, al final, el sistema completo. Se presta a **trabajo en equipo**.

![[04_Circuitos_Logicos_Combinacionales/img/47.png]]

![[04_Circuitos_Logicos_Combinacionales/img/48.png|405]]

## 4-15 Representación de datos en HDL

### Literales (valores escalares)
Un número sin prefijo es **decimal**. En VHDL:

| Sistema | VHDL | Ejemplo (el patrón `11001`) |
|---|---|---|
| Binario | `B"..."` | `B"11001"` |
| Hexadecimal | `X"..."` | `X"19"` |
| Decimal | sin prefijo | `25` |

(Como referencia: 11001₂ = 19₁₆ = 25₁₀.)

### Arreglos (vectores) de bits
Una señal de varios bits tiene **un nombre** y **un índice por bit**. Ejemplo: puerto `P1` de 8 bits: bit más significativo `P1[7]`, menos significativo `P1[0]` (en VHDL se escribe con paréntesis: `p1(7)`, `p1(0)`).

- Asignación: el origen va a la **derecha**, el destino a la **izquierda**. Hacer que el LSB de P5 controle el MSB de P1: en VHDL `p1(7) <= p5(0);`.

### Declaración en VHDL
```vhdl
-- puerto de entrada de 8 bits (en ENTITY)
PORT ( p1 :IN BIT_VECTOR(7 DOWNTO 0) );

-- señal intermedia de 8 bits (en ARCHITECTURE, antes de BEGIN)
SIGNAL temp :BIT_VECTOR(7 DOWNTO 0);

-- conectar todos los bits, o solo uno
temp    <= p1;          -- los 8 bits
temp(0) <= p1(0);       -- solo el LSB
```
- Sintaxis: **nombre : modo tipo (intervalo)**. El intervalo va **entre paréntesis**, con `DOWNTO` (de MSB a LSB).
- Repaso 4-15: una entrada de 6 bits `botones_pulsar` → `botones_pulsar :IN BIT_VECTOR(5 DOWNTO 0);` y su MSB a la salida `z` → `z <= botones_pulsar(5);`.

### VHDL es muy estricto con los tipos
- `BIT_VECTOR` (arreglo de bits) **≠** un entero. **No** puedes asignar un entero a un `BIT_VECTOR` ni conectar un puerto `INTEGER` a salidas `BIT_VECTOR`.
- Las asignaciones y operaciones deben hacerse entre **objetos del mismo tipo**.
- Un `BIT_VECTOR` **no** se puede comparar con `X"5"` ni con `3`; solo con **literales de cadena** entre comillas dobles, de la misma longitud (ej. `"101"`).

### Tipos de datos comunes en VHDL

| Tipo | Posibles valores | Ejemplo |
|---|---|---|
| `BIT` | `'0'` `'1'` (entre **comillas simples**) | `a :IN BIT;` |
| `BIT_VECTOR` | cadenas como `"0101"` (entre **comillas dobles**) | `datos_bcd :BIT_VECTOR(3 DOWNTO 0);` |
| `STD_LOGIC` | `'0'` `'1'` `'z'` `'x'` `'-'` … | `controlador :STD_LOGIC;` |
| `STD_LOGIC_VECTOR` | cadenas de esos valores | `dbus :STD_LOGIC_VECTOR(3 DOWNTO 0);` |
| `INTEGER RANGE` | enteros del intervalo | `z :INTEGER RANGE -32 TO 31;` |

### STD_LOGIC
Tipos del estándar **IEEE** (bibliotecas de funciones comunes). **`STD_LOGIC` ≈ `BIT`** y **`STD_LOGIC_VECTOR` ≈ `BIT_VECTOR`**, pero con **más valores** (Tabla 4-10):

| Valor | Significado |
|---|---|
| `'1'` | 1 lógico (igual que BIT) |
| `'0'` | 0 lógico (igual que BIT) |
| `'z'` | **Alta impedancia** (lógica de tres estados, cap. 8) |
| `'-'` | **No importa** (como en los mapas K) |
| `'U'` | Sin inicializar |
| `'X'` | Desconocido |
| `'W'` | Desconocido débil |
| `'L'` | 0 débil |
| `'H'` | 1 débil |

> Las **bibliotecas** son colecciones de código reutilizable (**macrofunciones**, p. ej. dispositivos TTL estándar); para usarlas, **los tipos de tus señales deben coincidir** con los de las funciones.

> Para usar `STD_LOGIC` hay que declarar la biblioteca al inicio del archivo:
> ```  LIBRARY ieee;
>      USE ieee.std_logic_1164.ALL;
> ```

## 4-16 Tablas de verdad con HDL

Si introduces la **tabla de verdad directamente** en HDL, te ahorras pasos intermedios (expresión, mapa K).

### Concatenación con `&`
Para tratar bits independientes como un **vector ordenado** (MSB a la izquierda):
```vhdl
bits_ent <= a & b & c;      -- a = MSB, c = LSB
```
- Las entradas se declaran como **bits individuales** en la ENTITY (ninguno es "más significativo" por sí mismo); **el orden de la concatenación lo decide**.

### Asignación de señal seleccionada: `WITH ... SELECT` (Fig. 4-51)
Circuito de **mayoría** del ejemplo 4-7:

```vhdl
-- Figura 4-7 en VHDL
ENTITY fig4_51 IS
  PORT ( a, b, c :IN BIT;      -- declara bits de entrada individuales
         y       :OUT BIT);
END fig4_51;

ARCHITECTURE verdad OF fig4_51 IS
  SIGNAL bits_ent :BIT_VECTOR(2 DOWNTO 0);
BEGIN
  bits_ent <= a & b & c;       -- concatena los bits de entrada en un vector
  WITH bits_ent SELECT
    y <= '0' WHEN "000",       -- tabla de verdad
         '0' WHEN "001",
         '0' WHEN "010",
         '1' WHEN "011",
         '0' WHEN "100",
         '1' WHEN "101",
         '1' WHEN "110",
         '1' WHEN "111";
END verdad;
```

- **`WITH señal SELECT objetivo <= valor WHEN patrón, ...;`** asigna `y` según el valor de `bits_ent`. Cada línea es **una fila de la tabla de verdad**.
- Es una **instrucción concurrente** (el compilador sintetiza el circuito completo).
- `y` es `BIT` → recibe `'0'` o `'1'`. `bits_ent` es `BIT_VECTOR` de 3 bits → se compara con **cadenas de 3 bits**.
- **Debes cubrir todos los casos** (aquí los 8).

**Ejemplo 4-33 (concatenar):** señales `demasiado_caliente`, `demasiado_frio`, `muy_bien` combinadas con lo caliente a la izquierda y lo frío a la derecha:
```vhdl
SIGNAL demasiado_caliente, demasiado_frio, muy_bien :BIT;
SIGNAL estado_temp :BIT_VECTOR(2 DOWNTO 0);
...
estado_temp <= demasiado_caliente & muy_bien & demasiado_frio;
```
- Primero se **declaran** las señales en la ARCHITECTURE (antes de BEGIN) y luego se escriben las asignaciones **concurrentes** entre BEGIN y END.

## 4-17 Estructuras de control de decisiones en HDL

### Concurrente vs. secuencial
- **Concurrentes:** el orden en que se escriben **no importa** (todo ocurre "al mismo tiempo").
- **Secuenciales:** se evalúan **en el orden en que se escriben**; **el orden sí afecta el circuito**.
- En VHDL las instrucciones secuenciales (`IF`, `CASE`) deben ir dentro de un **`PROCESS`**.

### ¿Por qué usar IF en lugar de tabla de verdad?
Para datos numéricos, listar cada valor es tedioso. Es más natural razonar: *"uso abrigo si la temperatura es **menor de 14**; en caso contrario, no"*. Para ello, la entrada se declara como un **número** (INTEGER con intervalo) y se compara con `<`, `>`, `<=`, `>=`.

### IF / THEN / ELSE
Circuito del ejemplo 4-8 (batería > 6 V) con la entrada como número de 4 bits `valor_digital` (bit 3 = MSB … bit 0 = LSB):

```vhdl
ENTITY fig4_55 IS
  PORT ( valor_digital :IN INTEGER RANGE 0 TO 15;  -- entrada de 4 bits
         z             :OUT BIT);
END fig4_55;

ARCHITECTURE decision OF fig4_55 IS
BEGIN
  PROCESS (valor_digital)               -- lista de sensibilidad
  BEGIN
    IF (valor_digital > 6) THEN
      z <= '1';
    ELSE
      z <= '0';
    END IF;
  END PROCESS;
END decision;
```

| Elemento | Qué hace |
|---|---|
| `INTEGER RANGE 0 TO 15` | Tipo **numérico**; el intervalo **0–15** le dice al compilador que son **4 bits**. (`RANGE` da límites del **valor**, no índices de bits.) |
| `PROCESS (valor_digital)` | **Lista de sensibilidad:** cada vez que cambia `valor_digital`, se **vuelve a evaluar** el proceso. |
| `IF ... THEN ... ELSE ... END IF;` | Si la condición es verdadera hace una acción; si no, la otra. |
| `END PROCESS;` | Termina la sección secuencial. |

- Un **entero** sí se puede comparar con `<`, `>`; un `BIT_VECTOR` **no**.
- Diagrama de flujo: **IF/THEN** (hacer o no hacer) e **IF/THEN/ELSE** (elegir entre dos acciones).

![[04_Circuitos_Logicos_Combinacionales/img/52.png|576]]

### ELSIF: varias decisiones
Selecciona **una de muchas** acciones: se evalúan las condiciones **en orden** y se ejecuta **solo la de la primera verdadera**. Sirve con **desigualdades** y rangos.

Ejemplo: indicador de rango de temperatura con un ADC de 4 bits:
- `valor ≤ 8` → **demasiado frío**
- `8 < valor < 11` → **templado** (muy bien)
- en otro caso → **demasiado caliente**

```vhdl
ENTITY fig4_59 IS
  PORT ( valor_digital :IN INTEGER RANGE 0 TO 15;    -- entrada de 4 bits
         demasiado_frio, templado, demasiado_caliente :OUT BIT);
END fig4_59;

ARCHITECTURE quetancaliente OF fig4_59 IS
  SIGNAL estado :BIT_VECTOR(2 DOWNTO 0);  -- guarda el estado de los tres indicadores
BEGIN
  PROCESS (valor_digital)
  BEGIN
    IF (valor_digital <= 8) THEN
      estado <= "100";
    ELSIF (valor_digital > 8 AND valor_digital < 11) THEN
      estado <= "010";
    ELSE
      estado <= "001";
    END IF;
  END PROCESS;

  -- asigna los bits de estado a la salida
  demasiado_frio     <= estado(2);
  templado           <= estado(1);
  demasiado_caliente <= estado(0);
END quetancaliente;
```

- Se usa una **señal intermedia** `estado` (un patrón de 3 bits, uno por indicador) y al final **cada bit se conecta a su salida** (instrucciones concurrentes, fuera del `PROCESS`).
- La palabra clave es **`ELSIF`** (no `ELSE IF`).

![[04_Circuitos_Logicos_Combinacionales/img/56.png|445]]

![[57.png|504]]
### CASE
Evalúa **un objeto** y busca **el caso** (valor) que coincide; cada caso tiene sus acciones.

| `IF/ELSIF` | `CASE` |
|---|---|
| Relaciona acciones con **afirmaciones verdaderas** (condiciones) | Relaciona acciones con **un valor único** de un objeto |
| Puede haber varias verdaderas, pero se ejecuta **solo la primera** | **Solo una coincidencia** posible |

Circuito de la copiadora del ejemplo 4-9 (`S = P' + QR`) con `CASE`; salida `s = 0` solo para `P Q R = 100, 101, 110`:

```vhdl
ENTITY fig4_61 IS
  PORT ( p, q, r :IN BIT;                -- declara 3 bits de entrada
         s       :OUT BIT);
END fig4_61;

ARCHITECTURE copia OF fig4_61 IS
  SIGNAL estado :BIT_VECTOR(2 DOWNTO 0);
BEGIN
  estado <= p & q & r;                   -- enlaza los bits en orden

  PROCESS (estado)
  BEGIN
    CASE estado IS
      WHEN "100"  => s <= '0';
      WHEN "101"  => s <= '0';
      WHEN "110"  => s <= '0';
      WHEN OTHERS => s <= '1';
    END CASE;
  END PROCESS;
END copia;
```

- **`CASE objeto IS`** y **`WHEN patrón => acción;`** (la flecha es **`=>`**, no `<=`).
- **`WHEN OTHERS`** cubre todos los demás casos (**obligatorio** si no listas todos).
- Termina con `END CASE;`.
- Para comparar contra patrones de bits, el objeto es un `BIT_VECTOR` formado por **concatenación (`&`)** de bits individuales.

### Ejemplo 4-35: detector de monedas de máquina despachadora (Fig. 4-62 a 4-64)
Detectores de monedas de **25 (Q), 10 (D) y 5 (N) centavos**; solo puede haber **una moneda a la vez**. La salida es un **número de 5 bits** (hasta 25) con el valor en centavos.

```vhdl
ENTITY fig4_64 IS
  PORT ( q, d, n   :IN BIT;                         -- 25, 10 y 5 centavos
         centavos  :OUT INTEGER RANGE 0 TO 25);     -- valor de las monedas
END fig4_64;

ARCHITECTURE detector OF fig4_64 IS
  SIGNAL monedas :BIT_VECTOR(2 DOWNTO 0);           -- agrupa los sensores
BEGIN
  monedas <= (q & d & n);                           -- asigna sensores al grupo

  PROCESS (monedas)
  BEGIN
    CASE monedas IS
      WHEN "001"  => centavos <= 5;
      WHEN "010"  => centavos <= 10;
      WHEN "100"  => centavos <= 25;
      WHEN OTHERS => centavos <= 0;
    END CASE;
  END PROCESS;
END detector;
```

- `INTEGER RANGE 0 TO 25` hace que el compilador use **5 bits** para la salida.
- El resto de combinaciones (ninguna moneda, o varias que no pueden ocurrir) → 0.

![[62.png|573]]

### Errores típicos
- `CASE` y `IF` **solo dentro de `PROCESS`** (en VHDL-93 estándar), y el `PROCESS` necesita su `BEGIN`.
- `=>` en `WHEN`, `<=` en asignaciones, `=` en comparaciones (`IF a = '1' THEN`).
- Un `CASE` **sin `WHEN OTHERS`** da error si no cubres todos los valores posibles.
- Asignar `'1'` a un `BIT_VECTOR` o `"1"` a un `BIT` da **error de tipos**: `BIT` → comillas **simples**, `BIT_VECTOR` → comillas **dobles**.

### Elegir la estructura de control
| Situación                                                           | Estructura                      |
| ------------------------------------------------------------------- | ------------------------------- |
| Hacer algo o no hacerlo                                             | `IF/THEN`                       |
| Elegir entre **dos** acciones                                       | `IF/THEN/ELSE`                  |
| Elegir entre **muchas** acciones según **rangos/condiciones**       | `IF/ELSIF/ELSE`                 |
| Elegir entre muchas acciones según un **valor exacto** de un objeto | `CASE`                          |
| Copiar una **tabla de verdad** completa                             | `WITH ... SELECT` (concurrente) |

## Términos importantes
asignación de señal seleccionada · cero esfuerzo de inserción (ZIF) · comprobación de paridad · condición "no importa" · diseño jerárquico · encapsulado dual en línea (DIP) · estructuras de control · habilitar/deshabilitar · lista de sensibilidad · TTL · CMOS · XNOR · XOR · producto de sumas · SSI/MSI/LSI/VLSI/ULSI/GSI · suma de productos · vectores de prueba · mapa de Karnaugh · concatenación · `PROCESS` · `CASE` · `ELSIF`



**Siguiete capitulo: [[50_Indice_y_Resumen]]**