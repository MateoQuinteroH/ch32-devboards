# Librerías compartidas

Librerías KiCad usadas por **todas** las placas de la familia.

| Carpeta | Contenido |
|---|---|
| `symbols/ch32.kicad_sym` | Símbolos propios (MCU, conectores, etc.) |
| `footprints/ch32.pretty/` | Footprints propios |
| `3dmodels/` | Modelos 3D (`.step` / `.wrl`) |

Los proyectos cargan estas librerías con rutas relativas (`${KIPRJMOD}/../../lib/...`) desde sus `sym-lib-table` y `fp-lib-table`, así que funcionan en cualquier máquina tras clonar el repositorio.

En los footprints propios, referencia los modelos 3D con la ruta `${KIPRJMOD}/../../lib/3dmodels/<modelo>.step`, que no depende de ninguna configuración local.
