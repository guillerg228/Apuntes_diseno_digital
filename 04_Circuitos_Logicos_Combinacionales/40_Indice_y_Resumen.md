
**Capitulo anterior: [30_Indice_y_Resumen](30_Indice_y_Resumen.md)**

# Cap. 3 – Descripción de los circuitos lógicos: índice y resumen

> Es el capítulo del **álgebra booleana y las compuertas**: las tres operaciones (OR, AND, NOT), NAND/NOR, teoremas, DeMorgan, símbolos y las primeras descripciones en **VHDL**. Todo el diseño combinacional y secuencial se construye sobre esto.
> **Notación:** `A'` = A negada (barra superior en el libro); `·` = AND, `+` = OR; **act. BAJO** = activa en BAJO.

## Mapa de archivos

| Archivo                                   | Secciones   | Contenido                                                                            |
| ----------------------------------------- | ----------- | ------------------------------------------------------------------------------------ |
| [41_Sop_Simplificacion_y_Diseno](41_Sop_Simplificacion_y_Diseno.md)        | 3-1 a 3-5   | Variables booleanas, **tablas de verdad**, **OR, AND, NOT**, formas de onda          |
| [42_Mapas_de_Karnaugh](42_Mapas_de_Karnaugh.md)                  | 3-6 a 3-9   | Expresiones de circuitos, precedencia, evaluación, tabla de análisis, **NOR y NAND** |
| [43_XOR_XNOR_Paridad_Habilitacion](43_XOR_XNOR_Paridad_Habilitacion.md)      | 3-10 a 3-12 | **Teoremas booleanos (1–15)**, **DeMorgan (16, 17)**, **NAND/NOR universales**       |
| [44_CIS_Digitales_y_Diagnostico_Fallas](44_CIS_Digitales_y_Diagnostico_Fallas.md) | 3-13 a 3-16 | **Símbolos alternos**, activo en ALTO/BAJO, IEEE/ANSI, 6 métodos de descripción      |
| [45_PLD_y_HDL_VHDL](45_PLD_y_HDL_VHDL.md)                     | 3-17 a 3-20 | **HDL/VHDL**: ENTITY/ARCHITECTURE, señales intermedias, operadores (+ extras)        |


## Hoja de repaso rápida

### Las compuertas (2 entradas)
| A | B | OR | AND | NOR | NAND | XOR (cap. 4) |
|---|---|---|---|---|---|---|
| 0 | 0 | 0 | 0 | 1 | 1 | 0 |
| 0 | 1 | 1 | 0 | 0 | 1 | 1 |
| 1 | 0 | 1 | 0 | 0 | 1 | 1 |
| 1 | 1 | 1 | 1 | 0 | 0 | 0 |

- **OR:** salida 1 si **cualquier** entrada es 1. **AND:** salida 1 solo si **todas** son 1. **NOT:** invierte.
- **NOR = OR + inversor; NAND = AND + inversor.**
- Tabla de verdad con **N entradas = 2^N combinaciones**.
- **Precedencia:** paréntesis → **AND antes que OR**; si hay barra sobre una expresión, se opera adentro y **después** se invierte.
- AND como **habilitador/inhibidor:** con B = 1 la salida sigue a A; con B = 0 es 0.

### Teoremas (para simplificar)
| Una variable | | Varias variables | |
|---|---|---|---|
| `x·0 = 0` | `x·1 = x` | `x + y = y + x` | `x·y = y·x` (conmutativa) |
| `x·x = x` | `x·x' = 0` | `x(y+z) = xy + xz` | (distributiva; también para **factorizar**) |
| `x + 0 = x` | `x + 1 = 1` | `x + xy = x` | (absorción) |
| `x + x = x` | `x + x' = 1` | `x + x'y = x + y` | `x' + xy = x' + y` |

### DeMorgan
- **`(x + y)' = x'·y'`** y **`(x·y)' = x' + y'`**.
- **Regla práctica:** rompe la barra larga **y cambia el operador** (· ↔ +); repite hasta que solo haya variables individuales negadas; dobles barras se cancelan.
- ⚠️ `(A + B)' ≠ A' + B'`.
- Circuitos: **NOR ≡ AND con entradas negadas**; **NAND ≡ OR con entradas negadas**.

### Universalidad
| Con NAND | Con NOR |
|---|---|
| NOT: entradas unidas | NOT: entradas unidas |
| AND: NAND + NAND(inversor) | OR: NOR + NOR(inversor) |
| OR: invertir A y B, luego NAND | AND: invertir A y B, luego NOR |

Ej. 3-18: `x = AB + CD` con **3 NAND** (un solo 74LS00).

### Símbolos y niveles activos
- **Estándar:** sin burbujas en las entradas. **Alternativo:** con burbujas en las entradas (se obtiene **invirtiendo entradas y salida y cambiando AND↔OR**). Son **el mismo circuito físico**.
- **Sin burbuja = activa en ALTO; con burbuja = activa en BAJO.**
- Interpretación: salida activa cuando **todas** (símbolo AND) o **cualquiera** (símbolo OR) de las entradas están activas.
- **Regla de dibujo:** conecta **salidas con burbuja a entradas con burbuja**.
- **IEEE/ANSI:** rectángulos con `&` (AND), `≥1` (OR), `1` (NOT) y triángulo de inversión.

### Seis formas de describir un circuito
Lenguaje natural · tabla de verdad · símbolos tradicionales · símbolos IEEE/ANSI · expresión booleana · diagrama de tiempos.

### VHDL en 6 líneas
```vhdl
ENTITY compuerta_and IS
  PORT ( a, b :IN BIT; y :OUT BIT);
END compuerta_and;
ARCHITECTURE ckt OF compuerta_and IS
BEGIN  y <= a AND b;  END ckt;
```
- **ENTITY** = bloque (E/S: modo + tipo); **ARCHITECTURE** = operación; `<=` asignación **concurrente**.
- **`SIGNAL`** = señal intermedia (antes de `BEGIN`).
- ➕ VHDL **no tiene precedencia AND/OR**: usa **paréntesis** (`(a AND b) OR c`).

## Errores frecuentes
- Confundir `(A + B)'` con `A' + B'` (y `(AB)'` con `A'B'`).
- Olvidar que en OR **1 + 1 = 1**.
- Dibujar una NAND "mal" porque se olvida que su símbolo alternativo es una **OR con burbujas en las entradas**.
- En VHDL, mezclar `AND` y `OR` sin paréntesis.

## Resumen oficial del capítulo (versión corta)
1. El álgebra booleana es la herramienta matemática para analizar y diseñar circuitos digitales.
2. Operaciones básicas: **OR, AND, NOT**.
3. OR: ALTO con cualquier entrada en ALTO. AND: ALTO solo con todas en ALTO. NOT: nivel opuesto.
4. **NOR** = OR + inversor; **NAND** = AND + inversor.
5. Los **teoremas** y reglas booleanas simplifican expresiones y circuitos.
6. **NAND y NOR** implementan cualquier operación básica.
7. Símbolos **alternativos o estándar** según la salida sea activa en ALTO o en BAJO.
8. **IEEE/ANSI**: símbolos rectangulares con notaciones de dependencia.
9. Los **HDL** son un método importante para describir circuitos digitales.
10. Todo código HDL se **documenta con comentarios**.
11. Toda descripción en HDL tiene **definición de entradas/salidas** + **operación**.
12. Hay **nodos ocultos / señales locales** inaccesibles desde fuera.



**Siguiente capitulo: [50_Indice_y_Resumen](50_Indice_y_Resumen.md)**
