# Presentación Ejecutiva: Análisis de Fallos en Plantas Solares Fotovoltaicas

**Instrucciones para NotebookLM:** Crear una presentación PowerPoint de 10 diapositivas. Cada sección corresponde a una diapositiva. Usar títulos como encabezados y el contenido como bullets o texto principal. El tono debe ser persuasivo para ejecutivos.

---

## DIAPOSITIVA 1: Portada

**Título:** Análisis de Fallos en Plantas Solares — Business Case Ejecutivo

**Contenido:**
- Caso Business Analytics — Planta Solar Fotovoltaica
- Análisis de anomalías detectadas en 2 plantas
- Marzo 2025

---

## DIAPOSITIVA 2: El problema

**Título:** Situación actual: anomalías sin diagnóstico

**Contenido:**
- La compañía detectó comportamientos anómalos en **2 plantas solares**
- La subcontrata de mantenimiento no ha identificado el origen
- Período analizado: 34 días (15 mayo – 17 junio 2020)
- 2 plantas, 44 inversores (22 por planta), datos cada 15 minutos
- **El equipo de Data Science ha localizado, clasificado y cuantificado las anomalías**

---

## DIAPOSITIVA 3: Lo que descubrimos

**Título:** Hallazgos clave del análisis de datos

**Contenido:**
- **Error de escala corregido:** La potencia DC de la Planta 1 estaba multiplicada por 10 (error de un decimal). Ya corregido.
- **Irradiación correcta:** No es la causa. Ambas plantas reciben radiación solar normal.
- **El fallo no está en la conversión DC-AC:** La correlación es casi perfecta; el problema está en paneles o sensores.
- **1.335 fallos** en horario de producción (6–18 h) que impactan directamente los ingresos

---

## DIAPOSITIVA 4: Magnitud del problema

**Título:** 1.335 fallos en horario solar — 100% impacto económico

**Contenido:**
- **Potencia cero (54%):** Hay sol pero no se genera nada — desconexión, protecciones o fallo de paneles
- **Potencia baja (46%):** Rendimiento muy inferior al esperado — degradación, suciedad o sombrado
- Todos los fallos ocurren en horas 6–18 h — coinciden con la generación
- **Resultado:** Cada fallo = energía no vendida en el momento de mayor valor

---

## DIAPOSITIVA 5: Dónde actuar — Planta 2 es el foco

**Título:** Planta 2 concentra el 72% de las incidencias

**Contenido:**
- **Planta 1:** 373 fallos (28%)
- **Planta 2:** 962 fallos (72%) — **2,6 veces más** que P1
- Los 10 inversores más problemáticos son **todos de Planta 2**
- Días críticos: 10-jun, 1-jun, 24-may, 20-may (P2)
- **Acción:** Priorizar inspección en P2 y en los inversores identificados

---

## DIAPOSITIVA 6: Impacto económico (AS-IS)

**Título:** Coste actual: ~17.900 €/año en ingresos perdidos

**Contenido:**
- 1.335 fallos × 25 kWh media ≈ **33.375 kWh** no producidos en 34 días
- Extrapolación anual: **~358.000 kWh** no generados
- A 0,05 €/kWh: **~17.900 €/año** de ingresos perdidos
- Rango de sensibilidad: **10.700 € – 25.000 €/año**
- **Además:** Costes de mantenimiento reactivo y visitas no planificadas

---

## DIAPOSITIVA 7: La solución (TO-BE)

**Título:** Tres palancas para recuperar valor

**Contenido:**
1. **Corrección de fallos detectados** — inspección y reparación de inversores y paneles prioritarios
2. **Monitoreo proactivo** — dashboards y alertas basados en este análisis
3. **Mejora de calidad de datos** — menos tramos faltantes, trazabilidad completa

**Escenario base (70% corrección):**
- Recuperación de **12.530 €** en ingresos
- Reducción de **1.000 €** en gastos de mantenimiento
- **Beneficio total: 13.530 €/año**

---

## DIAPOSITIVA 8: Business Case — Estratégico y Financiero

**Título:** El proyecto se justifica en ambas dimensiones

**Contenido:**

*Estratégico:*
- Mayor fiabilidad operativa y trazabilidad
- Reducción del riesgo de fallos no detectados
- Menor dependencia del mantenimiento externo
- Alineado con la dirección de la compañía

*Financiero:*
- **Escenario conservador (40%):** 7.660 € beneficio anual
- **Escenario base (70%):** 13.530 € beneficio anual
- **Escenario optimista (90%):** 18.110 € beneficio anual

---

## DIAPOSITIVA 9: Retorno de la inversión

**Título:** Payback inferior a 6 meses en todos los escenarios

**Contenido:**
- Inversión estimada (escenario base): **5.000 €** (ingeniería, reparaciones, monitoreo)
- Beneficio anual: **13.530 €**
- **Payback: ~4–5 meses**
- **ROI año 1: 271%**
- Rentable incluso en el escenario más conservador

---

## DIAPOSITIVA 10: Recomendación y próximos pasos

**Título:** Aprobar el proyecto y actuar con prioridad

**Contenido:**
1. **Aprobar** el proyecto de corrección — retorno estratégico y financiero positivo
2. **Priorizar** los 10 inversores identificados (todos en Planta 2)
3. **Implementar** monitoreo proactivo con dashboards
4. **Revisar** el contrato de mantenimiento — criterios de calidad y resolución de anomalías
5. **Extender** el análisis a períodos más largos cuando haya más datos

**Conclusión:** El análisis de datos ha transformado un problema sin diagnóstico en un plan de acción cuantificado y rentable.
