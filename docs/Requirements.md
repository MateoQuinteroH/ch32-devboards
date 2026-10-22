# Requerimientos de la familia CH32 DevBoards

Este documento define qué debe incluir cada placa de la familia. Los códigos (O = obligatorio, R = recomendado, D = deseable) provienen del análisis inicial de placas de desarrollo existentes y se mantienen para facilitar la trazabilidad en issues, commits y pull requests.

| Placa | Microcontrolador |
|---|---|
| X035 | CH32X035C8T6 |
| L103 | CH32L103 |

## Requerimientos comunes (X035 y L103)

| ID | Requerimiento | Justificación |
|---|---|---|
| O1 | Todos los pines en paso de 2,54 mm, en dos filas, con serigrafía clara | Uso en protoboard; la falta de rotulación es la queja más común |
| O2 | Ancho compatible con protoboard, dejando al menos una fila libre por lado | Uso en protoboard estándar |
| O3 | Documentación abierta completa: pinout, esquemático PDF, proyecto KiCad, BOM con referencias, guía de inicio | Requisito de hardware abierto y de certificación OSHWA |
| R1 | UART accesible en los headers | Mensajes por serie sin depurador |
| R2 | Agujeros de montaje | Fijación en proyectos y carcasas |
| R3 | Jumper o punto de medición de consumo en la alimentación del MCU | Medir el consumo real del microcontrolador |
| R4 | Serigrafía completa: marca del pin 1, nombre de la placa, revisión (`${REVISION}`), commit (`${GIT_HASH}`) y orientación del USB | Trazabilidad de cada placa física a su versión del diseño |
| D1 | LED RGB direccionable compatible con 3,3 V | Indicación de estado y demostraciones |

## Requerimientos específicos de la L103

| ID | Requerimiento |
|---|---|
O4 |   USB PD como sink, rango SPR (5, 9, 12, 15 y 20 V) |	
O5 |   USB PD como source |
O5 |   Puerto dedicado para pantalla OLED |
O6 |   Medición de potencia entregada a la carga |
R5 |   Footprint para transceptor CAN	|
R6 |   Medición de la tensión de VBUS |
D2 |   Soporte para batería LiPo con conector JST-PH |	
D3 |   Medición de la corriente de la salida R11 con resistencia shunt amplificada por el OPA/PGA interno	| 
D4 |   Modo source de 5 V mediante un segundo interruptor de carga, deshabilitado por defecto |	

## Requerimientos específicos de la X035

Ninguno por ahora. La X035 trabaja fija a 3,3 V.

## Descartados

| ID | Requerimiento | Motivo |
|---|---|---|
R1 |	Conector Qwiic / STEMMA QT |	Descartado
R6 |	Desacoplo según datasheet |	No es un requerimiento: es parte obligatoria del diseño
R8 |	Selección de lógica 3,3 V / 5 V en la X035 |	Descartado; la X035 trabaja a 3,3 V
D2 |	Footprint para flash SPI / EEPROM I²C	| Descartado
D4 |	Pads castellados	| Descartado
D5 |	Cabecera Cortex de 10 pines a 1,27 mm	| Los CH32 usan SDI de 2 hilos; ya cubierto por O7
D6 |	Pin AREF / VREF accesible	| Descartado
D7 |	Zona de prototipado	| Descartado
D8 |	Cristal HSE en la L103	| Descartado (ver punto abierto sobre CAN)
D9 |	Compatibilidad con bases mikroBUS	| Descartado
D12 |	USB PD EPR (28–48 V) en la L103	| Exige rediseñar toda la cadena de VBUS para 48 V. La v1 se limita a SPR (20 V)

## Arquitectura de alimentación de la L103
USB-C ─ TVS ≥22 V ─ MOSFET OVP (O14) ─ VBUS_PROT ─┬─ buck 5 V (O13) ─ SYS_5V ─┬─ cargador LiPo (D1)
   │                                              │                          └─ OR con batería (O3) ─ LDO 3,3 V (O2) ─ MCU
   CC1/CC2 ─ protección CC (O12) ─ PB6/PB7        ├─ interruptor de carga + PTC ─ header VBUS_OUT (R11)
                                                  └─ divisor ×11 ─ ADC / CMP (R12)

En batería, el buck queda fuera de la ruta para que su consumo en reposo no domine el modo Standby del MCU.

## Puntos de diseño abiertos

Consecuencias de los requerimientos que deben resolverse durante el diseño del esquemático.

* Protección de CC (O12). Elegir entre TCPP01-M12 (ST: mejor documentado, en LCSC C1121848, OVP de VBUS hasta 22 V, orientado a sink) y CH211 (WCH: hasta 28 V, menos documentación en inglés). Si se usa TCPP01-M12, verificar en su datasheet (DS12900) cómo convive su función de dead battery con las Rd externas de O1.
* Encapsulados QFN. Las protecciones de CC disponibles son QFN, lo que choca con el criterio "soldable a mano: sin QFN ni BGA" del README. Propuesta: el MCU y los pasivos siguen siendo soldables a mano, y la protección PD se monta con ensamblado PCBA.
* Rd fijas y modo source (O1, D11). Con Rd fijas de 5,1 kΩ el modo source no funciona. Si se mantiene D11, las Rd deben poder desconectarse (MOSFET o jumper). Decidir antes del layout.
Corriente de la salida VBUS (R11). Limitar la corriente anunciada en el firmware a lo que soporten el header de 2,54 mm, las pistas y el interruptor de carga.
* CAN sin HSE (R10, D8). Con D8 descartado, el CAN usa el HSI interno. Comprobar que su tolerancia sea suficiente para las velocidades objetivo; para CAN FD a alta velocidad conviene reconsiderar un footprint de HSE en PD0/PD1.
* Capacitancia en CC. La especificación USB PD exige entre 200 y 600 pF en cada línea CC; sumar la capacitancia de la protección y completar con condensadores.
* LED RGB a 3,3 V (D3). El WS2812B clásico requiere al menos 3,5 V. Elegir una variante que funcione a 3,3 V, en encapsulado soldable a mano (5050 o 3535).
* Ubicación del jumper de consumo (R5). Debe medir solo el consumo del microcontrolador, excluyendo cargador, LED RGB y circuito de VBUS.
* Regulador con batería (O2, D1). Elegir un LDO de muy baja caída y bajo consumo en reposo, y el circuito que alterna entre SYS_5V y batería.
* Firmware PD. Partir del ejemplo sink del EVT oficial de WCH. No anunciar PPS hasta probarlo; las fuentes de WCH no lo confirman.

## Formato físico

Pendiente de definir. Debe cumplir O8 y O9, ser común a ambas placas y alojar los componentes adicionales de la L103.
