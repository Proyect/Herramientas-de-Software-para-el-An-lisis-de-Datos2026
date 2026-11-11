## Conclusión integradora (Sprints 1 a 3)

### Problema que resuelve el sistema
El sistema convierte el registro crudo de los radares del puerto en información confiable para fiscalizar excesos de velocidad. En el Sprint 1 se limpió y normalizó el registro de movimientos (fechas y horas inválidas, matrículas y muelles con separadores, cálculo de duración y exceso) y quedaron 447 infracciones depuradas. En el Sprint 2 se procesaron las 100 fotos de los radares y, con OCR y un cruce por matrícula, 431 infracciones (96,42 %) quedaron respaldadas por una imagen. En el Sprint 3 todo eso pasó a una base relacional (`transito_portuario`: 20 buques, 8 radares, 447 movimientos y 431 evidencias) que se consulta con SQLAlchemy, los datos y binarios quedaron versionados con DVC, y se agregó una base vectorial para reconocer buques por su imagen. De punta a punta, el sistema responde qué buque cometió cada infracción, con qué radar, en qué muelle y con qué evidencia, y permite saber con qué versión de los datos se obtuvo cada resultado.

### Ventaja de la base vectorial frente a usar solo OCR
El OCR depende de que la matrícula se lea bien: en el Sprint 2 fallaron justamente las fotos nocturnas, borrosas o lejanas, y aparecieron confusiones como A/4, G/6 u O/0. La base vectorial no lee texto, compara la imagen completa del buque (forma del casco, colores, estructura) a través del embedding de OpenCLIP. Así se puede identificar un buque aunque la matrícula sea ilegible, y además sirve para validar el OCR: si la matrícula leída no coincide con el buque más parecido visualmente, la evidencia se marca para revisión. La búsqueda devuelve una distancia, de modo que el sistema sabe cuán segura es cada identificación.

### Limitaciones del sistema actual
- Cobertura vectorial baja: solo 5 de los 20 buques (25 %) tienen una imagen `complete` asociada, así que los otros 15 no se pueden reconocer visualmente.
- Las consultas con recortes `plates` no son confiables contra una base armada con imágenes completas: el embedding de una matrícula recortada representa texto y fondo, no el buque.
- La evidencia se asocia por matrícula y no por movimiento: las imágenes no traen fecha, hora ni radar, por lo que una misma foto respalda todas las infracciones de un buque.
- La columna `buque_id` del dataset original es inconsistente; hubo que identificar los buques por matrícula, y un error de tipeo en la matrícula genera un buque distinto.
- Es un pipeline por lotes sobre SQLite y archivos locales: no hay ingesta en tiempo real, concurrencia ni control de acceso.
- El umbral de distancia para aceptar una identificación se fijó con pocas imágenes y habría que calibrarlo con más datos.

### Cambios para producción en el Puerto de Rosario
- **Captura**: que cada radar envíe la foto junto con la fecha, la hora, el radar y el movimiento, para vincular la evidencia con la infracción puntual, y sumar iluminación infrarroja para las tomas nocturnas.
- **Datos**: reemplazar SQLite por PostgreSQL (por ejemplo en Supabase, que ya se usa como remoto de DVC) con `pgvector` o un servidor ChromaDB, para tener una sola base consultable por varios usuarios a la vez, con respaldos y permisos.
- **Base vectorial**: guardar varias imágenes por buque y de distintos ángulos, e incorporar automáticamente cada nueva foto validada, para ampliar la cobertura más allá del 25 % actual; usar plates y completes en colecciones separadas.
- **Identificación**: combinar OCR y similitud visual en una sola decisión, y enviar a revisión humana los casos en que no coincidan o la distancia supere el umbral.
- **Operación**: ejecutar el pipeline como servicio programado o por eventos, con monitoreo de la calidad de los datos, y mantener DVC para auditar qué versión del dataset y del modelo se usó en cada sanción.
