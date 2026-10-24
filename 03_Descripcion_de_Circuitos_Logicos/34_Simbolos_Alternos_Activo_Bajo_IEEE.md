
**Indice: [[30_Indice_y_Resumen]]**
**Seccion anterior: [[33_Teoremas_Demorgan_y_Universalidad]]**
# Símbolos alternos, activo en ALTO/BAJO, IEEE/ANSI

## 3-13 Representaciones alternas de compuertas

Cada compuerta tiene un **símbolo estándar** y un **símbolo alternativo** (**son el mismo circuito físico**). Se obtiene el alternativo así:

1. **Invertir todas las entradas y la salida** (agregar burbujas donde no hay, quitarlas donde sí hay).
2. **Cambiar la operación**: AND ↔ OR (el inversor no cambia de operación).

![[33.png]]

| Compuerta | Símbolo estándar | Símbolo alternativo | Equivalencia (DeMorgan) |
|---|---|---|---|
| AND | AND | OR con burbujas en entradas **y** salida | `A·B = (A' + B')'` |
| OR | OR | AND con burbujas en entradas **y** salida | `A + B = (A'·B')'` |
| NAND | AND con burbuja en salida | OR con burbujas en las entradas | `(AB)' = A' + B'` |
| NOR | OR con burbuja en salida | AND con burbujas en las entradas | `(A + B)' = A'·B'` |
| NOT | triángulo con burbuja en salida | triángulo con burbuja en entrada | `A'` |

**Puntos a recordar:**
1. Las equivalencias valen para cualquier número de entradas.
2. **Símbolos estándar: sin burbujas en las entradas. Símbolos alternativos: con burbujas en las entradas.**
3. Estándar y alternativo = **mismo circuito físico**.
4. NAND y NOR son inversoras (burbuja en entrada **o** salida); AND y OR son no inversoras (el alternativo tiene burbujas en **ambos** lados).

### Niveles lógicos activos
- **Sin burbuja** → activa en **ALTO** (el estado ALTO es el activo).
- **Con burbuja** → activa en **BAJO** (el estado BAJO es el activo).

### Cómo interpretar un símbolo
1. Mira qué nivel (0 o 1) es el activo en las entradas y cuál en la salida.
2. La salida pasa a su estado activo cuando:
   - **todas** las entradas están activas → símbolo **AND**
   - **cualquiera** de las entradas está activa → símbolo **OR**

| Compuerta y símbolo | Interpretación |
|---|---|
| NAND estándar | La salida cambia a **BAJO** solo cuando **todas** las entradas están en ALTO |
| NAND alternativo | La salida cambia a **ALTO** cuando **cualquier** entrada está en BAJO |
| OR estándar | La salida cambia a **ALTO** cuando **cualquier** entrada está en ALTO |
| OR alternativo | La salida cambia a **BAJO** solo cuando **todas** las entradas están en BAJO |
| NOR estándar | La salida cambia a **BAJO** cuando **cualquier** entrada está en ALTO |
| NOR alternativo | La salida cambia a **ALTO** solo cuando **todas** las entradas están en BAJO |
| AND estándar | La salida cambia a **ALTO** solo cuando **todas** las entradas están en ALTO |
| AND alternativo | La salida cambia a **BAJO** cuando **cualquier** entrada está en BAJO |

(Las dos interpretaciones de cada compuerta son **maneras distintas de decir lo mismo**.)

## 3-14 ¿Cuál representación usar?

**Regla general para dibujar esquemáticos:**
> Siempre que sea posible, elige los símbolos de modo que **salidas con burbuja se conecten a entradas con burbuja**, y **salidas sin burbuja a entradas sin burbuja**.

Así se puede **seguir la señal** y entender el circuito sin calcular tablas.

### Ejemplo: tres NAND, `Z = AB + CD`
- **Versión (b), Z activa en ALTO:** NAND 3 con símbolo alternativo (OR con burbujas). Z pasa a ALTO cuando **A=B=1 o C=D=1**.
- **Versión (c), Z activa en BAJO:** NAND 1 y 2 con símbolo alternativo. Z pasa a BAJO solo cuando (A **o** B en BAJO) **y** (C **o** D en BAJO).
- Ambas dan la misma tabla de verdad (Z = 1 si AB = 1 o CD = 1).
- **¿Cuál usar?** Depende de si la acción posterior ocurre cuando Z = 1 (activa en ALTO → versión b) o cuando Z = 0 (activa en BAJO → versión c).

![[03_Descripcion_de_Circuitos_Logicos/img/36.png|587]]


### Niveles asignados / no asignados

Es evidente que los términos asignado y no asignado son sinónimos de activo e
inactivo, respectivamente:

| Término | Significado |
|---|---|
| **Asignado** (asserted) | señal en su estado **activo** |
| **No asignado** (unasserted) | señal en su estado **inactivo** |

### Etiquetado de señales
- **Activa en BAJO:** nombre con **barra superior** (la barra superior es sólo una manera de enfatizar que estas señales son activas en BAJO:

![[act_b.png]]

- **Señales biestado:** tienen función en ambos niveles, y se etiquetan con ambos nombres, ej. **RD/WR**: ALTO = lectura (RD), BAJO = escritura (WR).


## 3-15 Símbolos IEEE/ANSI (Estándar 91-1984)

- Usa **símbolos rectangulares** para todas las compuertas y circuitos.
- Una **notación de dependencia** dentro del rectángulo indica cómo dependen las salidas de las entradas.
- El pequeño **triángulo** (en vez de la burbuja) indica inversión / activo en BAJO.

| Compuerta | Notación dentro del rectángulo |
|---|---|
| AND | **&** (salida activa cuando todas las entradas están activas) |
| OR | **≥1** (salida activa cuando cualquier entrada está activa) |
| NOT | **1** (una sola entrada) + triángulo en la salida |
| NAND | & + triángulo de inversión en la salida |
| NOR | ≥1 + triángulo de inversión en la salida |

![[03_Descripcion_de_Circuitos_Logicos/img/41.png|461]]

El estándar IEEE/ANSI aún no es muy aceptado, aunque los libros de datos de CI suelen incluir **ambos**.
Una NOR con salida activa en ALTO en IEEE/ANSI = rectángulo con **&** y triángulos en las entradas 

## 3-16 Resumen de métodos para describir circuitos lógicos

Seis formas de describir una función lógica:
1. Declaraciones lógicas en **lenguaje natural**
2. **Tablas de verdad**
3. **Símbolos tradicionales**
4. **Símbolos IEEE/ANSI**
5. **Expresiones de álgebra booleana**
6. **Diagramas de tiempos**

**Ejemplo: La siguiente expresión describe la manera en que un circuito lógico necesita operar para poder controlar un indicador de advertencia del cinturón de seguridad de un
automóvil.

>Si el conductor está presente Y (AND) el conductor NO (NOT) tiene el cinturón enganchado Y (AND) el interruptor de encendido está activado, ENTONCES encender la luz de advertencia.

Describa el circuito utilizando álgebra booleana, diagramas esquemáticos con sím-
bolos lógicos, tablas de verdad y diagramas de tiempos.

`luz_advertencia = conductor_presente · cinturon_enganchado' · interruptor_encendido`

| conductor_presente | cinturon_enganchado | interruptor_encendido | luz_advertencia |
|---|---|---|---|
| 0 | 0 | 0 | 0 |
| 0 | 0 | 1 | 0 |
| 0 | 1 | 0 | 0 |
| 0 | 1 | 1 | 0 |
| 1 | 0 | 0 | 0 |
| **1** | **0** | **1** | **1** |
| 1 | 1 | 0 | 0 |
| 1 | 1 | 1 | 0 |

![[42ab.png]]

![[42c.png]]

Cada método sirve mejor según la situación; lo importante es **poder describir y comunicar** la operación de un sistema digital.






**Siguiente seccion: [[35_HDL_VHDL]]**