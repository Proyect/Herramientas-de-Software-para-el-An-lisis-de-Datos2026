# CHANGELOG

## Sprint 2

[Ejercicio 04]
- Extracción de matrículas con easyocr y carga en group_images.json.
- Reintento del OCR sobre la imagen ecualizada cuando no hay match.
- Matching posicional de caracteres alfanuméricos con umbral del 75%.
- Generación de port_log/data/processed/port_movements_image.csv.

[Ejercicio 03]
- Conversión a escala de grises (03_01_gray).
- Ecualización de histograma (03_02_equalized).
- Suavizado con blur gaussiano 5x5 (03_03_blur).
- Detección de bordes con Canny (03_04_canny).

[Ejercicio 02]
- Listado de imágenes con su tamaño en KB.
- Separación en plates y completes por relación de aspecto.
- Generación de port_log/data/interim/group_images.json.
- Cálculo de resolución, área y tamaño promedio por grupo.
- Función mostrar_muestra para visualizar imágenes en grilla de 2 columnas.

[Ejercicio 01]
- Creación de la rama Sprint_2 a partir de Sprint_1.
- Descarga y descompresión del dataset de imágenes en port_log/data/raw/imgs.
- Verificación de los archivos del Sprint 1 y conteo de registros.
- Actualización del README.md al Sprint 2.

## Sprint 1

[Ejercicio 07]
- Redacción de la conclusión en port_log/reports/conclusion.md.

[Ejercicio 06]
- Respuestas a las preguntas de análisis sobre el dataset limpio.

[Ejercicio 05]
- Generación y exportación de 6 gráficos en port_log/data/interim/plots.

[Ejercicio 04]
- Creación de la clase PortAnalyzer con sus 6 métodos de análisis.

[Ejercicio 03]
- Normalización de fechas, horas, matrículas y muelles.
- Cálculo de duracion_horas, exceso_velocidad_real y exceso_velocidad.
- Eliminación de nulos críticos, outliers (IQR) y filas sin infracción.
- Exportación del dataset limpio y del resumen estadístico.

[Ejercicio 02]
- Descarga del dataset raw en port_log/data/raw/port_movements.csv.
- Exploración inicial: primeras/últimas filas, tipos de datos y nulos.
- Cálculo del porcentaje de completitud por columna.

[Ejercicio 01]
- Creación de la rama Sprint_1.
- Creación de la estructura de directorios del proyecto port_log.
- Creación de README.md y CHANGELOG.md.
