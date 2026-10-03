# Conclusión - Port Log Sprint 2

## Validación visual de las infracciones
De las 447 infracciones depuradas en el Sprint 1, **431 (96.42%) pudieron validarse visualmente**: quedaron asociadas a una imagen cuya matrícula coincide en al menos un 75% con la registrada. Las 16 infracciones restantes pertenecen al buque ZIM-NORTE, del que no se obtuvo ninguna lectura válida (2 de ellas están en estado PENDIENTE). De las 100 imágenes, 96 tuvieron match y cubren 19 de los 20 buques del dataset.

Hay que aclarar que el cruce se hace por matrícula: las imágenes no traen fecha, hora ni radar, por lo que la evidencia confirma que el buque fue fotografiado, pero no permite vincular una foto con un movimiento puntual.

## Plates vs completes
El grupo **plates** resultó el más útil para el OCR (100% de match contra 90% de completes). Al ser recortes ajustados, la matrícula ocupa toda la imagen y el OCR la lee como un único bloque de texto, sin distraerse con el casco ni con el fondo. En las completes la matrícula ocupa una porción pequeña de la escena, con menos píxeles por carácter y más ruido alrededor.

## Condiciones de captura
Lo que más afectó al matching fue la **combinación de poca luz y falta de nitidez**. Las cuatro imágenes sin match son completes: dos nocturnas y muy borrosas, una borrosa y una con la matrícula legible solo en parte; en ellas el OCR devuelve fragmentos sin sentido o nada. Las imágenes oscuras pero nítidas se leyeron bien en su mayoría, y en las plates con bajo contraste el reintento sobre la imagen ecualizada recuperó las lecturas que fallaban. También aparecen las confusiones típicas del OCR (A por 4, G por 6, I por 1, O por 0), que el umbral del 75% tolera.

## Mejoras propuestas
- **Captura**: iluminación infrarroja o flash para las tomas nocturnas, menor tiempo de exposición para evitar el desenfoque por movimiento y que las cámaras entreguen siempre el recorte de la matrícula. Guardar junto a cada imagen la fecha, la hora y el radar para asociarla a un movimiento puntual y no solo al buque.
- **Algoritmo**: detectar la región de la matrícula en las completes (ROI por contornos y relación de aspecto) antes del OCR, corregir las confusiones letra/número según el patrón de las matrículas, crear el lector de easyocr una sola vez en lugar de en cada llamada y reemplazar la comparación posicional por una distancia de edición (Levenshtein), que no penaliza tanto un carácter faltante o sobrante.
