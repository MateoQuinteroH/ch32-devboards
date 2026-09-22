# Cómo contribuir

¡Gracias por tu interés! Esta guía asegura que el diseño se mantenga consistente y fabricable.

## Herramientas

- **KiCad 9.x.** No guardes archivos con otra versión mayor: KiCad no permite abrir archivos de versiones más nuevas en versiones anteriores.
- Git y una cuenta de GitHub.

## Flujo de trabajo

1. Abre un *issue* describiendo el cambio **antes** de empezar, sobre todo si vas a tocar un PCB. Dos personas editando el mismo layout generan conflictos que no se pueden fusionar.
2. Crea una rama desde `main` con un nombre descriptivo:
   - `feat/x035-qwiic` para funcionalidades nuevas
   - `fix/l103-can-termination` para correcciones
   - `docs/pinout-x035` para documentación
3. Haz commits pequeños y con mensaje claro. Cuando puedas, separa los cambios de esquemático y de layout.
4. Abre un pull request. El CI ejecutará ERC y DRC; el PR solo se fusiona con el CI en verde.

## Convenciones de diseño

### Símbolos y campos obligatorios
Todo componente del esquemático debe tener estos campos, porque la BOM se genera a partir de ellos:

| Campo | Ejemplo |
|---|---|
| `MPN` | `CH32X035C8T6` |
| `Manufacturer` | `WCH` |
| `LCSC` | `C1234567` |
| `Mouser` | (opcional) |
| `Digikey` | (opcional) |

Los componentes que no se montan llevan el atributo *Do not populate* (DNP).

### Librerías
- Usa solo librerías de `lib/` o las oficiales de KiCad.
- Si creas un símbolo o footprint nuevo, agrégalo a `lib/` e incluye su modelo 3D en `lib/3dmodels/`.
- Los modelos 3D se referencian con la variable `${CH32_3DMODELS}`.

### Nombres de nets
- Alimentación: `VBUS`, `VIN`, `+5V`, `+3V3`, `VDD_MCU`, `GND`.
- Señales: nombre de la función, no del pin (`USB_DP`, `USB_CC1`, `SWDIO`, `LED_USER`).

### Soldadura manual
El objetivo del proyecto es que cualquiera pueda soldar la placa a mano:
- Pasivos de **0603 como mínimo** (0805 preferible).
- **Sin QFN, BGA, WLCSP ni encapsulados con pad térmico inaccesible.**
- Deja espacio alrededor de los componentes para la punta del cautín.

## Versionado

- Tags por placa: `x035-v1.0`, `l103-v1.1`.
- **Versión mayor:** cambia el pinout o la compatibilidad mecánica.
- **Versión menor:** correcciones compatibles.
- Antes de crear un tag, actualiza el campo **Revision** del bloque de título (se imprime en la serigrafía con `${REVISION}`) y el `CHANGELOG.md`.

## Erratas

Si encuentras un error en una placa ya fabricada, abre un issue con la plantilla **Errata de hardware**. Una vez confirmado, se documenta en el `ERRATA.md` de esa placa.

## Licencias

Al contribuir aceptas que tu aporte se publique bajo la licencia de la carpeta correspondiente (ver `REUSE.toml`).
