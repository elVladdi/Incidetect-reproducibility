# Paquete de replicación metodológica INI

**Modalidad B: especificación metodológica sin publicación de código fuente.** Este paquete permite construir una implementación independiente sobre datos propios o autorizados; no contiene software histórico, datos del TFM, imágenes, identificadores reales ni modelos entrenados.

01 valida metadatos, transforma imágenes y particiona. 02 entrena/evalúa HOG/SVM. 03 ejecuta P1–P4 CNN. 04 entrena ResNet50 en dos fases. 05 calcula ROC, Precision–Recall y AP. 06 selecciona `tau*` sólo en validation. 07 genera Grad-CAM del modelo final. 08 aplica el `tau*` ya fijado a test. Ninguna etapa posterior puede usar test para optimizar hiperparámetros o umbral.
