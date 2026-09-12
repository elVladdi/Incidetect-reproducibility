# 07 Grad-CAM

Use el backbone/submodelo `resnet50`. La última capa convolucional objetivo es `conv5_block3_out`, dentro del backbone; no confunda el nombre del submodelo con la capa objetivo. Para salida positiva, obtenga activaciones de esa capa y el gradiente de la salida positiva respecto de ellas; promedie espacialmente gradientes por canal, combine ponderadamente activaciones, aplique ReLU, normalice a [0,1], reescale a la imagen y superponga el mapa.

El umbral visual .5 se mantiene separado de `tau*`, que gobierna evaluación operativa. **GAP-F10-04** sólo cubre la selección histórica completa de ejemplos: una réplica debe declarar su propio criterio y no atribuirlo al TFM.
