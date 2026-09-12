# 05 ROC Precision Recall y AP

Etapa basada en scores del ResNet50 ya entrenado, aplicada separadamente a train, validation y test. Para cada partición obtenga la probabilidad positiva; construya `roc_curve`, calcule ROC-AUC (`roc_auc_score` o equivalente), construya `precision_recall_curve` y calcule **Average Precision (AP)** (`average_precision_score` o equivalente). Genere y exporte curvas y valores por partición.

Esta etapa no selecciona ni ajusta un umbral operativo usando test y no añade matrices ni métricas binarias a umbral .5. AP sigue siendo distinto del área trapezoidal bajo la curva PR calculada separadamente por etapa 08.
