
**Capitulo anterior: [[10_Indice_y_Resumen]]**

# Cap. 2 – Sistemas y códigos numéricos: índice y resumen

> Es el capítulo de **conversiones y códigos**: pasar entre binario, decimal y hexadecimal, y conocer los códigos que usan 1s y 0s para representar números (BCD, Gray) y caracteres (ASCII), además del bit de paridad. Se usa en **todo** lo que sigue (direcciones de memoria, datos, HDL).
> **Notación:** `A'` = A negada; subíndices ₂, ₁₀, ₁₆ indican la base.

## Mapa de archivos

| Archivo                            | Secciones | Contenido                                                                                        |
| ---------------------------------- | --------- | ------------------------------------------------------------------------------------------------ |
| [[21_Conversiones_y_Hexadecimal]]  | 2-1 a 2-3 | Binario↔decimal, división repetida, **hexadecimal**, Tabla 2-1, hex↔binario, conteo hex          |
| [[22_BCD_y_gray]]                  | 2-4 a 2-6 | **Código BCD**, **código Gray**, tabla integradora (Tabla 2-3)                                   |
| [[23_Byte_Nibble_Palabra_y_ASCII]] | 2-7, 2-8  | Byte, nibble, palabra y **código ASCII**                                                         |
| [[24_Paridad_y_Aplicaciones]]      | 2-9, 2-10 | **Bit de paridad** (par/impar), 5 aplicaciones (CD-ROM, Intel Hex, direcciones, serie asíncrona) |

## Hoja de repaso rápida

### Métodos de conversión (el resumen del propio libro)
| De → A | Método |
|---|---|
| Binario/Hex → **Decimal** | **Suma ponderada** de cada posición (pesos 2ⁿ o 16ⁿ) |
| Decimal → **Binario/Hex** | **División repetida** entre 2 / 16; el **primer residuo es el LSB** y el último el MSB (se lee de abajo hacia arriba) |
| Binario → **Hex** | Grupos de **4 bits** (desde la derecha, rellenando con ceros) → 1 dígito hex |
| Hex → **Binario** | Cada dígito → **4 bits** |

### Tabla Hex – Dec – Bin (¡memorízala!)
| Hex | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | A | B | C | D | E | F |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Dec | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 |
| Bin | 0000 | 0001 | 0010 | 0011 | 0100 | 0101 | 0110 | 0111 | 1000 | 1001 | 1010 | 1011 | 1100 | 1101 | 1110 | 1111 |

### Fórmulas de alcance
- **N bits** → valores de **0 a 2^N − 1** (2^N valores). 8 bits → 255; 16 bits → 65,535.
- **N dígitos hex** → **0 a 16^N − 1**. 4 dígitos → 0 a 65,535 (FFFF).
- **Bits para contar hasta X:** el menor N con 2^N − 1 ≥ X (ej.: 12,500 → 14 bits; 1 millón → 20 bits).

### Códigos en una tabla
| Código | Idea | Propiedad clave |
|---|---|---|
| **Binario directo** | todo el número decimal → binario | el más compacto |
| **BCD** | **cada dígito decimal → 4 bits** (0000–1001) | conversión fácil a decimal; usa más bits; **1010–1111 son inválidos** |
| **Gray** | secuencia de patrones | **solo cambia 1 bit** entre pasos sucesivos (evita errores en codificadores de eje) |
| **ASCII** | 7 bits (128 códigos) para caracteres | se guarda como byte con **relleno de 0** |

**Ejemplo comparativo:** 137₁₀ = `10001001` (binario, 8 bits) = `0001 0011 0111` (BCD, 12 bits).

### Conversión binario ↔ Gray
- **MSB igual.**
- **Binario → Gray:** cada bit = XOR de bits binarios vecinos (G₂ = B₂, G₁ = B₂ ⊕ B₁, G₀ = B₁ ⊕ B₀).
- **Gray → binario:** B₂ = G₂, B₁ = B₂ ⊕ G₁, B₀ = B₁ ⊕ G₀.

### Unidades de datos
| Término | Tamaño |
|---|---|
| **Bit** | 1 |
| **Nibble** | 4 bits (1 dígito hex o BCD) |
| **Byte** | 8 bits |
| **Palabra** | depende de la **ruta de datos** del sistema (8, 16, 32, 64 bits…) |

### ASCII – lo que más se usa
| Grupo | Hex |
|---|---|
| Dígitos `0`–`9` | **30–39** |
| Mayúsculas `A`–`Z` | **41–5A** |
| Minúsculas `a`–`z` | **61–7A** (= mayúscula + 20₁₆) |
| Espacio | 20 |
| LF / CR | 0A / 0D |

### Bit de paridad
- Bit **extra** para detectar errores de **un solo bit**.
- **Paridad par:** el total de 1s (con el bit de paridad) es **par**. **Impar:** es **impar**.
- Ej.: `1000011` ("C") tiene tres 1s → par: bit = **1**; impar: bit = **0**.
- **No detecta errores dobles** y **no sabe cuál bit falló** (no corrige).
- Transmisor y receptor deben **acordar** el tipo (par o impar).

### Trama serie asíncrona (Fig. 2-5)
Línea inactiva = **1** → **bit de inicio (0)** → **7 bits ASCII (LSB primero)** → **bit de paridad** → **bit de paro (1)**.
Ej.: `#` = 23₁₆ = 0100011 (tres 1s) → paridad par = 1.

## Errores frecuentes
- Leer los residuos de la **división repetida al revés** (el primero es el **LSB**).
- Olvidar que en **BCD no existen** los grupos 1010 a 1111.
- Confundir **BCD** con **binario directo**.
- En hex, olvidar que A–F valen 10–15 al convertir a decimal.
- Agrupar de 4 en 4 **desde la izquierda** al pasar binario → hex (hay que hacerlo **desde el punto/derecha**).

## Resumen oficial del capítulo (versión corta)
1. El **hexadecimal** es una forma eficiente de representar cantidades binarias (cada dígito = 4 bits).
2. **Hex ↔ binario:** cada dígito corresponde a **4 bits**.
3. **División repetida** (entre 2 o 16) para pasar de decimal a binario o hex.
4. Con **N bits** se representan valores de **0 a 2^N − 1**.
5. **BCD:** cada dígito decimal → su equivalente binario de 4 bits.
6. **Gray:** solo cambia un bit entre patrones sucesivos.
7. **Byte** = 8 bits, **nibble** = 4 bits, **palabra** = depende del sistema.
8. **Alfanuméricos:** grupos de bits para todos los caracteres del teclado; **ASCII** es el más usado.
9. **Paridad:** se adjunta un bit especial a cada grupo transmitido.


**Siguiente capitulo: [[30_Indice_y_Resumen]]**