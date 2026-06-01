# PROJECT CHARTER
## Market Intelligence: Lifestyle Drugs & Bienestar en México (2024-2026)

**Última actualización:** 31 de Mayo 2026  
**Estado:** Pre-Ejecución - Listo para Fase 1  
**Responsable:** Business Analyst (Usuario)  

---

## 1. PROPÓSITO ESTRATÉGICO

Estructurar un análisis integral de **Market Intelligence** sobre el consumo de fármacos de estilo de vida, nootrópicos y suplementos en México (2024-2026), identificando:

- **Drivers demográficos y socioeconómicos** que explican adopción
- **Brecha entre posicionamiento de marketing vs comportamiento real** del consumidor
- **Segmentación psicográfica** de perfiles de adopción
- **Tendencias emergentes** en optimización humana farmacológica

**Público Final:** Reclutadores en consulting (IQVIA, McKinsey, Euromonitor), tomadores de decisión empresariales (Fika)

---

## 2. PREGUNTA CENTRAL DE NEGOCIO

**¿Cuál es la brecha entre el *posicionamiento esperado* y el *comportamiento real* del consumidor mexicano en el segmento de fármacos de estilo de vida y suplementos? ¿Cómo explican factores demográficos, psicosociales y socioeconómicos esta divergencia?**

Sub-preguntas:
1. ¿Quién adopta lifestyle drugs (Ozempic, nootrópicos, adaptógenos) en México?
2. ¿Cuáles son los 3-4 perfiles psicográficos distintivos?
3. ¿Qué promete el marketing vs qué entrega en la realidad (reviews, comportamiento)?
4. ¿Cómo varía adopción por geografía (CDMX vs provincia)?
5. ¿Cuál es la tendencia de crecimiento 2024-2026?

---

## 3. METODOLOGÍA

**Tipo de Análisis:** Market Intelligence estilo Euromonitor/Gartner/McKinsey

**Enfoque:**
- Recolección **multisector** (datos públicos + web scraping + redes sociales)
- Análisis exploratorio (EDA) con rigor estadístico
- Segmentación psicográfica basada en comportamiento
- Síntesis narrativa orientada a decisiones estratégicas

**Stack Técnico:**
- Python (Pandas, NumPy, Scikit-learn)
- Web scraping (Beautiful Soup, Selenium)
- Análisis de sentimiento (NLP básico)
- Visualización (Matplotlib, Seaborn, Plotly)
- Control de versión (Git/GitHub)

---

## 4. STAKEHOLDERS Y ROLES

| Rol | Responsabilidad | Decisiones |
|-----|-----------------|-----------|
| **Usuario (BA)** | Dirección estratégica, validación de calidad, síntesis de insights | Criterios de limpieza, narrativa final, recomendaciones |
| **Claude Code (Automatización)** | Generación de scripts, limpieza, EDA, visualizaciones | Implementación técnica, optimización de código |
| **Proyecto Fika** | Contexto operacional, validación de realismo | Recomendaciones finales |
| **Portfolio IQVIA** | Inspiración metodológica, público objetivo | Frame profesional, rigor consulting |

---

## 5. SCOPE

### INCLUIDO (In Scope):
✅ Recolección de datos de 5+ fuentes validadas  
✅ Limpieza y validación de calidad  
✅ EDA exploratorio (demografía, tendencias, sentimiento)  
✅ Segmentación psicográfica (3-4 perfiles)  
✅ Identificación de brecha promesa vs realidad  
✅ Dashboards interactivos  
✅ Narrativa de insights + recomendaciones estratégicas  
✅ Documentación reproducible (GitHub público)  

### EXCLUIDO (Out of Scope):
❌ Encuestas formales (online survey tools con muestras n>100)  
❌ Trabajo de campo presencial  
❌ Análisis econométrico complejo  
❌ Validación clínica/farmacológica de productos  
❌ Recomendaciones de política pública  
❌ Análisis de competencia específica de marcas  

---

## 6. OBJETIVOS SMART

| # | Objetivo | Métrica | Timeline |
|---|----------|---------|----------|
| **O1** | Recopilar datos de 5+ fuentes con validación de calidad | 100% de fuentes documentadas + data dictionary | Semana 1 |
| **O2** | Generar 3-4 segmentos psicográficos diferenciados | K-means clustering validado + narrativa de perfiles | Semana 2-3 |
| **O3** | Identificar 3-4 "killing insights" accionables | Insights con recomendaciones específicas para IQVIA + Fika | Semana 3 |
| **O4** | Crear dashboard interactivo con 5+ visualizaciones clave | Dashboard HTML interactivo, deployment en GitHub Pages | Semana 3 |
| **O5** | Documentar análisis con rigor profesional | Notebooks comentados + README ejecutivo | Semana 4 |

---

## 7. ENTREGABLES PRINCIPALES

```
📊 ENTREGABLES FINALES:
├── 01_EXECUTIVE_SUMMARY.md (2-3 página, C-level ready)
├── 02_DATA_COLLECTION_REPORT.md (fuentes, metodología, n)
├── 03_QUALITY_ASSURANCE_REPORT.md (validaciones aplicadas)
├── 04_EXPLORATORY_DATA_ANALYSIS.ipynb (código + insights preliminares)
├── 05_SEGMENTATION_ANALYSIS.ipynb (perfiles psicográficos)
├── 06_PROMISE_VS_REALITY.ipynb (brecha marketing vs reviews)
├── 07_DASHBOARD_INTERACTIVO.html (visualización principal)
├── 08_STRATEGIC_RECOMMENDATIONS.md (para IQVIA + Fika)
└── 09_GITHUB_REPOSITORY (público, documentado, reproducible)
```

---

## 8. TIMELINE Y HITOS (4 SEMANAS)

```
SEMANA 1 (2-4 Jun): Planificación + Recolección Fase 1
├── 1.1 Definición de keywords + estrategia web scraping
├── 1.2 Descarga INEGI (ENOE, ENIGH)
├── 1.3 Setup Google Trends (5 años histórico)
└── HITO: Datos brutos listos para limpieza

SEMANA 2 (5-11 Jun): Limpieza + EDA Inicial
├── 2.1 Web scraping ejecutado (Amazon, MercadoLibre, Reddit)
├── 2.2 Consolidación y limpieza de datos (decisiones validadas)
├── 2.3 EDA exploratorio (demografía, tendencias, sentimiento)
└── HITO: Datos limpios + insights preliminares documentados

SEMANA 3 (12-18 Jun): Análisis Avanzado + Segmentación
├── 3.1 Segmentación psicográfica (K-means + interpretación)
├── 3.2 Análisis de brecha promesa vs realidad
├── 3.3 Comparativas geográficas
├── 3.4 Narrativa de insights clave
└── HITO: 3-4 killing insights identificados + documentados

SEMANA 4 (19-25 Jun): Visualizaciones + Entregables Finales
├── 4.1 Dashboard interactivo
├── 4.2 Executive summary
├── 4.3 Recomendaciones estratégicas (IQVIA + Fika)
├── 4.4 GitHub documentación final
└── HITO: Proyecto 100% entregable
```

---

## 9. CRITERIOS DE ÉXITO (GO/NO-GO)

**ÉXITO = Todos estos criterios cumplidos:**

| # | Criterio | Métrica | Go/No-Go |
|---|----------|---------|----------|
| **C1** | Datos validados | ≥5 fuentes documentadas, data quality report completado | ✅ Go |
| **C2** | Segmentación clara | 3-4 segmentos con narrativa diferenciada | ✅ Go |
| **C3** | Insights accionables | ≥3 killing insights con recomendaciones específicas | ✅ Go |
| **C4** | Rigor profesional | Documentación reproducible, notebooks comentados | ✅ Go |
| **C5** | Timeline respetado | Entregables listos en 4 semanas | ✅ Go |

**NO-GO scenarios (proyecto se reconsidera):**
- ❌ <3 fuentes de datos validadas
- ❌ Segmentación ambigua o sin narrativa clara
- ❌ Insights no accionables o demasiado genéricos
- ❌ Documentación incompleta

---

## 10. RESTRICCIONES Y SUPUESTOS

### Restricciones:
- **Timeline:** 4 semanas máximo (agresivo pero posible)
- **Recursos:** Usuario (part-time) + Claude Code (full automation)
- **Presupuesto:** $0 (datos públicos + web scraping)
- **Recolección:** Internet-first, sin encuestas formales

### Supuestos:
- ✓ Datos INEGI están disponibles y accesibles
- ✓ Web scraping legal en sitios públicos (Amazon, MercadoLibre, Reddit)
- ✓ Tamaño de muestra web scrapeada (5-7 productos) es suficiente para insights
- ✓ Sentimiento en reviews es indicador válido de brecha promesa vs realidad
- ✓ Segmentación psicográfica con 3-4 clusters es interpretable

### Riesgos y Mitigación:

| Riesgo | Probabilidad | Impacto | Mitigación |
|--------|--------------|---------|-----------|
| Web scraping bloqueado | Media | Alto | Cambiar a APIs públicas, usar delays, respetar robots.txt |
| Datos INEGI incompletos | Baja | Medio | Usar ENOE + ENIGH en paralelo, documentar gaps |
| Segmentación no clara | Baja | Alto | Aumentar a 5-6 clusters, usar validación de silhouette |
| Timeline ajustado | Alta | Alto | Priorizar: O1→O3→O4 (O5 si sobra tiempo) |

---

## 11. GOBERNANZA Y DECISIONES

### Toma de Decisiones:
- **Estratégicas** (scope, timeline, go/no-go): Usuario final
- **Técnicas** (limpieza, validación, EDA): Usuario guiado por Claude Code
- **Implementación** (código, pushes, CI/CD): Claude Code

### Escalación:
- Si bloqueador técnico: Claude Code propone solución, usuario aprueba
- Si datos insuficientes: Revisar Data Collection Strategy, pivotar fuentes
- Si timeline en riesgo: Reducir scope a O1→O3→O4

### Comunicación:
- Asana: Tracking de tareas + bloqueadores
- GitHub: Source of truth para código + documentación
- Este documento: Referencia para cualquier desviación

---

## 12. DEFINICIONES Y GLOSARIO

| Término | Definición |
|---------|-----------|
| **Lifestyle Drugs** | Fármacos para optimización humana (no tratamiento de enfermedad): Ozempic, nootrópicos, adaptógenos |
| **Brecha Promesa vs Realidad** | Divergencia entre claim de marketing + sentimiento real en reviews de clientes |
| **Segmentación Psicográfica** | Clustering de consumidores por motivaciones, valores, comportamiento (no solo demografía) |
| **Killing Insight** | Descubrimiento poco evidente pero altamente accionable para decisiones estratégicas |
| **Web Scraping Legal** | Extracción de datos de sitios públicos respetando robots.txt, terms of service, delays entre requests |

---

## 13. AUTORIZACIÓN Y FIRMAS

| Rol | Nombre | Firma | Fecha |
|-----|--------|-------|-------|
| BA / Dueño Proyecto | [Usuario] | ✅ | 31-May-2026 |
| Tutor / Claude Code Manager | Claude | ✅ | 31-May-2026 |

---

**Documento activo. Última revisión: 31 de Mayo 2026. Próxima revisión: Semana 2 (10 Jun)**
