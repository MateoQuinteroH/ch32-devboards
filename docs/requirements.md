# Requerimientos de la familia CH32 DevBoards

Este documento define qué debe incluir cada placa de la familia. Los códigos (O = obligatorio, R = recomendado, D = deseable) provienen del análisis inicial de placas de desarrollo existentes y se mantienen para facilitar la trazabilidad en issues, commits y pull requests.

| Placa | Microcontrolador | Encapsulado |
|---|---|---|
| X035 | CH32X035C8T6 | LQFP48 |
| L103 | CH32L103 | LQFP48 |

El desacoplo, el circuito de reset y demás elementos del circuito mínimo recomendados por los datasheets de WCH no se listan como requerimientos: forman parte obligatoria de cualquier diseño.

## Requerimientos comunes (X035 y L103)

| ID | Requerimiento | Justificación |
|---|---|---|
| O1 | USB-C con dos resistencias CC de 5,1 kΩ a GND, una por línea | Sin ellas la placa no se alimenta con cargadores y cables C-a-C |
| O2 | Regulador de 3,3 V con margen de corriente y disipación adecuada | Evita caídas de tensión al conectar periféricos |
| O3 | Protección de entrada: polaridad inversa y OR entre fuentes de alimentación | Evita daños por conexión errónea o por varias fuentes a la vez |
| O4 | Botón de reset | Queja frecuente en placas que no lo incluyen |
| O5 | Botón o jumper de acceso al bootloader | Permite cargar firmware por USB sin programador |
| O6 | LED de encendido y al menos un LED de usuario en un pin serigrafiado | Diagnóstico básico y primer programa (blink) |
| O7 | Cabecera de depuración WCH SDI de 4 pines: DIO, CLK, 3V3, GND | Programación y depuración con WCH-LinkE |
| O8 | Todos los pines en paso de 2,54 mm, en dos filas, con serigrafía clara | Uso en protoboard; la falta de rotulación es la queja más común |
| O9 | Ancho compatible con protoboard, dejando al menos una fila libre por lado | Uso en protoboard estándar |
| O10 | Documentación abierta completa: pinout, esquemático PDF, proyecto KiCad, BOM con referencias, guía de inicio | Requisito de hardware abierto y de certificación OSHWA |
| R2 | UART accesible en los headers | Mensajes por serie sin depurador |
| R3 | Agujeros de montaje | Fijación en proyectos y carcasas |
| R4 | Protección ESD en las líneas USB (D+, D−, VBUS) | Robustez ante descargas al conectar el cable |
| R5 | Jumper o punto de medición de consumo en la alimentación del MCU | Medir el consumo real del microcontrolador |
| R7 | Serigrafía completa: marca del pin 1, nombre de la placa, revisión (`${REVISION}`), commit (`${GIT_HASH}`) y orientación del USB | Trazabilidad de cada placa física a su versión del diseño |
| D3 | LED RGB direccionable compatible con 3,3 V | Indicación de estado y demostraciones |

## Requerimientos específicos de la L103

| ID | Requerimiento | Justificación |
|---|---|---|
| R9 | Cristal de 32,768 kHz y alimentación VBAT para el RTC | Aprovecha el RTC y los modos de bajo consumo del CH32L103 |
| R10 | Footprint para transceptor CAN | Aprovecha el controlador CAN integrado |
| D1 | Cargador LiPo con conector JST-PH y medición del voltaje de batería | Uso portátil; complementa el bajo consumo del chip |

## Requerimientos específicos de la X035

Ninguno por ahora. La X035 trabaja fija a 3,3 V.

## Descartados

| ID | Requerimiento | Motivo |
|---|---|---|
| R1 | Conector Qwiic / STEMMA QT | Descartado |
| R6 | Desacoplo según datasheet | No es un requerimiento: es parte obligatoria del diseño |
| R8 | Selección de lógica 3,3 V / 5 V en la X035 | Descartado; la X035 trabaja a 3,3 V |
| D2 | Footprint para flash SPI / EEPROM I²C | Descartado |
| D4 | Pads castellados | Descartado |
| D5 | Cabecera Cortex de 10 pines a 1,27 mm | Los CH32 usan SDI de 2 hilos; ya cubierto por O7 |
| D6 | Pin AREF / VREF accesible | Descartado |
| D7 | Zona de prototipado | Descartado |
| D8 | Cristal HSE en la L103 | Descartado |
| D9 | Compatibilidad con bases mikroBUS | Descartado |

## Puntos de diseño abiertos

Consecuencias de los requerimientos que deben resolverse durante el diseño del esquemático.

- [ ] **CAN y USB en la L103 (R10).** Por defecto comparten los pines PA11/PA12. Conectar el transceptor a los pines remapeados (previsiblemente PB8/PB9) para que ambos funcionen a la vez. Confirmar en el datasheet del CH32L103.
- [ ] **LED RGB a 3,3 V (D3).** El WS2812B clásico requiere al menos 3,5 V. Elegir una variante que funcione a 3,3 V, en encapsulado soldable a mano (5050 o 3535).
- [ ] **VBAT en la L103 (R9, D1).** Decidir si VBAT se alimenta desde la LiPo o desde una pila de botón.
- [ ] **Ubicación del jumper de consumo (R5).** Debe medir solo el consumo del microcontrolador, excluyendo cargador y LED RGB.
- [ ] **Regulador con batería (O2, D1).** En la L103, elegir un regulador de muy baja caída para aprovechar la LiPo cerca de su descarga, y un circuito que alterne entre USB y batería.
- [ ] **Pines de la L103.** Verificar contra el datasheet oficial los pines de USB, CC, SDI, LSE y VBAT, que en el análisis inicial se tomaron de familias compatibles.

## Formato físico

Pendiente de definir. Debe cumplir O8 y O9, ser común a ambas placas y alojar los componentes adicionales de la L103.
