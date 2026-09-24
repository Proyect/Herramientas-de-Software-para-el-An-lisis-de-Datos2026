# Conclusión - Port Log Sprint 1

## Calidad del dataset heredado
De 1500 registros originales quedaron 447 infracciones válidas
(70.20% de los registros fue descartado por errores, outliers o por no
constituir infracción). Los errores más frecuentes fueron: fechas en formatos mixtos o
imposibles (ej. 32/13/2021), horas en formato de 12 hs, con segundos o fuera de rango,
matrículas y muelles con minúsculas y caracteres especiales, valores nulos y
tonelajes negativos o desproporcionados.

## Patrones de infracción
- Turno con más infracciones: Tarde (124 casos).
- Muelle con más infracciones: MUELLE-B (79 casos).
- Tipo de carga más frecuente: TRIGO (15.44%).
- Exceso promedio real: 3.02 nudos.

## Impacto de no limpiar los datos
Incorporar los datos sin depurar generaría estadísticas falsas (duraciones y excesos
absurdos por outliers), infractores duplicados por diferencias de escritura en las
matrículas y registros imposibles de ubicar en el tiempo, afectando decisiones de
control y sanciones.

## Propuesta de mejora
Validar los datos en el momento de la captura: campos de fecha y hora con selector
(formato único YYYY-MM-DD y 24 hs), matrícula y muelle elegidos de un catálogo
cerrado en lugar de texto libre, y rangos permitidos para tonelaje y velocidad.
