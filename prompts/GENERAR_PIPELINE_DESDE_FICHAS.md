# Prompt maestro para implementación independiente

En una sesión limpia, acepte `ruta=completa` o `ruta=modelo_final`; si falta, use completa. Antes de implementar, lea `DATASET_SPEC.md`, `PIPELINE_ORDER.md`, `ROUTES.md`, `ENVIRONMENT_REFERENCE.md`, `RESULTS_REFERENCE.md` y las ocho fichas aplicables en `specs/01...08`. Estas son la única fuente metodológica y prevalecen en el orden declarado por el paquete.

No acceda al repositorio interno, código/notebooks históricos, datos restringidos, checkpoints ni contexto previo. Genere software nuevo e independiente y exija un dataset propio/autorizado compatible. Imponga validaciones de IDs, separación train/validation/test, ausencia de fuga, `tau*` sólo en validation y test sólo final. Mantenga AP separado de área trapezoidal PR. `modelo_final` no puede afirmar comparación ni superioridad externa.

Si una decisión esencial sigue abierta, detenga el componente y registre `GAP-RECON-XX`; no invente. Al final entregue reporte de documentos leídos, decisiones derivadas, contratos, validaciones y brechas. No pruebe ni simule reconstrucción ciega en esta fase.
