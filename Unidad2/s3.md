# Unidad 2 - Semana 5: Selección y Codificación de Datos


## 1. Encaminamiento y Conversión de Datos

**¿Por qué encaminar datos?** En un sistema digital no es viable tener un cable físico exclusivo entre cada componente por eso se usa buses compartidos e interruptores lógicos para elegir qué fuente transmite o hacia qué destino va la información.

---

## 2. Multiplexor (MUX) y Demultiplexor (DEMUX)

Son operaciones **duales** (inversas entre sí) encargadas de dirigir el flujo de datos:

* **Multiplexor (MUX — Selector):** Tiene $2^n$ entradas de datos, $n$ líneas de selección y **1 salida**. El código binario de selección decide cuál entrada pasa a la salida.
    * *Estructura interna:* Compuertas `NOT` (para selección), compuertas `AND` (una por canal) y una `OR` final que agrupa todo.
* **Demultiplexor (DEMUX — Distribuidor):** Tiene **1 entrada** de datos, $n$ líneas de selección y $2^n$ salidas. Envía el dato de entrada hacia el destino elegido y mantiene las demás salidas en `0`.
    * *Estructura interna:* Compuertas `NOT` y compuertas `AND` independientes por cada salida (sin `OR` final).

### Ejemplo: Enrutamiento de sensores (MUX 4:1 y DEMUX 1:4)
Supongamos un sistema de monitoreo ambiental con 4 sensores conectados a un **MUX 4:1** con los siguientes estados actuales:
* $I_0 = 0$ (Humedad baja)
* $I_1 = 1$ (Temperatura alta)
* $I_2 = 0$ (Presión normal)
* $I_3 = 1$ (Nivel de luz alto)

1. **Lectura con MUX:** Si quiero leer el sensor de temperatura ($I_1$), configuro las líneas de selección en $S_1S_0 = 01_2$ (1 en decimal). La salida entrega $Y = I_1 = 1$.
2. **Distribución con DEMUX:** Ese bit $D = 1$ entra a un **DEMUX 1:4** encargado de activar actuadores. Si selecciono $S_1S_0 = 10_2$ (canal 2, ventilador), las salidas del DEMUX serán:
    * $Y_0 = 0$, $Y_1 = 0$, **$Y_2 = 1$** (Activa el ventilador), $Y_3 = 0$.

---

## 3. Codificadores y Decodificadores

Mientras el MUX/DEMUX mueven datos sin alterarlos, estos bloques **transforman el código**:

* **Codificador:** Comprime información. Tiene hasta $2^n$ entradas y $n$ salidas. Entrega el código binario correspondiente a la línea de entrada activa. *(Si es con prioridad, prevalece la entrada de índice más alto).*
    * *Estructura interna:* Compuertas `OR`.
* **Decodificador:** Expande información. Recibe $n$ bits binarios y activa exclusivamente **1 de sus $2^n$ salidas** (funciona como detector de minitérminos).
    * *Estructura interna:* Compuertas `NOT` y `AND`.

### Ejemplo: Panel de control de 4 botones (Codificador 4 a 2)
Diseñamos un panel con 4 pulsadores ($D_0, D_1, D_2, D_3$) conectado a un codificador de 4 a 2 líneas con salidas $A_1A_0$:

* **Ecuaciones de diseño:**
  $$A_1 = D_2 + D_3$$
  $$A_0 = D_1 + D_3$$
* **Prueba de funcionamiento:** Si presiono el pulsador $D_3$ ($D_3 = 1$ y el resto en `0`), obtenemos $A_1 = 1$ y $A_0 = 1$ ($11_2 = 3$).
* **Proceso inverso (Decodificador 2 a 4):** Si enviamos $A_1A_0 = 11$ a un decodificador 2 a 4, la salida será $Y_0=0, Y_1=0, Y_2=0, \mathbf{Y_3=1}$.

---

## 4. Decodificador BCD a 7 Segmentos

Es un decodificador especializado que traduce un dígito decimal codificado en binario de 4 bits (**BCD**: entradas $D, C, B, A$ desde `0000` hasta `1001`) en 7 señales de control (`a, b, c, d, e, f, g`) para encender los LEDs de un display numérico.

* **Cátodo común (ej. 74LS48):** Los segmentos encienden con un `1` lógico.
* **Ánodo común (ej. 74LS47):** Los segmentos encienden con un `0` lógico.

### Ejemplo: Mostrar el dígito `7` en Cátodo Común
Queremos visualizar el número **7** en el display:

1. **Entrada BCD:** El 7 en binario de 4 bits es $DCBA = 0111$.
2. **Análisis visual del display:** Para dibujar el `7` se requieren encender únicamente los segmentos superior (`a`), superior derecho (`b`) e inferior derecho (`c`).
3. **Vector de salida:**
   | Dígito | BCD ($DCBA$) | `a` | `b` | `c` | `d` | `e` | `f` | `g` |
   | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
   | **7** | `0111` | **1** | **1** | **1** | 0 | 0 | 0 | 0 |

---

## 5. Expansión de Bloques Funcionales

Cuando necesitamos manejar más canales de los que ofrece un integrado estándar, conectamos bloques pequeños en cascada.

### Ejemplo: Construir un MUX 8:1 usando dos MUX 4:1 y un MUX 2:1
Para manejar 8 entradas ($I_0$ a $I_7$) con 3 líneas de selección ($S_2, S_1, S_0$):
1. Conecto $I_0 \dots I_3$ al **MUX 4:1 (Bajo)** y $I_4 \dots I_7$ al **MUX 4:1 (Alto)**.
2. Ambos MUX comparten las líneas menos significativas $S_1$ y $S_0$ en paralelo.
3. Las salidas de ambos bloques entran a un **MUX 2:1** controlado por el bit más significativo $S_2$.

* **Prueba:** Si quiero leer $I_5$ ($S_2S_1S_0 = 101_2$):
    * Con $S_1S_0 = 01$, el MUX Bajo elige $I_1$ y el MUX Alto elige $I_5$.
    * Como $S_2 = 1$, el MUX 2:1 final selecciona la rama superior, entregando $Y = I_5$.

---

## 6. Implementación de Funciones Lógicas con MUX

Un multiplexor de $2^n$ canales puede implementar **cualquier función booleana de $n$ variables directamente desde su tabla de verdad**, sin necesidad de simplificar por álgebra de Boole ni Mapas de Karnaugh.

### Ejemplo: Función de alarma de 3 sensores en un MUX 8:1
Implementar la función de activación de alarma $F(A, B, C) = \Sigma m(0, 3, 5, 6)$ usando un MUX 8:1:

1. Conecto las variables a la selección: $A \rightarrow S_2$, $B \rightarrow S_1$, $C \rightarrow S_0$.
2. Cableo cada canal de entrada ($I_0 \dots I_7$) directamente a `+Vcc` (`1`) o `GND` (`0`) según la tabla de verdad:

| Minitérmino | $A (S_2)$ | $B (S_1)$ | $C (S_0)$ | Salida $F$ | Conexión en el MUX 8:1 |
| :---: | :---: | :---: | :---: | :---: | :--- |
| $m_0$ | 0 | 0 | 0 | **1** | $I_0 \rightarrow +V_{CC}$ (`1`) |
| $m_1$ | 0 | 0 | 1 | **0** | $I_1 \rightarrow GND$ (`0`) |
| $m_2$ | 0 | 1 | 0 | **0** | $I_2 \rightarrow GND$ (`0`) |
| $m_3$ | 0 | 1 | 1 | **1** | $I_3 \rightarrow +V_{CC}$ (`1`) |
| $m_4$ | 1 | 0 | 0 | **0** | $I_4 \rightarrow GND$ (`0`) |
| $m_5$ | 1 | 0 | 1 | **1** | $I_5 \rightarrow +V_{CC}$ (`1`) |
| $m_6$ | 1 | 1 | 0 | **1** | $I_6 \rightarrow +V_{CC}$ (`1`) |
| $m_7$ | 1 | 1 | 1 | **0** | $I_7 \rightarrow GND$ (`0`) |

---


