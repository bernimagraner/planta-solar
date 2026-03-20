# Informe de Calidad de Datos
## Fase de Calidad de Datos — Caso Planta Solar

**Proyecto:** Business Analytics — Análisis de Plantas Fotovoltaicas  
**Fecha:** Marzo 2025  
**Fuente:** Notebook `01_importacion_datos.ipynb`

---

## 1. Resumen Ejecutivo

El análisis de calidad de datos se ha aplicado a cuatro conjuntos de datos: dos de generación (Planta 1 y Planta 2) y dos de sensores meteorológicos (Planta 1 y Planta 2). Las principales conclusiones son:

- **Cobertura temporal:** Todos los datasets comparten el mismo período y los mismos 34 días.
- **Error corregido:** La potencia DC de la Planta 1 estaba multiplicada por 10; se ha corregido.
- **Completitud:** Los datasets no son regulares: faltan tramos horarios, sobre todo en la Planta 1.
- **Heterogeneidad:** La Planta 2 presenta mejor calidad de datos que la Planta 1.

---

## 2. Fuentes de Datos Analizadas

| Dataset | Descripción | Registros | Período |
|---------|-------------|-----------|---------|
| **p1g** | Planta 1 - Generación (22 inversores) | 68.778 | 15/05/2020 - 17/06/2020 |
| **p2g** | Planta 2 - Generación (22 inversores) | 67.698 | 15/05/2020 - 17/06/2020 |
| **p1s** | Planta 1 - Sensores meteorológicos | 3.182 | 15/05/2020 - 17/06/2020 |
| **p2s** | Planta 2 - Sensores meteorológicos | 3.259 | 15/05/2020 - 17/06/2020 |

**Intervalo temporal base:** 15 minutos (96 tramos por día).

---

## 3. Proceso de Calidad Realizado

### 3.1 Importación y formato

- Conversión de `fecha_hora` a datetime (formatos dd-mm-yyyy y yyyy-mm-dd).
- Sustitución de `id_planta` (4135001, 4136001) por códigos legibles (p1, p2).
- Renombrado de sensores meteorológicos (sensor_p1, sensor_p2).

### 3.2 Verificación de cobertura de fechas

- **Resultado:** Todos los datasets tienen exactamente los mismos 34 días.
- **Continuidad:** No falta ningún día intermedio en ningún dataset.
- **Período analizado:** 15 de mayo de 2020 a 17 de junio de 2020.

### 3.3 Completitud de tramos horarios

Se esperan 96 tramos de 15 minutos por día. Resultados por dataset:

| Dataset | Días completos | Días incompletos | Tramos faltantes | % días completos |
|---------|----------------|------------------|------------------|------------------|
| p1g | 24 | 10 | 106 | 70,6% |
| p2g | 29 | 5 | 5 | 85,3% |
| p1s | 26 | 8 | 82 | 76,5% |
| p2s | 29 | 5 | 5 | 85,3% |

**Conclusión:** Los datasets de la Planta 2 (p2g, p2s) son más completos que los de la Planta 1 (p1g, p1s).

---

## 4. Error de Calidad Detectado y Corregido

### Potencia DC — Planta 1 (p1g)

**Problema:** Los valores de `potencia_dc_kw` en la Planta 1 estaban multiplicados por 10 respecto a valores esperados.

**Evidencia:** Comparación con `potencia_ac_kw` y patrones típicos en instalaciones fotovoltaicas (DC > AC por pérdidas de conversión, pero en proporciones razonables).

**Acción:** División de `potencia_dc_kw` entre 10.

```python
p1g['potencia_dc_kw'] = p1g['potencia_dc_kw'] / 10
```

---

## 5. Insights Clave

### INSIGHT 1: Anomalía de escala en potencia DC (Planta 1)

La potencia DC de la Planta 1 presentaba un error de escala (multiplicada por 10). Se corrigió y se verificó la coherencia con la potencia AC.

### INSIGHT 2: Los datasets no son regulares

Aunque todos los datasets tienen los mismos días, **en todos faltan tramos horarios**. El problema es más acusado en la Planta 1 que en la Planta 2 (106 vs 5 tramos faltantes en generación).

**Patrones de tramos faltantes:**
- **Planta 1:** Los tramos más afectados son madrugada (00:00–02:45) y mediodía (11:45–15:45).
- **Planta 2:** Faltan pocos tramos (16:00, 14:00, 17:45, 23:15) en días concretos.

### INSIGHT 3: Distribución de pérdidas por inversor

- **Planta 1:** Los inversores tienen niveles similares de registros. La pérdida de datos parece estar distribuida en los tramos horarios, no en inversores concretos.
- **Planta 2:** Hay 4 inversores con más pérdida de datos que el resto. Conviene investigar más en la fase de EDA.

---

## 6. Recomendaciones para Fases Posteriores

| Prioridad | Recomendación |
|-----------|---------------|
| Alta | Definir una malla temporal de referencia (96 tramos/día) y alinear todos los datasets mediante reindexado o merge. |
| Alta | Documentar las fechas o períodos con huecos y contrastarlos con registros de mantenimiento o incidencias. |
| Media | Validar la corrección de `potencia_dc_kw` en p1g con datos históricos o de referencia cuando estén disponibles. |
| Media | Profundizar en el análisis de los 4 inversores de la Planta 2 con mayor pérdida de datos. |
| Baja | Revisar la integridad de `energia_total_kwh` y su relación con `energia_diaria_kwh`. |

---

## 7. Conclusión Final

La calidad de los datos es **aceptable para análisis exploratorio y modelado**, siempre que se apliquen las correcciones y alineaciones descritas. Las principales limitaciones son:

1. **Irregularidad en series temporales:** Faltan tramos horarios, sobre todo en la Planta 1.
2. **Error de escala corregido:** Potencia DC de la Planta 1 multiplicada por 10 (ya corregida).
3. **Diferencias entre plantas:** La Planta 2 presenta mejor completitud que la Planta 1.

Con una estrategia adecuada de preprocesamiento temporal y alineación, los datos pueden utilizarse con confianza para análisis de rendimiento, cálculo del Performance Ratio (PR), predicción de generación e integración de variables meteorológicas y de generación.

---

*Documento generado a partir del análisis realizado en el notebook `01_importacion_datos.ipynb`.*
