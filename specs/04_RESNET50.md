# 04 ResNet50

Repita el canal gris para RGB 224×224×3. ResNet50 ImageNet sin top; dentro del modelo convierta entrada [0,1] a escala 0..255 y aplique preprocesado estándar ResNet50. Backbone inicialmente congelado y llamado `training=false`; GAP, Dense 256, BN, ReLU, Dropout .5, salida sigmoide. Semilla global 42; class weights balanced calculados sólo en train. Adam/BCE/accuracy.

Fase 1: LR 1e-4, hasta 10, batch 32, EarlyStopping val_loss patience 5/restaurar mejores pesos, checkpoint val_loss mejor y CSV. Fase 2: LR 1e-5, hasta 20, mismos callbacks. Habilite conv5 no-BN y mantenga BN backbone congeladas; no agregue `base_model.trainable=true`. F-09 confirmó actualización efectiva conv5 bajo esta semántica.
