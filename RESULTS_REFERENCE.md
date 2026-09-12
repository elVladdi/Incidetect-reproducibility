# Referencia de resultados

## Resultados históricos del TFM
Son referencia del dataset restringido y no se sustituyen. ResNet histórico: ROC-AUC test 0.859626, AP de 05/06 0.734204 y `tau*` 0.6017084718.

## Verificación posterior F-08/F-09
F-09 confirmó fine-tuning convolucional efectivo. HOG+SVM fue exacto. CNN: P1/P2/P4 excedieron tolerancia; P3 coincidió en test pero no sustituye P4, que sigue como configuración histórica final. El diagnóstico es D2 (ejecución procedimentalmente equivalente; variación computacional plausible), sin causa única demostrada. No se declara reproducción numérica exacta externa de CNN: una implementación independiente puede producir métricas distintas aunque sea procedimentalmente equivalente.

ResNet P03 quedó dentro de tolerancia: `tau*` de validation 0.5992966890, test `[[36,8],[4,13]]`, accuracy 0.803279, precision 0.619048, recall 0.764706, F1 0.684211, ROC-AUC 0.860963, AP 05/06 0.728646 y área trapezoidal PR de 08 0.720702. Latencia post-hoc, no histórica: CPU/batch 1/50 warmups/200 medidas, mediana 123.767 ms con tensor y 338.374 ms extremo a extremo.

## Réplicas futuras
No existen aún ni tienen métricas garantizadas. Reentrene, seleccione umbral y evalúe sobre el dataset externo; no transfiera estos valores.
