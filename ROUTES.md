# Alcances públicos de reproducción

Este paquete permite reconstruir el método completo del TFM o una rama individual con datos propios/autorizados. Elegir una rama individual **no equivale** a reproducir la comparación completa de los tres modelos.

## Alcances disponibles

| `alcance` | Etapas | Resultado metodológico esperado |
|---|---|---|
| `completo` | `01 -> 02 -> 03 -> 04 -> 05 -> 06 -> 07 -> 08` | Reconstruye el experimento comparativo con HOG+SVM, CNN ligera y ResNet50; después completa la evaluación ampliada del ResNet50. |
| `hog_svm` | `01 -> 02` | Reconstruye únicamente HOG+SVM y su evaluación definida en la especificación 02. |
| `cnn_ligera` | `01 -> 03` | Reconstruye únicamente la CNN ligera, incluidas las pasadas y calibraciones definidas en la especificación 03. |
| `resnet50` | `01 -> 04 -> 05 -> 06 -> 07 -> 08` | Reconstruye la rama ResNet50, incluida evaluación ROC/PR/AP, selección de umbral en validation, Grad-CAM y evaluación final. |

`alcance=completo` es el valor por defecto del prompt maestro.

## Reglas comunes

- Use únicamente datos propios o autorizados compatibles con `DATASET_SPEC.md`.
- La etapa 01 genera la pertenencia de su propia réplica a train/validation/test.
- No ajuste hiperparámetros ni umbrales con test.
- En ResNet50, `tau*` se selecciona exclusivamente en validation (etapa 06) y se aplica después a test (etapa 08).
- Una ejecución `hog_svm`, `cnn_ligera` o `resnet50` no puede afirmar que reconstruyó la comparación completa.
- Los resultados históricos del TFM son referencias, no objetivos ni umbrales transferibles.
- Una réplica con otro dominio, equipo, población o prevalencia debe documentar esas diferencias.

Para el orden detallado consulte [PIPELINE_ORDER.md](PIPELINE_ORDER.md) y las especificaciones en [`specs/`](specs/).
