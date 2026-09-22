# Placa X035 — CH32X035C8T6

## Crear el proyecto KiCad

Esta carpeta ya contiene `sym-lib-table`, `fp-lib-table` y `kibot.yaml`. Para empezar:

1. En KiCad: *Archivo → Nuevo proyecto*, elige **esta carpeta** y usa el nombre **`x035`**
   (se crearán `x035.kicad_pro`, `x035.kicad_sch` y `x035.kicad_pcb`).
2. Si KiCad pregunta por las tablas de librerías, conserva las existentes.
3. En *Archivo → Configuración de página* del esquemático y del PCB, llena **Título**, **Revisión** y **Autor**.
4. Coloca en la serigrafía un texto con `X035 ${REVISION} ${GIT_HASH}` para que la revisión y el commit queden impresos.

## Archivos

| Archivo | Uso |
|---|---|
| `x035.kicad_pro` / `.kicad_sch` / `.kicad_pcb` | Fuentes del diseño |
| `sym-lib-table`, `fp-lib-table` | Enlazan las librerías de `lib/` |
| `kibot.yaml` | Generación automática de fabricación y documentación |
| `ERRATA.md` | Errores conocidos por revisión |

## Generar salidas localmente (opcional)

Con [KiBot](https://github.com/INTI-CMNB/KiBot) instalado:

```bash
kibot -c kibot.yaml -b x035.kicad_pcb -e x035.kicad_sch -d ../../output/x035
```
