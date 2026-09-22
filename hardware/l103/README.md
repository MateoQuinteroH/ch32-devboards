# Placa L103 — CH32L103

## Crear el proyecto KiCad

Esta carpeta ya contiene `sym-lib-table`, `fp-lib-table` y `kibot.yaml`. Para empezar:

1. En KiCad: *Archivo → Nuevo proyecto*, elige **esta carpeta** y usa el nombre **`l103`**
   (se crearán `l103.kicad_pro`, `l103.kicad_sch` y `l103.kicad_pcb`).
2. Si KiCad pregunta por las tablas de librerías, conserva las existentes.
3. En *Archivo → Configuración de página* del esquemático y del PCB, llena **Título**, **Revisión** y **Autor**.
4. Coloca en la serigrafía un texto con `L103 ${REVISION} ${GIT_HASH}` para que la revisión y el commit queden impresos.

## Archivos

| Archivo | Uso |
|---|---|
| `l103.kicad_pro` / `.kicad_sch` / `.kicad_pcb` | Fuentes del diseño |
| `sym-lib-table`, `fp-lib-table` | Enlazan las librerías de `lib/` |
| `kibot.yaml` | Generación automática de fabricación y documentación |
| `ERRATA.md` | Errores conocidos por revisión |

## Generar salidas localmente (opcional)

Con [KiBot](https://github.com/INTI-CMNB/KiBot) instalado:

```bash
kibot -c kibot.yaml -b l103.kicad_pcb -e l103.kicad_sch -d ../../output/l103
```
