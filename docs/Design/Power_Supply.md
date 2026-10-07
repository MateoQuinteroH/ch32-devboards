# Power supply design 

The CH32L103 microcontroller will use a 3.3V power supply. This will be supplied using the AP2112K-3.3 LDO in SOT-25-5 package.


## 1. Thermal analysis

Assuming a 5V input at ia current of 150mA, the dissipated power is:

* P<sub>D</sub> = (V<sub>OUT</sub>-V<sub>IN</sub>)*I<sub>OUT</sub>

* P<sub>D</sub> = (5V - 3-3V) * 0.150A = 1.7V * 1A = 0.255W

* T<sub>J</sub>=T<sub>A(MAX)</sub> + P<sub>D</sub>(Thermal Resistance (junction-to-ambient)) 

* T<sub>J</sub>=25°C + (0.255W)(184°C/W) = 25°C + 46.92°C = 71.92°C


For a 600mA current (100% of its capacity), we have:

* P<sub>D</sub> = (5V - 3-3V) * 0.6A = 1.7V * 0.6A = 1.02W

* T<sub>J</sub>=25°C + (1.02W)(184°C/W) = 25°C + 65.45°C = 212.68°C

If the maximum current is drawn from the regulator, temperature will raise to a value where the chip will be destroyed. In order to ensure a safe operation in all the current range, a buck converter will be placed before the LDO.

## 2. Buck converter

The dropout voltage of the AP2112K-3.3 is 0.25V (for 600mA) with a maximum guaranteed of 0.4V.

to ensure proper operation:
* V<sub>IN AP2112K-3.3 </sub> = V<sub>OUT</sub> + V<sub>DROPOUTMAX</sub> = 3.3V + 0.4V = 3.7V

The Buck converter must ensure a constant output between 3.8V and 3.9V.

The converter used is the TLV62569DBVR from TI in SOT-23-5 package.


## 1. Parámetros de Diseño Básicos
* **Voltaje de Entrada (\(V_{IN}\)):** 5.0 V (Alimentación USB)
* **Voltaje de Salida Objetivo (\(V_{OUT}\)):** 3.8 V (Voltaje de entrada óptimo para el LDO considerando un Dropout máximo de 400 mV)
* **Corriente de Carga Máxima (\(I_{OUT}\)):** 600 mA (0.6 A)
* **Frecuencia de Conmutación del Buck (\(f_{SW}\)):** 1.5 MHz (Fijo por el TLV62569DBVR)
* **Voltaje de Referencia interno (\(V_{FB}\)):** 0.6 V

---

### Cálculo del Divisor de Voltaje Resistivo (R₁ y R₂)
El pin de realimentación (`FB`) del TLV62569 regula la salida comparando el voltaje escalado externamente con su referencia interna de 0.6 V.

La ecuación general de transferencia es:
\[V_{OUT} = V_{FB} \cdot \left(1 + \frac{R_1}{R_2}\right)\]

Para mitigar el ruido en el nodo de alta impedancia pero manteniendo un consumo despreciable, se fija una resistencia inferior comercial estándar (R₂) dentro de la norma recomendada (< 100 kΩ):
\[R_2 = 18 \text{ k}\Omega \quad (\pm 1\%)\]

Despejando la resistencia superior (R₁):
\[R_1 = R_2 \cdot \left(\frac{V_{OUT}}{V_{FB}} - 1\right)\]
\[R_1 = 18 \text{ k}\Omega \cdot \left(\frac{3.8 \text{ V}}{0.6 \text{ V}} - 1\right)\]
\[R_1 = 18 \text{ k}\Omega \cdot (6.3333 - 1) = 95.999 \text{ k}\Omega \approx \mathbf{96 \text{ k}\Omega}\]

#### Componentes Seleccionados (Tolerancia 1%, E96):
* **R₁ (Entre \(V_{OUT}\) y FB):** **96 kΩ** (O valor comercial más cercano: 95.3 kΩ)
* **R₂ (Entre FB y GND):** **18 kΩ**

---

### Cálculo del Inductor de Potencia (L)
Para una operación estable a alta frecuencia (1.5 MHz), se define un rizado de corriente objetivo (\(\Delta I_L\)) del 30% sobre la corriente nominal máxima del sistema (600 mA):
\[\Delta I_L = 0.30 \cdot 0.6 \text{ A} = 0.18 \text{ A}\]

La fórmula general para calcular la inductancia mínima requerida es:
\[L = \frac{V_{OUT} \cdot (V_{IN} - V_{OUT})}{\Delta I_L \cdot f_{SW} \cdot V_{IN}}\]

Sustituyendo los valores de diseño:
\[L = \frac{3.8 \text{ V} \cdot (5.0 \text{ V} - 3.8 \text{ V})}{0.18 \text{ A} \cdot 1.500.000 \text{ Hz} \cdot 5.0 \text{ V}}\]
\[L = \frac{3.8 \cdot 1.2}{1.350.000} = \frac{4.56}{1.350.000} \approx 3.377 \ \mu\text{H}\]

#### Componente Seleccionado (Especificación para compra):
* **Valor Comercial Recomendado:** **3.3 μH** (Se puede incrementar a **4.7 μH** para obtener una salida con menor rizado).
* **Corriente de Saturación (\(I_{SAT}\)):** Mínimo **1.5 A** (Para tolerar transitorios de arranque y cortocircuitos temporales sin saturar el núcleo magnético).
* **Tipo:** Blindado (*Shielded*) de montaje superficial (SMD), idealmente encapsulado metálico de bajo perfil (ej. 1212 o 2016).

---

### Capacitores de Filtro Adicionales (MLCC, X5R/X7R)
* **\(C_{IN}\) (Entrada del Buck):** **10 μH** / 10V (Cerámico, posicionado pegado al pin `VIN` del integrado).
* **\(C_{OUT}\) (Salida del Buck / Entrada LDO):** **22 μH** / 10V (Cerámico, estabiliza el rizado del Buck y actúa como tanque de entrada para el AP2112K).

