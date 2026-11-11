# Modelo lógico - Sprint 3

### Modelo lógico

```
┌──────────────┐ 1      N ┌────────────────────────┐ N      1 ┌──────────────┐
│    BUQUE     │──────────│       MOVIMIENTO       │──────────│    RADAR     │
├──────────────┤          ├────────────────────────┤          ├──────────────┤
│ PK id        │          │ PK movimiento_id       │          │ PK radar_id  │
│ UK matricula │          │ FK buque_id            │          └──────────────┘
└──────────────┘          │ FK radar_id            │
                          │    muelle              │
                          │    tipo_carga, origen  │
                          │    fecha/hora ingreso  │
                          │    fecha/hora egreso   │
                          │    duracion_horas      │
                          │    tonelaje_declarado  │
                          │    velocidad_ingreso   │
                          │    velocidad_maxima_muelle │
                          │    exceso_velocidad_real   │
                          │    exceso_velocidad    │
                          │    estado_despacho     │
                          └───────────┬────────────┘
                                      │ 1
                                      │
                                      │ 0..1
                          ┌───────────┴────────────┐
                          │       EVIDENCIA        │
                          ├────────────────────────┤
                          │ PK id                  │
                          │ FK,UK movimiento_id    │
                          │    archivo_imagen      │
                          │    matricula_imagen    │
                          │    ratio               │
                          │    grupo               │
                          └────────────────────────┘
```

| Entidad | Atributos | Clave |
|---|---|---|
| **Buque** | `id`, `matricula` | PK `id` (subrogada); `matricula` única |
| **Radar** | `radar_id` | PK `radar_id` (R-01 … R-08) |
| **Movimiento** | `movimiento_id`, `buque_id`, `radar_id`, `muelle`, `tipo_carga`, `origen`, `fecha_ingreso`, `hora_ingreso`, `fecha_egreso`, `hora_egreso`, `duracion_horas`, `tonelaje_declarado`, `velocidad_ingreso`, `velocidad_maxima_muelle`, `exceso_velocidad_real`, `exceso_velocidad`, `estado_despacho` | PK `movimiento_id`; FK `buque_id`, `radar_id` |
| **Evidencia** | `id`, `movimiento_id`, `archivo_imagen`, `matricula_imagen`, `ratio`, `grupo` | PK `id`; FK única `movimiento_id` |

**Cardinalidades**

- Buque **1 : N** Movimiento — un buque tiene muchos movimientos y cada movimiento es de un solo buque.
- Radar **1 : N** Movimiento — cada movimiento lo registra un único radar.
- Movimiento **1 : 0..1** Evidencia — la FK `movimiento_id` es única en Evidencia, por eso un movimiento tiene a lo sumo una imagen (puede no tener).
- Muelle: es un atributo de Movimiento (cada movimiento ocurre en un único muelle). Como no está asociado al radar, un radar puede aparecer en movimientos de distintos muelles (relación N:M radar–muelle resuelta a través de Movimiento).

**Decisiones de diseño**

- El buque se identifica por `matricula`: en el CSV la columna `buque_id` no es consistente (una misma matrícula aparece con varios `buque_id` distintos), por eso se usa una clave subrogada propia y la matrícula como clave única.
- Los atributos de velocidad, tonelaje, estado y tiempos dependen de cada ingreso, por eso pertenecen a Movimiento y no a Buque.
- `ratio` se guarda como proporción (0 a 1) y `grupo` indica si la imagen es un recorte de matrícula (`plates`) o una imagen completa (`completes`).
