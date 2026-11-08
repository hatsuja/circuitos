# Resumen Semana 2

---

## 1. Álgebra de Boole


### 1.1. Postulados de Huntington y Principio de Dualidad

El **Principio de Dualidad** establece que si una expresión booleana es válida, la expresión obtenida al intercambiar los operadores $+$ por $\cdot$ (y viceversa) junto con los elementos neutros $0$ por $1$ (y viceversa) también es estrictamente válida.

| Postulado | Forma OR | Forma AND (Dual) |
| :--- | :--- | :--- |
| **Identidad** | $X + 0 = X$ | $X \cdot 1 = X$ |
| **Complemento** | $X + X' = 1$ | $X \cdot X' = 0$ |
| **Conmutativa** | $X + Y = Y + X$ | $X \cdot Y = Y \cdot X$ |
| **Distributiva** | $X \cdot (Y + Z) = (X \cdot Y) + (X \cdot Z)$ | $X + (Y \cdot Z) = (X + Y) \cdot (X + Z)$ |

#### La distributiva notable: $A + BC = (A + B)(A + C)$
---

### 1.2. Tabla de Consulta de Teoremas Booleanos

| Teorema | Forma OR | Forma AND |
| :--- | :--- | :--- |
| **Elemento nulo** | $X + 1 = 1$ | $X \cdot 0 = 0$ |
| **Idempotencia** | $X + X = X$ | $X \cdot X = X$ |
| **Involución** | (X')' = X |
| **Absorción** | $X + XY = X$ | $X(X + Y) = X$ |
| **Absorción del complemento** | $X + X'Y = X + Y$ | $X(X' + Y) = XY$ |
| **Adyacencia** | $XY + XY' = X$ | $(X + Y)(X + Y') = X$ |
| **Consenso** | $XY + X'Z + YZ = XY + X'Z$ | $(X+Y)(X'+Z)(Y+Z) = (X+Y)(X'+Z)$ |

---

## 2. Leyes de De Morgan

1. **Primera Ley:** $(A \cdot B)' = A' + B'$
2. **Segunda Ley:** $(A + B)' = A' \cdot B'$

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

## 3. Minitérminos y Maxitérminos

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

## 4. Formas Canónicas

Cualquier función lógica puede representarse de forma única mediante sus minitérminos o maxitérminos:

1. **Suma de Productos Canónica (SOP - *Sum of Products*):**
   $$F = \sum m(\text{filas donde } F = 1)$$
2. **Producto de Sumas Canónico (POS - *Product of Sums*):**
   $$F = \prod M(\text{filas donde } F = 0)$$

---
