# Validación Tamizaje RD

Página para que un oftalmólogo gradúe fondos de ojo **a ciegas** y compare su graduación con el modelo v4 de retinopatía diabética
(MobileNetV3-Large 384 px + Grad-CAM) y con el segmentador de lesiones (MA, HE, EX, SE). Proyecto de investigación de la Universidad de Monterrey.

**Abrir:** https://victorinox93.github.io/validacion-rd/

## Privacidad

Los modelos se ejecutan dentro del navegador con ONNX Runtime Web (WebAssembly). Las imágenes se leen en memoria, se analizan
y se descartan al pasar a la siguiente: **no se suben a ningún servidor ni se guardan**. GitHub solo entrega la página y los modelos.
El registro de la sesión contiene únicamente texto y números y se exporta como CSV.

## Uso

1. Abrir la página y, en *Verificación del modelo en este navegador*, correr **Verificar casos de prueba** (debe dar 3/3 y 3/3).
2. Llenar evaluador y equipo de captura; dejar activo el **modo ciego**.
3. Arrastrar las imágenes (con nombres codificados, sin datos del paciente) y graduar cada una antes de ver el resultado del modelo.
4. Al terminar, **Guardar CSV**.

## Contenido

| Ruta | Qué es |
|---|---|
| `index.html` | Página; preprocesamiento idéntico a `cv2.INTER_AREA` (verificado bit a bit) |
| `modelos/clasificador.part*.txt` | `modelo_rd_fp32.onnx` en base64, en 3 partes |
| `modelos/segmentador.part*.txt` | `segmentador_lesiones_fp32.onnx` en base64, en 3 partes |
| `modelos/ort*` | ONNX Runtime Web 1.22.0 |
| `casos/` | 3 casos de prueba con valores esperados del notebook |

Uso local: `python -m http.server 8000` en esta carpeta y abrir `http://localhost:8000`.

> Herramienta de tamizaje en investigación, no de diagnóstico. Sus resultados no deben usarse para decisiones clínicas.
