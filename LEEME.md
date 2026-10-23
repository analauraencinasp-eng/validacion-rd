# Validación Tamizaje RD (versión local, sin internet)

Página para que un oftalmólogo gradúe fondos de ojo **a ciegas** y los compare con el modelo v4 y el segmentador de lesiones.
Los modelos corren dentro del navegador (ONNX Runtime Web + WebAssembly). Las imágenes **no se suben ni se guardan**: se leen
en memoria, se analizan y se descartan al pasar a la siguiente. El registro (CSV) solo contiene texto y números.

## Cómo abrirla

1. Doble clic en `iniciar_validacion.bat` (necesita Python; con Anaconda ya está). Se abre el navegador en `http://localhost:8000`.
   Deja abierta la ventana negra mientras uses la página; ciérrala al terminar.
2. Abre *Verificación del modelo en este navegador* → **Verificar casos de prueba**. Debe salir *Clasificador 3/3 · Segmentador 3/3*.
3. Llena evaluador y equipo de captura, arrastra las imágenes y gradúa cada una antes de ver el resultado del modelo.
4. Al terminar, **Guardar CSV**. El registro también queda en el navegador (solo texto) hasta que se borre.

No funciona con doble clic directo sobre `index.html` (el navegador bloquea la carga de modelos desde `file://`).

## Publicarla en internet (opcional)

La carpeta completa es un sitio estático: se puede subir tal cual a GitHub Pages, Netlify o un servidor de la UDEM.
Aun publicada, las imágenes se procesan en la computadora de quien la abre; el servidor solo entrega los modelos.

## Contenido

| Ruta | Qué es |
|---|---|
| `index.html` | La página (preprocesamiento idéntico a `cv2.INTER_AREA`, verificado bit a bit) |
| `modelos/clasificador.part*.txt` | `modelo_rd_fp32.onnx` en base64, en 3 partes |
| `modelos/segmentador.part*.txt` | `segmentador_lesiones_fp32.onnx` en base64, en 3 partes |
| `modelos/ort*` | ONNX Runtime Web 1.22.0 (motor WebAssembly) |
| `casos/` | Los 3 casos de prueba de la app |

Para cambiar de modelo: regenerar las partes con `base64 -w0 modelo.onnx | split -b 8400000 -d -a 1 - clasificador.part` y
renombrar a `.txt`; actualizar `umbral` y `version` en el bloque `CFG` de `index.html`.

Herramienta de tamizaje en investigación, no de diagnóstico.
