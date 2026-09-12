# 03 CNN ligera

## Reglas comunes
Antes de cada entrenamiento formal fije semilla global 42 en Python, NumPy y TensorFlow. Entrada gris 224×224×1. Arquitectura: bloques Conv2D 3×3 same con 32, 64 y 128 filtros; cada bloque BatchNormalization, ReLU y MaxPool 2×2. Head: Flatten, Dense 256, BatchNormalization, ReLU, Dropout 0.5 y salida sigmoide. Adam y BCE, excepto pérdida focal P3.

Todo entrenamiento usa EarlyStopping `val_loss`, patience indicada y `restore_best_weights=true`; ModelCheckpoint `val_loss`, `save_best_only=true`; y CSVLogger. P3/P4 añaden ReduceLROnPlateau sobre `val_loss`, factor .5, patience 3, `min_lr=1e-6`.

## Pasadas y calibración
- **P1:** LR .001, batch 32, máximo 50 épocas, patience 10; class weights balanced calculados sólo en train. Se evalúa a umbral .5. Entrena con arrays NumPy y debe barajar train en cada época (`shuffle=true`).
- **P2:** no reentrena; reutiliza P1. Calibra en validation con grid ascendente .05, .10, …, .95, maximizando F1 de la clase positiva. El mejor candidato sólo se reemplaza ante F1 estrictamente mayor: en empate se conserva el primer, por tanto menor, umbral empatado. Después aplica ese umbral fijado a train/validation/test.
- **P3:** LR .0003, batch 32, máximo 80, patience 15, sin class weights; focal `alpha=.75`, `gamma=2`, con ReduceLROnPlateau. Entrena con arrays NumPy y `shuffle=true`; después calibra su propio umbral en validation con el mismo grid/desempate y lo aplica a train/validation/test.
- **P4:** LR .001, batch 32, máximo 80, patience 15, sin class weights, con ReduceLROnPlateau. Augmentation sólo en entrenamiento: traslación horizontal/vertical .05, zoom .05 y contraste .10. Tras entrenar, calibra su propio umbral en validation con el mismo grid/desempate y lo aplica a train/validation/test.

## Batches balanceados P4
Separe train por clase. Baraje/repite clase 0 con semilla 42 y clase 1 con 43; muestree ambos flujos con pesos .5/.5 y semilla 42. Use `steps_per_epoch=ceil(2*max(n0,n1)/batch_size)`. Validation conserva batches normales, sin shuffle; este mecanismo no equivale al shuffle de arrays de P1/P3.

## Estado de evidencia
P4 sigue como configuración histórica final. F-08: P1/P2/P4 fuera de tolerancia; P3 coincidió en test pero no sustituye P4. D2 no tiene causa única demostrada. La especificación permite equivalencia procedimental, no identidad numérica externa.
