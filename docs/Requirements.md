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
| O2 | 	Regulador de 3,3 V con margen de corriente y disipación adecuada. En la L103 se alimenta desde el riel de 5 V del buck (O13), nunca directo desde VBUS | Evita caídas de tensión al conectar periféricos |
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
| R4 | Protección ESD en las líneas USB (D+, D−, VBUS). En la L103, todo lo conectado a VBUS debe tolerar al menos 22 V (ver O14) | Robustez ante descargas al conectar el cable |
| R5 | Jumper o punto de medición de consumo en la alimentación del MCU | Medir el consumo real del microcontrolador |
| R7 | Serigrafía completa: marca del pin 1, nombre de la placa, revisión (`${REVISION}`), commit (`${GIT_HASH}`) y orientación del USB | Trazabilidad de cada placa física a su versión del diseño |
| D3 | LED RGB direccionable compatible con 3,3 V | Indicación de estado y demostraciones |

## Requerimientos específicos de la L103

| ID | Requerimiento | Justificación |
|---|---|---|
O11 |	USB PD como sink, rango SPR (5, 9, 12, 15 y 20 V), con CC1/CC2 conectados a PB6/PB7 |	Aprovecha el controlador y PHY USB PD integrados (BMC y CRC por hardware, PD 2.0/3.0)
O12 |	Protección en serie de CC1/CC2 contra cortocircuito a VBUS, con ESD (TCPP01-M12 de ST o CH211 de WCH) |	PB6/PB7 toleran 5,5 V como máximo absoluto; un TVS no protege contra un corto sostenido a 20 V
O13 |	Regulador conmutado (buck) de VBUS a 5 V con entrada de 4,5–28 V (p. ej. TPS54202, LCSC C191884) |	Un regulador lineal desde 20 V disiparía demasiado
O14 |	Protección de sobretensión en VBUS con MOSFET de corte, y TVS en VBUS con tensión de trabajo ≥ 22 V	Protege la placa ante fuentes defectuosas. | Los arreglos ESD USB pensados para VBUS de 5 V no sirven aquí
R9 |	Cristal de 32,768 kHz en PC14/PC15 y alimentación VBAT para el RTC desde pila de botón o desde 3V3 con diodo, nunca desde la LiPo	Aprovecha el RTC y los modos de bajo consumo. | VBAT admite como máximo 3,6 V y una LiPo cargada llega a 4,2 V
R10 |	Footprint para transceptor CAN, conectado a los pines remapeados PB8/PB9	| Aprovecha el controlador CAN (con CAN FD en el C8T6) sin chocar con USB en PA11/PA12
R11 |	VBUS negociado disponible en un header dedicado, a través de un interruptor de carga controlado por GPIO, con fusible PTC, LED indicador y serigrafía de advertencia de tensión	| Permite usar la placa como fuente PD programable para otros proyectos
R12 |	Medición de la tensión de VBUS con divisor (≈ ×11) hacia un canal ADC; opcionalmente también hacia un comparador interno para corte rápido de R11 |	Verificar que el contrato negociado se cumple antes de habilitar la salida
D1 |	Cargador LiPo con conector JST-PH y medición del voltaje de batería mediante divisor |	Uso portátil; complementa el bajo consumo del chip
D10 |	Medición de la corriente de la salida R11 con resistencia shunt amplificada por el OPA/PGA interno	| Demuestra los periféricos analógicos del chip
D11 |	Modo source de 5 V mediante un segundo interruptor de carga, deshabilitado por defecto |	Permite experimentar con los roles source y DRP del controlador PD

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
