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


### Basic Design Parameters
* **Input voltage (\(V_{IN}\)):** 5.0 V (USB)
* **Target Output Voltage (\(V_{OUT}\)):** 3.8 V (Optimal input voltage for the LDO, considering a maximum dropout of 400 mV.)
* **Maximum Charging Current (\(I_{OUT}\)):** 600 mA (0.6 A)
* **Buck Switching Frequency (\(f_{SW}\)):** 1.5 MHz (Fixed by TLV62569DBVR)
* **Internal reference voltage (\(V_{FB}\)):** 0.6 V

---

### Resistive Voltage Divider Calculation (R₁ y R₂)
The feedback pin (`FB`) of TLV62569 regulates the output by comparing the externally scaled voltage with its internal 0.6 V reference.

The general transfer equation is:
\[V_{OUT} = V_{FB} \cdot \left(1 + \frac{R_1}{R_2}\right)\]

To mitigate noise at the high-impedance node while maintaining negligible power consumption, a standard commercial lower resistor (R₂) is selected within the recommended range (< 100 kΩ):
\[R_2 = 18 \text{ k}\Omega \quad (\pm 1\%)\]

Clearing the upper resistance (R₁):
\[R_1 = R_2 \cdot \left(\frac{V_{OUT}}{V_{FB}} - 1\right)\]
\[R_1 = 18 \text{ k}\Omega \cdot \left(\frac{3.8 \text{ V}}{0.6 \text{ V}} - 1\right)\]
\[R_1 = 18 \text{ k}\Omega \cdot (6.3333 - 1) = 95.999 \text{ k}\Omega \approx \mathbf{96 \text{ k}\Omega}\]

#### Selected Components (Tolerance 1%, E96):
* **R₁ (Between \(V_{OUT}\) y FB):** **96 kΩ** (O closest commercial value: 95.3 kΩ)
* **R₂ (Between FB and GND):** **18 kΩ**

---

### Power inductor calculation (L)
For stable high-frequency operation (1.5 MHz), a target current ripple (\(\Delta I_L\)) of 30% of the system's maximum rated current (600 mA) is defined.:
\[\Delta I_L = 0.30 \cdot 0.6 \text{ A} = 0.18 \text{ A}\]

The general formula for calculating the minimum required inductance is:
\[L = \frac{V_{OUT} \cdot (V_{IN} - V_{OUT})}{\Delta I_L \cdot f_{SW} \cdot V_{IN}}\]

Substituting the design values:
\[L = \frac{3.8 \text{ V} \cdot (5.0 \text{ V} - 3.8 \text{ V})}{0.18 \text{ A} \cdot 1.500.000 \text{ Hz} \cdot 5.0 \text{ V}}\]
\[L = \frac{3.8 \cdot 1.2}{1.350.000} = \frac{4.56}{1.350.000} \approx 3.377 \ \mu\text{H}\]

#### Selected Component (Purchase Specification):
* **Recommended Commercial Value:** **3.3 μH** (It can be increased to **4.7 μH** to obtain an output with lower ripple).
* **Saturation Current (\(I_{SAT}\)):** Minimum **1.5 A** (To withstand startup transients and temporary short circuits without saturating the magnetic core.).
* **Typw:** Shielded (SMD), ideally in a low-profile metal package (ex. 1212 or 2016).

---

### Additional Filter Capacitors (MLCC, X5R/X7R)
* **\(C_{IN}\) (Buck input):** **10 μH** / 10V (Ceramic capacitor, positioned right next to the IC's `VIN` pin.).
* **\(C_{OUT}\) (Buck output / LDO input):** **22 μH** / 10V (Ceramic; it stabilizes the Buck converter's ripple and acts as an input reservoir for the AP2112K.).

