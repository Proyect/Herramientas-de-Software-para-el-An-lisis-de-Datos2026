# Port Log - Sprint 2

## Objetivo
Aplicar conocimientos de tratamiento de imágenes y programación limpia sobre el
contexto del sistema portuario.

## Introducción y contexto
Los radares ubicados en los accesos a los muelles capturan evidencia fotográfica de
las infracciones de velocidad. Las cámaras asociadas fotografían la zona de proa donde
está pintada la matrícula del buque. En algunos casos el sistema recorta
automáticamente la zona de matrícula (`plates`); en otros, entrega la imagen completa
(`completes`).

El sistema presenta limitaciones: no todas las infracciones tienen imagen asociada, no
todas las imágenes corresponden a una infracción real (falsos positivos del radar) y
puede haber errores de detección óptica (imágenes borrosas, nocturnas o lejanas).

En este sprint se preprocesan las imágenes (escala de grises, ecualización, suavizado y
bordes), se extrae la matrícula con OCR y se cruza con las infracciones depuradas en el
Sprint 1 para responder: **¿qué infracciones tienen evidencia visual válida?**

## Sprint anterior
- Sprint 1: limpieza y análisis exploratorio del registro de movimientos portuarios
  (`port_log/data/interim/port_movements.csv`).
