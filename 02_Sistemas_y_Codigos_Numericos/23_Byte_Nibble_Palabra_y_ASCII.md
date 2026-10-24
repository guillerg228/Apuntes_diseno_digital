
**Indice: [[20_Indice_y_Resumen]]**
**Seccion anterior: [[23_Byte_Nibble_Palabra_y_ASCII]]**
# Byte, nibble, palabra y ASCII

## 2-7 Byte, nibble y palabra

| Término | Definición |
|---|---|
| **Bit** | 1 dígito binario |
| **Nibble** | **4 bits** (½ byte; = 1 dígito hex = 1 dígito BCD) |
| **Byte** | **8 bits** (unidad básica de datos en microcomputadoras) |
| **Palabra** | Grupo de bits con el que opera un sistema; depende del **ancho de la ruta de datos** (ej. microondas: 8 bits; PC de escritorio: 64 bits) |

**Ejemplos:**
- 32 bits = 4 bytes.
- 2 bytes (16 bits) → valor máximo = 2¹⁶ − 1 = **65,535**.
- 846569 en BCD: 6 dígitos × 4 = 24 bits = **3 bytes** → `1000 0100 | 0110 0101 | 0110 1001`.
- Un byte tiene **2 nibbles**. El nibble menos significativo de `1001 0101` es `0101` = 5.

**Repaso:** 235₁₀ necesita 1 byte · mayor decimal en BCD con 2 bytes = 9999 · un nibble representa 1 dígito hex · 1 nibble por dígito BCD.

## 2-8 Códigos alfanuméricos

Representan **letras, números, signos de puntuación y funciones de control** (todo lo que hay en el teclado).

### ASCII (American Standard Code for Information Interchange)
- Código de **7 bits** → 2⁷ = **128 códigos**.
- Se guarda como **byte** agregando un 0 al MSB (**relleno con 0s**).
- Para obtener el binario: convertir el valor hex a binario y tomar los 7 bits menos significativos.

**Ejemplo 2-13:** `\` = 5C₁₆ = 0101 1100 → ASCII de 7 bits = **1011100**. 

### Referencia rápida (Tabla 2-4 condensada)

| Grupo | Hex | Decimal |
|---|---|---|
| Dígitos `0`–`9` | 30–39 | 48–57 |
| Mayúsculas `A`–`Z` | 41–5A | 65–90 |
| Minúsculas `a`–`z` | 61–7A | 97–122 |
| Espacio | 20 | 32 |
| NUL (nulo) | 00 | 0 |
| Avance de línea (LF) | 0A | 10 |
| Retorno de carro (CR) | 0D | 13 |
| Escape (ESC) | 1B | 27 |
| Suprimir (DEL) | 7F | 127 |

Otros útiles: `!`=21 · `#`=23 · `$`=24 · `(`=28 · `)`=29 · `+`=2B · `-`=2D · `/`=2F · `=`=3D · `>`=3E · `@`=40 · `\`=5C.

**Trucos:**
- Dígito `n` en ASCII = `30₁₆ + n` (ej. `3` = 33).
- Minúscula = mayúscula + `20₁₆` (ej. `A`=41, `a`=61).
- Los códigos 00–1F son caracteres de control (no imprimibles).

![[4t.png]]



**Siguiente seccion: [[24_Paridad_y_Aplicaciones]]**
