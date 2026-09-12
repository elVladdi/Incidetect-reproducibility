# Incidetect — replicación metodológica para clasificación de imágenes INI

Este repositorio publica la **especificación metodológica** de un piloto de clasificación binaria de imágenes de **Inspección No Intrusiva (INI)** en contexto aduanero. Su finalidad es que un tercero pueda construir una implementación nueva con **datos propios o autorizados**, reproducir el procedimiento completo o una rama concreta y continuar la investigación sin depender del código histórico ni del dataset restringido del estudio original.

Las clases del estudio son:

- `0 = Sin incidencia`
- `1 = Con incidencia`

El repositorio está dirigido a investigadores, profesionales de aduanas/INI, estudiantes y desarrolladores, y a cualquier equipo que disponga de imágenes INI legalmente accesibles y etiquetas defendibles.

## Qué se hizo en el estudio original

El experimento final utilizó **404 imágenes/observaciones independientes**: **117 Con incidencia** y **287 Sin incidencia**. Esta composición fue la del dataset experimental del estudio original y **no debe interpretarse como prevalencia operacional natural**.

Se evaluaron tres enfoques:

1. **HOG + SVM**, como línea base clásica.
2. **CNN ligera**, entrenada desde cero mediante cuatro configuraciones/pasadas.
3. **ResNet50 con transfer learning y fine-tuning**, seleccionado como modelo final del estudio original.

El estudio fue un piloto experimental. ResNet50 fue el enfoque llevado adelante para la evaluación ampliada, selección de umbral operativo y Grad-CAM, pero **no debe interpretarse como un modelo universalmente superior ni como un sistema validado para producción**.

## Resultados históricos de referencia

Los siguientes resultados corresponden al **test histórico del dataset restringido del estudio original**. Para HOG+SVM y CNN P4, precision, recall, F1 y FPR se derivan determinísticamente de las matrices históricas indicadas.

| Modelo/configuración | Matriz test `[TN, FP, FN, TP]` | Accuracy | Precision (+) | Recall (+) | F1 (+) | FPR | ROC-AUC | AP |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| HOG + SVM | `[32, 12, 9, 8]` | 0.6557 | 0.4000 | 0.4706 | 0.4324 | 0.2727 | — | — |
| CNN ligera P4, configuración histórica final | `[21, 23, 4, 13]` | 0.5574 | 0.3611 | 0.7647 | 0.4906 | 0.5227 | — | — |
| ResNet50 final, `tau*=0.6017084718` | `[36, 8, 4, 13]` | 0.8033 | 0.6190 | 0.7647 | 0.6842 | 0.1818 | 0.8596 | 0.7342 |

Estos valores son **referencias históricas del estudio original**, no objetivos garantizados para un nuevo dataset. En el criterio predictivo conjunto del trabajo, ResNet50 cumplió individualmente `ROC-AUC >= 0.85` y `FPR <= 0.20`, pero **no cumplió Recall >= 0.80** (`0.7647`), por lo que el criterio conjunto **no se alcanzó**.

Una verificación post-hoc posterior encontró:

- HOG+SVM: reproducción exacta de las matrices y métricas verificadas.
- CNN ligera: variación computacional relevante en P1/P2/P4; P3 coincidió en test, pero no reemplaza P4 como configuración histórica final. No se demostró una causa única.
- ResNet50 y sus etapas de evaluación: resultados dentro de la tolerancia prerregistrada.

Véase [RESULTS_REFERENCE.md](RESULTS_REFERENCE.md) para separar resultados históricos, verificación posterior y resultados de futuras réplicas.

## Qué contiene este repositorio

El paquete contiene:

- contratos y reglas de preparación de datos;
- especificaciones de las etapas 01–08;
- parámetros y decisiones metodológicas recuperadas;
- reglas de separación `train / validation / test`;
- evaluación ROC/PR y Average Precision;
- selección de `tau*` exclusivamente en validation;
- evaluación final en test;
- especificación de Grad-CAM;
- referencias de resultados históricos y límites de evidencia;
- un prompt maestro opcional para ayudar a una IA/agente de programación a crear una implementación nueva;
- un ejemplo sintético de estructura de dataset;
- un manifiesto SHA-256 para comprobar integridad.

## Qué no contiene

Este repositorio **no contiene**:

- las 404 imágenes originales del estudio original;
- identificadores operativos reales;
- CSV o metadata privados;
- código fuente o notebooks históricos;
- checkpoints o modelos entrenados;
- secretos, rutas locales o artefactos internos restringidos.

La frontera de publicación se detalla en [PUBLICATION_BOUNDARY.md](PUBLICATION_BOUNDARY.md).

## Por qué no se publica el dataset original

Las imágenes y los datos originales tienen restricciones de acceso y no publicación. Por ello, la replicación externa se plantea con **datos propios o autorizados**. Este repositorio no atribuye esas restricciones a una norma, convenio o fundamento institucional específico que no esté documentado.

La ausencia del dataset original significa que un tercero puede **replicar metodológicamente** el procedimiento, pero no se exige que reproduzca numéricamente las métricas históricas.

## Qué puede reproducir

Elija uno de estos alcances:

| `alcance` | Etapas | Qué reconstruye |
|---|---|---|
| `completo` | `01 -> 02 -> 03 -> 04 -> 05 -> 06 -> 07 -> 08` | Comparación completa HOG+SVM, CNN ligera y ResNet50, más evaluación ampliada del ResNet50. |
| `hog_svm` | `01 -> 02` | Solo la rama HOG+SVM y su evaluación definida en la etapa 02. |
| `cnn_ligera` | `01 -> 03` | Solo la rama CNN ligera y sus configuraciones/procedimientos. |
| `resnet50` | `01 -> 04 -> 05 -> 06 -> 07 -> 08` | Rama del modelo final ResNet50, evaluación ROC/PR/AP, umbral, Grad-CAM y evaluación final. |

Solo `alcance=completo` permite afirmar que se reconstruyó la **comparación de los tres enfoques**. Ejecutar una rama individual no demuestra el ranking comparativo del estudio original ni superioridad sobre otros modelos.

Véanse [ROUTES.md](ROUTES.md) y [PIPELINE_ORDER.md](PIPELINE_ORDER.md).

## Inicio rápido

1. Descargue o clone el repositorio completo:
   ```bash
   git clone https://github.com/elVladdi/Incidetect-reproducibility.git
   cd Incidetect-reproducibility
   ```
2. Trabaje desde la raíz que contiene `README.md`, `specs/`, `prompts/`, `examples/` y `manifest/`.
3. Si necesita una ejecución auditada, verifique los hashes de [manifest/SHA256SUMS.txt](manifest/SHA256SUMS.txt).
4. Prepare un dataset propio/autorizado conforme a [DATASET_SPEC.md](DATASET_SPEC.md). Puede usar [examples/dataset_example.csv](examples/dataset_example.csv) como ejemplo sintético de estructura.
5. Registre y configure su entorno conforme a [ENVIRONMENT_REFERENCE.md](ENVIRONMENT_REFERENCE.md).
6. Elija `alcance=completo`, `hog_svm`, `cnn_ligera` o `resnet50`.
7. Lea [ROUTES.md](ROUTES.md), [PIPELINE_ORDER.md](PIPELINE_ORDER.md) y las especificaciones aplicables en [`specs/`](specs/).
8. Implemente las etapas en el orden indicado y registre el commit/versión de este paquete que utilizó, junto con sus propias versiones de dependencias y cualquier decisión de implementación legítimamente abierta.
9. Cuando el procedimiento requiera elegir un umbral, hágalo **solo con validation**. Mantenga test fuera de decisiones adaptativas y úselo para la evaluación final correspondiente.
10. Reporte las métricas obtenidas como resultados de **su propio dataset y ejecución**. No presente `tau*` ni las métricas históricas del estudio original como valores transferibles.

## Preparación de datos

El contrato público está en [DATASET_SPEC.md](DATASET_SPEC.md). En términos mínimos:

- una fila por imagen con `image_id,label`;
- `image_id` debe ser un identificador propio de su réplica, no un ID real del estudio original;
- `label=0` significa Sin incidencia y `label=1` Con incidencia;
- cada observación debe corresponder a una imagen independiente;
- la etapa 01 realiza la transformación a 224×224×1 y el particionado estratificado 70/15/15 con semilla 42.

La réplica genera su propia pertenencia a train/validation/test; no existe una membership histórica pública que deba reconstruirse.

## Entorno de ejecución

[ENVIRONMENT_REFERENCE.md](ENVIRONMENT_REFERENCE.md) distingue el entorno histórico documentado de los entornos usados en verificaciones posteriores. No existe un lockfile histórico completo recuperado. Una implementación nueva debe registrar sus versiones efectivas y no presentar un entorno posterior como si fuera idéntico al histórico.

## Orden técnico

Las especificaciones son la fuente operativa para implementar:

- [01 — Procesamiento y particionado](specs/01_PROCESAMIENTO_PARTICIONADO.md)
- [02 — HOG + SVM](specs/02_HOG_SVM.md)
- [03 — CNN ligera](specs/03_CNN_LIGERA.md)
- [04 — ResNet50](specs/04_RESNET50.md)
- [05 — ROC, Precision–Recall y AP](specs/05_EVALUACION_ROC_PR_AP.md)
- [06 — Umbral operativo](specs/06_UMBRAL_OPERATIVO_RESNET50.md)
- [07 — Grad-CAM](specs/07_GRADCAM.md)
- [08 — Evaluación final](specs/08_EVALUACION_FINAL.md)

No cambie parámetros científicos para hacer que una réplica se parezca artificialmente a los resultados históricos.

## Uso opcional del prompt maestro

[prompts/GENERAR_PIPELINE_DESDE_FICHAS.md](prompts/GENERAR_PIPELINE_DESDE_FICHAS.md) puede entregarse a una IA/agente de programación para crear una **implementación nueva e independiente**. El prompt:

- presupone que se descargó/clonó el repositorio completo;
- define la raíz del paquete;
- permite los cuatro alcances públicos;
- obliga a leer las especificaciones aplicables;
- prohíbe completar silenciosamente decisiones metodológicas esenciales no documentadas.

El prompt no contiene el código histórico y no garantiza reproducir sus métricas.

## Cómo interpretar una nueva réplica

Una réplica externa debe separar:

1. **método reconstruido**: si la implementación respeta las especificaciones;
2. **datos propios**: composición, procedencia, condiciones de adquisición y posibles diferencias de dominio;
3. **resultados propios**: métricas, curvas, matrices y umbral seleccionados en esa ejecución.

No transfiera `tau*=0.6017084718` a un dataset nuevo. La etapa 06 exige seleccionar el umbral con **validation** del nuevo conjunto y la etapa 08 aplica después ese valor fijo a test.

Tampoco use las métricas históricas como criterio de aceptación automática. Cambios de prevalencia, equipo, energía, tipo de carga, adquisición, tamaño muestral o dominio pueden cambiar el desempeño.

## Limitaciones

- **Dataset restringido:** impide una reproducción numérica externa exacta basada en las mismas 404 imágenes.
- **Tamaño muestral:** 404 observaciones constituyen una base limitada para estimar estabilidad y generalización. El estudio original no aisló causalmente el efecto del tamaño muestral sobre el desempeño de HOG+SVM o CNN.
- **Gobernanza histórica del test:** el test quedó fuera del ajuste de pesos y de la selección de `tau*`, pero sus métricas fueron consultadas durante la comparación de configuraciones/modelos; por ello, puede existir optimismo de selección.
- **CNN:** la verificación posterior observó discrepancias en P1/P2/P4 sin una causa única demostrada; la especificación permite replicación procedimental, no identidad numérica garantizada.
- **Trazabilidad histórica incompleta:** no se recuperaron con identidad forense todos los metadatos históricos de ejecución, membership por muestra, lockfile/hardware completos ni la selección completa de ejemplos Grad-CAM.
- **Uso:** el paquete no constituye validación de despliegue productivo ni sustituye evaluación humana y de dominio.

## Líneas de investigación futura

Este repositorio puede utilizarse como base para:

- ampliar el número de imágenes reales autorizadas;
- construir curvas de aprendizaje con subconjuntos anidados;
- repetir entrenamientos con múltiples semillas y/o remuestreos;
- estudiar incertidumbre y estabilidad de métricas;
- realizar validación multisitio o multiequipo cuando existan datos;
- cuantificar `domain shift`;
- comparar arquitecturas adicionales bajo el mismo protocolo;
- realizar validación prospectiva;
- estudiar subgrupos cuando exista metadata operacional estructurada y trazable;
- evaluar calibración, robustez y criterios operativos en nuevas poblaciones.

Estas líneas son propuestas de continuación; no deben confundirse con análisis ya realizados en el estudio original.

## Mapa del repositorio

| Archivo/directorio | Función |
|---|---|
| [README.md](README.md) | Presentación pública y manual de reproducción. |
| [DATASET_SPEC.md](DATASET_SPEC.md) | Contrato de datos para una réplica externa. |
| [ENVIRONMENT_REFERENCE.md](ENVIRONMENT_REFERENCE.md) | Entornos documentados y dependencias funcionales. |
| [ROUTES.md](ROUTES.md) | Alcances públicos disponibles. |
| [PIPELINE_ORDER.md](PIPELINE_ORDER.md) | Orden y dependencia de las etapas. |
| [RESULTS_REFERENCE.md](RESULTS_REFERENCE.md) | Resultados históricos y verificación posterior. |
| [SCOPE_AND_EVIDENCE.md](SCOPE_AND_EVIDENCE.md) | Qué evidencia existe y cuáles son sus límites. |
| [PUBLICATION_BOUNDARY.md](PUBLICATION_BOUNDARY.md) | Qué se publica y qué queda excluido. |
| [`specs/`](specs/) | Especificaciones metodológicas 01–08. |
| [prompts/GENERAR_PIPELINE_DESDE_FICHAS.md](prompts/GENERAR_PIPELINE_DESDE_FICHAS.md) | Prompt opcional para generar una implementación independiente. |
| [examples/dataset_example.csv](examples/dataset_example.csv) | Ejemplo sintético del contrato de datos. |
| [manifest/SHA256SUMS.txt](manifest/SHA256SUMS.txt) | Manifiesto de integridad del paquete. |

## Nota final

Este repositorio es un **punto de partida reproducible para investigación derivada**, no una entrega de software productivo ni una publicación del dataset original. Si utiliza el paquete, documente qué commit consultó, qué datos empleó, qué decisiones tomó y qué resultados obtuvo.
