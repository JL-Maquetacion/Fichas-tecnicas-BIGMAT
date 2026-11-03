# Guía para Claude

Proyecto: web estática de un solo fichero (`index.html`) que genera fichas técnicas en PDF desde un Excel local. Lee `README.md` para el formato de datos y la estructura.

## Reglas

- Mantener todo en `index.html` (HTML, CSS y JS). Sin build, sin frameworks, sin servidor.
- Sin base de datos ni subida de datos: Excel e imágenes se procesan solo en el navegador.
- Scripts externos solo por CDN con **versión fijada** (cdnjs). No usar `localStorage` para datos de producto.
- No imprimir ni pegar los bloques base64 (`ASE`, `BM`, logo de cabecera): son muy largos. Edita con `str_replace` sobre trozos concretos.
- La ficha mide 105 × 148 mm; las posiciones de `.page > *` están en mm. Si cambias una caja, revisa `fit()` (los textos se encogen entre `data-max` y `data-min`).
- Respetar la paleta (azul `#004b8d`, rojo `#ed1c24`) y Montserrat.
- Textos de interfaz en español.

## Cómo probar

Abrir `index.html` en el navegador, cargar `ejemplo/productos_ejemplo.csv` y una carpeta de imágenes, y comprobar vista previa y descarga de PDF (una ficha y todas, en A4/A5/A6).
