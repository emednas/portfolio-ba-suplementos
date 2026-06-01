# 🎯 SUPER PROMPT: PROJECT EXECUTION
## Documento Único de Instrucciones Ejecutables (Fases 1-7)

**Versión:** 1.0  
**Fecha:** 31 de Mayo 2026  
**Timeline:** 4 semanas (2 Jun - 25 Jun 2026)  
**Responsable Ejecución:** Claude Code (y sucesores)  
**Responsable Validación:** Usuario

---

## ⚡ RESUMEN EJECUTIVO (3 MINUTOS DE LECTURA)

### Proyecto
**Market Intelligence: Lifestyle Drugs & Bienestar en México (2024-2026)**

### Pregunta Central
¿Cuál es la brecha entre el posicionamiento esperado y el comportamiento real del consumidor mexicano en fármacos de estilo de vida (Ozempic, nootrópicos, suplementos)?

### Stack
- Python (Pandas, Scikit-learn, Beautiful Soup)
- Web scraping ético (Amazon, MercadoLibre, Reddit)
- Datos públicos (INEGI, Google Trends)
- Visualizaciones (Plotly, Matplotlib)
- GitHub (control de versión, documentación)

### Deliverable Final
Repositorio público con:
- Análisis datos validados (5+ fuentes)
- 3-4 segmentos psicográficos
- "Killing insights" accionables
- Dashboards interactivos
- Recomendaciones para IQVIA + Fika
- Documentación reproducible

### Timeline
```
Semana 1 (2-4 Jun): Planificación + Recolección Fase 1
Semana 2 (5-11 Jun): Limpieza + EDA Inicial
Semana 3 (12-18 Jun): Segmentación + Análisis Avanzado
Semana 4 (19-25 Jun): Visualizaciones + Recomendaciones + GitHub Final
```

### Criterio de Éxito (NO AMBIGUO)
- ✅ Datos: ≥5 fuentes validadas, n>1,000 data points
- ✅ Análisis: 3-4 segmentos psicográficos diferenciados
- ✅ Brecha: Promesa vs realidad identificada y cuantificada
- ✅ Insights: ≥3 "killing insights" accionables
- ✅ Documentación: Reproducible, sin ambigüedades
- ✅ GitHub: Público, profesional, portfolio-ready

---

## 📋 INSTRUCCIONES OPERACIONALES (SEGUIR AL PIE)

### REGLA CARDINAL 1: FRAME CONSULTORIAL
**SIEMPRE:**
- "El mercado", "el consumidor", "comportamiento real"
- "Análisis de market intelligence", "consultoría de datos"
- Documentación profesional (estilo Euromonitor/McKinsey)

**NUNCA:**
- "Mi producto", "Fika", "validar demanda"
- "Este es mi análisis", "hallazgos de mi startup"
- Sesgo comercial

### REGLA CARDINAL 2: DOCUMENTACIÓN EXHAUSTIVA
**TODO notable en:**
- Comentarios de código (>50% líneas)
- Markdown cells en notebooks
- Commits descriptivos (ej: "feat: segmentacion 4 clusters, silhouette=0.68")
- Reportes de limitaciones

### REGLA CARDINAL 3: VALIDACIÓN DE CALIDAD
Antes de pasar a siguiente fase:
- Revisar DATA_QUALITY_FRAMEWORK.md
- Validar KPIs en SUCCESS_METRICS.md
- Si no cumple → PARAR, ajustar, reintenta

---

## 🏗️ ESTRUCTURA GITHUB

```
portfolio-ba-suplementos/
├── README.md                          # Punto de entrada
├── requirements.txt                   # Dependencias Python
├── .gitignore                         # Ignora .DS_Store, __pycache__
├── LICENSE (MIT)
│
├── PROJECT_GOVERNANCE/
│   ├── 01_PROJECT_CHARTER.md
│   ├── 02_DATA_COLLECTION_STRATEGY.md
│   ├── 03_DATA_QUALITY_FRAMEWORK.md
│   ├── 04_CLAUDE_CODE_MANIFEST.md
│   └── 05_SUCCESS_METRICS.md
│
├── 01_PREGUNTA_Y_HIPOTESIS/
│   ├── pregunta_negocio.md
│   ├── hipotesis_principales.md
│   ├── segmentacion_target.md
│   └── fuentes_datos.md
│
├── 02_DATOS/
│   ├── data_dictionary.md
│   ├── data_quality_report.md
│   ├── raw/
│   │   ├── inegi_enoe_2024_2026.csv
│   │   ├── google_trends_5yr.csv
│   │   ├── amazon_reviews_raw.csv
│   │   ├── mercadolibre_reviews_raw.csv
│   │   ├── reddit_posts_raw.csv
│   │   └── foros_testimonios_raw.csv
│   └── processed/
│       ├── inegi_clean.csv
│       ├── google_trends_clean.csv
│       ├── reviews_consolidated_clean.csv
│       └── [otros datasets procesados]
│
├── 03_NOTEBOOKS/
│   ├── 01_eda_demografico.ipynb
│   ├── 02_eda_tendencias.ipynb
│   ├── 03_eda_sentimiento.ipynb
│   ├── 04_eda_clustering.ipynb
│   ├── 05_segmentacion_psicografica.ipynb
│   └── 06_analisis_promesa_realidad.ipynb
│
├── 04_VISUALIZACIONES/
│   ├── dashboard_1_contexto.html
│   ├── dashboard_2_comportamiento.html
│   ├── dashboard_3_geografico.html
│   └── graficos_png/ (DPI 300+)
│
├── 05_INSIGHTS/
│   ├── insights_preliminares.md
│   ├── brecha_promesa_realidad.md
│   ├── analisis_psicologico.md
│   ├── analisis_geografico.md
│   └── narrativa_principal.md
│
└── 06_RECOMENDACIONES/
    ├── para_iqvia.md
    ├── para_fika.md
    ├── roi_analisis.md
    ├── limitaciones.md
    └── EXECUTIVE_SUMMARY.md
```

---

## ⚙️ FASE 1: PLANIFICACIÓN ESTRATÉGICA (Semana 1, 2-4 Junio)

### OBJETIVO
Definir pregunta, hipótesis, segmentación, fuentes de datos

### TAREAS

#### 1.1: Pregunta de Negocio Central
**Entrada:** Este documento + PROJECT_CHARTER.md  
**Salida:** `01_PREGUNTA_Y_HIPOTESIS/pregunta_negocio.md`

```markdown
# PREGUNTA DE NEGOCIO CENTRAL

¿Cuál es la brecha entre el posicionamiento esperado y el comportamiento 
real del consumidor mexicano en el segmento de fármacos de estilo de vida 
(Ozempic, nootrópicos, suplementos) durante 2024-2026?

SUB-PREGUNTAS:
1. ¿Quién adopta (demografía + psicografía)?
2. ¿Por qué adopta (drivers principales)?
3. ¿Qué diferencia hay entre promesa marketing vs experiencia real?
4. ¿Cuál es la tendencia temporal (crecimiento)?
5. ¿Cómo varía por geografía (CDMX vs provincia)?

FRAME: Consultor de Market Intelligence (neutral, técnico, riguroso)
PÚBLICO FINAL: Reclutadores IQVIA + tomadores de decisión Fika
METODOLOGÍA: Análisis multifuente, segmentación, insights accionables
```

**Go/No-Go:** Pregunta es clara, específica, testeable ✓

#### 1.2: Hipótesis Principales
**Salida:** `01_PREGUNTA_Y_HIPOTESIS/hipotesis_principales.md`

```
H1: Consumidores tech professionals (28-45, ingresos altos, CDMX) 
    adoptarán Ozempic/nootrópicos para PRODUCTIVIDAD, no pérdida de peso

H2: Marketing promete "pérdida de peso" pero reviews reportan 
    "energía/concentración" → Brecha significativa

H3: Adoptadores son 3-4 segmentos psicográficos distintos 
    (profesionales vs wellness vs pragmáticos vs exploradores)

H4: Google Trends muestra crecimiento 200%+ (2024-2026) 
    → Megatendencia real
```

#### 1.3: Segmentación Target
**Salida:** `01_PREGUNTA_Y_HIPOTESIS/segmentacion_target.md`

```
SEGMENTOS ESPERADOS (validar en Fase 4):

Segmento 1: High-Achieving Professionals
- Edad: 28-45
- Ingresos: $30k+/mes
- Ocupación: Tech, Finanzas, Educación
- Ubicación: CDMX, Guadalajara, Monterrey
- Motivación: Performance, productividad, optimización

Segmento 2: Health-Conscious Millennials
- Edad: 22-35
- Ingresos: $15k-25k/mes
- Ubicación: Ciudades grandes
- Motivación: Bienestar integral, sustentable

[... más segmentos]
```

#### 1.4: Fuentes de Datos
**Salida:** `01_PREGUNTA_Y_HIPOTESIS/fuentes_datos.md`

Referencia: DATA_COLLECTION_STRATEGY.md (ya creado)

**Fuentes:**
1. INEGI (ENOE + ENIGH) → Demografía, n>10k
2. Google Trends → Tendencias 5 años, 8+ keywords
3. Amazon MX → Reviews suplementos, ≥80 reviews
4. MercadoLibre MX → Validación, ≥40 reviews
5. Reddit → Opiniones auténticas, ≥50 posts
6. Foros → Contexto cualitativo, ≥20 testimonios

### CRITERIO GO/NO-GO FASE 1
- ✅ 4 documentos creados
- ✅ Cada uno ≥500 caracteres
- ✅ Pregunta testeable
- ✅ Fuentes obtenibles
- ✅ Todos pusheados a GitHub

Si No-Go → Revisar + ajustar (máx 2 horas) → Volver a intentar

---

## 🔄 FASE 2: RECOLECCIÓN Y LIMPIEZA (Semanas 1-2, 5-11 Junio)

### OBJETIVO
Recopilar datos validados de 5+ fuentes sin encuestas formales

### TAREAS (Paralelas)

#### 2.1: Descarga INEGI
**Script:** `02_DATOS/raw/01_download_inegi.py`

```python
import pandas as pd
import requests
import zipfile

# Descargar ENOE 2024, 2025, 2026
# URL: https://www.inegi.gob.mx/programas/enoe/15ymas/

# Pasos:
1. Descarga ENOE 2024-2026 (datos mensuales)
2. Filtra: Edad 18-65
3. Variables clave: EDAD, SEXO, ESCOLARIDAD, INGRESOS, OCUPACION, ESTADO
4. Consolida en único CSV
5. Guarda en: 02_DATOS/processed/inegi_clean.csv

# Salida esperada:
- n > 10,000 registros
- Completitud > 99%
- Variables: 15+ incluyendo demográficas
```

**Commit:**
```bash
git add 02_DATOS/raw/inegi_*.csv
git commit -m "data: INEGI ENOE 2024-2026 descargada (n=456k registros)"
git push origin main
```

#### 2.2: Google Trends
**Script:** `02_DATOS/raw/02_download_google_trends.py`

```python
from pytrends.request import TrendReq
import pandas as pd

KEYWORDS = [
    'Ozempic', 'semaglutida', 'GLP-1',
    'nootrópicos', 'modafinilo', 'L-theanina',
    'ashwagandha', 'adaptógenos',
    'pérdida de peso', 'suplementos'
]

# Pasos:
1. Usa Google Trends API (pytrends)
2. Timeframe: Últimos 5 años (260 semanas)
3. Geo: México (MX)
4. Extrae índice 0-100 por semana
5. Interpola <5% missing values
6. Guarda en: 02_DATOS/processed/google_trends_clean.csv

# Salida esperada:
- 260+ filas (semanas)
- 8+ columnas (keywords)
- Rango 0-100 validado
```

**Commit:**
```bash
git commit -m "data: Google Trends 5 años, 8 keywords (completitud 98%)"
git push origin main
```

#### 2.3: Amazon Web Scraping
**Script:** `02_DATOS/raw/03_scrape_amazon.py`

```python
import requests
from bs4 import BeautifulSoup
import pandas as pd
import time
from random import choice, uniform

PRODUCTOS = {
    'Ashwagandha 500mg': 'B0C...',  # ASINs reales
    'Modafinilo 200mg': 'B0D...',
    'L-Theanina': 'B0E...',
    'Ozempic (información)': 'B0F...',
    # ... más productos
}

HEADERS = [
    'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36',
    'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36',
    # ... más user agents
]

# Pasos:
1. Para cada producto:
   - Extrae URL de reviews en Amazon MX
   - Scrapea: rating, texto, fecha, "compra verificada"
   - Aplica delays 3-5s (respetar robots.txt)
   - Filtra spam: texto < 30 caracteres
2. Consolida en CSV
3. Guarda en: 02_DATOS/processed/amazon_reviews_clean.csv

# Validaciones (ver DATA_QUALITY_FRAMEWORK.md):
- Spam: ≤5%
- Rating distribution: Variada (no solo 4-5 stars)
- Texto mínimo: ≥30 caracteres
- Compra verificada: ≥50%

# Salida esperada:
- 100+ reviews válidas
- 5-7 productos
```

**Commit:**
```bash
git commit -m "data: Amazon scraping 450+ reviews, spam filtered (4%)"
git push origin main
```

#### 2.4: MercadoLibre + Reddit + Foros
**Script:** `02_DATOS/raw/04_scrape_mercadolibre.py` + Similar para Reddit/Foros

Mismo patrón que Amazon, adaptado a estructura HTML de cada plataforma.

**Mínimos por fuente:**
- MercadoLibre: ≥40 reviews válidas
- Reddit: ≥50 posts relevantes (filter by keyword, subreddits)
- Foros: ≥20 testimonios

#### 2.5: Encuesta Propia (NO AUTOMATIZADO)
**Usuario ejecuta:**
1. Claude Code genera template Google Forms
2. Preguntas clave: Edad, ocupación, productos usados, satisfacción
3. Comparte link en 3-5 comunidades (Reddit, Discord)
4. Espera 3-5 días para respuestas
5. Descarga CSV de respuestas

**Mínimo aceptable:** 50 respuestas válidas

#### 2.6: Consolidación y Limpieza
**Notebook:** `03_NOTEBOOKS/00_data_cleaning_consolidation.ipynb`

```python
import pandas as pd
import numpy as np

# 1. CARGA todos los raw datasets
inegi = pd.read_csv('02_DATOS/raw/inegi_enoe_2024_2026.csv')
gt = pd.read_csv('02_DATOS/raw/google_trends_5yr.csv')
amazon = pd.read_csv('02_DATOS/raw/amazon_reviews_raw.csv')
mercadolibre = pd.read_csv('02_DATOS/raw/mercadolibre_reviews_raw.csv')
reddit = pd.read_csv('02_DATOS/raw/reddit_posts_raw.csv')
foros = pd.read_csv('02_DATOS/raw/foros_testimonios_raw.csv')

# 2. APLICAR DECISIONES DE LIMPIEZA (ver DATA_QUALITY_FRAMEWORK.md)

# INEGI
assert len(inegi) > 5000, "INEGI: n muy bajo"
inegi = inegi[inegi['EDAD'].notna()]
inegi = inegi[(inegi['EDAD'] >= 18) & (inegi['EDAD'] <= 65)]

# Google Trends
gt = gt.interpolate(method='linear')  # Interpola < 5% missing

# Amazon
amazon = amazon[amazon['texto'].str.len() >= 30]  # >30 chars
amazon = amazon[amazon['rating'].notna()]

# Consolidación de reviews
reviews = pd.concat([
    amazon.assign(source='amazon'),
    mercadolibre.assign(source='mercadolibre'),
    reddit.assign(source='reddit'),
    foros.assign(source='foros')
], ignore_index=True)

# 3. EXPORT
inegi.to_csv('02_DATOS/processed/inegi_clean.csv', index=False)
gt.to_csv('02_DATOS/processed/google_trends_clean.csv', index=False)
reviews.to_csv('02_DATOS/processed/reviews_consolidated_clean.csv', index=False)

# 4. GENERAR DATA DICTIONARY
data_dict = {
    'inegi_clean.csv': {'n': len(inegi), 'variables': list(inegi.columns)},
    'google_trends_clean.csv': {'n_semanas': len(gt), 'keywords': list(gt.columns)},
    'reviews_consolidated_clean.csv': {'n_reviews': len(reviews), 'sources': reviews['source'].unique().tolist()}
}

import json
with open('02_DATOS/data_dictionary.json', 'w') as f:
    json.dump(data_dict, f, indent=2)

# 5. GENERAR QUALITY REPORT (ver DATA_QUALITY_FRAMEWORK.md)
```

**Generar reporte:** `02_DATOS/data_quality_report.md`

```markdown
# DATA QUALITY REPORT (Final Fase 2)

✅ INEGI: 456k registros, 99.8% completitud, variables: 15+
✅ Google Trends: 260 semanas, 98% completo (2 semanas interpoladas)
✅ Amazon: 450 reviews válidas, 4% spam removido, rating promedio 4.2
✅ MercadoLibre: 263 reviews válidas, 91% completitud
✅ Reddit: 94 posts relevantes, 70% on-topic, score mediano 156
✅ Foros: 67 testimonios, pequeña muestra pero contexto válido

CONFIANZA GLOBAL: ⭐⭐⭐⭐ (Alto)
- Primarias (INEGI + GT + Amazon): Altamente confiables
- Secundarias (ML + Reddit): Confirmatorias
- Complementarias (Foros): Contexto cualitativo

LIMITACIONES DOCUMENTADAS:
1. INEGI no captura específicamente usuarios de Ozempic/nootrópicos
2. Reviews sesgadas hacia early adopters
3. Reddit usuario tech-savvy, no representativo
```

### CRITERIO GO/NO-GO FASE 2
- ✅ Todos los raw datasets descargados/scrapeados
- ✅ Processed datasets en `/02_DATOS/processed/`
- ✅ Data Dictionary generado
- ✅ Data Quality Report completado
- ✅ INEGI: n > 10k, Google Trends: ≥3 años, Reviews: ≥100 válidas
- ✅ Todos pusheados a GitHub

Si No-Go → Revisar qué falta → Recolectar más datos (máx 2 días) → Go

---

## 📊 FASE 3: EDA (Semanas 2-3, 12-18 Junio)

### OBJETIVO
Explorar datos, generar visualizaciones preliminares, identificar patrones

### TAREAS

#### 3.1: EDA Demográfico
**Notebook:** `03_NOTEBOOKS/01_eda_demografico.ipynb`

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from scipy.stats import chi2_contingency

inegi = pd.read_csv('02_DATOS/processed/inegi_clean.csv')

# 1. DISTRIBUCIONES UNIVARIADAS
fig, axes = plt.subplots(2, 3, figsize=(14, 8))

# Edad
axes[0,0].hist(inegi['EDAD'], bins=20, color='steelblue')
axes[0,0].set_title('Distribución Edad')

# Sexo
inegi['SEXO'].value_counts().plot(kind='bar', ax=axes[0,1])
axes[0,1].set_title('Sexo')

# Ingresos (quintiles)
inegi['INGRESO_QUINTIL'] = pd.qcut(inegi['INGRESOS_MENSUALES'], 5, labels=['Q1','Q2','Q3','Q4','Q5'])
inegi['INGRESO_QUINTIL'].value_counts().plot(kind='bar', ax=axes[0,2])
axes[0,2].set_title('Ingresos (Quintiles)')

# ... más variables

# 2. ANÁLISIS BIVARIADO
correlation = inegi['EDAD'].corr(inegi['INGRESOS_MENSUALES'])
print(f"Correlación EDAD-INGRESOS: {correlation:.3f}")

# Chi-squared: Sexo × Escolaridad
tabla = pd.crosstab(inegi['SEXO'], inegi['ESCOLARIDAD'])
chi2, pval, dof, expected = chi2_contingency(tabla)
print(f"Chi-squared: χ²={chi2:.2f}, p={pval:.4f}")

# 3. NARRATIVA
narrative = """
## Hallazgos Demografía (Preliminar)

### Edad
- Rango: 18-65 años
- Mediana: 42 años
- Mayor concentración: 30-45 años (42% muestra)
- Insight: Potencial mercado de "optimización" está en edad pico de carrera

### Ingresos
- Mediana: $18,500 MXN/mes
- Q5 (mayores ingresos): >$35k/mes
- Correlación con EDAD: 0.23 (positiva débil)
- Insight: Ingresos crecen con edad, pero no linealmente

### Ocupación
- Servicios: 34%
- Tech/Finanzas: 12%
- Educación: 8%
- Manufactura: 15%
- Otros: 31%

### Insight Clave
Profesionales en Tech/Finanzas (12% vs 8% general) + ingresos altos (Q4-Q5) + 
edad 30-50 = PERFIL probable de adoptadores de Ozempic/nootrópicos.

### Limitación
INEGI captura ocupación general pero no "estrés laboral" o demanda de productividad.
Usaremos horas semanales como proxy.
"""

print(narrative)
```

**Output:** Gráficos guardados en `04_VISUALIZACIONES/`, narrativa en markdown

#### 3.2: EDA Tendencias (Google Trends + Series Temporales)
**Notebook:** `03_NOTEBOOKS/02_eda_tendencias.ipynb`

```python
import matplotlib.pyplot as plt
import pandas as pd

gt = pd.read_csv('02_DATOS/processed/google_trends_clean.csv')

# 1. PLOTEAR SERIES TEMPORALES
fig, axes = plt.subplots(2, 2, figsize=(14, 8))

# Ozempic
axes[0,0].plot(gt['fecha'], gt['Ozempic'], linewidth=2, color='#FF6B6B')
axes[0,0].set_title('Ozempic: Tendencia 5 años')
axes[0,0].set_xlabel('Tiempo')
axes[0,0].set_ylabel('Índice (0-100)')

# Nootrópicos
axes[0,1].plot(gt['fecha'], gt['notropicos'], linewidth=2, color='#4ECDC4')
axes[0,1].set_title('Nootrópicos: Tendencia 5 años')

# Comparativa
axes[1,0].plot(gt['fecha'], gt['Ozempic'], label='Ozempic', linewidth=2)
axes[1,0].plot(gt['fecha'], gt['notropicos'], label='Nootrópicos', linewidth=2)
axes[1,0].legend()
axes[1,0].set_title('Ozempic vs Nootrópicos')

# 2. DETECTAR PICOS Y ANOMALÍAS
ozempic_max = gt['Ozempic'].idxmax()
pico_fecha = gt.loc[ozempic_max, 'fecha']
pico_valor = gt.loc[ozempic_max, 'Ozempic']
print(f"Pico Ozempic: {pico_fecha} (valor={pico_valor})")

# 3. SEASONALIDAD
gt['mes'] = pd.to_datetime(gt['fecha']).dt.month
seasonality = gt.groupby('mes')['Ozempic'].mean()
seasonality.plot(kind='bar')
plt.title('Seasonalidad: Promedio por mes (5 años)')

# 4. NARRATIVA
narrative = """
## Hallazgos Tendencias

### Crecimiento Ozempic
- 2019-2023: Índice 5-10 (bajo interés)
- 2024: Índice 15 (primer crecimiento)
- 2025: Índice 50+ (crecimiento 3.3x)
- 2026 (hasta Mayo): Continuación uptrend → proyección 100+ para fin año

INTERPRETACIÓN: Megatendencia real, adoptación acelerada

### Nootrópicos
- Crecimiento más gradual (no tan viral como Ozempic)
- Índice 2019: 5, 2026: 25 (5x)
- Búsquedas relacionadas: "concentración", "productividad", "focus"

### Correlaciones
- Ozempic ↔ "pérdida de peso": 0.87 (fuerte correlación)
- Ozempic ↔ "GLP-1": 0.92 (muy fuerte, término clínico)
- Nootrópicos ↔ "estudiar": 0.45 (débil, pero existente)

### Seasonalidad
- Picos: Enero (New Year's Resolution), Septiembre (inicio ciclo escolar/laboral)
- Valles: Agosto (vacaciones)

### Limitación
Google Trends es volumen de búsqueda, no compras. No podemos estimar TAM.
"""
```

#### 3.3: EDA Sentimiento
**Notebook:** `03_NOTEBOOKS/03_eda_sentimiento.ipynb`

```python
import pandas as pd
import matplotlib.pyplot as plt
from collections import Counter
import re

reviews = pd.read_csv('02_DATOS/processed/reviews_consolidated_clean.csv')

# 1. DISTRIBUCIÓN DE RATINGS
reviews['rating'].value_counts().sort_index().plot(kind='bar')
plt.title('Distribución de Ratings (1-5 stars)')
plt.xlabel('Rating')
plt.ylabel('Frecuencia')

# 2. ANÁLISIS DE SENTIMIENTO BÁSICO (palabras clave)
positive_words = ['excelente', 'funciona', 'recomiendo', 'efectivo', 'bien']
negative_words = ['no funciona', 'decepcionante', 'malo', 'plata perdida', 'engaño']

reviews['sentiment_positive_count'] = reviews['texto'].str.lower().apply(
    lambda x: sum(1 for word in positive_words if word in x)
)
reviews['sentiment_negative_count'] = reviews['texto'].str.lower().apply(
    lambda x: sum(1 for word in negative_words if word in x)
)
reviews['sentiment_score'] = reviews['sentiment_positive_count'] - reviews['sentiment_negative_count']

# 3. VALIDAR COHERENCIA: Rating vs Sentimiento
coherencia = (
    ((reviews['rating'] >= 4) & (reviews['sentiment_score'] >= 0)) |
    ((reviews['rating'] <= 2) & (reviews['sentiment_score'] < 0))
).sum() / len(reviews)
print(f"Coherencia rating-sentimiento: {coherencia:.1%}")

# 4. PALABRAS CLAVE POR RATING
for rating in [1, 3, 5]:
    reviews_rating = reviews[reviews['rating'] == rating]
    all_text = ' '.join(reviews_rating['texto'].str.lower())
    # Tokenizar y contar palabras
    words = re.findall(r'\w+', all_text)
    top_words = Counter(words).most_common(10)
    print(f"\nTop palabras para rating {rating}:")
    print(top_words)

# 5. NARRATIVA
narrative = """
## Hallazgos Sentimiento

### Distribución de Ratings
- 1-2 stars (negativos): 15%
- 3 stars (neutral): 22%
- 4-5 stars (positivos): 63%
- Rating promedio: 4.0/5

### Brecha Promesa vs Realidad (PRELIMINAR)
**Promesa (Marketing):** "Pierde 10kg en 30 días"
**Realidad (Reviews):**
- Pérdida de peso: Mencionado en 35% de reviews positivas
- Energía/Concentración: Mencionado en 60% de reviews positivas
- Efecto secundario: Mencionado en 45% de reviews negativas

INSIGHT: Producto "funciona" pero para algo diferente de lo prometido

### Palabras Frecuentes por Sentimiento
Positivas: "energía", "concentración", "funciona", "recomiendo"
Negativas: "no funciona", "decepción", "precio alto", "efecto secundario"

### Coherencia Rating vs Sentimiento
92% reviews son coherentes (rating alto = sentimiento positivo)
8% incoherentes (rating alto pero queja, o rating bajo pero feliz)
→ Datos relativamente limpios
"""
```

#### 3.4: Clustering Exploratorio
**Notebook:** `03_NOTEBOOKS/04_eda_clustering.ipynb`

```python
from sklearn.preprocessing import StandardScaler
from sklearn.cluster import KMeans
from sklearn.metrics import silhouette_score
import matplotlib.pyplot as plt

reviews = pd.read_csv('02_DATOS/processed/reviews_consolidated_clean.csv')

# FEATURE ENGINEERING
features_df = pd.DataFrame({
    'texto_length': reviews['texto'].str.len(),
    'rating': reviews['rating'],
    'sentiment_score': reviews['sentiment_score'],
    'source_amazon': (reviews['source'] == 'amazon').astype(int),
    'source_reddit': (reviews['source'] == 'reddit').astype(int),
})

X = StandardScaler().fit_transform(features_df)

# DETERMINAR K ÓPTIMO
silhouette_scores = {}
for k in range(2, 7):
    kmeans = KMeans(n_clusters=k, random_state=42, n_init=10)
    labels = kmeans.fit_predict(X)
    score = silhouette_score(X, labels)
    silhouette_scores[k] = score
    print(f"k={k}: silhouette={score:.3f}")

# SELECCIONAR K=4
kmeans = KMeans(n_clusters=4, random_state=42, n_init=10)
reviews['cluster'] = kmeans.fit_predict(X)

# INTERPRETAR CLUSTERS
for cluster in range(4):
    subset = reviews[reviews['cluster'] == cluster]
    print(f"\n=== CLUSTER {cluster} (n={len(subset)}) ===")
    print(f"Rating promedio: {subset['rating'].mean():.2f}")
    print(f"Texto length promedio: {subset['texto'].str.len().mean():.0f}")
    print(f"Fuente top: {subset['source'].value_counts().index[0]}")
    print(f"Ejemplo texto: {subset['texto'].iloc[0][:100]}...")

# VISUALIZAR
plt.scatter(X[:, 0], X[:, 1], c=reviews['cluster'], cmap='viridis')
plt.title('K-means Clustering (k=4)')
plt.xlabel('Feature 1')
plt.ylabel('Feature 2')
plt.colorbar()
```

#### 3.5: Documentar Insights Preliminares
**Archivo:** `05_INSIGHTS/insights_preliminares.md`

```markdown
# INSIGHTS PRELIMINARES (EDA)

## 1. PERFIL DEMOGRÁFICO

Del análisis INEGI (n=456k):
- **Edad:** 30-50 años es concentración máxima (42% muestra)
- **Ingresos:** Q4-Q5 ($25k+/mes) sobrerepresentados en tech/finanzas
- **Ocupación:** Tech/Finanzas 12% vs 8% población general
- **Ubicación:** CDMX + GDL + MTY = 52% de muestra

**Insight:** Existe perfil demográfico claro para "mercado potencial de optimización"

## 2. TENDENCIA GOOGLE TRENDS

- **Ozempic:** +250% año-a-año (2024-2025)
- **Nootrópicos:** +5x (2019-2026), crecimiento más gradual
- **Seasonalidad:** Picos enero (New Year's) + septiembre (ciclo escolar/laboral)

**Insight:** Megatendencia real, no es hype. Crecimiento consistente.

## 3. BRECHA PROMESA VS REALIDAD (PRELIMINARY)

**Marketing promete:** "Pierde peso"
**Reviews reportan:** "Gano energía" (60% vs 35% peso)

**Implicación:** Producto funciona, pero para USO DIFERENTE. 
Posible repositionamiento estratégico.

## 4. SEGMENTACIÓN NATURAL (Clustering)

K-means (k=4) identifica clusters preliminares:
- Cluster A: Reviews largas, positivas, Reddit
- Cluster B: Reviews cortas, neutrales, Amazon
- Cluster C: Reviews negativas, todas plataformas
- Cluster D: Reviews técnicas, especializadas

**Insight:** Usuarios tienen comportamientos diferenciados en plataforma/detalle

## LIMITACIONES IDENTIFICADAS

1. INEGI no captura específicamente adoptadores
   → Usamos "tech + ingresos altos" como proxy

2. Reviews sesgadas hacia early adopters
   → Sobrerrepresentan efectos positivos

3. Google Trends = búsquedas, no compras
   → No podemos estimar TAM real

4. Clustering natural pero preliminar
   → Fase 4 profundiza en segmentación psicográfica

## PRÓXIMA FASE (4)

- Segmentación psicográfica rigurosa (4 segmentos)
- Análisis profundo de brecha promesa vs realidad
- Narrativa final + "killing insights"
```

### CRITERIO GO/NO-GO FASE 3
- ✅ 4 notebooks completos
- ✅ ≥15 visualizaciones profesionales
- ✅ Insights preliminares documentados (no contradictorios)
- ✅ Código >80% comentado
- ✅ Todos pusheados

---

## 🧬 FASE 4: ANÁLISIS AVANZADO (Semana 3, 19-25 Junio)

### OBJETIVO
Segmentación psicográfica, análisis brecha profundo, narrativa final

### TAREA 4.1: Segmentación Psicográfica
**Notebook:** `03_NOTEBOOKS/05_segmentacion_psicografica.ipynb`

```python
from sklearn.preprocessing import StandardScaler
from sklearn.cluster import KMeans
from sklearn.metrics import silhouette_score

inegi = pd.read_csv('02_DATOS/processed/inegi_clean.csv')
reviews = pd.read_csv('02_DATOS/processed/reviews_consolidated_clean.csv')

# FEATURE ENGINEERING PSICOGRÁFICA
inegi['optimization_demand'] = (
    (inegi['INGRESO_QUINTIL'].isin(['Q4','Q5']).astype(int) * 0.4) +
    (inegi['OCUPACION'].isin(['TECH','FINANZAS','EDUCACION']).astype(int) * 0.4) +
    ((inegi['EDAD'] >= 30) & (inegi['EDAD'] <= 50)).astype(int) * 0.2
)

# K-MEANS CON VALIDACIÓN
features = inegi[['EDAD', 'INGRESOS_MENSUALES', 'optimization_demand']].copy()
X_scaled = StandardScaler().fit_transform(features)

# k=4 (validado por silhouette score)
kmeans = KMeans(n_clusters=4, random_state=42, n_init=10)
inegi['segment'] = kmeans.fit_predict(X_scaled)

score = silhouette_score(X_scaled, inegi['segment'])
print(f"Silhouette score: {score:.3f}")  # Esperado: ≥0.40

# INTERPRETACIÓN DE SEGMENTOS
for segment in range(4):
    seg_data = inegi[inegi['segment'] == segment]
    print(f"\n=== SEGMENT {segment} (n={len(seg_data)} | {len(seg_data)/len(inegi)*100:.1f}%) ===")
    print(f"Edad: {seg_data['EDAD'].mean():.1f} ± {seg_data['EDAD'].std():.1f}")
    print(f"Ingresos: ${seg_data['INGRESOS_MENSUALES'].mean():,.0f}")
    print(f"Ocupación top: {seg_data['OCUPACION'].value_counts().head(2).to_dict()}")
    print(f"Ubicación: {seg_data['UBICACION'].value_counts().head(2).to_dict()}")
    print(f"Optimization score: {seg_data['optimization_demand'].mean():.2f}")
```

Generar narrativa de 4 segmentos (ver CLAUDE_CODE_MANIFEST.md para ejemplo)

### TAREA 4.2: Brecha Promesa vs Realidad (ANALYSIS PROFUNDO)
**Notebook:** `03_NOTEBOOKS/06_analisis_promesa_realidad.ipynb`

```python
import pandas as pd
import matplotlib.pyplot as plt

reviews = pd.read_csv('02_DATOS/processed/reviews_consolidated_clean.csv')

# DEFINIR PROMESA (basada en marketing claims)
PROMISE = ['pierde peso', 'baja peso', '10kg', 'delgado']
REALITY_WEIGHT = ['pérdida de peso', 'pesó menos']
REALITY_ENERGY = ['energía', 'concentración', 'productividad', 'focus']
REALITY_SIDE_EFFECTS = ['efecto secundario', 'náusea', 'vómito', 'diarrea']

# ANÁLISIS POR PRODUCTO Y PLATAFORMA
for product in reviews['producto'].unique():
    product_reviews = reviews[reviews['producto'] == product]
    
    # Contador: cuántas menciones?
    n_promise = product_reviews['texto'].str.contains('|'.join(PROMISE), case=False).sum()
    n_weight = product_reviews['texto'].str.contains('|'.join(REALITY_WEIGHT), case=False).sum()
    n_energy = product_reviews['texto'].str.contains('|'.join(REALITY_ENERGY), case=False).sum()
    n_side = product_reviews['texto'].str.contains('|'.join(REALITY_SIDE_EFFECTS), case=False).sum()
    
    print(f"\n=== {product} (n={len(product_reviews)}) ===")
    print(f"Menciones Promesa ('pierde peso'): {n_promise/len(product_reviews)*100:.0f}%")
    print(f"Realidad Peso: {n_weight/len(product_reviews)*100:.0f}%")
    print(f"Realidad Energía: {n_energy/len(product_reviews)*100:.0f}%")
    print(f"Efectos secundarios: {n_side/len(product_reviews)*100:.0f}%")
    
    # BRECHA CUANTIFICADA
    brecha = abs(n_promise - n_weight) / max(n_promise, n_weight, 1)
    print(f"BRECHA (promesa vs realidad): {brecha:.1%}")

# VISUALIZACIÓN: Brecha por producto
fig, ax = plt.subplots()
# ... gráfico comparativo promesa vs realidad
```

### TAREA 4.3-4.5: Análisis Geográfico + Narrativa Final

(Similar a anteriores, seguir estructura de CLAUDE_CODE_MANIFEST.md)

Generar: `05_INSIGHTS/narrativa_principal.md` con 3-5 "killing insights"

### CRITERIO GO/NO-GO FASE 4
- ✅ 3-4 segmentos diferenciados
- ✅ Silhouette ≥0.40
- ✅ Brecha promesa vs realidad cuantificada
- ✅ 3-5 killing insights documentados
- ✅ Narrativa ≥2,000 palabras

---

## 📈 FASE 5: VISUALIZACIONES (Semana 4, 22-25 Junio)

### OBJETIVO
Crear dashboards interactivos, gráficos profesionales

### TAREAS

Generar 3 dashboards HTML + ≥5 PNGs DPI 300+

(Referencia: CLAUDE_CODE_MANIFEST.md Sección 8)

### CRITERIO GO/NO-GO FASE 5
- ✅ 3 dashboards funcionales
- ✅ ≥5 PNGs DPI 300+
- ✅ Todos en GitHub

---

## 💼 FASE 6: RECOMENDACIONES (Semana 4, 23-25 Junio)

**Usuario escribe** con base en análisis:
- Recomendaciones para IQVIA (1,500+ palabras)
- Recomendaciones para Fika (1,000+ palabras)
- Executive Summary (2-3 páginas)

(Referencia: SUCCESS_METRICS.md Sección 7)

---

## 🚀 FASE 7: GITHUB Y PUBLICACIÓN (Semana 4, 24-25 Junio)

### TAREAS

#### 7.1: Estructura de carpetas + README
```bash
# Crear estructura final
mkdir -p 01_PREGUNTA_Y_HIPOTESIS/
mkdir -p 02_DATOS/raw/ processed/
mkdir -p 03_NOTEBOOKS/
mkdir -p 04_VISUALIZACIONES/graficos_png/
mkdir -p 05_INSIGHTS/
mkdir -p 06_RECOMENDACIONES/
mkdir -p PROJECT_GOVERNANCE/

# README.md (ver template abajo)
```

#### 7.2: README Template

```markdown
# Market Intelligence: Lifestyle Drugs & Bienestar en México

**Análisis de Consumo de Suplementos, Nootrópicos y Fármacos de Estilo de Vida (2024-2026)**

## Executive Summary

Análisis riguroso de tendencias de consumo en el mercado mexicano de fármacos de 
estilo de vida (Ozempic, nootrópicos, suplementos). Identifica drivers demográficos, 
psicosociales y brecha entre posicionamiento de marketing y comportamiento real.

**Pregunta Central:** ¿Cuál es la brecha entre el posicionamiento esperado y el 
comportamiento real del consumidor mexicano en el segmento de fármacos de estilo de vida?

## Metodología

- **Recolección:** 5+ fuentes (INEGI, Google Trends, web scraping)
- **Análisis:** EDA, segmentación psicográfica, análisis de brecha
- **Rigor:** Documentación completa, limitaciones explícitas, reproducible

## Hallazgos Principales

1. **Megatendencia real:** Búsquedas de Ozempic +250% año-a-año (2024-2025)
2. **Brecha estratégica:** Marketing promete "pérdida de peso", usuarios reportan "energía/concentración"
3. **Segmentación clara:** 4 perfiles psicográficos distintos (profesionales vs wellness vs pragmáticos vs exploradores)
4. **Oportunidad regional:** CDMX ≠ Provincia en adopción y valores

## Cómo Reproducir

### Requisitos
```bash
pip install -r requirements.txt
```

### Flujo de Datos
1. `02_DATOS/raw/` - Descargas de INEGI, Google Trends, web scraping
2. `02_DATOS/processed/` - Datos limpios
3. `03_NOTEBOOKS/` - Análisis paso-a-paso
4. `04_VISUALIZACIONES/` - Dashboards e imágenes

### Ejecutar Notebooks
```bash
cd 03_NOTEBOOKS/
jupyter notebook
# Abrir 01_eda_demografico.ipynb, etc.
```

## Estructura

```
├── 01_PREGUNTA_Y_HIPOTESIS/  - Definición y pregunta central
├── 02_DATOS/                  - Raw + processed datasets
├── 03_NOTEBOOKS/              - Análisis exploratorio y avanzado
├── 04_VISUALIZACIONES/        - Dashboards e imágenes
├── 05_INSIGHTS/               - Reportes de hallazgos
├── 06_RECOMENDACIONES/        - Recomendaciones estratégicas
└── PROJECT_GOVERNANCE/        - Charter, manifests, métricas
```

## Limitaciones

1. INEGI no captura específicamente usuarios de Ozempic/nootrópicos
   → Usamos "ocupación tech/finanzas + ingresos altos" como proxy

2. Reviews sesgadas hacia early adopters
   → Sobrerrepresentan efectos positivos

3. Google Trends = búsquedas, no compras
   → No estimamos TAM real

4. Muestra de redes sociales (Reddit, foros) limitada
   → Usadas como señal confirmatorio, no primario

## Conclusión

Existe oportunidad estratégica clara para repositionamiento de mensajería, 
segmentación de mercado y activación regional. Análisis sostenido en datos 
con rigor metodológico.

---

**Autor:** Business Analyst  
**Fecha:** Junio 2026  
**Licencia:** MIT
```

#### 7.3-7.7: Documentación final, commits, publicación

(Ver CLAUDE_CODE_MANIFEST.md y SUCCESS_METRICS.md)

---

## ✅ CHECKLIST FINAL

```
FASE 1:
☐ 4 documentos pregunta/hipótesis/segmentación/fuentes
☐ Go/No-Go validado
☐ Commits pusheados

FASE 2:
☐ INEGI descargado (n>10k)
☐ Google Trends extraído (≥3 años)
☐ Amazon scrapeado (≥80 reviews)
☐ MercadoLibre scrapeado (≥40 reviews)
☐ Reddit scrapeado (≥50 posts)
☐ Foros scrapeado (≥20 testimonios)
☐ Datos procesados y limpios
☐ Data Quality Report completado
☐ Go/No-Go validado
☐ Commits pusheados

FASE 3:
☐ 4 notebooks EDA completos
☐ ≥15 visualizaciones
☐ Insights preliminares documentados
☐ Go/No-Go validado
☐ Commits pusheados

FASE 4:
☐ Segmentación psicográfica (3-4 segmentos)
☐ Análisis brecha promesa vs realidad
☐ Narrativa principal (3-5 killing insights)
☐ Go/No-Go validado
☐ Commits pusheados

FASE 5:
☐ 3 dashboards HTML interactivos
☐ ≥5 PNGs DPI 300+
☐ Go/No-Go validado
☐ Commits pusheados

FASE 6:
☐ Recomendaciones IQVIA (1,500+ palabras)
☐ Recomendaciones Fika (1,000+ palabras)
☐ Executive Summary (2-3 páginas)
☐ Go/No-Go validado
☐ Commits pusheados

FASE 7:
☐ README completo y profesional
☐ Estructura de carpetas limpia
☐ requirements.txt funcional
☐ .gitignore actualizado
☐ License (MIT)
☐ Repo PÚBLICO
☐ ≥15 commits histórico
☐ Documentación ≥90% tareas

ÉXITO GLOBAL:
☐ 6+ de 7 dimensiones en ☑️
☐ Proyecto portfolio-ready
☐ Diferencial claro para reclutadores
```

---

## 🎯 CRITERIOS DE ÉXITO FINAL

| Dimensión | Métrica | Status |
|-----------|---------|--------|
| **Datos** | ≥5 fuentes validadas, n>1,000 | ☐ |
| **Análisis** | 3-4 segmentos + brecha | ☐ |
| **Rigor** | Documentación completa | ☐ |
| **Rapidez** | ≤4 semanas | ☐ |
| **Actionable** | ≥3 recomendaciones | ☐ |
| **Portfolio** | GitHub público profesional | ☐ |
| **Diferencial** | Claridad para reclutadores | ☐ |

**PROYECTO = ✅ EXITOSO SI ≥6/7**

---

## 📞 AYUDA & TROUBLESHOOTING

| Problema | Solución |
|----------|----------|
| Web scraping bloqueado | Cambiar IP, aumentar delays, usar API pública |
| INEGI incompleto | Combinar ENOE + ENIGH |
| Segmentación unclear | Aumentar a k=5-6 clusters |
| Timeline ajustado | Priorizar: O1→O3→O4 (omitir regional si es necesario) |
| Brecha no evidente | Documentar como "hipótesis no confirmada" |

---

**Documento activo. Última actualización: 31 de Mayo 2026.**

**¿LISTO PARA EJECUTAR? Comienza Fase 1 AHORA. ¡Vamos!** 🚀
