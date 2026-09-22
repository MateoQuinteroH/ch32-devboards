# CH32 DevBoards

Familia de tarjetas de desarrollo de **hardware abierto** basadas en microcontroladores RISC-V de WCH, diseñadas en KiCad para poder **soldarse a mano** (sin QFN ni BGA).

| Placa | MCU | Encapsulado | Enfoque | Estado |
|---|---|---|---|---|
| [X035](hardware/x035/) | CH32X035C8T6 | LQFP48 | USB-C + PD, lógica 3,3 V / 5 V, analógica | 🚧 En diseño |
| [L103](hardware/l103/) | CH32L103 | LQFP48 | Bajo consumo, CAN, RTC | 🚧 En diseño |

<!-- Sustituye por un render o foto cuando exista: docs/images/x035-top.png -->

## Características

> Pendiente: la lista final de requerimientos se documentará aquí.

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
