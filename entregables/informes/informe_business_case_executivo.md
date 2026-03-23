# Informe Ejecutivo: Business Case — Anomalías en Plantas Solares Fotovoltaicas

**Proyecto:** Análisis de datos para detección de problemas de producción  
**Destinatarios:** Dirección y comité ejecutivo  
**Fecha:** Marzo 2025  
**Versión:** 1.0

---

## 1. Resumen Ejecutivo

La compañía de generación solar fotovoltaica ha detectado **comportamientos anómalos** en 2 de sus plantas. La subcontrata de mantenimiento no ha identificado el origen. El análisis de datos ha permitido **localizar, clasificar y cuantificar** las anomalías, y evaluar el impacto económico de su corrección.

Este informe presenta un **Business Case** estructurado en torno a:

- **BC Estratégico vs BC Financiero:** cuantificación del valor aportado y soporte a la decisión de ejecutar o no el proyecto de corrección.
- **AS-IS:** línea base actual (pérdidas identificadas).
- **TO-BE:** escenarios con distintas palancas aplicadas.
- **Escenarios múltiples:** conservador, base y optimista.

**Conclusión principal:** La corrección de los fallos detectados se justifica desde el punto de vista estratégico (fiabilidad operativa, trazabilidad, cumplimiento) y ofrece un **retorno económico positivo** en un horizonte anual, incluso en escenarios conservadores.

---

## 2. Contexto y Objetivos del Proyecto

### 2.1 Situación inicial

- **Problema:** Comportamientos anómalos en 2 plantas solares; el mantenimiento subcontratado no identifica la causa.
- **Objetivo:** Analizar los datos de sensores y medidores para detectar dónde están los problemas de producción.
- **Período analizado:** 34 días (15 mayo – 17 junio 2020).
- **Plantas:** 2 plantas (p1, p2), 22 inversores cada una (44 total).
- **Granularidad:** Ventanas de 15 minutos.

### 2.2 KPIs analizados

| KPI | Descripción |
|-----|-------------|
| Irradiación | Energía solar incidente (Wh/m²) |
| Temperatura ambiente/módulo | °C |
| Potencia DC | kW generados en continua |
| Potencia AC | kW entregados en alterna |
| Eficiencia inversor | AC/DC × 100 (%) |

---

## 3. Palancas de Ingresos y/o Ahorros

Las palancas que influyen sobre el objetivo de negocio (generar corriente AC) son:

| # | Palanca | Impacto en ingresos/ahorros |
|---|--------|-----------------------------|
| 1 | **Irradiación** | Base de la generación DC; su correcta captación permite maximizar ingresos |
| 2 | **Estado de los paneles** | Suciedad o averías reducen DC; limpieza/reparación → incremento de ingresos |
| 3 | **Eficiencia de los inversores** | Pérdidas DC→AC; mantenimiento → reducción de pérdidas = más ingresos |
| 4 | **Medidores y sensores** | Fallos de medición generan pérdida de trazabilidad y detección tardía de fallos |

En este proyecto, el análisis de datos ha permitido identificar anomalías que afectan a **las palancas 2, 3 y 4**.

---

## 4. AS-IS: Línea Base

### 4.1 Fallos detectados (horario solar 6–18 h)

| Tipo de fallo | Registros | % | Descripción |
|---------------|-----------|---|-------------|
| **Potencia cero con irradiación** | 722 | 54,1% | Hay sol pero no se genera potencia (posible fallo de medición, paneles o inversor) |
| **Baja ratio potencia/irradiación** | 613 | 45,9% | Bajo rendimiento respecto a la irradiación recibida |
| **Total (horario solar)** | **1.335** | 100% | Ventanas de 15 min con anomalía |

*En el período completo (24 h) se registran 8.127 fallos; el 1.335 corresponden al horario de producción (6–18 h), que es el relevante para ingresos.*

### 4.2 Distribución por planta

- **Planta 1:** Mayor número de fallos y menor completitud de datos (106 tramos faltantes vs 5 en P2).
- **Planta 2:** Mayor concentración de fallos en inversores concretos (posibles problemas localizados).

### 4.3 Cálculo del impacto (AS-IS)

**Metodología:**

- Cada ventana de 15 min con fallo equivale a **0 kWh** generados cuando, en condiciones normales, correspondería ~25 kWh por inversor (promedio horario diurno).
- 1.335 fallos × 25 kWh ≈ **33.375 kWh** de energía no producida en el período de 34 días.
- Extrapolación anual: 33.375 × (365/34) ≈ **358.000 kWh/año**.
- Con un precio de venta de **0,05 €/kWh** (referencia mercado solar): **≈ 17.900 €/año** de ingresos perdidos.

**Rango de sensibilidad:** Con 15–35 kWh/fallo, el impacto anual oscila entre 10.700 € y 25.000 €. Los fallos de “baja ratio” representan producción reducida, no nula; y hay que sumar costes indirectos (mantenimiento reactivo, riesgo reputacional, etc.).

---

## 5. Asunciones para el Business Case

| Asunción | Valor | Fuente/justificación |
|----------|-------|----------------------|
| Precio venta energía | 0,05 €/kWh | Mercado solar, referencia conservadora |
| Período de análisis | 34 días | Datos disponibles |
| Ventanas de fallo (horario solar) | 1.335 | Análisis 03_analisis_insights.ipynb |
| Energía media por ventana “perdida” | 25 kWh | Promedio producción diurna por inversor |
| Coste mantenimiento correctivo | 500 €/visita | Referencia sector |
| Visitas evitables/año (estimado) | 2–4 | Con monitoreo proactivo |
| % fallos corregibles (escenario base) | 70% | Asunción moderada |
| Horizonte de análisis | 1 año | Ciclo presupuestario típico |

---

## 6. TO-BE: Beneficios Esperados con las Palancas Aplicadas

### 6.1 Palancas aplicadas en TO-BE

1. **Corrección de fallos detectados** (paneles, inversores, sensores).
2. **Monitoreo proactivo** basado en los análisis realizados (dashboards, alertas).
3. **Mejora de la calidad de datos** (menos tramos faltantes, alineación temporal).

### 6.2 Escenarios

Los beneficios se estiman como **incremento de ingresos** y **reducción de gastos** respecto al AS-IS.

| Escenario | Descripción | % corrección fallos | Incremento ingresos (anual) | Reducción gastos (anual) | Beneficio total anual |
|-----------|-------------|---------------------|-----------------------------|---------------------------|------------------------|
| **Conservador** | Corrección parcial, solo inversores más críticos | 40% | +7.160 € | +500 € (1 visita evitada) | **7.660 €** |
| **Base** | Corrección moderada con monitoreo proactivo | 70% | +12.530 € | +1.000 € (2 visitas) | **13.530 €** |
| **Optimista** | Corrección amplia + mejora calidad de datos | 90% | +16.110 € | +2.000 € (4 visitas) | **18.110 €** |

*Los importes de ingresos se basan en la energía recuperada; los de gastos, en visitas de mantenimiento evitadas.*

### 6.3 Resumen visual (AS-IS vs TO-BE)

```
                    AS-IS              TO-BE (Base)
Ingresos energía   -17.900 €/año  →   -5.370 €/año  (70% corrección)
Gastos manten.     +2.000 €/año   →   +1.000 €/año  (2 visitas menos)
────────────────────────────────────────────────────
Impacto neto       Pérdida             Ahorro ≈ 13.530 €/año
```

---

## 7. Business Case Estratégico vs Financiero

### 7.1 BC Estratégico

| Criterio | Evaluación |
|----------|------------|
| **Alineación con objetivos** | Detección y corrección de anomalías → mayor fiabilidad operativa |
| **Riesgo operativo** | Reducción del riesgo de fallos no detectados y detección tardía |
| **Trazabilidad** | Mejor calidad de datos y capacidad de análisis |
| **Cumplimiento** | Mayor control sobre contratos, garantías y métricas de rendimiento |
| **Reputación** | Menor dependencia del mantenimiento externo y mejora de la imagen interna |

**Conclusión estratégica:** El proyecto aporta valor en fiabilidad, trazabilidad y control; es coherente con la dirección estratégica de la compañía.

### 7.2 BC Financiero

| Métrica | Conservador | Base | Optimista |
|---------|-------------|------|-----------|
| Beneficio anual (€) | 7.660 | 13.530 | 18.110 |
| Inversión estimada (€) | 3.000 | 5.000 | 8.000 |
| Payback (años) | 0,4 | 0,4 | 0,4 |
| ROI (año 1) | 255% | 271% | 226% |

*Inversión estimada: ingeniería, reparaciones menores, sistema de monitoreo/dashboards.*

**Conclusión financiera:** El proyecto es **rentable** en los tres escenarios; el escenario base ofrece un payback inferior a 1,5 años.

---

## 8. Recomendaciones para la Dirección

1. **Aprobar el proyecto** de corrección de anomalías, dado el retorno estratégico y financiero positivo.
2. **Priorizar** los inversores con mayor número de fallos (lista disponible en el análisis).
3. **Implementar monitoreo proactivo** con dashboards basados en el análisis realizado.
4. **Revisar el contrato de mantenimiento** para incorporar criterios de calidad de datos y resolución de anomalías.
5. **Extender el análisis** a períodos más largos cuando haya datos históricos adicionales.

---

## 9. Anexos

### A. Fuentes de datos y metodología

- **Notebooks:** `01_importacion_datos.ipynb`, `02_preparacion_variables.ipynb`, `03_analisis_insights.ipynb`
- **Informe calidad:** `docs/informe_calidad_datos.md`
- **Objetivos:** `docs/proyecto-objetivos.md`

### B. Tipos de fallo (detalle)

| Código | Descripción |
|--------|-------------|
| `potencia_cero_con_irrad` | Irradiación > 0,5 Wh/m² y potencia DC < 1 kW |
| `baja_ratio_potencia_irrad` | Ratio potencia_dc/irradiación < 600 (bajo rendimiento) |
| `eficiencia_baja` | Eficiencia inversor < 95% (problema conversión DC-AC) |

### C. Limitaciones

- Período corto (34 días); extrapolación anual con margen de incertidumbre.
- Precio de energía y costes de mantenimiento son estimaciones.
- No se incluyen costes de oportunidad ni impacto en contratos PPA concretos.

---

*Documento generado a partir del análisis del proyecto PROYECTO_PLANTA_SOLAR.*
