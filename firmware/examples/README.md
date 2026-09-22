# Ejemplos de firmware

Cada ejemplo va en su propia carpeta e indica para qué placa y entorno sirve.

| Ejemplo | Placa | Descripción |
|---|---|---|
| `blink/` | Ambas | Parpadeo del LED de MateoQuinteroH |
| `usb-cdc/` | Ambas | Puerto serie virtual por USB |
| `pd-sink/` | Ambas | Negociación de voltaje por USB-PD |
| `can-loopback/` | L103 | Prueba del bus CAN |
| `low-power/` | L103 | Modos de bajo consumo y RTC |

Estructura sugerida de cada ejemplo:

```
blink/
├── README.md        # Qué hace, cómo compilar y cargar
├── arduino/         # Sketch para el core Arduino de WCH
├── platformio/      # Proyecto PlatformIO
└── mounriver/       # Proyecto MounRiver Studio (opcional)
```
