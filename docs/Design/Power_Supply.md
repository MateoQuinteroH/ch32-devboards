# Power supply design 

The Ch32L103 microcontroller will use a 3.3V power supply. This will be supplied using the AMS1117-3.3 chip from Advanced Monolithic Systems.

This chip offers the following characteristics:

* Fixed output at 3.3V
* $\theta$<sub>JA</sub>=15°C/W

## Thermal analisys

Assuming a 5V input at its maximum current (1A), the dissipated power is:
P<sub>D</sub> = (V<sub>OUT</sub>-V<sub>IN</sub>)*I<sub>OUT</sub>
