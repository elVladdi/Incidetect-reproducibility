# Prompt maestro para generar una implementación independiente

Repositorio público de procedencia: `https://github.com/elVladdi/Incidetect-reproducibility`

## Preparación obligatoria

1. **Recomendación:** descargue o clone el repositorio completo antes de ejecutar este prompt.
2. Defina `PAQUETE_RAIZ` como la carpeta local que contiene, como mínimo:
   - `README.md`
   - `DATASET_SPEC.md`
   - `ENVIRONMENT_REFERENCE.md`
   - `ROUTES.md`
   - `PIPELINE_ORDER.md`
   - `specs/`
   - `prompts/`
   - `examples/`
   - `manifest/`
3. Trabaje con rutas relativas a `PAQUETE_RAIZ`.
4. No use como fuente metodológica archivos privados o externos a `PAQUETE_RAIZ`.
5. Lea primero `PAQUETE_RAIZ/README.md`.
6. Si la ejecución requiere trazabilidad estricta, valide `PAQUETE_RAIZ/manifest/SHA256SUMS.txt` antes de implementar.
7. Exija un dataset **propio o autorizado** compatible con `PAQUETE_RAIZ/DATASET_SPEC.md`. No solicite ni reconstruya datos, IDs, código, notebooks o checkpoints históricos del TFM.

Este prompt ayuda a crear **software nuevo e independiente** a partir de las especificaciones públicas. No contiene código histórico y no garantiza reproducir las métricas históricas.

## Parámetro de alcance

Acepte exactamente uno de estos valores:

- `alcance=completo` — valor por defecto si el usuario no especifica otro.
- `alcance=hog_svm`
- `alcance=cnn_ligera`
- `alcance=resnet50`

Antes de generar código, informe qué alcance se utilizará.

### `alcance=completo`

Lea:

- `DATASET_SPEC.md`
- `ENVIRONMENT_REFERENCE.md`
- `ROUTES.md`
- `PIPELINE_ORDER.md`
- `SCOPE_AND_EVIDENCE.md`
- `RESULTS_REFERENCE.md`
- `specs/01_PROCESAMIENTO_PARTICIONADO.md`
- `specs/02_HOG_SVM.md`
- `specs/03_CNN_LIGERA.md`
- `specs/04_RESNET50.md`
- `specs/05_EVALUACION_ROC_PR_AP.md`
- `specs/06_UMBRAL_OPERATIVO_RESNET50.md`
- `specs/07_GRADCAM.md`
- `specs/08_EVALUACION_FINAL.md`

Implemente el experimento comparativo completo:

`01 -> 02 HOG+SVM -> 03 CNN ligera -> 04 ResNet50 -> 05 -> 06 -> 07 -> 08`

Solo este alcance permite declarar que se reconstruyó la comparación completa de los tres enfoques.

### `alcance=hog_svm`

Lea:

- `DATASET_SPEC.md`
- `ENVIRONMENT_REFERENCE.md`
- `ROUTES.md`
- `PIPELINE_ORDER.md`
- `specs/01_PROCESAMIENTO_PARTICIONADO.md`
- `specs/02_HOG_SVM.md`

Implemente:

`01 -> 02`

No afirme haber reconstruido la comparación completa ni el modelo final ResNet50.

### `alcance=cnn_ligera`

Lea:

- `DATASET_SPEC.md`
- `ENVIRONMENT_REFERENCE.md`
- `ROUTES.md`
- `PIPELINE_ORDER.md`
- `SCOPE_AND_EVIDENCE.md`
- `specs/01_PROCESAMIENTO_PARTICIONADO.md`
- `specs/03_CNN_LIGERA.md`

Implemente:

`01 -> 03`

Respete P1–P4 y las reglas de calibración definidas en la especificación 03. No afirme haber reconstruido la comparación completa.

### `alcance=resnet50`

Lea:

- `DATASET_SPEC.md`
- `ENVIRONMENT_REFERENCE.md`
- `ROUTES.md`
- `PIPELINE_ORDER.md`
- `SCOPE_AND_EVIDENCE.md`
- `RESULTS_REFERENCE.md`
- `specs/01_PROCESAMIENTO_PARTICIONADO.md`
- `specs/04_RESNET50.md`
- `specs/05_EVALUACION_ROC_PR_AP.md`
- `specs/06_UMBRAL_OPERATIVO_RESNET50.md`
- `specs/07_GRADCAM.md`
- `specs/08_EVALUACION_FINAL.md`

Implemente:

`01 -> 04 -> 05 -> 06 -> 07 -> 08`

Este alcance reconstruye únicamente la rama ResNet50 y no demuestra la comparación completa ni superioridad externa frente a HOG+SVM o CNN.

## Reglas metodológicas obligatorias

- Mantenga separación explícita entre train, validation y test.
- Ajuste transformaciones entrenables y modelos solo con train, salvo el uso de validation definido en las especificaciones.
- Seleccione/calibre umbrales únicamente con validation cuando el procedimiento lo requiera.
- En ResNet50, seleccione `tau*` en validation y aplíquelo después a test **sin reajuste**.
- No use test para tomar decisiones adaptativas de arquitectura, hiperparámetros o umbral.
- Mantenga **Average Precision (AP)** separado del área trapezoidal bajo la curva Precision–Recall.
- No transfiera `tau*`, métricas, matrices o checkpoints históricos a datos externos.
- Registre semilla, versiones de dependencias, hardware relevante y criterios de selección de artefactos de su propia ejecución.
- Si el dataset difiere en equipo, dominio, población o condiciones de adquisición, documente el posible `domain shift`.
- No presente resultados de un dataset externo como reproducción numérica del TFM.

## Decisiones abiertas y brechas

Antes de programar cada etapa, clasifique cada decisión como:

1. **fijada por la especificación**;
2. **detalle de implementación legítimamente abierto**;
3. **decisión metodológica esencial no documentada**.

Si encuentra una decisión esencial no documentada:

- no la invente silenciosamente;
- detenga el componente afectado;
- registre `GAP-RECON-XX`;
- explique qué información falta y por qué impide una reconstrucción fiel.

No utilice conocimiento del código histórico para rellenar la brecha.

## Salida esperada de la IA/agente

Entregue:

1. alcance utilizado;
2. lista de documentos leídos desde `PAQUETE_RAIZ`;
3. matriz `decisión -> fuente -> estado (fijada/abierta/brecha)`;
4. estructura de la implementación nueva;
5. código nuevo correspondiente al alcance solicitado;
6. validaciones de contratos de entrada/salida;
7. controles explícitos contra fuga entre train/validation/test;
8. registro del procedimiento de selección de umbral, si aplica;
9. reporte de métricas producido por la nueva ejecución, si el usuario aporta datos y autoriza ejecutar;
10. lista de brechas `GAP-RECON-XX`, si existen;
11. commit/versión del paquete de especificaciones utilizado.

La implementación generada debe quedar separada de `PAQUETE_RAIZ` o en un directorio de trabajo que el usuario autorice; no modifique silenciosamente las especificaciones públicas.

## Interpretación de resultados

Use `RESULTS_REFERENCE.md` únicamente como referencia histórica y de límites. Una réplica correcta puede producir métricas diferentes.

No declare que ResNet50 "debe ganar" en un dataset externo. No declare cumplimiento de criterios del TFM a partir de valores históricos. Evalúe cualquier criterio nuevo sobre los resultados de la nueva ejecución.

## Cierre

Si todas las decisiones esenciales del alcance elegido están especificadas, genere una implementación independiente y documentada. Si falta una decisión esencial, entregue primero la brecha y detenga únicamente el componente afectado en lugar de completar la metodología por suposición.
