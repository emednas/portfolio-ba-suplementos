# SUCCESS METRICS & GO/NO-GO CRITERIA
## Definición No Ambigua de Éxito (Por Fase)

**Propósito:** Establecer criterios objetivos de éxito para cada fase  
**Responsable:** Usuario (final decision maker) + Claude Code (tracking)  
**Última actualización:** 31 de Mayo 2026

---

## 1. ESTRUCTURA GENERAL

Cada fase tiene:
1. **KPIs cuantitativos** (números concretos)
2. **Criterios cualitativos** (calidad de output)
3. **Go/No-Go decision point** (procedes o repites)
4. **Acción if No-Go** (qué haces si no pasa)

---

## 2. FASE 1: PLANIFICACIÓN ESTRATÉGICA (Semana 1)

### 2.1 KPIs Cuantitativos

| Métrica | Target | Realidad | Status |
|---------|--------|---------|--------|
| Documentos creados | 4 | ___ | ☐ |
| Palabras por documento | ≥500 | ___ | ☐ |
| Commits a GitHub | ≥4 | ___ | ☐ |

### 2.2 Criterios Cualitativos

```
PREGUNTA_NEGOCIO.md:
✓ Pregunta clara y específica (no genérica)
✓ Menciona "brecha", "comportamiento real", "mercado"
✓ Establece scope (geografía, timeframe, categorías)

HIPOTESIS_PRINCIPALES.md:
✓ ≥3 hipótesis concretas
✓ Cada hipótesis es testeable (no "el mercado crecerá")
✓ Ejemplo: "Consumidores tech profesionales adoptarán Ozempic para productividad, no peso"

SEGMENTACION_TARGET.md:
✓ Describe 3-4 segmentos demográficos
✓ Incluye edad, ingresos, ocupación, ubicación
✓ Justifica por qué estos segmentos

FUENTES_DATOS.md:
✓ Lista ≥5 fuentes
✓ Especifica n mínimo para cada fuente
✓ Menciona limitaciones de cada fuente
```

### 2.3 Go/No-Go Decision

**GO if:**
- ✅ 4 documentos pusheados a GitHub
- ✅ Cada documento ≥500 caracteres
- ✅ Pregunta es testeable y específica
- ✅ Fuentes de datos son obtenibles

**NO-GO if:**
- ❌ <3 documentos
- ❌ Pregunta es vaga ("¿cuál es el mercado?")
- ❌ Fuentes data son imposibles de obtener

**Acción if No-Go:**
1. Usuario + Claude Code sesión de refinamiento
2. Redefine pregunta central (máx 2 horas)
3. Vuelve a hacer Fase 1
4. Si persiste: **ESCALAR** (posible pivote de proyecto)

---

## 3. FASE 2: RECOLECCIÓN Y LIMPIEZA (Semanas 1-2)

### 3.1 KPIs Cuantitativos (Mínimos Absolutos)

| Fuente | Métrica | Target | Realidad | Status |
|--------|---------|--------|---------|--------|
| INEGI | n registros | >10,000 | ___ | ☐ |
| Google Trends | semanas histórico | ≥156 (3 años) | ___ | ☐ |
| Amazon | reviews válidas | ≥80 | ___ | ☐ |
| MercadoLibre | reviews válidas | ≥40 | ___ | ☐ |
| Reddit | posts relevantes | ≥50 | ___ | ☐ |
| Foros | testimonios | ≥20 | ___ | ☐ |
| **CONSOLIDADO** | **Total data points** | **≥1,000** | ___ | ☐ |

### 3.2 KPIs de Calidad (Por Fuente)

```
INEGI:
- Completitud: ≥99% (missings <1%)
- Variables críticas: EDAD, SEXO, INGRESOS, OCUPACION, UBICACION
- Timeframe: 2024-2026

GOOGLE TRENDS:
- Completitud: ≥95% semanas (interpolar <5% faltantes)
- Rango validado: 0-100 escala oficial
- Keywords: ≥8 términos relevantes
- Anomalías documentadas: Picos explicados

AMAZON:
- Spam detectado & removido: ≤5%
- Rating distribution: Variación (no solo 4-5 stars)
- Texto mínimo: ≥30 caracteres/review
- Verificación de compra: ≥50% "Compra verificada"

MERCADOLIBRE:
- Calidad vendedor: Rating vendedor ≥4.0
- Spam: ≤5%
- Consistencia con Amazon: Precio ±30% (documentada si diferencia)

REDDIT:
- On-topic: ≥70% posts relevantes (manual check 10% muestra)
- Idioma: ≥80% español
- Score mínimo: Distribuido (no todos posts con 0 upvotes)
- Spam: ≤10%

FOROS:
- Longitud mínima: ≥50 caracteres/testimonio
- Perfil verificable: ≥60% perfiles con antigüedad ≥3 meses
- Contenido: Experiencias reales (no publicidad)
```

### 3.3 Go/No-Go Decision

**GO if:**
- ✅ TODAS las métricas cuantitativas alcanzadas
- ✅ INEGI + Google Trends + Amazon en status "GO"
- ✅ Data Quality Report completado (sin blockers críticos)
- ✅ Todos los datos procesados en GitHub (`02_DATOS/processed/`)

**CONDITIONAL GO if:**
- ⚠️ 1 fuente no alcanza mínimo, pero otras compensan
  - Ej: Amazon solo 70 reviews (vs 80 target), pero MercadoLibre + Reddit compensan
  - Acción: Documentar limitación explícitamente

**NO-GO if:**
- ❌ INEGI incompleto o <5,000 registros
- ❌ Google Trends <1 año de historia
- ❌ Total data points <500 (muy bajo)
- ❌ >30% spam en cualquier fuente
- ❌ Quality Report identifica "blocker crítico"

**Acción if No-Go:**
1. Claude Code intenta solución (cambiar fuente, recolectar más)
2. Si persiste: Usuario decide PIVOTE
   - Opción A: Extender timeline a Semana 3
   - Opción B: Reducir scope (omitir fuente)
   - Opción C: **PARAR PROYECTO** (si compromete integridad)

---

## 4. FASE 3: EDA (Semanas 2-3)

### 4.1 KPIs Cuantitativos

| Métrica | Target | Realidad | Status |
|---------|--------|---------|--------|
| Notebooks completados | 4 | ___ | ☐ |
| Visualizaciones | ≥15 | ___ | ☐ |
| Líneas de código comentadas | ≥80% | ___ | ☐ |
| Commits | ≥5 | ___ | ☐ |

### 4.2 Criterios Cualitativos (Por Notebook)

#### EDA DEMOGRÁFICO
```
✓ Distribuciones univariadas (edad, sexo, ingresos, ocupación)
✓ Análisis bivariado (correlaciones, chi-squared tests)
✓ Segmentaciones preliminares (quintiles ingresos, grupos edad)
✓ Narrativa: "Quién es el mercado potencial?" (≥200 palabras)
✓ Limitaciones documentadas (ej: INEGI no captura específicamente usuarios Ozempic)
```

#### EDA TENDENCIAS
```
✓ Gráficos de series temporales (5 años, keywords clave)
✓ Análisis de tendencia (crecimiento/decaimiento)
✓ Detección de picos/anomalías (ej: máximo en enero = New Year's resolution?)
✓ Correlaciones entre términos (Ozempic vs "pérdida de peso")
✓ Narrativa: "¿Cómo crece el interés?" + "¿Qué eventos explican picos?"
```

#### EDA SENTIMIENTO
```
✓ Distribución de ratings (histograma 1-5 stars)
✓ Análisis de polaridad (positivo/negativo)
✓ Top palabras por sentimiento (word clouds)
✓ Matriz de confusión: Rating vs Sentimiento (¿son coherentes?)
✓ Narrativa: "¿Qué dicen realmente los usuarios?" + inconsistencias detectadas
```

#### CLUSTERING EXPLORATORIO
```
✓ Determinación de k óptimo (silhouette score)
✓ Interpretación de clusters (características principales)
✓ Visualización (scatter, dendrograma)
✓ Narrativa: "¿Cuáles son los segmentos naturales?" (PRELIMINAR)
```

### 4.3 Go/No-Go Decision

**GO if:**
- ✅ 4 notebooks en GitHub
- ✅ ≥15 visualizaciones de calidad
- ✅ Código >80% comentado
- ✅ Insights preliminares claros (no contradictorios)
- ✅ Limitaciones documentadas para cada hallazgo

**CONDITIONAL GO if:**
- ⚠️ 1 notebook incompleto, pero insights están
  - Acción: Completar faltante en 1 día

**NO-GO if:**
- ❌ <3 notebooks
- ❌ Insights contradictorios (ej: "crece" vs "decrece" para mismo keyword)
- ❌ Visualizaciones pobres o ilegibles
- ❌ Sin mencionar limitaciones

**Acción if No-Go:**
1. Claude Code revisa/corrige notebooks
2. Usuario identifica contradicciones
3. Resuelve en máx 1 día
4. Si no: **ESCALAR** (posible sesgo en datos)

---

## 5. FASE 4: ANÁLISIS AVANZADO (Semana 3)

### 5.1 KPIs Cuantitativos

| Métrica | Target | Realidad | Status |
|---------|--------|---------|--------|
| Segmentos identificados | 3-4 | ___ | ☐ |
| Silhouette score | ≥0.40 | ___ | ☐ |
| Notebooks análisis | ≥2 | ___ | ☐ |
| Palabras narrativa final | ≥2,000 | ___ | ☐ |

### 5.2 Criterios Cualitativos

#### SEGMENTACIÓN PSICOGRÁFICA
```
✓ 3-4 segmentos diferenciados (no overlap)
✓ Cada segmento tiene:
  - Demografía (edad, ingresos, ocupación)
  - Motivación (por qué compran)
  - Comportamiento (cómo compran)
  - Productos interés
  - Narrativa de 200+ palabras
✓ Validación: Silhouette score ≥0.40 (clusteres bien separados)
✓ Distribución: Segmentos > 15% población cada uno (no one outlier)
```

#### BRECHA PROMESA VS REALIDAD
```
✓ Identifica específicamente:
  - Qué PROMETE marketing (investigar packaging, ads, influencers)
  - Qué DICE la realidad (análisis de reviews, Reddit posts)
  - Brecha cuantificada (ej: "30% promete peso, 70% reporta energía")
✓ Por lo menos 2-3 ejemplos concretos
✓ Implicación: "¿Es producto fallido o mal posicionado?"
✓ Narrativa: ≥300 palabras
```

#### ANÁLISIS GEOGRÁFICO (Opcional si hay tiempo)
```
✓ Comparativa: CDMX vs Guadalajara vs Monterrey vs Provincia
✓ Diferencias en: Ocupación, ingresos, preferencia de productos
✓ Implicación: Marketing diferenciado sí/no justificado
```

#### SÍNTESIS NARRATIVA PRINCIPAL
```
✓ 3-5 "killing insights" clave (máximo 50 palabras c/u)
✓ Cada insight tiene: Hallazgo + Implicación estratégica
✓ Narrativa coherente (no desconectados)
✓ Diferenciador para IQVIA claro
✓ Total: ≥2,000 palabras
```

### 5.3 Go/No-Go Decision

**GO if:**
- ✅ 3-4 segmentos diferenciados
- ✅ Silhouette ≥0.40
- ✅ Brecha promesa vs realidad identificada (cuantificada)
- ✅ 3-5 killing insights documentados
- ✅ Narrativa ≥2,000 palabras
- ✅ Todos pusheados a GitHub

**CONDITIONAL GO if:**
- ⚠️ Silhouette 0.35-0.39 (algo bajo, pero interpretable)
  - Acción: Aumentar a k=5, revisar features
- ⚠️ Brecha no tan dramática como esperado
  - Acción: Documentar como "Hipótesis no confirmada"

**NO-GO if:**
- ❌ <3 segmentos
- ❌ Silhouette <0.30 (clusteres mal definidos)
- ❌ No hay "brecha" identificable (todos los insights son obvios)
- ❌ Narrativa vaga o incompleta

**Acción if No-Go:**
1. Usuario + Claude Code revizan qué salió mal
   - ¿Datos insuficientes?
   - ¿Features no representativas?
   - ¿Hipótesis original era errónea?
2. Opciones:
   - **Opción A:** Ajustar modelo (más features, diferentes k)
   - **Opción B:** Pivotar narrativa (contar la historia que SÍ hay)
   - **Opción C:** SIMPLIFICAR (2 segmentos vs 4, enfocarse en brecha)

---

## 6. FASE 5: VISUALIZACIONES (Semana 4)

### 6.1 KPIs Cuantitativos

| Métrica | Target | Realidad | Status |
|---------|--------|---------|--------|
| Dashboards interactivos | 3 | ___ | ☐ |
| Visualizaciones PNG (DPI 300+) | ≥5 | ___ | ☐ |
| Commits | ≥2 | ___ | ☐ |
| Tamaño archivo HTML | <10MB total | ___ | ☐ |

### 6.2 Criterios Cualitativos

#### DASHBOARD 1: CONTEXTO DE MERCADO
```
✓ Título claro
✓ Mapa de México (heatmap de interés por región)
✓ Series temporal Google Trends (últimas 5 años)
✓ Distribuciones demográficas (edad, ingresos)
✓ Interactividad: Filtros por fecha, región
✓ Anotaciones de eventos (si aplica)
```

#### DASHBOARD 2: COMPORTAMIENTO CONSUMIDOR
```
✓ Segmentos psicográficos (gráfico circular o barras)
✓ Rating vs Sentimiento (scatter)
✓ Palabras clave por segmento (word cloud interactivo)
✓ Línea temporal de adopción (curva en forma de S?)
```

#### DASHBOARD 3: ANÁLISIS GEOGRÁFICO
```
✓ Comparativa CDMX vs Guadalajara vs otros
✓ Diferencias de "interés" (búsquedas, reviews)
✓ Ocupaciones por región
✓ Implicación: "¿Mercado diferenciado sí/no?"
```

#### PNGS ALTA CALIDAD
```
✓ DPI ≥300 (para impresión)
✓ Colores profesionales (no default matplotlib)
✓ Ejes etiquetados
✓ Leyendas claras
✓ Ejemplos:
  - Tendencia Ozempic 5 años
  - Segmentos scatter plot
  - Brecha promesa vs realidad (gráfico comparativo)
```

### 6.3 Go/No-Go Decision

**GO if:**
- ✅ 3 dashboards HTML funcionales (sin errores)
- ✅ ≥5 PNGs DPI 300+
- ✅ Todos en GitHub
- ✅ Dashboards cargan sin lag

**CONDITIONAL GO if:**
- ⚠️ 2 dashboards (3º incompleto)
  - Acción: Completar en 1 día

**NO-GO if:**
- ❌ <2 dashboards
- ❌ Dashboards con errores (gráficos rotos, datos inconsistentes)
- ❌ PNGs baja calidad

---

## 7. FASE 6: RECOMENDACIONES (Semana 4)

### 7.1 KPIs Cuantitativos

| Métrica | Target | Realidad | Status |
|--------|--------|---------|--------|
| Reportes recomendaciones | 2 | ___ | ☐ |
| Palabras (IQVIA) | ≥1,500 | ___ | ☐ |
| Palabras (Fika) | ≥1,000 | ___ | ☐ |
| Executive summary | 1 | ___ | ☐ |

### 7.2 Criterios Cualitativos

#### RECOMENDACIONES PARA IQVIA
```
✓ Mínimo 3 recomendaciones estratégicas
✓ Cada recomendación tiene:
  - Situación (hallazgo que la genera)
  - Acción (qué hacer específicamente)
  - Impacto esperado (cuantitativamente si es posible)
✓ Ejemplos de recomendaciones:
  - "Segmentar messaging: Ozempic para productividad (tech pros), no peso"
  - "Expandir en CDMX primero, luego replicar Guadalajara"
  - "Validar brecha promesa-realidad con estudios clínicos más rigurosos"
✓ Rigor: Sostenidas en datos (no opinión)
✓ Tono: Consultoría neutral (no venta)
```

#### RECOMENDACIONES PARA FIKA
```
✓ Mínimo 2-3 recomendaciones operacionales
✓ Áreas: Estrategia de producto, marketing, pricing
✓ Ejemplos:
  - "Mensaje actual ('pierde peso') no conecta con realidad (energía)"
  - "Target: 28-45 años, CDMX/Guadalajara, ingresos Q3-Q5"
  - "Colaborar con influencers fitness (no weight-loss)"
✓ Viable: Implementable en próximos 3-6 meses
```

#### EXECUTIVE SUMMARY
```
✓ 2-3 páginas máximo
✓ Estructura:
  - Pregunta central (1 párrafo)
  - Metodología (1 párrafo)
  - 3-5 hallazgos clave
  - 3-5 recomendaciones
  - Limitaciones & siguiente pasos
✓ Tono: C-level ready (sin jargon)
✓ Debe ser autónomo (leer sin leer reportes completos)
```

### 7.3 Go/No-Go Decision

**GO if:**
- ✅ Reportes IQVIA + Fika completos
- ✅ Executive summary ≥2 páginas
- ✅ Recomendaciones sostenidas en datos
- ✅ Tono consultorial (neutral, profesional)
- ✅ Todo pusheado

**NO-GO if:**
- ❌ <2 recomendaciones
- ❌ Recomendaciones no sostenidas en análisis
- ❌ Tono vendedor o sesgado

---

## 8. FASE 7: GITHUB Y PUBLICACIÓN (Semana 4)

### 8.1 KPIs Cuantitativos

| Métrica | Target | Realidad | Status |
|---------|--------|---------|--------|
| README completo | ✓ | ___ | ☐ |
| Estructura carpetas | Correcta | ___ | ☐ |
| Documentación | ≥90% tareas | ___ | ☐ |
| requirements.txt | ✓ | ___ | ☐ |
| Repo PÚBLICO | ✓ | ___ | ☐ |
| Commits totales | ≥15 | ___ | ☐ |

### 8.2 Criterios Cualitativos

#### README
```
✓ Sección "About"
✓ Pregunta central del proyecto
✓ Metodología (1 párrafo)
✓ Estructura de carpetas
✓ Cómo reproducir (instrucciones técnicas)
✓ Hallazgos principales (bullet points)
✓ Limitaciones
✓ Autor + fecha
✓ NO: Menciones a "Fika", "validación de producto"
✓ SÍ: "Market Intelligence", "tendencias de consumo"
```

#### DOCUMENTACIÓN
```
✓ PROJECT_CHARTER.md
✓ DATA_COLLECTION_STRATEGY.md
✓ DATA_QUALITY_FRAMEWORK.md
✓ DATA_DICTIONARY.md
✓ Cada notebook con markdown cells explicando lógica
✓ Comments en código >50% líneas
```

#### ESTRUCTURA CARPETAS
```
✓ /01_PREGUNTA_Y_HIPOTESIS/ (documentos iniciales)
✓ /02_DATOS/ con subcarpetas raw/ + processed/
✓ /03_NOTEBOOKS/ (todos los .ipynb)
✓ /04_VISUALIZACIONES/ (dashboards + PNGs)
✓ /05_INSIGHTS/ (reportes de hallazgos)
✓ /06_RECOMENDACIONES/ (reporte final)
✓ /PROJECT_GOVERNANCE/ (charters, manifests)
✓ README.md (raíz)
✓ requirements.txt
✓ .gitignore
✓ LICENSE (MIT)
```

### 8.3 Go/No-Go Decision

**GO if:**
- ✅ Repo público + README completo
- ✅ Documentación ≥90% tareas
- ✅ Estructura limpia
- ✅ Requirements.txt funcional

**NO-GO if:**
- ❌ Repo privado o sin README
- ❌ Estructura desorganizada

---

## 9. KPI GLOBAL: ¿PROYECTO "EXITOSO"?

**EL PROYECTO SE CONSIDERA EXITOSO SI:**

| Dimensión | Criterio | Status |
|-----------|----------|--------|
| **Datos** | ≥5 fuentes validadas, n>1,000 data points | ☐ |
| **Análisis** | 3-4 segmentos claros + brecha identificada | ☐ |
| **Rigor** | Documentación completa, limitaciones explícitas | ☐ |
| **Rapidez** | Completado en ≤4 semanas | ☐ |
| **Actionable** | ≥3 recomendaciones sostenidas en datos | ☐ |
| **Presentable** | GitHub público, README profesional | ☐ |
| **Portfolio Value** | Diferencia clara para reclutadores (rapidez + rigor) | ☐ |

**ÉXITO GLOBAL = ≥6/7 dimensiones en ☑️**

---

## 10. TRACKING & REPORTE DIARIO

**Claude Code genera (diariamente):**

```markdown
# DAILY PROGRESS REPORT - Día X

**Fase:** [X]
**Tareas completadas hoy:**
- ✓ Tarea 1
- ✓ Tarea 2
- ⏳ Tarea 3 (75% completada)

**KPIs Running:**
- Documentos en GitHub: 12/18
- Visualizaciones: 8/15
- Commits: 7/15

**Bloqueadores:** Ninguno
**Próxima tarea:** [X]
**Timeline:** On track / At risk / Behind

**Commits de hoy:**
- commit 1
- commit 2
```

---

## 11. FINAL: MATRIZ DE RIESGOS

Si alguna fase falla:

| Fase | Riesgo | Mitigación |
|------|--------|-----------|
| 1 | Pregunta mal definida | Sesión 2 horas con usuario para refinar |
| 2 | Web scraping bloqueado | Cambiar fuentes o usar APIs, agregar tiempo |
| 3 | EDA genera contradicciones | Revisión de datos, posible recolecta adicional |
| 4 | Segmentación no clara | Aumentar a k=5-6, simplificar features |
| 5 | Dashboards fallan | Usar visualizaciones estáticas si es necesario |
| 6 | Recomendaciones débiles | Aceptar hallazgo "neutral", documentar como aprendizaje |
| 7 | GitHub issues | Documentación manual, publicación en LinkedIn sin repo |

---

**Documento activo. Próxima revisión: Semana 2 (11 Jun)**
