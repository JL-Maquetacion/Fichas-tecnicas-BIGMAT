# Generador de fichas técnicas

Aplicación web **estática** (un único `index.html`, sin servidor ni base de datos) que genera fichas técnicas de producto en PDF a partir de un Excel y una carpeta de imágenes.

- **Los datos viven en tu ordenador.** El Excel y las imágenes se leen en el navegador; no se suben a ningún sitio. La "base de datos" es el propio fichero Excel/CSV.
- **Publicación:** GitHub Pages sirve el `index.html` tal cual.

## Cómo se usa

1. Abre la web (la URL de GitHub Pages de este repositorio).
2. **1 · Datos:** selecciona el Excel (`.xlsx`, `.xls` o `.csv`). Se usa la primera hoja.
3. **2 · Imágenes y logos:** selecciona la carpeta con las imágenes de producto, logos y pictogramas.
4. Revisa las fichas con las flechas o el desplegable, elige tamaño de papel (A4, A5 o A6) y pulsa **Descargar PDF de esta ficha** o **de todas**.

El panel de estado avisa de los productos sin imagen, logo o picto emparejado.

## Formato del Excel

Primera fila = cabeceras. No importan mayúsculas ni tildes (`Característica 1` = `CARACTERISTICA_1`). Hay un ejemplo en [`ejemplo/productos_ejemplo.csv`](ejemplo/productos_ejemplo.csv).

| Cabecera | Para qué sirve |
|---|---|
| `Codigo LN` | Referencia del producto (se rellena con ceros hasta 8 dígitos). Imprescindible junto con `Modelo` para que la fila sea un producto. |
| `Tipología de Producto` | Título de la cabecera de la ficha y clave de plantilla. |
| `Modelo` | Modelo, debajo de la tipología. |
| `Marca` | Si no hay `Nombre Logo`, se busca `<Marca>.png` en la carpeta; si no se encuentra, se imprime el texto. |
| `Nombre Imagen` | Opcional. Si falta, se usa `<Codigo LN>.png`. |
| `Nombre Logo` | Opcional. Si falta, se usa `<Marca>.png`. |
| `Nombre Caracteristica 1..5` y `Característica 1..5` | Las 5 filas de la tabla (nombre y valor). |
| `Incluye` | Lista de lo que incluye; separa elementos con salto de línea, `;` o `\|`. |
| `Añade y mejora tu compra` | Texto de la caja roja. |
| `Picto ventaja` | Nombre del fichero del pictograma (esquina de la imagen). |

**Filas plantilla:** una fila con `Tipología de Producto` pero **sin** `Codigo LN` ni `Modelo` actúa como plantilla: sus `Nombre Caracteristica 1..5` se copian a los productos de esa tipología que los tengan vacíos. Así defines los nombres de las características una vez por tipología.

**Emparejado de imágenes:** sin distinguir mayúsculas y con o sin extensión. La imagen de producto se busca por los 8 primeros caracteres del nombre de fichero igual al `Codigo LN` (con ceros), por `Nombre Imagen` o por el código.

## Estructura del repositorio

```
index.html                   # Toda la aplicación (HTML + CSS + JS). Logos incrustados en base64.
ejemplo/productos_ejemplo.csv# Ejemplo de datos
.nojekyll                    # Evita que GitHub Pages procese el sitio con Jekyll
CLAUDE.md                    # Guía para editar el proyecto con Claude
README.md
```

Dentro de `index.html`:

- `<style>`: diseño de la página y de la ficha (`.page` mide 105 × 148 mm y se escala con `--z`).
- `pageHTML(r)`: construye el HTML de una ficha a partir de una fila.
- `fit()`: reduce el tamaño de letra hasta que el texto cabe en su caja (`data-max` / `data-min` en pt).
- Carga de Excel: manejador `$('fx').onchange`. Carga de imágenes: `loadImgs`.
- `makePdf(all)`: renderiza cada ficha con html2canvas, la añade a jsPDF y descarga el PDF.
- Constantes `ASE` y `BM`: imágenes incrustadas (sello y logo del pie).

Librerías (por CDN, cdnjs): SheetJS `xlsx` 0.18.5, `jsPDF` 2.5.1, `html2canvas` 1.4.1. Fuente: Montserrat (Google Fonts). **Se necesita conexión a internet** para cargarlas.

## Desarrollo local

No hay build. Abre `index.html` en el navegador, o sirve la carpeta con `python3 -m http.server` y entra en <http://localhost:8000>.

## Publicar en GitHub Pages

1. Sube el contenido de esta carpeta a la rama `main`.
2. En GitHub: **Settings → Pages → Build and deployment → Source: Deploy from a branch → Branch: `main` / `(root)` → Save**.
3. En un minuto aproximadamente la web queda en `https://<usuario>.github.io/<repositorio>/`.

Cada `push` a `main` actualiza la web.

## Editar con Claude

Abre el repositorio en Claude Code (o pega `index.html` en Claude) y pide el cambio. Lee antes [`CLAUDE.md`](CLAUDE.md).
