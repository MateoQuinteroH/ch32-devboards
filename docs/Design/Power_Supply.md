# Power supply design 

The Ch32L103 microcontroller will use a 3.3V power supply. This will be supplied using the AP2112K-3.3 LDO in SOT-25-5 package.


## Thermal analisys

Assuming a 5V input at ia current of 150mA, the dissipated power is:

P<sub>D</sub> = (V<sub>OUT</sub>-V<sub>IN</sub>)*I<sub>OUT</sub>

P<sub>D</sub> = (5V - 3-3V) * 0.150A = 1.7V * 1A = 0.255W

T<sub>J</sub>=T<sub>A(MAX)</sub> + P<sub>D</sub>(Thermal Resistance (junction-to-ambient)) 

T<sub>J</sub>=25°C + (0.255W)(184°C/W) = 25°C + 46.92°C = 71.92°C


For a 600mA current (100% of its capacity), we have:

P<sub>D</sub> = (5V - 3-3V) * 0.6A = 1.7V * 0.6A = 1.02W

T<sub>J</sub>=25°C + (1.02W)(184°C/W) = 25°C + 65.45°C = 212.68°C
