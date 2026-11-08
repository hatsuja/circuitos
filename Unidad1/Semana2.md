# Semana 2

---

## 1. Álgebra de Boole


### 3.1. Postulados de Huntington y Principio de Dualidad

El **Principio de Dualidad** establece que si una expresión booleana es válida, la expresión obtenida al intercambiar los operadores $+$ por $\cdot$ (y viceversa) junto con los elementos neutros $0$ por $1$ (y viceversa) también es estrictamente válida.

| Postulado | Forma OR | Forma AND (Dual) |
| :--- | :--- | :--- |
| **Identidad** | $X + 0 = X$ | $X \cdot 1 = X$ |
| **Complemento** | $X + X' = 1$ | $X \cdot X' = 0$ |
| **Conmutativa** | $X + Y = Y + X$ | $X \cdot Y = Y \cdot X$ |
| **Distributiva** | $X \cdot (Y + Z) = (X \cdot Y) + (X \cdot Z)$ | $X + (Y \cdot Z) = (X + Y) \cdot (X + Z)$ |

#### La distributiva notable: $A + BC = (A + B)(A + C)$
Demostración algebraica de su validez partiendo del lado derecho:
$$
(A + B)(A + C) = AA + AC + AB + BC \quad \text{(Distributiva estándar)}
$$
$$
= A + AC + AB + BC \quad \text{(Idempotencia: } AA = A\text{)}
$$
$$
= A(1 + C + B) + BC \quad \text{(Factor común } A\text{)}
$$
$$
= A(1) + BC = A + BC \quad \text{(Elemento nulo: } 1 + \dots = 1\text{)}
$$

---

### 3.2. Tabla de Consulta de Teoremas Booleanos

| Teorema | Forma OR | Forma AND |
| :--- | :--- | :--- |
| **Elemento nulo** | $X + 1 = 1$ | $X \cdot 0 = 0$ |
| **Idempotencia** | $X + X = X$ | $X \cdot X = X$ |
| **Involución** | \multicolumn{2}{c|}{$(X')' = X$} |
| **Absorción** | $X + XY = X$ | $X(X + Y) = X$ |
| **Absorción del complemento** | $X + X'Y = X + Y$ | $X(X' + Y) = XY$ |
| **Adyacencia** | $XY + XY' = X$ | $(X + Y)(X + Y') = X$ |
| **Consenso** | $XY + X'Z + YZ = XY + X'Z$ | $(X+Y)(X'+Z)(Y+Z) = (X+Y)(X'+Z)$ |

---

## 4. Leyes de De Morgan

Permiten transformar productos negados en sumas de complementos y sumas negadas en productos de complementos:

1. **Primera Ley:** $(A \cdot B)' = A' + B'$
2. **Segunda Ley:** $(A + B)' = A' \cdot B'$

*Regla mnemotécnica:* **«Rompe la barra, cambia el operador»**.

### Generalización a $n$ variables
$$
(A + B + C + \dots + N)' = A' \cdot B' \cdot C' \dots N'
$$
$$
(A \cdot B \cdot C \dots N)' = A' + B' + C' + \dots + N'
$$

### Equivalencias esquemáticas
* Una compuerta $\text{NAND}$ equivale a una compuerta $\text{OR}$ con entradas invertidas (*Negative-OR*).
* Una compuerta $\text{NOR}$ equivale a una compuerta $\text{AND}$ con entradas invertidas (*Negative-AND*).

---

## 5. Minitérminos y Maxitérminos

Para un sistema de $n$ variables de entrada, existen $2^n$ combinaciones posibles.

* **Minitérmino ($m_i$):** Término producto ($\text{AND}$) que contiene a **todas** las variables del sistema. Vale $1$ **únicamente** en la combinación decimal $i$.
  * Variable con valor $1 \implies$ entra directa ($X$).
  * Variable con valor $0 \implies$ entra complementada ($X'$).
* **Maxitérmino ($M_i$):** Término suma ($\text{OR}$) que contiene a **todas** las variables del sistema. Vale $0$ **únicamente** en la combinación decimal $i$.
  * Variable con valor $0 \implies$ entra directa ($X$).
  * Variable con valor $1 \implies$ entra complementada ($X'$).

### Tabla Canónica para 3 Variables ($A, B, C$)

| Fila ($i$) | $A$ | $B$ | $C$ | Minitérmino $m_i$ (vale $1$) | Maxitérmino $M_i$ (vale $0$) |
| :---: | :---: | :---: | :---: | :---: | :---: |
| 0 | 0 | 0 | 0 | $m_0 = A'B'C'$ | $M_0 = A + B + C$ |
| 1 | 0 | 0 | 1 | $m_1 = A'B'C$ | $M_1 = A + B + C'$ |
| 2 | 0 | 1 | 0 | $m_2 = A'BC'$ | $M_2 = A + B' + C$ |
| 3 | 0 | 1 | 1 | $m_3 = A'BC$ | $M_3 = A + B' + C'$ |
| 4 | 1 | 0 | 0 | $m_4 = AB'C'$ | $M_4 = A' + B + C$ |
| 5 | 1 | 0 | 1 | $m_5 = AB'C$ | $M_5 = A' + B + C'$ |
| 6 | 1 | 1 | 0 | $m_6 = ABC'$ | $M_6 = A' + B' + C$ |
| 7 | 1 | 1 | 1 | $m_7 = ABC$ | $M_7 = A' + B' + C'$ |

---

## 6. Formas Canónicas

Cualquier función lógica puede representarse de forma única mediante sus minitérminos o maxitérminos:

1. **Suma de Productos Canónica (SOP - *Sum of Products*):**
   $$F = \sum m(\text{filas donde } F = 1)$$
2. **Producto de Sumas Canónico (POS - *Product of Sums*):**
   $$F = \prod M(\text{filas donde } F = 0)$$

---

## 7. Procedimiento de Simplificación y Costo en Hardware

La simplificación booleana busca reducir el costo de implementación física (menos compuertas, menos pines y menos circuitos integrados).

### Procedimiento Sistemático con Álgebra
1. **Buscar pares adyacentes:** $XY + XY' = X$.
2. **Duplicar términos mediante idempotencia ($X = X + X$):** Permite usar un mismo término para asociarlo con múltiples pares adyacentes.
3. **Aplicar absorción:** $X + XY = X$.
4. **Aplicar absorción del complemento:** $X + X'Y = X + Y$.
5. **Eliminar términos redundantes por consenso:** $XY + X'Z + YZ = XY + X'Z$.

### Caso de Estudio: Circuito de la Alarma

Requerimiento: activarse cuando al menos dos sensores ($A, B, C$) estén activos.

$$
F = \sum m(3, 5, 6, 7) = A'BC + AB'C + ABC' + ABC
$$

*Simplificación aplicando duplicación por idempotencia ($ABC = ABC + ABC + ABC$):*
$$
F = (A'BC + ABC) + (AB'C + ABC) + (ABC' + ABC)
$$
$$
F = BC(A' + A) + AC(B' + B) + AB(C' + C)
$$
$$
F = AB + AC + BC
$$

### Comparación de Costo de Hardware

| Parámetro | Versión Canónica ($\sum m$) | Versión Simplificada |
| :--- | :--- | :--- |
| **Expresión** | $A'BC + AB'C + ABC' + ABC$ | $AB + AC + BC$ |
| **Compuertas requeridas** | $3 \text{ NOT} + 4 \text{ AND}_3 + 1 \text{ OR}_4$ | $3 \text{ AND}_2 + 1 \text{ OR}_3$ |
| **Circuitos Integrados (74LS)** | **$4\text{ CI}$** ($7404 + 2\times 7411 + 7432$) | **$2\text{ CI}$** ($7408 + 7432$) |
| **Ahorro de hardware** | Línea base | **$50\%$ de reducción en chips** |
