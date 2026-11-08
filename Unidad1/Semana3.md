# Semana III
---

## 1. Mapas de Karnaugh (K-Maps)

Un mapa de Karnaugh es una representación visual de la tabla de verdad en forma de matriz bidimensional. Su objetivo es aplicar de forma directa y visual el teorema de adyacencia lógica:

$$
X \cdot Y + X \cdot Y' = X(Y + Y') = X
$$

### 1.1. El Rol Crucial del Código Gray

Las filas y columnas de un mapa de Karnaugh se ordenan siguiendo la secuencia de **Código Gray** ($00, 01, 11, 10$):
* Entre dos celdas contiguas (horizontal o verticalmente) **solo cambia 1 bit**.
* Gracias a este ordenamiento, dos celdas adyacentes físicamente en el mapa representan minitérminos lógicamente adyacentes.
* *Nota fundamental:* La columna/fila $11$ antecede a la $10$, por lo que $m_3$ se ubica antes que $m_2$, y $m_7$ antes que $m_6$.

---

### 1.2. Estructura de Mapas de 3 y 4 Variables

#### Mapa de 3 Variables ($A, B, C$)
* Filas: Variable $A \in \{0, 1\}$.
* Columnas: Variables $BC \in \{00, 01, 11, 10\}$.
* Total: $2^3 = 8$ celdas.

| $A \backslash BC$ | $00$ | $01$ | $11$ | $10$ |
| :---: | :---: | :---: | :---: | :---: |
| **0** | $m_0$ | $m_1$ | $m_3$ | $m_2$ |
| **1** | $m_4$ | $m_5$ | $m_7$ | $m_6$ |

#### Mapa de 4 Variables ($A, B, C, D$)
* Filas: Variables $AB \in \{00, 01, 11, 10\}$.
* Columnas: Variables $CD \in \{00, 01, 11, 10\}$.
* Total: $2^4 = 16$ celdas.

| $AB \backslash CD$ | $00$ | $01$ | $11$ | $10$ |
| :---: | :---: | :---: | :---: | :---: |
| **00** | $m_0$ | $m_1$ | $m_3$ | $m_2$ |
| **01** | $m_4$ | $m_5$ | $m_7$ | $m_6$ |
| **11** | $m_{12}$ | $m_{13}$ | $m_{15}$ | $m_{14}$ |
| **10** | $m_8$ | $m_9$ | $m_{11}$ | $m_{10}$ |

---

### 1.3. Reglas Estrictas de Agrupamiento

1. **Tamaño en potencia de dos ($2^k$):** Los grupos de unos (`1`) solo pueden contener $1, 2, 4, 8$ o $16$ celdas.
2. **Forma rectangular o cuadrada:** Se agrupa únicamente en bloques rectangulares. Se prohíben agrupaciones en forma de «L», diagonales o de tamaños no potencia de dos (grupos de 3, 5, 6 celdas son inválidos).
3. **Relación entre tamaño y simplificación:** Un grupo de $2^k$ celdas en una función de $n$ variables elimina exactamente $k$ variables, produciendo un término producto con $n - k$ literales.
4. **Adyacencia toroidal (bordes continuos):**
   * El extremo superior es adyacente al inferior.
   * El extremo izquierdo es adyacente al derecho.
   * **Las cuatro esquinas** ($m_0, m_2, m_8, m_{10}$) forman un grupo válido de 4 celdas cuya simplificación directa es $B'D'$.

---

## 2. Implicantes Primos y Esenciales

* **Implicante Primo (PI):** Cualquier agrupación rectangular válida de tamaño $2^k$ que no puede fusionarse en un grupo mayor.
* **Implicante Primo Esencial (EPI):** Es un implicante primo que cubre **al menos un `1` que ningún otro grupo puede cubrir**. 
* **Regla de minimalidad:** Todos los implicantes primos esenciales forman parte **obligatoria** de la función mínima final. Si tras seleccionar los EPI quedan unos por cubrir, se seleccionan los implicantes primos secundarios que cubran el resto con el menor costo de literales.

---

## 3. Condiciones Indiferentes (*Don't Cares* / $X$)

En muchas aplicaciones prácticas existen combinaciones de entrada que **nunca ocurrirán** (por ejemplo, los valores $1010_2$ a $1111_2$ en un decodificador BCD) o cuyas salidas no tienen efecto sobre el sistema. Estas entradas se representan con una «X»:

$$
F = \sum m(\text{minitérminos}) + d(\text{indiferentes})
$$

### Reglas para el uso de las $X$ en el K-Map

| Caso | Acción | Justificación |
| :--- | :--- | :--- |
| **Como 1** | Se incluye en el grupo. | Si agranda un lazo de `1`s (ej. pasar de 2 a 4 celdas), reduce la cantidad de literales del término. |
| **Como 0** | Se ignora. | Ninguna $X$ está obligada a ser cubierta si no ayuda a simplificar un `1`. |
| **Nunca sola** | **Prohibido.** | Un grupo formado exclusivamente por celdas $X$ añade un término redundante sin beneficio. |

---

## 4. Universalidad de Compuertas NAND y NOR

Una compuerta es **universal** si permite construir por sí sola cualquier función lógica sin recurrir a otros tipos de compuertas.

### 4.1. Construcción de Operaciones Básicas

| Operación | Implementación con Solo $\text{NAND}$ | Implementación con Solo $\text{NOR}$ |
| :---: | :---: | :---: |
| **$\text{NOT}(A)$** | $(A \cdot A)' = A'$ | $(A + A)' = A'$ |
| **$\text{AND}(A, B)$** | $((A \cdot B)')' = A \cdot B$ | $(A' + B')' = A \cdot B$ |
| **$\text{OR}(A, B)$** | $(A' \cdot B')' = A + B$ | $((A + B)')' = A + B$ |

---

### 4.2. Conversión Sistemática de Niveles

#### Suma de Productos (SOP) a Red $\text{NAND}-\text{NAND}$
Dada una suma de términos producto, se aplica doble negación y leyes de De Morgan:

$$
F = P_1 + P_2 \iff F = \overline{\overline{P_1 + P_2}} = \overline{\overline{P_1} \cdot \overline{P_2}}
$$

* **Primer nivel:** Compuertas $\text{NAND}$ para calcular cada producto negado ($\overline{P_i}$).
* **Segundo nivel:** Una compuerta $\text{NAND}$ final que agrupa los resultados del primer nivel.
* **Inversión de literales:** Si una variable entra negada (ej. $B'$), se genera conectando ambas entradas de una compuerta $\text{NAND}$ a la variable $B$.

#### Producto de Sumas (POS) a Red $\text{NOR}-\text{NOR}$
De forma análoga y dual, para un producto de sumas:

$$
F = S_1 \cdot S_2 \iff F = \overline{\overline{S_1 \cdot S_2}} = \overline{\overline{S_1} + \overline{S_2}}
$$

* **Criterio de selección:**
  * Si la forma mínima es **SOP** $\implies$ Conviene implementar en topología **$\text{NAND}-\text{NAND}$**.
  * Si la forma mínima es **POS** $\implies$ Conviene implementar en topología **$\text{NOR}-\text{NOR}$**.

---

## 5. Optimización y Métricas en Hardware (Familia TTL 74LS)

En ingeniería digital, la métrica final de diseño no es solo la cantidad de literales en papel, sino el **conteo total de chips (circuitos integrados)** en la placa física:

* Una solución clásica con compuertas surtidas ($\text{AND}$, $\text{OR}$, $\text{NOT}$) requiere abrir varios encapsulados:
  * $74\text{LS}04$ ($6$ inversores $\text{NOT}$)
  * $74\text{LS}08$ ($4$ compuertas $\text{AND}$ de 2 entradas)
  * $74\text{LS}32$ ($4$ compuertas $\text{OR}$ de 2 entradas)
* Al unificar todo el diseño bajo la universalidad $\text{NAND}$, se emplea un único tipo de circuito integrado:
  * $74\text{LS}00$ ($4$ compuertas $\text{NAND}$ de 2 entradas)
* **Resultado:** Reducción drástica del área de circuito impreso, simplificación del inventario de componentes y menor retardo de propagación acumulado.
