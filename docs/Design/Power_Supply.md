# Power supply design 

The Ch32L103 microcontroller will use a 3.3V power supply. This will be supplied using the AMS1117-3.3 chip from Advanced Monolithic Systems.


## Thermal analisys

Assuming a 5V input at its maximum current (1A), a top side copper area of 1000 Sq.mm and a bottom side copper area of 2500 Sq.mm, the dissipated power is:

P<sub>D</sub> = (V<sub>OUT</sub>-V<sub>IN</sub>)*I<sub>OUT</sub>

P<sub>D</sub> = (5V - 3-3V) * 1A = 1.7V * 1A = 1.7W

T<sub>J</sub>=T<sub>A(MAX)</sub> + P<sub>D</sub>(Thermal Resistance (junction-to-ambient)) 

T<sub>J</sub>=25°C + (1.7W)(55°C/W) = 25°C + 93.5°C = 118.5°C

This is below (but dangerously close) to the maximum 125°C value specified in the datasheet.

