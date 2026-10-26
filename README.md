# CH32 DevBoards

Familia de tarjetas de desarrollo de **hardware abierto** basadas en microcontroladores RISC-V de WCH, diseñadas en KiCad.

| Placa | MCU | Encapsulado | Enfoque | Estado |
|---|---|---|---|---|
| [X035](hardware/x035/) | CH32X035C8T6 | LQFP48 | USB-C + PD, lógica 3,3 V / 5 V, analógica | 🚧 En diseño |
| [L103](hardware/l103/) | CH32L103 | LQFP48 | Bajo consumo, CAN, RTC | 🚧 En diseño |

<!-- Sustituye por un render o foto cuando exista: docs/images/x035-top.png -->

## Características

**Comunes a ambas placas**

- USB-C para alimentación, programación y comunicación (compatible con cargadores C-a-C)
- Botones de reset y bootloader: carga de firmware por USB sin programador
- Cabecera de depuración SDI para WCH-LinkE
- LED de encendido, LED de usuario y LED RGB direccionable
- UART accesible y jumper para medir el consumo del microcontrolador
- Todos los pines en paso de 2,54 mm, compatible con protoboard
- Agujeros de montaje
- Soldable a mano: sin encapsulados QFN ni BGA

**Funcionalidades de la L103**
- Negociar 5, 9, 15 o 20 V con cualquier cargador USB-C PD, y 12 V si el cargador lo ofrece.
- Ajustar el voltaje en pasos finos con cargadores PPS, si el firmware lo implementa.
- Alimentar otros proyectos (motores, tiras LED, bombas, otras placas) a través de la bornera de salida.
- Encender y apagar la salida de potencia desde el firmware.
- Protección cortocircuito y sobrecorriente en la salida.
- Medir en tiempo real el voltaje, la corriente y la potencia que consumen las cargas.
- Medir el consumo del microcontrolador en modo activo.
- Medir el consumo real en sleep con un instrumento externo (PPK2 u otro) mediante jumpers de corte.
- Funcionar con batería Li-ion 1S y recargarla por USB-C.
- Usar la placa mientras la batería se carga.
- Ver el estado de carga y leer el voltaje de la batería.
- Conectar pantallas OLED y sensores por el puerto Qwiic/STEMMA QT.
- Comunicarse por bus CAN, si se instala el transceptor opcional.
- Comunicarse con el PC por USB mientras la placa negocia PD.
- Ejecutar modelos ML ultraligeros sobre datos de energía y sensores sencillos: clasificar cargas, detectar anomalías y estimar el estado de la batería.
- Registrar datos y enviarlos al PC para entrenar modelos.
- Cargar firmware por USB sin herramientas extra.
- Depurar con WCH-LinkE mediante pads SDI.

Los requerimientos completos, con su justificación, están en [docs/requirements.md](docs/requirements.md).
## Empezar

1. Consigue la placa: descarga los archivos de fabricación de la [última release](../../releases) o fabrícala desde el código fuente.
2. Instala las herramientas: consulta [docs/getting-started.md](docs/getting-started.md).
3. Carga un ejemplo de [firmware/examples](firmware/examples/).

## Estructura del repositorio

```
hardware/   Proyectos KiCad de cada placa (x035, l103)
lib/        Símbolos, footprints y modelos 3D compartidos
firmware/   Ejemplos y firmware de prueba de fabricación
docs/       Pinout, guía de ensamblaje manual, primeros pasos
```

## Requisitos para editar el diseño

- **KiCad 9.x** (no abras ni guardes los archivos con otra versión mayor).
- Todas las librerías están incluidas en `lib/`; no se necesita nada instalado aparte de KiCad.

## Contribuir

Las contribuciones son bienvenidas. Lee [CONTRIBUTING.md](CONTRIBUTING.md) antes de abrir un pull request.

## Licencias

| Contenido | Licencia |
|---|---|
| Hardware (`hardware/`, `lib/`) | [CERN-OHL-S-2.0](LICENSES/CERN-OHL-S-2.0.txt) |
| Firmware (`firmware/`) | [MIT](LICENSES/MIT.txt) |
| Documentación (`docs/`, archivos `.md`) | [CC BY-SA 4.0](LICENSES/CC-BY-SA-4.0.txt) |
| Configuración (`.github/`, `.gitignore`, etc.) | [CC0 1.0](LICENSES/CC0-1.0.txt) |

El detalle por archivo está en [REUSE.toml](REUSE.toml) (especificación [REUSE](https://reuse.software)).

<!-- Cuando obtengas la certificación: [![OSHW](docs/images/oshw-logo.svg)](https://certification.oshwa.org/) UID: XX000000 -->
