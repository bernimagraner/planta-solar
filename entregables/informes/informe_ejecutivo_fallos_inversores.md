# Informe Ejecutivo: Análisis Detallado de Fallos en Inversores

**Proyecto:** Planta Solar Fotovoltaica — Caso Business Analytics  
**Fuente:** Notebook `03_analisis_insights.ipynb`  
**Fecha:** Marzo 2025

---

## Resumen de Insights de los 3 Notebooks (desde el principio al final)

### Notebook 01 — Importación y Calidad de Datos

| # | Insight | Descripción |
|---|---------|-------------|
| **1** | **Error de escala en potencia DC (Planta 1)** | Los valores de `potencia_dc_kw` en la Planta 1 estaban **multiplicados por 10** (error de un orden de magnitud / un decimal). La corrección aplicada fue dividir entre 10. Esto se detectó al comparar con `potencia_ac_kw` y los patrones típicos en fotovoltaica (DC > AC por pérdidas de conversión, pero en proporciones razonables). |
| **2** | Datasets no regulares | Faltan tramos horarios en todos los datasets. La Planta 1 es la más afectada (106 tramos faltantes vs 5 en Planta 2). Patrones: madrugada y mediodía en P1; pocos tramos dispersos en P2. |
| **3** | Distribución de pérdidas por inversor | P1: pérdida distribuida en tramos horarios. P2: 4 inversores con más pérdida de datos que el resto. |

### Notebook 02 — Preparación de Variables

| # | Insight | Descripción |
|---|---------|-------------|
| **4** | Variables derivadas | Creación de variables temporales (mes, día, hora, time) y de eficiencia DC-AC. El tablon analítico preparado integra generación y sensores meteorológicos. |
| **5** | Agregaciones diarias | Construcción de `df_dia` con agregaciones (min, mean, max) por planta, inversor y fecha. Datasets intermedios guardados como pickle para el análisis. |

### Notebook 03 — Análisis e Insights

| # | Insight | Descripción |
|---|---------|-------------|
| **6** | Irradiación correcta en ambas plantas | La irradiación es normal y esperable en ambas plantas. P2 tiene mayor temperatura y menor irradiación, lo cual es coherente con factores locales (humedad, altitud, exposición). El problema no está en la captación de luz. |
| **7** | Correlación DC-AC | La correlación DC–AC es casi perfecta; el fallo está en la captación (paneles) o en la medida del sensor, no en la conversión del inversor. |
| **8** | Fallos detectados | Se identifican dos tipos: **potencia_cero** (DC nula con irradiación) y **potencia_baja** (ratio potencia/irradiación bajo). La Planta 2 concentra ~72% de los fallos en horario solar. |
| **9** | Inversores prioritarios | Los top 10 inversores más afectados son candidatos prioritarios para inspección. Concentración en P2 (2020-06-10, 2020-06-01, etc.). |

---

## INFORME EJECUTIVO: ANÁLISIS DETALLADO DE FALLOS EN INVERSORES

*(Datos filtrados: horario solar 6–18 h)*

---

### 1. Distribución General de Fallos

| Tipo | Registros | % |
|------|-----------|---|
| **POTENCIA_CERO** | 722 | 54,1% |
| **POTENCIA_BAJA** | 613 | 45,9% |
| **Total** | **1.335** | 100% |

- **Potencia cero:** Potencia DC nula en tramo horario. Puede indicar desconexión, mantenimiento o fallo de medición.
- **Potencia baja:** Potencia < 70% de la media de horas adyacentes. Indica caída puntual respecto al patrón esperado.

---

### 2. Fallos por Planta

| Planta | Potencia baja | Potencia cero | Total |
|--------|---------------|---------------|-------|
| **p1** | 289 | 84 | 373 |
| **p2** | 324 | 638 | 962 |

**Conclusión:** La Planta P2 presenta **2,6 veces** más incidencias que P1.

---

### 3. Top 10 Inversores con Más Incidencias

| Planta | Inversor | n_incidencias | % |
|--------|----------|---------------|---|
| p2 | LYwnQax7tkwH5Cb | 65 | 4,9% |
| p2 | 81aHJ1q11NBPMrL | 64 | 4,8% |
| p2 | rrq4fwE8jgrTyWY | 60 | 4,5% |
| p2 | xoJJ8DcxJEcupym | 56 | 4,2% |
| p2 | PeE6FRyGXUgsRhN | 54 | 4,0% |
| p2 | Et9kgGMDl729KT4 | 53 | 4,0% |
| p2 | 9kRcWv60rDACzjR | 50 | 3,7% |
| p2 | Quc1TzYxW2pYoWX | 45 | 3,4% |
| p2 | vOuJvMaM2sgwLmb | 45 | 3,4% |
| p2 | oZZkBaNadn6DNKz | 45 | 3,4% |

**Acción requerida:** Estos inversores deben ser revisados prioritariamente para mantenimiento preventivo o correctivo.

---

### 4. Días con Mayor Concentración de Fallos

| Planta | Fecha | n_incidencias |
|--------|-------|---------------|
| p2 | 2020-06-10 | 284 |
| p2 | 2020-06-01 | 266 |
| p2 | 2020-05-24 | 208 |
| p2 | 2020-05-20 | 204 |
| p1 | 2020-06-05 | 192 |
| p1 | 2020-06-14 | 92 |
| p1 | 2020-05-20 | 89 |

---

### 5. Distribución Horaria de Fallos

| Hora | n_incidencias |
|------|---------------|
| 18 | 350 |
| 12 | 162 |
| 11 | 142 |
| 13 | 139 |
| 6 | 124 |
| 10 | 114 |
| 14 | 109 |
| 17 | 74 |
| 9 | 48 |
| 16 | 22 |
| 7 | 22 |
| 15 | 21 |
| 8 | 8 |

**Insight:** Todos los fallos analizados ocurren en horario de producción (6–18 h), por lo que el impacto económico es máximo al coincidir con horas de generación.

---

### 6. Top 15 Combinaciones Críticas (Planta-Fecha-Inversor-Hora)

| id_planta | fecha | id_inversor | hora | tipo_fallo | n_registros | potencia_media |
|-----------|-------|-------------|------|------------|-------------|----------------|
| p2 | 2020-06-10 | mqwcsP2rE7J0TFp | 11 | potencia_cero | 4 | 0,0 |
| p2 | 2020-06-10 | rrq4fwE8jgrTyWY | 12 | potencia_cero | 4 | 0,0 |
| p2 | 2020-06-10 | rrq4fwE8jgrTyWY | 11 | potencia_cero | 4 | 0,0 |
| p2 | 2020-06-10 | rrq4fwE8jgrTyWY | 10 | potencia_cero | 4 | 0,0 |
| p2 | 2020-06-10 | mqwcsP2rE7J0TFp | 12 | potencia_cero | 4 | 0,0 |
| p2 | 2020-06-10 | NgDl19wMapZy17u | 12 | potencia_cero | 4 | 0,0 |
| p2 | 2020-06-10 | NgDl19wMapZy17u | 11 | potencia_cero | 4 | 0,0 |
| p2 | 2020-06-10 | NgDl19wMapZy17u | 10 | potencia_cero | 4 | 0,0 |
| p2 | 2020-06-10 | Quc1TzYxW2pYoWX | 10 | potencia_cero | 4 | 0,0 |
| p2 | 2020-06-10 | PeE6FRyGXUgsRhN | 13 | potencia_cero | 4 | 0,0 |
| p2 | 2020-06-10 | mqwcsP2rE7J0TFp | 10 | potencia_cero | 4 | 0,0 |
| p2 | 2020-06-10 | WcxssY2VbP4hApt | 12 | potencia_cero | 4 | 0,0 |
| p2 | 2020-06-10 | Et9kgGMDl729KT4 | 10 | potencia_cero | 4 | 0,0 |
| p2 | 2020-06-10 | Et9kgGMDl729KT4 | 12 | potencia_cero | 4 | 0,0 |
| p2 | 2020-06-01 | xMbIugepa2P7lBB | 18 | potencia_cero | 4 | 0,0 |

---

### 7. Diagnóstico de Causas Potenciales

#### POTENCIA CERO

- Desconexión total del inversor
- Activación de protecciones (sobrecorriente, sobretensión, temperatura)
- Pérdida de comunicación con el controlador
- Mantenimiento no programado o parada de emergencia
- Fallo en el string DC o problema en los módulos fotovoltaicos

#### POTENCIA BAJA ANÓMALA

- Degradación progresiva del inversor
- Sombreado parcial o suciedad en módulos
- Fallo intermitente en componentes internos
- Curtailment o limitación externa de potencia
- Desbalance en strings o pérdida de MPPTs

---

### 8. Recomendaciones Estratégicas

#### Acciones inmediatas

1. Inspección técnica de los inversores del top 10 (ver sección 3)
2. Revisión de logs de protecciones y alarmas en días críticos identificados
3. Verificación de conexiones DC y estado de strings fotovoltaicos

#### Acciones de mediano plazo

4. Implementar sistema de monitorización predictiva en inversores críticos
5. Evaluar costes de reemplazo vs. reparación en equipos recurrentes
6. Optimizar calendario de mantenimiento preventivo basado en patrones horarios

#### Impacto económico

7. Cuantificar energía no generada durante episodios de fallo
8. Calcular pérdidas económicas y comparar con coste de soluciones
9. Priorizar inversiones en equipos con mayor impacto en disponibilidad

---

*Documento generado a partir del análisis del proyecto PROYECTO_PLANTA_SOLAR. Basado en `03_analisis_insights.ipynb`.*
