# 01 Procesamiento y particionado

## Entrada pública
Tabla `image_id,label`, donde `image_id` es un identificador único de imagen, no un número INI; `label=0` significa Sin incidencia y `label=1` Con incidencia. No use ni requiera el CSV histórico. Una réplica produce su propia membership. Unidad: una imagen = una INI = una observación independiente.

## Transformación
Lea cada imagen y conviértala a escala de grises modo `L`. Para objetivo 224×224, calcule `scale=min(224/orig_w,224/orig_h)`; las dimensiones nuevas son `max(1,int(orig_w*scale))` y `max(1,int(orig_h*scale))`. Redimensione con interpolación bilineal, pegue centrado en un lienzo negro 224×224 con offsets enteros `(224-new_w)//2` y `(224-new_h)//2`. Convierta a `float32`, divida por 255 y entregue shape `(224,224,1)` en [0,1].

## Particiones
Dos divisiones estratificadas con semilla 42: primero train 70% y temporal 30%; después temporal en validation/test 50/50. Resultado 70/15/15. Entradas/salidas de la nueva réplica: tensores y etiquetas train/validation/test más tabla de membership propia. Test no se usa adaptativamente.
