# Paquete de replicación metodológica INI

**Modalidad B: especificación metodológica sin publicación de código fuente.** Este paquete permite construir una implementación independiente sobre datos propios o autorizados; no contiene software histórico, datos del TFM, imágenes, identificadores reales ni modelos entrenados.

## Alcance
La reproducibilidad computacional interna corresponde al equipo autor con datos autorizados y fuentes consolidadas, sin afirmar identidad forense de cada corrida histórica ni una ejecución F-08 global congelada. La reproducción exacta externa no está garantizada porque las 404 imágenes originales están restringidas. La replicación metodológica externa consiste en reimplementar y entrenar el método con datos INI propios/autorizados.

## Rutas
**Ruta A, normativa y por defecto:** 01 → 02 HOG/SVM → 03 CNN ligera → 04 ResNet50 → 05 ROC/PR/AP → 06 umbral → 07 Grad-CAM → 08 evaluación final.

**Ruta B, opcional:** 01 → 04 → 05 → 06 → 07 → 08. No replica la comparación de tres modelos ni demuestra superioridad de ResNet50 en datos externos.

## Datos externos y límites
Use sólo imágenes autorizadas con etiquetas defendibles equivalentes a `Sin incidencia=0` y `Con incidencia=1`. Una réplica intradominio conserva modalidad y condiciones de adquisición comparables; una interdominio (p. ej. contenedores, vehículos o equipaje) exige documentar domain shift. Reentrene y evalúe: no transfiera métricas ni `tau*` del TFM.

## Estudios futuros
La Ruta A puede apoyar curvas de aprendizaje con subconjuntos anidados, semillas/remuestreos y test no adaptativo. El TFM no demuestra causalidad entre tamaño muestral y menor desempeño de HOG/SVM o CNN.
