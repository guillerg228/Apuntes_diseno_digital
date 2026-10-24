
**Indice: [[30_Indice_y_Resumen]]**
**Seccion anterior: [[31_Algebra_booleana_Tablas_OR_AND_NOT]]**
# Expresiones, evaluación, implementación, NOR y NAND

## 3-6 Describir circuitos en forma algebraica

Cualquier circuito se describe con OR, AND y NOT. Se escribe la expresión **compuerta por compuerta**, de izquierda a derecha.

- (a): AND(A,B) → OR con C ⇒ **x = AB + C**
- (b): OR(A,B) → AND con C ⇒ **x = (A + B)·C** (los paréntesis son necesarios)

![[03_Descripcion_de_Circuitos_Logicos/img/13.png|404]]
### Precedencia de operadores
1. **Paréntesis** primero.
2. **AND antes que OR** (igual que × antes que + en álgebra normal).
   - `A + BC` = A OR (B AND C)  ≠  `(A + B)C`

### Circuitos con inversores
La salida de un inversor es la expresión de entrada con barra encima.
- Inversor en A → OR con B ⇒ **x = A' + B**
- OR de A y B → inversor ⇒ **x = (A + B)'**

> ⚠️ **`(A + B)'` ≠ `A' + B'`** y **`(AB)'` ≠ `A'B'`**. La barra larga invierte toda la expresión; las barras cortas invierten cada variable por separado.

![[03_Descripcion_de_Circuitos_Logicos/img/15.png]]

**Fig. 3-15 :**
- (a) `x = A'BC(A + D)'`
- (b) `x = {D + [(A + B)C]'}·E`

## 3-7 Evaluar la salida de un circuito

Se sustituyen los valores en la expresión. **Reglas de orden:**
1. Primero las **inversiones de términos individuales**.
2. Luego lo que esté entre **paréntesis**.
3. **AND antes que OR** (salvo paréntesis).
4. Si hay barra sobre una expresión, se hacen las operaciones de adentro y **después se invierte**.

Ejemplo: `x = A'BC(A + D)'` con A=0, B=1, C=1, D=1 → 1·1·1·(0+1)' = 1·1·1·0 = **0**.
Con todas las entradas en 1: circuito (a) x = 0, circuito (b) x = 1.

### Análisis con tabla de verdad (la mejor forma)
Ventajas: analizas **una compuerta a la vez**, puedes **verificar tu trabajo** y la tabla sirve luego para **detectar fallas**.

Procedimiento con x = A'B + BC:
1. Lista todas las combinaciones de entradas (orden binario).
2. Agrega una columna por cada **nodo intermedio** (u = A', v = A'B, w = BC).
3. Llena columna por columna y al final x = v + w.

| A | B | C | u=A' | v=A'B | w=BC | x=v+w |
|---|---|---|---|---|---|---|
| 0 | 0 | 0 | 1 | 0 | 0 | 0 |
| 0 | 0 | 1 | 1 | 0 | 0 | 0 |
| 0 | 1 | 0 | 1 | 1 | 0 | 1 |
| 0 | 1 | 1 | 1 | 1 | 1 | 1 |
| 1 | 0 | 0 | 0 | 0 | 0 | 0 |
| 1 | 0 | 1 | 0 | 0 | 0 | 0 |
| 1 | 1 | 0 | 0 | 0 | 0 | 0 |
| 1 | 1 | 1 | 0 | 0 | 1 | 1 |

### Detección de fallas con la tabla
Prueba cada combinación; si alguna da salida incorrecta, compara cada nodo intermedio con el valor de la tabla:
- Nodo **correcto** → el problema está **a la derecha** de ese nodo.
- Nodo **incorrecto** → el problema está **a la izquierda** (o el nodo está en corto con algo).

![[03_Descripcion_de_Circuitos_Logicos/img/16.png|548]]

**Ejemplo 3-6:** tabla para `x = A'BC(A + D)'` con t = A'BC y v = (A+D)'. Resultado: **x = 1 solo cuando ABCD = 0110**.

## 3-8 Implementar circuitos a partir de una expresión

Se dibuja el circuito directamente desde la expresión:
- `x = A·B·C` → una AND de 3 entradas.
- `x = A' + B` → OR de 2 entradas con inversor en A.
- Una suma de productos (ej. `y = AC + BC' + A'BC` ) → **un AND por cada término** (con inversores donde haya barras) y todos entran a un **OR**.
- Producto de sumas (ej. `x = (A + B)(B' + C)` ) → una OR por cada paréntesis y una AND al final.

![[03_Descripcion_de_Circuitos_Logicos/img/17.png|589]]

![[03_Descripcion_de_Circuitos_Logicos/img/18.png|535]]

## 3-9 Compuertas NOR y NAND

### NOR = OR + inversor
- **x = (A + B)'** — símbolo OR con **burbuja** en la salida.
- Salida en **ALTO solo cuando todas las entradas están en BAJO**; en BAJO si cualquier entrada es ALTO.

| A | B | OR | **NOR** |
|---|---|---|---|
| 0 | 0 | 0 | **1** |
| 0 | 1 | 1 | **0** |
| 1 | 0 | 1 | **0** |
| 1 | 1 | 1 | **0** |
![[03_Descripcion_de_Circuitos_Logicos/img/19.png|358]]
### NAND = AND + inversor
- **x = (AB)'** — símbolo AND con **burbuja** en la salida.
- Salida en **BAJO solo cuando todas las entradas están en ALTO**; en ALTO en cualquier otro caso.

| A | B | AND | **NAND** |
|---|---|---|---|
| 0 | 0 | 0 | **1** |
| 0 | 1 | 0 | **1** |
| 1 | 0 | 0 | **1** |
| 1 | 1 | 1 | **0** |
![[03_Descripcion_de_Circuitos_Logicos/img/22.png|382]]

### Tabla resumen de las 6 compuertas (2 entradas)

| A | B | OR | AND | NOR | NAND |
|---|---|---|---|---|---|
| 0 | 0 | 0 | 0 | 1 | 1 |
| 0 | 1 | 1 | 0 | 0 | 1 |
| 1 | 0 | 1 | 0 | 0 | 1 |
| 1 | 1 | 1 | 1 | 0 | 0 |
(NOT: A' invierte la entrada.)





**Siguiente seccion: [[33_Teoremas_Demorgan_y_Universalidad]]**