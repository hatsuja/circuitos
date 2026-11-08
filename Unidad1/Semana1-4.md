#Diseño de Circuitos
---

## Semana 1: Sistemas Numéricos, Códigos y Compuertas Lógicas

### 1. Conversión
* **Decimal a Binario:** Divide el número para 2 una y otra vez anotando los residuos. Al final, lee los residuos de abajo hacia arriba.
* **El atajo de los bloques (Binario $\leftrightarrow$ Hexa / Octal):**
  * Cada dígito **hexadecimal** equivale exactamente a un paquete de **4 bits** ($16 = 2^4$).  
    *Ejemplo:* $1110 \; 0101_2 \rightarrow \text{E}5_{16}$.
  * Cada dígito **octal** equivale a un paquete de **3 bits** ($8 = 2^3$).  
    *Ejemplo:* $011 \; 100 \; 101_2 \rightarrow 345_8$.

### 2. Números negativos: Complemento a Dos ($C_2$)
Los circuitos no tienen un signo «$-$» físico. Por eso, el bit de la izquierda (**MSB**) nos dice el signo: **0 es positivo** y **1 es negativo**.

* **¿Cómo volver negativo un número?**
  1. Escribe el número en binario completando la cantidad de bits fijada (por ejemplo, 8 bits).
  2. Invierte todos los bits (los $0$ pasan a $1$ y viceversa).
  3. Súmale $1$ al resultado.
* **Rango útil en $n$ bits:** Va desde $-2^{n-1}$ hasta $2^{n-1}-1$.  
  *(En 4 bits puedes representar desde $-8$ hasta $+7$)*.

### 3. Códigos que debes distinguir
* **BCD (8421):** Toma cada dígito del $0$ al $9$ y lo guarda en 4 bits independientes. Del $10$ al $15$ son combinaciones prohibidas (inválidas).
* **Gray:** Código donde entre un número y el siguiente **solo cambia un bit a la vez**. Evita confusiones en sensores de giro y es la base de los mapas de Karnaugh.
* **ASCII:** La tabla que le da un número binario a cada letra o símbolo (por ejemplo, `'A'` $= 65_{10} = 01000001_2$).

### 4. Las compuertas elementales
* **NOT:** Invierte la señal ($Y = A'$).
* **AND:** Exigente; da $1$ **solo si todas** las entradas son $1$ ($Y = A \cdot B$).
* **OR:** Relajada; da $1$ si **al menos una** entrada es $1$ ($Y = A + B$).
* **XOR:** Detecta diferencias; da $1$ si las entradas son **distintas**, y $0$ si son iguales ($Y = A \oplus B$).
* **Regla de tablas:** Si tienes $n$ entradas, tu tabla de verdad siempre tendrá $2^n$ filas.

---

## Semana 2: Álgebra de Boole y Formas Canónicas

La regla de oro de esta semana es sencilla: **lo que simplificas en papel te ahorra chips, cables y dinero en el circuito real**. Además, *la tabla manda*: si dos ecuaciones producen la misma tabla de verdad, hacen exactamente lo mismo.

### 1. Formas Canónicas (extraer la fórmula desde la tabla)
* **SOP (Suma de Productos / Minitérminos $\sum m$):**
  * Te fijas solo en las filas donde la salida es **$1$**.
  * En cada término multiplicas las variables: si la variable vale $1$ va normal ($A$), si vale $0$ va negada ($A'$).
* **POS (Producto de Sumas / Maxitérminos $\prod M$):**
  * Te fijas solo en las filas donde la salida es **$0$**.
  * En cada término sumas las variables con lógica invertida: si vale $0$ va normal ($A$), si vale $1$ va negada ($A'$).
* **Consejo práctico:** Mira la tabla; si hay menos ceros que unos, te conviene simplificar POS; si hay menos unos, arranca con SOP.

### 2. Reglas clave para simplificar
* **Leyes de De Morgan:** Rompe la línea de negación y cambia la operación:
  * $(A + B)' = A' \cdot B'$
  * $(A \cdot B)' = A' + B'$
* **Adyacencia:** $AB + AB' = A$ (si dos términos son idénticos salvo por una letra negada, esa letra desaparece).
* **Absorción:** $A + AB = A$ y también $A + A'B = A + B$.
* **Idempotencia ($X = X + X$):** Puedes "clonar" un término las veces que quieras para combinarlo con varios grupos vecinos.

---

## Semana 3: Mapas de Karnaugh y Compuertas Universales

El álgebra larga cansa y es fácil cometer errores. El Mapa de Karnaugh es un método gráfico infalible para simplificar funciones de 3 y 4 variables.

### 1. Reglas para armar grupos en Karnaugh
1. **Columnas y filas en código Gray:** Se ordenan estrictamente como $00, 01, 11, 10$ para que celdas pegadas solo difieran en un bit.
2. **Tamaños válidos:** Los grupos solo pueden ser de potencias de 2 ($1, 2, 4, 8, 16$ celdas). Nunca grupos de 3, ni de 5, ni en diagonal, ni en "L".
3. **Cuanto más grande, mejor:** Un grupo más grande elimina más variables y deja una fórmula más pequeña.
4. **El mapa se enrolla:** Los bordes izquierdo y derecho se tocan; la parte de arriba y de abajo también. Las 4 esquinas del mapa forman un solo grupo de 4.
5. **Lectura del grupo:** Te quedas únicamente con las variables que **no cambian de valor** en todo el grupo.

### 2. Condiciones Indiferentes (*Don't Cares* / $X$)
Son combinaciones de entrada que nunca van a pasar en la vida real (como los números del 10 al 15 en un lector BCD):
* **Cómo usarlas:** Trátalas como un $1$ **solo si te ayudan a hacer un grupo de unos más grande**.
* Si no te sirven, déjalas como $0$. **Nunca** hagas un grupo de puras $X$.

### 3. Universalidad NAND y NOR
En una fábrica sale más barato comprar cajas enteras del mismo chip (ejemplo, 74LS00 con puras NAND) que comprar compuertas variadas.
* Cualquier función en forma **SOP** se convierte directamente a una red de dos niveles **NAND-NAND** aplicando doble negación.
* Cualquier función en forma **POS** se convierte directamente a **NOR-NOR**.

---

## Semana 4: Circuitos Aritméticos y Comparadores

Aquí pasamos de la lógica pura a enseñarle a los circuitos a hacer cuentas matemáticas, que es la base de la ALU (*Unidad Aritmético Lógica*) de cualquier procesador.

### 1. Sumadores: Bloque por bloque
* **Semisumador (Half Adder):** Suma únicamente 2 bits ($A$ y $B$).
  * Salida Suma: $S = A \oplus B$
  * Salida Acarreo: $C = A \cdot B$
  * *Limitación:* No puede recibir un acarreo previo.
* **Sumador Completo (Full Adder):** Suma 3 bits: $A$, $B$ y el acarreo que llega arrastrado del paso anterior ($C_{in}$).
* **Sumador de 4 bits en cascada:** Encadena 4 sumadores completos pasando el acarreo de uno al siguiente.

### 2. El truco del Sumador / Restador con XOR
Para restar en binario sumamos el número en complemento a dos: $A - B = A + B' + 1$.  
En hardware no fabricamos un restador separado; usamos una compuerta **XOR como inversor controlado** mediante una señal de control $M$:
* **Si $M = 0$ (Modo Suma):**
  * La XOR deja pasar a $B$ idéntico ($B \oplus 0 = B$).
  * Aporta $C_{in} = 0$. El circuito realiza $A + B$.
* **Si $M = 1$ (Modo Resta):**
  * La XOR invierte los bits de $B$ ($B \oplus 1 = B'$).
  * Inyecta $C_{in} = 1$ en el primer sumador (aportando el $+1$). El circuito realiza $A + B' + 1$.

### 3. Overflow (Desbordamiento): Cuando el resultado no cabe
Ocurre al operar números con signo cuando el resultado supera el rango disponible (por ejemplo, en 4 bits: $+5 + +4 = +9$, pero en 4 bits con signo $+9$ se lee como $-7$).
* **Regla de oro:** Se detecta comparando el acarreo que entra a la última etapa ($C_3$) con el acarreo que sale de ella ($C_4$):
  $$V = C_3 \oplus C_4$$
* Si $V = 1$, hubo overflow (resultado corrupto).
* *Ojo de examen:* Que el acarreo final $C_{out} = 1$ **no** significa error; el único que confirma error de signo es el indicador de overflow $V$.

### 4. Comparador de Magnitud (ejemplo CI 7485)
Permite comparar dos palabras binarias $A$ y $B$:
* **¿Son iguales ($A = B$)?** Se aplica una compuerta XNOR a cada par de bits (dando $1$ cuando ambos coinciden) y se unen todas las salidas con una AND global.
* **¿Es mayor ($A > B$)?** Se analiza desde la izquierda (el bit más significativo / MSB hacia abajo): el primer bit donde haya una diferencia define cuál número es mayor.
