# Requerimientos de la familia CH32 DevBoards

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

### Generales

* R-01. Microcontrolador WCH CH32L103.
* R-02. Precio de venta muy por debajo de 25 USD. El costo unitario objetivo queda por definir tras cotizar el BOM.
* R-03. Formato físico por definir. Candidatos: Feather o XIAO.
* R-04. Hardware open source, con licencia abierta explícita.

### USB y potencia

* R-05. Puerto USB-C en rol PD Sink, hasta 20 V. Sin rol Source ni DRP en la v1.
* R-06. Datos USB Full Speed simultáneos con la negociación PD.
* R-07. Salida a bornera con el voltaje negociado, controlada por load switch.
* R-08. eFuse con límite de corriente en la salida. Corriente máxima por definir: 3 A con cable normal, 5 A requiere e-marker.
* R-09. La bornera arranca apagada hasta que el firmware la habilita.
* R-10. Comportamiento de la bornera sin cargador PD por definir: apagada o voltaje de batería.
* R-11. Riel de 3,3 V con regulador de muy bajo consumo en reposo.

### Batería

* R-12. Batería 1S Li-ion/LiPo con conector JST-PH 2.0 y polaridad Adafruit.
* R-13. Cargador conmutado con power-path.
* R-14. Entrada para NTC y LED de estado de carga.
* R-15. Lectura del voltaje de la batería por ADC.

### Medición

* R-16. Medición de voltaje y corriente de la bornera, con INA externo o con shunt más el OPA interno (por decidir según costo).
* R-17. Jumpers de corte para medir el consumo del micro con instrumento externo.
* R-18. LEDs desconectables para medir el sleep real.
* R-19. Consumo total de la placa en sleep medido y publicado.

### Protección

* R-20. TVS en VBUS y protección contra polaridad inversa.
* R-21. Todo lo conectado a VBUS soporta 25 V o más.

### Expansión

* R-22. Conector Qwiic/STEMMA QT (I2C).
* R-23. Footprint opcional para transceptor CAN y su conector.

### Software

* R-24. Bootloader USB de fábrica compatible con herramientas abiertas; ideal UF2.
* R-25. Pads SDI para WCH-LinkE.
* R-26. Soporte en PlatformIO desde el lanzamiento; Arduino cuando el core sea estable.
* R-27. SDK en C para MounRiver o GCC.
* R-28. Librería con API simple, por ejemplo pd.request(), power.read() y model.classify().
* R-29. Tres demos: fuente programable con OLED, perfilador de consumo y clasificador de cargas con ML.
* R-30. Herramienta de registro de datos a CSV y scripts en Python para entrenar y exportar modelos.

### Documentación

* R-31. Bilingüe español–inglés.
* R-32. Esquemáticos comentados, cálculos de diseño, archivos KiCad y BOM.
* R-33. Hoja de datos de la placa con mediciones reales y límites de la bornera.
* R-34. Guía de seguridad para trabajar con 20 V.
* R-35. Tutoriales graduales y diario de diseño público.

### Marca y cumplimiento

* R-36. No usar el logo USB ni declarar certificación USB-IF.
* R-37. No incluir la celda de litio en envíos internacionales.

## Formato físico

Pendiente de definir. Debe cumplir O8 y O9, ser común a ambas placas y alojar los componentes adicionales de la L103.
