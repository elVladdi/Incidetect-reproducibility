# Paquete de replicación metodológica INI

**Modalidad B: especificación metodológica sin publicación de código fuente.** Este paquete permite construir una implementación independiente sobre datos propios o autorizados; no contiene software histórico, datos del TFM, imágenes, identificadores reales ni modelos entrenados.

## Contrato de datos
Una fila por imagen: `image_id,label`; `image_id` es un ID único de imagen y no un número INI. Unidad experimental: 1 imagen = 1 INI = 1 observación independiente. Clases: 0 Sin incidencia, 1 Con incidencia. El TFM histórico tuvo 404 observaciones (117 positivas, 287 negativas); no se publican sus archivos ni membership.

Entradas externas: imágenes INI autorizadas, legibles y etiquetadas; no use rutas o IDs del TFM. Salidas esperadas: tensores, tabla de partición, métricas, figuras y artefactos de modelo de la implementación nueva.
