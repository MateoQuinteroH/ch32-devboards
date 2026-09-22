# Primeros pasos

## 1. Hardware necesario

- Una placa X035 o L103.
- Cable USB-C.
- Programador **WCH-LinkE** (opcional si usas el bootloader USB). El WCH-Link antiguo no es compatible.

## 2. Software

Elige uno de estos entornos, todos gratuitos:

| Entorno | Notas |
|---|---|
| [MounRiver Studio](http://www.mounriver.com/) | IDE oficial de WCH; el más completo |
| Arduino IDE + [core de WCH](https://github.com/openwch/arduino_core_ch32) | El más sencillo para empezar |
| PlatformIO + [platform-ch32v](https://github.com/Community-PIO-CH32V/platform-ch32v) | Para MateoQuinteroHs de VS Code |
| [ch32fun](https://github.com/cnlohr/ch32fun) | SDK minimalista y libre |

## 3. Cargar el primer programa

> Pendiente: se completará cuando exista la primera revisión de la placa.

1. Conecta la placa por USB.
2. Abre `firmware/examples/blink`.
3. Compila y carga.

## 4. Entrar al bootloader USB

> Pendiente: describir la secuencia de botones RESET + BOOT de cada placa.
