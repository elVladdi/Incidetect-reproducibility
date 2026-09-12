# Alcance y evidencia

Modalidad B: especificación pública sin código ni datos restringidos. Precedencia: evidencia técnica F-08/F-09, después implementación consolidada F-06 y documentación histórica. No existe ejecución global F-08 congelada.

## Estado de ramas
HOG+SVM reprodujo exactamente. CNN: P1, P2 y P4 quedaron fuera de tolerancia; P3 coincidió en test pero no sustituye P4; P4 continúa como configuración histórica final. Dictamen D2: ejecución procedimentalmente equivalente y variación computacional plausible, sin causa única demostrada. No se declara reproducción numérica exacta externa de CNN: una implementación nueva puede obtener métricas distintas incluso si respeta la especificación. ResNet50/05–08 quedó dentro de tolerancia; F-09 confirmó fine-tuning convolucional efectivo.

## Brechas vigentes
- **GAP-F10-01:** membership histórica por muestra no recuperada.
- **GAP-F10-02:** hash DVC, MLflow/run_id, commit/tag y fecha exacta por corrida histórica no recuperados.
- **GAP-F10-03:** lockfile y hardware históricos completos no recuperados.
- **GAP-F10-04:** selección histórica completa de ejemplos Grad-CAM no recuperada; el nombre de capa sí está recuperado.
