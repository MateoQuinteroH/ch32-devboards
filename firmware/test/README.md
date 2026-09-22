# Firmware de prueba de fabricación (bring-up)

Firmware para verificar una placa recién soldada. Debe comprobar, en orden:

1. Encendido y LED de power.
2. Programación por WCH-LinkE (SDI).
3. LED de MateoQuinteroH y botones.
4. Enumeración USB (dispositivo CDC).
5. Negociación USB-PD.
6. Periféricos específicos de cada placa (selección 3,3/5 V en X035; CAN y RTC en L103).

El resultado se reporta por el puerto serie USB y con patrones de parpadeo del LED.
