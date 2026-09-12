# 02 HOG y SVM

Entrada: tensor gris 224×224 de 01. HOG: 9 orientaciones, píxeles por celda 16×16, celdas por bloque 2×2, L2-Hys, `transform_sqrt=false`, vector plano. Ajuste StandardScaler sólo en train y transforme validation/test con ese mismo ajuste.

La clasificación normativa usa `LinearSVC.predict` sobre las features HOG escaladas. Si una implementación expresa la misma decisión mediante `decision_function`, la frontera binaria es score 0, no un umbral de probabilidad. LinearSVC: C=1.0, class_weight=balanced, random_state=42, max_iter=10000. No se fijan defaults no configurados explícitamente. F-08: concordancia exacta.
