# Resumen Semana IV

---

## 1. Fundamentos de la Suma Binaria

La suma binaria bit a bit entre dos operandos produce dos salidas esenciales: el bit de **Suma ($S$)**, que representa el resultado que se escribe en la columna actual, y el bit de **Acarreo de salida ($C$ o $\text{Carry}$)**, que corresponde al valor transferido a la siguiente columna de mayor peso ($2 \cdot C + S = A + B$).

| Operación | Resultado Decimal | Binario | Bit Suma ($S$) | Bit Acarreo ($C$) |
| :---: | :---: | :---: | :---: | :---: |
| $0 + 0$ | $0$ | $0_2$ | $0$ | $0$ |
| $0 + 1$ | $1$ | $1_2$ | $1$ | $0$ |
| $1 + 0$ | $1$ | $1_2$ | $1$ | $0$ |
| $1 + 1$ | $2$ | $10_2$ | $0$ | $1$ |

Al operar con palabras binarias de múltiples bits, los acarreos se propagan secuencialmente de derecha a izquierda: un acarreo generado en la columna $i$ se suma como entrada en la columna $i+1$.

---

## 2. Semisumador (*Half Adder* - HA)

Diseñado exclusivamente para la columna de menor peso (columna $0$), ya que a su derecha no existe ningún acarreo entrante. Opera con dos entradas ($A$ y $B$).

### Tabla de Verdad

| $A$ | $B$ | Acarreo ($C$) | Suma ($S$) |
| :---: | :---: | :---: | :---: |
| 0 | 0 | 0 | 0 |
| 0 | 1 | 0 | 1 |
| 1 | 0 | 0 | 1 |
| 1 | 1 | 1 | 0 |

### Ecuaciones Lógicas e Implementación

* **Suma ($S$):** Vale $1$ cuando las entradas son diferentes (detección de paridad impar):
  $$S = A \oplus B = A'B + AB'$$
* **Acarreo ($C$):** Vale $1$ únicamente cuando ambas entradas son $1$:
  $$C = A \cdot B$$

*Limitación:* Es incapaz de recibir un acarreo entrante ($C_{\text{in}}$), por lo que no puede emplearse en etapas intermedias o superiores.

---

## 3. Sumador Completo (*Full Adder* - FA)

Circuito combinacional capaz de sumar tres entradas de un bit: los dos bits de los operandos ($A$ y $B$) y el bit de acarreo que proviene de la etapa previa ($C_{\text{in}}$). Produce la suma local ($S$) y el nuevo acarreo saliente ($C_{\text{out}}$).

### Tabla de Verdad

| $A$ | $B$ | $C_{\text{in}}$ | Cantidad de unos | $C_{\text{out}}$ | $S$ |
| :---: | :---: | :---: | :---: | :---: | :---: |
| 0 | 0 | 0 | 0 | 0 | 0 |
| 0 | 0 | 1 | 1 | 0 | 1 |
| 0 | 1 | 0 | 1 | 0 | 1 |
| 0 | 1 | 1 | 2 | 1 | 0 |
| 1 | 0 | 0 | 1 | 0 | 1 |
| 1 | 0 | 1 | 2 | 1 | 0 |
| 1 | 1 | 0 | 2 | 1 | 0 |
| 1 | 1 | 1 | 3 | 1 | 1 |

### Reglas de Diseño Rápido

* **Suma ($S$):** Vale $1$ cuando la cantidad de unos en las entradas es **impar** ($1$ o $3$):
  $$S = A \oplus B \oplus C_{\text{in}}$$
* **Acarreo ($C_{\text{out}}$):** Vale $1$ cuando hay **al menos dos unos** en las entradas:
  $$C_{\text{out}} = AB + AC_{\text{in}} + BC_{\text{in}} = AB + (A \oplus B)C_{\text{in}}$$

### Construcción a partir de Semisumadores

Un sumador completo puede construirse interconectando:
1. Un primer semisumador para calcular $S_1 = A \oplus B$ y $C_1 = A \cdot B$.
2. Un segundo semisumador que suma $S_1$ con $C_{\text{in}}$, obteniendo $S = S_1 \oplus C_{\text{in}}$ y $C_2 = S_1 \cdot C_{\text{in}}$.
3. Una compuerta $\text{OR}$ que combina los acarreos parciales: $C_{\text{out}} = C_1 + C_2$.

---

## 4. Sumador Paralelo en Cascada (*Ripple Carry Adder*)

Para sumar dos números binarios de $n$ bits ($A = A_{n-1}\dots A_0$ y $B = B_{n-1}\dots B_0$), se conectan $n$ sumadores completos en cascada:

* La salida $C_{\text{out}, i}$ de la etapa $i$ se introduce como $C_{\text{in}, i+1}$ de la etapa $i+1$.
* En la etapa $0$, se puede utilizar un semisumador o un sumador completo fijando $C_{\text{in}, 0} = 0$.
* **Circuito Integrado Comercial:** $74\text{LS}83$ / $74\text{LS}283$ (sumador binario de 4 bits).

### Retardo de Propagación

El bit de suma más significativo ($S_{n-1}$) y el acarreo final ($C_{\text{out}}$) no son válidos hasta que el acarreo haya terminado de propagarse a través de todas las etapas intermedias. Si cada sumador completo introduce un retardo $t_{\text{FA}}$, el tiempo total de estabilización es:

$$t_{\text{total}} \approx n \cdot t_{\text{FA}}$$

---

## 5. Circuito Sumador / Restador Multifunción

Aprovechando la aritmética de **complemento a dos** ($A - B = A + B' + 1$), un mismo sumador binario puede realizar tanto sumas como restas mediante el uso de compuertas $\text{XOR}$ como inversores controlados por una señal de modo $M$.

### La Compuerta XOR como Inversor Controlado

| Modo ($M$) | Entrada $B$ | Salida ($B \oplus M$) | Operación resultante |
| :---: | :---: | :---: | :---: |
| 0 | $0$ | $0$ | $B$ pasa idéntico (no se invierte) |
| 0 | $1$ | $1$ | $B$ pasa idéntico (no se invierte) |
| 1 | $0$ | $1$ | $B$ se invierte ($B'$) |
| 1 | $1$ | $0$ | $B$ se invierte ($B'$) |

### Conexión del Sistema

* Cada bit del operando $B_i$ entra a una compuerta $\text{XOR}$ junto con la línea de control $M$: $B_i^* = B_i \oplus M$.
* La línea $M$ se conecta simultáneamente al acarreo de entrada de la etapa cero ($C_{\text{in}, 0} = M$).
* **Cuando $M = 0$ (Suma):**
  $$B_i^* = B_i, \quad C_{\text{in}, 0} = 0 \implies \text{Salida} = A + B + 0 = A + B$$
* **Cuando $M = 1$ (Resta):**
  $$B_i^* = B_i', \quad C_{\text{in}, 0} = 1 \implies \text{Salida} = A + B' + 1 = A - B$$

---

## 6. Desbordamiento (*Overflow* - $V$)

El desbordamiento ocurre cuando el resultado de una operación aritmética con signo no puede representarse con la cantidad de bits asignada ($n$ bits en complemento a dos, rango $[-2^{n-1}, \; 2^{n-1}-1]$).

*Para 4 bits con signo:* Rango válido $[-8, \; +7]$.
* *Ejemplo de overflow:* $(+5) + (+4) = 0101_2 + 0100_2 = 1001_2 = -7_{10}$ (inválido: la suma de dos positivos dio negativo).

### Detección en Hardware

El desbordamiento **no** se mide con el acarreo final de salida ($C_{\text{out}}$). Se detecta comparando el acarreo que entra al bit de signo ($C_{n-1}$) con el acarreo que sale del bit de signo ($C_n$):

$$V = C_{n-1} \oplus C_n$$

* En un circuito de 4 bits:
  $$V = C_3 \oplus C_4$$
* Si $V = 0$: el resultado es matemáticamente correcto dentro del rango.
* Si $V = 1$: ha ocurrido desbordamiento (error en la representación con signo).
* *Distinción clave:* $C_{\text{out}} = 1$ en aritmética sin signo indica un acarreo legítimo; no implica desbordamiento en complemento a dos.

---

## 7. Comparadores de Magnitud

Circuito combinacional que compara dos palabras binarias de $n$ bits ($A$ y $B$) e indica si $A = B$, $A > B$ o $A < B$.

### 1. Condición de Igualdad ($A = B$)

Dos números son iguales si y solo si todos sus bits coincidentes son iguales. La compuerta $\text{XNOR}$ actúa como comparador de equivalencia bit a bit ($1$ si son iguales, $0$ si difieren):

$$x_i = A_i \odot B_i = (A_i \oplus B_i)' = A_i B_i + A_i' B_i'$$

Para un comparador de 4 bits:
$$(A = B) = x_3 \cdot x_2 \cdot x_1 \cdot x_0$$

### 2. Condición de Desigualdad ($A > B$)

Se inspecciona desde el bit más significativo (MSB). El primer bit en el que los números difieren define cuál es mayor:

$$(A > B) = A_3 B_3' + x_3 A_2 B_2' + x_3 x_2 A_1 B_1' + x_3 x_2 x_1 A_0 B_0'$$

* **Circuito Integrado Comercial:** $74\text{LS}85$ (comparador de magnitud de 4 bits con entradas de expansión en cascada).

---

[**Volver al Inicio**](https://github.com/hatsuja/circuitos/tree/main)
