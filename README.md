# Port Log - Sprint 3

## Objetivo
Aplicar conocimientos de bases de datos relacionales (SQLAlchemy), versionado de
datos (DVC) y búsqueda por similitud visual (ChromaDB) para profesionalizar el
sistema portuario.

## Introducción y contexto
El sistema creció en volumen y complejidad. En este sprint se busca:

1. **Persistencia estructurada**: pasar de CSVs a una base de datos relacional
   (`transito_portuario`) administrada con el ORM SQLAlchemy.
2. **Auditoría de cambios**: versionar con DVC (remoto S3 en Supabase) las
   imágenes, los gráficos y el dataset procesado, para saber qué versión de los
   datos se usó en cada análisis.
3. **Identificación visual de buques**: identificar un buque por similitud
   visual de su imagen con embeddings OpenCLIP almacenados en ChromaDB
   (`buque_vectorial`), sin depender de la OCR.

## Sprints anteriores
- Sprint 1: limpieza y análisis exploratorio del registro de movimientos
  (`port_log/data/interim/port_movements.csv`).
- Sprint 2: preprocesamiento de imágenes, OCR de matrículas y cruce con las
  infracciones (`port_log/data/processed/port_movements_image.csv`).
