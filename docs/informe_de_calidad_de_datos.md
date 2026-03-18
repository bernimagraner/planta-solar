# Informe Ejecutivo de Calidad de Datos

## Proyecto: Análisis de Planta Solar

**Fecha del informe:** Febrero 2025  
**Ámbito:** Datos de generación y sensores meteorológicos - Planta 1 y Planta 2

---

## 1. Resumen Ejecutivo

El presente informe recoge las conclusiones del análisis de calidad de datos realizado sobre los cuatro conjuntos de datos del proyecto de planta solar. Los hallazgos principales indican **irregularidades en las series temporales** y **discrepancias en la cobertura de fechas** entre los distintos datasets, así como **un error de escala detectado y corregido** en los datos de generación de la Planta 1.

---

## 2. Fuentes de Datos Analizadas

| Dataset | Descripción | Registros | Período |
|---------|-------------|-----------|---------|
| **P1G** | Planta 1 - Generación (inversores) | 68.778 | 15/05/2020 - 17/06/2020 |
| **P2G** | Planta 2 - Generación (inversores) | 67.698 | 15/05/2020 - 17/06/2020 |
| **P1S** | Planta 1 - Sensores meteorológicos | 3.182 | 15/05/2020 - 17/06/2020 |
| **P2S** | Planta 2 - Sensores meteorológicos | 3.259 | 15/05/2020 - 17/06/2020 |

---

## 3. Hallazgos Principales

### 3.1 Regularidad de las Series Temporales

**Conclusión: ninguna de las cuatro series temporales es estrictamente regular.**

- **Intervalo base:** 15 minutos en todos los datasets.
- **Problemas detectados:**
  - **P1G:** 9 intervalos diferentes entre fechas (p. ej. 15 min, 30 min, 1 h, 3 h, 4h15min, etc.).
  - **P2G:** 5 saltos con intervalos distintos al estándar de 15 minutos.
  - **P1S:** 8 intervalos irregulares detectados.
  - **P2S:** Serie con irregularidades en los intervalos.

**Impacto:** Los huecos y saltos en el tiempo pueden afectar a análisis temporales y modelado predictivo. Se recomienda rellenar o tratar explícitamente los intervalos faltantes.

### 3.2 Cobertura y Alineación de Fechas

| Dataset | Fechas únicas | ¿Mismas fechas que el resto? |
|---------|---------------|------------------------------|
| P1G | 3.158 | No |
| P2G | 3.259 | No |
| P1S | 3.182 | No |
| P2S | 3.259 | No |

**Conclusión:** Los cuatro datasets **no comparten exactamente las mismas fechas**. Existen fechas presentes en unos conjuntos y ausentes en otros, lo que obliga a una estrategia de alineación antes de cualquier análisis conjunto o modelado.

### 3.3 Rango Temporal

Todos los datasets comparten el mismo rango de fechas:
- **Inicio:** 15 de mayo de 2020, 00:00
- **Fin:** 17 de junio de 2020, 23:45

**Conclusión:** Aunque el rango global coincide, la cobertura interna difiere entre datasets.

---

## 4. Error de Calidad Corregido

### Potencia DC – Planta 1 (P1G)

**Problema detectado:** Los valores de `potencia_dc_kw` en la Planta 1 aparecían multiplicados por 10 respecto a valores esperados.

**Acción tomada:** División de la columna entre 10 para corregir la escala.

**Evidencia:** Comparación con `potencia_ac_kw` y patrones esperables en instalaciones fotovoltaicas.

---

## 5. Insights Clave

1. **Serie temporal no regular:** Los huecos y saltos sugieren posibles fallos de medición, mantenimiento o desconexiones. Conviene documentar su origen para interpretar mejor los datos.

2. **Heterogeneidad entre plantas:** P1 y P2 tienen distinta cobertura de fechas (3.158 vs 3.259 fechas únicas), lo que implica diferencias operativas o de disponibilidad de datos.

3. **Sensores vs. generación:** Los datasets de sensores (P1S, P2S) tienen una fecha por fila; los de generación (P1G, P2G) agrupan varios inversores por misma fecha, aumentando el volumen de filas.

4. **Formato de fechas:** Se detectaron diferencias de formato (p. ej. dd-mm-yyyy vs. yyyy-mm-dd) que requieren conversión explícita a datetime para operaciones correctas.

5. **Identificadores:** Los `id_planta` originales (4135001, 4136001) se han sustituido por códigos más legibles (p1, p2) para facilitar el análisis.

---

## 6. Recomendaciones

| Prioridad | Recomendación |
|-----------|---------------|
| Alta | Definir una malla temporal de referencia (p. ej. cada 15 minutos) y alinear todos los datasets mediante reindexado o merge. |
| Alta | Documentar las fechas o períodos con huecos o intervalos irregulares y contrastarlos con registros de mantenimiento o incidencias. |
| Media | Validar la corrección de `potencia_dc_kw` en P1G con datos históricos o de referencia cuando estén disponibles. |
| Media | Homogeneizar el formato de fechas en origen para evitar errores de conversión. |
| Baja | Revisar la integridad de `energia_total_kwh` y su relación con `energia_diaria_kwh` para detectar posibles inconsistencias. |

---

## 7. Conclusión Final

La calidad de los datos es **aceptable para análisis exploratorio y modelado**, siempre que se apliquen las correcciones y alineaciones descritas. Las principales limitaciones son:

- Irregularidades en las series temporales
- Fechas no coincidentes entre datasets
- Error de escala corregido en potencia DC de la Planta 1

Con una estrategia adecuada de preprocesamiento temporal y alineación, los datos pueden utilizarse con confianza para análisis de rendimiento, predicción de generación e integración de variables meteorológicas y de generación.

---

*Documento generado a partir del análisis realizado en el notebook `01_importacion_datos.ipynb`.*
