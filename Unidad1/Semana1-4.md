# Semana 1

## 1. Fundamentos de Sistemas Posicionales

Un sistema de numeración posicional se define mediante una base r, donde cada posición representa una potencia entera de dicha base:

$$N = d_n r^n + \dots + d_1 r^1 + d_0 r^0 + d_{-1} r^{-1} + \dots$$

* **Bit:** Dígito binario elemental (*binary digit*), toma valor $0$ o $1$.
* **Byte:** Agrupación ordenada de $8$ bits.
* **Potencias de 2 fundamentales:**

| Potencia | $2^7$ | $2^6$ | $2^5$ | $2^4$ | $2^3$ | $2^2$ | $2^1$ | $2^0$ |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **Valor decimal** | $128$ | $64$ | $32$ | $16$ | $8$ | $4$ | $2$ | $1$ |

### 1.1. Conversiones de Base

#### De Decimal a Binario (Divisiones sucesivas)
Se divide sucesivamente el valor decimal entre $2$ registrando los residuos hasta obtener cociente $0$. Los residuos se leen en orden inverso (del último al primero / de abajo hacia arriba):

*Ejemplo: Convertir 25 a binario:*
* $25 \div 2 = 12$, resto $= 1$ (LSB)
* $12 \div 2 = 6$, resto $= 0$
* $6 \div 2 = 3$, resto $= 0$
* $3 \div 2 = 1$, resto $= 1$
* $1 \div 2 = 0$, resto $= 1$ (MSB)

$$\implies 25_{10} = 11001_2$$
*Comprobación por pesos:* $16 + 8 + 1 = 25$.

#### Conversión por Bloques (Binario $\leftrightarrow$ Octal / Hexadecimal)
* **Hexadecimal (Base 16):** Cada dígito hexadecimal equivale exactamente a un bloque de $4$ bits ($16 = 2^4$).
* **Octal (Base 8):** Cada dígito octal equivale a un bloque de $3$ bits ($8 = 2^3$).


---

## 2. Representación de Números con Signo: Complemento a Dos ($C_2$)

En hardware no existe el signo «-» físico; por convención en complemento a dos con un ancho fijado de $n$ bits:
* **Bit más significativo (MSB = bit de la izquierda):**
  * `0`: Número positivo o cero.
  * `1`: Número negativo.

###  Obtención numero negativo $-A$
1. Escribir la magnitud en binario completando a la longitud de $n$ bits.
2. Invertir todos los bits (obtención del complemento a uno).
3. Sumar $1$ al resultado:

$$\text{Negativo}(A) = \text{NOT}(A) + 1$$

*Ejemplo (representar -5 en 8 bits):*
* $+5_{10} = 00000101_2$
* Inversión de bits: $11111010_2$
* Sumar $1$: $11111010_2 + 1 = 11111011_2$

---

## 3. Códigos Binarios

| Código | Características Principales | Aplicación Práctica |
| :--- | :--- | :--- |
| **BCD (8421)** | Codifica cada dígito decimal ($0$ a $9$) en un cuarteto de $4$ bits. Las combinaciones de $1010_2$ ($10$) a $1111_2$ ($15$) no son válidas. | Controladores de displays de 7 segmentos y voltímetros digitales. |
| **Código Gray** | Código no ponderado de distancia unitaria: entre dos estados consecutivos **solo cambia 1 bit**, evitando transiciones intermedias espurias. | Encoders de posición angular, sensores industriales y ordenamiento de mapas de Karnaugh. |
| **ASCII** | Código alfanumérico estándar de $7$ bits ($128$ combinaciones posibles) que codifica letras, números y caracteres de control. | Transmisión de texto y comunicación serial (ejemplo: `'A'` $= 65_{10} = 01000001_2 = 41_{16}$). |

---

## 4. Compuertas Lógicas

### Niveles Eléctricos
* Los circuitos electrónicos digitales operan con **rangos de voltaje**.
* Nivel lógico `0`: Rango de tensión cercano a $0\text{ V}$ (GND).
* Nivel lógico `1`: Rango de tensión cercano a $+5\text{ V}$ ($V_{CC}$).
* Entre ambos niveles existe una **zona de indeterminación** que debe evitarse en operación estacionaria.

### Compuertas Lógicas y Tablas de Verdad

Para un sistema con $n$ variables de entrada, la tabla de verdad consta de $2^n$ filas.

#### 1. Compuerta NOT (Inversor)
Invierte la señal de entrada: $Y = A'$ o $Y = \overline{A}$.

| $A$ | $Y$ |
| :-: | :-: |
| 0 | 1 |
| 1 | 0 |

#### 2. Compuerta AND (Producto Lógico)
La salida es `1` **únicamente** cuando todas sus entradas son `1`: $Y = A \cdot B$.

| $A$ | $B$ | $Y$ |
| :-: | :-: | :-: |
| 0 | 0 | 0 |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 1 | 1 | 1 |

#### 3. Compuerta OR (Suma Lógica)
La salida es `1` si al menos una de las entradas es `1`: $Y = A + B$.

| $A$ | $B$ | $Y$ |
| :-: | :-: | :-: |
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 1 |

#### 4. Compuerta XOR (OR Exclusiva)
La salida es `1` si las entradas son diferentes; es `0` si son iguales: $Y = A \oplus B = A'B + AB'$.

| $A$ | $B$ | $Y$ |
| :-: | :-: | :-: |
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

---

## 6. Modelado de Enunciados a Compuertas Lógicas

* **«La alarma suena solo si las dos puertas están abiertas»:** Ambas condiciones deben cumplirse simultáneamente $\implies$ **AND** ($Y = A \cdot B$).
* **«La luz se enciende si se pulsa cualquiera de los pulsadores»:** Al menos una señal activa la salida $\implies$ **OR** ($Y = A + B$).
* **«El sistema se activa si las dos selecciones son distintas»:** Detector de discrepancia $\implies$ **XOR** ($Y = A \oplus B$).
* **«El actuador se apaga cuando el sensor se activa»:** Inversión de estado $\implies$ **NOT** ($Y = S'$).

---

## 7. Ejemplo de Análisis Combinacional: Tabla de Verdad

Dada la función booleana de tres variables:

$$Y = A \cdot B + C'$$

Número de combinaciones: $2^3 = 8$ filas ($0$ a $7$).

| Fila Decimal | $A$ | $B$ | $C$ | $A \cdot B$ | $C'$ | $Y = A \cdot B + C'$ |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| 0 | 0 | 0 | 0 | 0 | 1 | **1** |
| 1 | 0 | 0 | 1 | 0 | 0 | **0** |
| 2 | 0 | 1 | 0 | 0 | 1 | **1** |
| 3 | 0 | 1 | 1 | 0 | 0 | **0** |
| 4 | 1 | 0 | 0 | 0 | 1 | **1** |
| 5 | 1 | 0 | 1 | 0 | 0 | **0** |
| 6 | 1 | 1 | 0 | 1 | 1 | **1** |
| 7 | 1 | 1 | 1 | 1 | 0 | **1** |
