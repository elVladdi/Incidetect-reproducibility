# Alcance y límites de evidencia

Este repositorio distingue entre **evidencia histórica**, **verificación post-hoc** y **replicación metodológica externa**.

## 1. Evidencia histórica

Los resultados históricos del TFM se conservan como referencia del experimento realizado sobre el dataset restringido. El paquete no reconstruye retrospectivamente metadatos que no fueron recuperados con certeza.

## 2. Verificación post-hoc

Se realizaron comprobaciones posteriores para evaluar trazabilidad y comportamiento del pipeline:

- HOG+SVM reprodujo exactamente las matrices y métricas verificadas.
- La CNN ligera mostró discrepancias en P1/P2/P4; P3 coincidió en test, pero P4 sigue siendo la configuración histórica final. No se demostró una causa única.
- ResNet50 y las etapas 05–08 quedaron dentro de las tolerancias definidas.
- Se confirmó técnicamente que la lógica de fine-tuning de ResNet50 actualiza capas convolucionales profundas en el entorno de referencia comprobado.
- La medición de latencia disponible es post-hoc y no se presenta como baseline histórico.

Estas verificaciones no sustituyen las métricas históricas del TFM.

## 3. Replicación metodológica externa

El repositorio público no contiene código histórico ni datos restringidos. Su propósito es permitir que un tercero construya una implementación nueva con datos propios/autorizados siguiendo las especificaciones publicadas.

Una réplica externa puede validar **equivalencia procedimental**, pero no se le exige identidad numérica con las métricas del TFM.

## Límites de trazabilidad recuperada

Permanecen documentadas las siguientes limitaciones:

- no se recuperó la pertenencia histórica por muestra a train/validation/test con identidad forense;
- no se recuperaron con certeza hashes/versiones DVC, `run_id` de MLflow, commit/tag y fecha exacta de cada corrida histórica;
- no se recuperó un lockfile histórico completo ni una caracterización completa del hardware histórico;
- no se recuperó la selección histórica completa de ejemplos usados para Grad-CAM, aunque la capa objetivo sí está especificada.

Estas brechas se publican como límites de evidencia. No deben completarse por inferencia.

## Implicación para el usuario

Al reutilizar el paquete, documente su propio dataset, entorno, particiones, decisiones abiertas y resultados. Si cambia el dominio, equipo, población o condiciones de adquisición, trate esa diferencia como posible `domain shift` y no atribuya al TFM resultados obtenidos bajo condiciones nuevas.
