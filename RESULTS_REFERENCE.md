# Referencia de resultados

Este archivo separa tres capas: resultados históricos del TFM, verificación post-hoc y resultados de futuras réplicas.

## Resultados históricos del TFM

Los valores siguientes corresponden al dataset restringido del TFM y no deben transferirse como objetivos a datos externos.

| Modelo/configuración | Matriz test `[TN, FP, FN, TP]` | Accuracy | Precision (+) | Recall (+) | F1 (+) | FPR | ROC-AUC | AP |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| HOG + SVM | `[32, 12, 9, 8]` | 0.6557 | 0.4000 | 0.4706 | 0.4324 | 0.2727 | — | — |
| CNN ligera P4, configuración histórica final | `[21, 23, 4, 13]` | 0.5574 | 0.3611 | 0.7647 | 0.4906 | 0.5227 | — | — |
| ResNet50 final | `[36, 8, 4, 13]` | 0.8033 | 0.6190 | 0.7647 | 0.6842 | 0.1818 | 0.8596 | 0.7342 |

En HOG+SVM y CNN, precision, recall, F1 y FPR de la tabla se derivan determinísticamente de las matrices históricas. No se publican ROC-AUC/AP comparables para esos dos modelos porque no están materialmente respaldados en el mismo alcance histórico comparable.

Para ResNet50, el umbral histórico fue `tau*=0.6017084718`, seleccionado en validation y aplicado después a test.

El criterio predictivo conjunto del TFM **no se cumplió**: ROC-AUC `0.8596 >= 0.85` y FPR `0.1818 <= 0.20` cumplieron individualmente, pero Recall `0.7647 < 0.80`.

## Verificación post-hoc

La verificación posterior no sustituye las cifras históricas:

- **HOG+SVM:** reproducción exacta de las matrices y métricas verificadas.
- **CNN ligera:** P1, P2 y P4 excedieron las tolerancias prerregistradas; P3 coincidió en test, pero no reemplaza P4 como configuración histórica final. El análisis concluyó que la ejecución era procedimentalmente equivalente con variación computacional plausible, sin una causa única demostrada.
- **ResNet50 y etapas 05–08:** dentro de tolerancia. En esa verificación se obtuvo `tau*=0.5992966890`, matriz test `[36,8,4,13]`, accuracy `0.803279`, precision `0.619048`, recall `0.764706`, F1 `0.684211`, ROC-AUC `0.860963` y AP en etapas 05/06 `0.728646`.
- En la etapa 08 de verificación se calculó por separado un **área trapezoidal bajo la curva Precision–Recall** de `0.720702`; no es el mismo estimador que Average Precision.
- La latencia medida posteriormente fue una verificación post-hoc en CPU, no una cifra histórica reconstruida.

La verificación técnica del fine-tuning confirmó que la lógica ResNet50 actualiza efectivamente variables convolucionales profundas en la fase correspondiente, sin demostrar identidad forense de un checkpoint histórico.

## Futuras réplicas

Una nueva réplica debe:

- usar sus propios datos/autorizados;
- producir su propia partición;
- entrenar sus propios modelos;
- seleccionar sus umbrales con validation;
- evaluar con test sin reajustar a partir de sus resultados;
- reportar las métricas obtenidas como resultados nuevos.

No transfiera `tau*`, matrices ni métricas históricas como valores esperados o garantizados.
