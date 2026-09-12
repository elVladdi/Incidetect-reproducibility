# Orden de ejecución del pipeline

Las etapas 01–08 describen una implementación independiente del método. La etapa 01 es común a todos los alcances; las ramas 02, 03 y 04 corresponden a los tres modelos evaluados.

## Etapas

1. **01 — Procesamiento y particionado:** valida metadatos, transforma imágenes a 224×224×1 y crea train/validation/test de la nueva réplica.
2. **02 — HOG + SVM:** extrae HOG, escala con parámetros ajustados solo en train, entrena LinearSVC y evalúa la rama clásica.
3. **03 — CNN ligera:** ejecuta las pasadas P1–P4 y sus reglas de calibración en validation.
4. **04 — ResNet50:** adapta a RGB y entrena ResNet50 en dos fases con transfer learning/fine-tuning.
5. **05 — ROC/PR/AP:** calcula scores y curvas ROC/Precision–Recall y Average Precision para ResNet50.
6. **06 — Umbral operativo:** selecciona `tau*` exclusivamente en validation bajo la restricción FPR definida.
7. **07 — Grad-CAM:** genera explicaciones locales para ResNet50 y mantiene separado el umbral visual 0.5 del umbral operativo.
8. **08 — Evaluación final:** aplica a test el `tau*` ya fijado y reporta las métricas finales del ResNet50.

## Dependencias por alcance

- `completo`: `01 -> 02`, `01 -> 03`, `01 -> 04 -> 05 -> 06 -> 07 -> 08`.
- `hog_svm`: `01 -> 02`.
- `cnn_ligera`: `01 -> 03`.
- `resnet50`: `01 -> 04 -> 05 -> 06 -> 07 -> 08`.

Las ramas HOG+SVM y CNN terminan con la evaluación definida en sus propias especificaciones; las etapas 05–08 pertenecen a la evaluación ampliada del ResNet50.

## Gobernanza train/validation/test

- Ajuste de modelos y transformaciones entrenables: train.
- Selección/calibración de umbrales cuando corresponda: validation.
- Evaluación final correspondiente: test.
- Test no debe utilizarse para decidir hiperparámetros, arquitectura o `tau*` en una nueva réplica.

Consulte [ROUTES.md](ROUTES.md) para elegir alcance y [`specs/`](specs/) para las reglas exactas.
