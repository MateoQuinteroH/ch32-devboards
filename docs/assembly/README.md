# Guía de ensamblaje manual

## Herramientas recomendadas

- Cautín con punta fina (cónica o de cincel pequeño) y control de temperatura.
- Flux (en lápiz o gel) y malla desoldadora.
- Pinzas, lupa o microscopio.
- Alcohol isopropílico para limpiar.

## Orden de soldadura sugerido

Se sueldan primero los componentes más bajos y los más difíciles:

1. **Microcontrolador (LQFP48).** Fija dos esquinas opuestas, verifica la alineación, suelda con arrastre y flux, y limpia puentes con malla.
2. **Conector USB-C.**
3. **Regulador y circuitos de protección.**
4. **Pasivos** (resistencias y condensadores).
5. **LEDs** (respeta la polaridad).
6. **Botones y conectores** (Qwiic, depuración).
7. **Headers de pines** al final. Usa una protoboard como guía para que queden rectos.

## Antes de conectar por primera vez

- [ ] Inspección visual: sin puentes de estaño en el MCU ni en el USB-C.
- [ ] Con multímetro: **sin cortocircuito** entre VBUS y GND, ni entre +3V3 y GND.
- [ ] Conecta con una fuente con límite de corriente (o un USB con medidor) y verifica 3,3 V.

<!-- Enlaza aquí el iBOM interactivo de la última release: muestra dónde va cada componente. -->
