# Entorno de referencia

## Capas distinguibles
1. **Histórico documentado:** Python 3.11.7; la evidencia disponible no constituye lockfile/hardware completo.
2. **Evidencia CNN histórica/local:** Python 3.11.14 y TensorFlow 2.15.1, preservados como evidencia específica P1–P4; no se armonizan retrospectivamente.
3. **F-09:** Python 3.12.14, TensorFlow 2.19.0, tf.keras/Keras 3.9.2, NumPy 1.26.4, scikit-learn 1.5.2, CPU. Verifica semántica de fine-tuning, no identidad histórica.
4. **F-08:** reejecución posterior con entorno principal TF 2.19/Keras 3.9 y CPU; no sustituye métricas históricas.

## Dependencias funcionales
Etapas 01–08 requieren Python, NumPy, pandas, Pillow, scikit-learn, scikit-image, TensorFlow/Keras, Matplotlib, joblib y Jupyter; Grad-CAM puede requerir OpenCV para superposición. Salvo las versiones explícitas F-09, la versión exacta de Pillow, scikit-image, joblib, OpenCV, pandas, Matplotlib y Jupyter no quedó fijada por la evidencia disponible. GAP-F10-03 mantiene lockfile/hardware completos como no recuperados. Una réplica debe registrar sus versiones efectivas.
