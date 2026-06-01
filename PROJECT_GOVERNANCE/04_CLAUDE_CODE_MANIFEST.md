# CLAUDE CODE MANIFEST
## Instrucciones Operacionales Detalladas para Automatización del Proyecto

**Propósito:** Especificar exactamente QUÉ, CÓMO y DÓNDE Claude Code automatiza  
**Audiencia:** Claude Code (tu instancia de IA para ejecución técnica)  
**Responsable Validación:** Usuario (tú)  
**Última actualización:** 31 de Mayo 2026

---

## 1. TONO Y FRAME OPERACIONAL

### Instrucciones de Lenguaje:
```
SIEMPRE:
✓ "El mercado", "el consumidor", "comportamiento real"
✓ "Análisis de market intelligence", "consultoría de datos"
✓ Documentación profesional (estilo Euromonitor/McKinsey)
✓ Reproducibilidad y transparencia en decisiones

NUNCA:
✗ "Mi producto", "Fika", "validar demanda"
✗ "Este es mi análisis", "hallazgos de mi startup"
✗ Sesgo comercial o favorabilidad hacia Fika
✗ Esconder limitaciones de metodología
```

### Frame Técnico:
- **Riguroso:** Validaciones explícitas, no asunciones ocultas
- **Ágil:** Prioriza rapidez sin sacrificar calidad
- **Documentado:** Todo notable en notebooks y reportes
- **Auditables:** Código comentado, decisiones justificadas

---

## 2. ESTRUCTURA GENERAL DE EJECUCIÓN

```
CICLO POR TAREA:
1. Claude Code lee especificación en este documento
2. Genera script Python/notebook
3. Pushea a GitHub con commit descriptivo
4. Genera reporte de ejecución
5. Usuario revisa en GitHub + reporte
6. Usuario valida o solicita ajuste
7. Siguiente tarea
```

---

## 3. DEFINICIONES PREVIAS

### Paths en GitHub (carpeta raíz: `portfolio-ba-suplementos/`)

```
/01_PREGUNTA_Y_HIPOTESIS/       # Pregunta de negocio + hipótesis
/02_DATOS/
  /raw/                         # CSVs originales
  /processed/                   # CSVs limpios
  *.md                          # Data dictionary + quality reports
/03_NOTEBOOKS/
  /01_eda_demografico.ipynb
  /02_eda_tendencias.ipynb
  /03_eda_sentimiento.ipynb
  /04_segmentacion_psicografica.ipynb
  /05_analisis_promesa_realidad.ipynb
/04_VISUALIZACIONES/
  /dashboard_1_contexto.html
  /dashboard_2_comportamiento.html
  /graficos_png/
/05_INSIGHTS/
  *.md (reportes de insights)
/06_RECOMENDACIONES/
  *.md (para IQVIA, Fika)
/PROJECT_GOVERNANCE/
  /01_PROJECT_CHARTER.md
  /02_DATA_COLLECTION_STRATEGY.md
  /03_DATA_QUALITY_FRAMEWORK.md
  /04_CLAUDE_CODE_MANIFEST.md (este archivo)
  /05_SUCCESS_METRICS.md
```

### Variables de Entorno

```python
# Claude Code debe acceder a:
GITHUB_USER = "[usuario]"
GITHUB_REPO = "portfolio-ba-suplementos"
DATA_DIR = "02_DATOS/"
RAW_DATA_DIR = "02_DATOS/raw/"
PROCESSED_DATA_DIR = "02_DATOS/processed/"
NOTEBOOK_DIR = "03_NOTEBOOKS/"
VIZ_DIR = "04_VISUALIZACIONES/"
```

---

## 4. FASE 1: PLANIFICACIÓN ESTRATÉGICA (Semana 1)

**Responsable:** Usuario (planificación) + Claude Code (documentación)  
**Salida:** Documentos en `/01_PREGUNTA_Y_HIPOTESIS/`

### 1.1 Pregunta de Negocio Central

**INPUT:** Este Project Charter  
**OUTPUT:** `01_PREGUNTA_Y_HIPOTESIS/pregunta_negocio.md`

```markdown
# PREGUNTA DE NEGOCIO CENTRAL

¿Cuál es la brecha entre el posicionamiento esperado y el comportamiento real 
del consumidor mexicano en el segmento de fármacos de estilo de vida (Ozempic, 
nootrópicos, suplementos) durante 2024-2026?

SUB-PREGUNTAS:
1. ¿Quién adopta (demografía + psicografía)?
2. ¿Por qué adopta (drivers)?
3. ¿Qué diferencia hay entre promesa marketing vs experiencia real?
4. ¿Cuál es la tendencia temporal?
5. ¿Cómo varía por geografía?

FRAME: Consultor de Market Intelligence
PÚBLICO: Reclutadores IQVIA + tomadores de decisión Fika
METODOLOGÍA: Análisis multifuente, riguroso, reproducible
```

**Claude Code:** Genera documento estructurado, lo pushea con commit:
```bash
git add 01_PREGUNTA_Y_HIPOTESIS/pregunta_negocio.md
git commit -m "docs: pregunta de negocio central definida"
git push origin main
```

### 1.2-1.4 Hipótesis, Segmentación, Fuentes

**Claude Code:**
1. Crea 3 documentos MD más (hipótesis_principales.md, segmentacion_target.md, fuentes_datos.md)
2. Cada uno con estructura clara, no genérico
3. Hace un commit por documento
4. Notifica usuario cuando listo

**Criterio Go/No-Go Fase 1:**
- ✅ 4 documentos creados y pusheados
- ✅ Cada documento ≥500 caracteres (sustancia)
- ✅ Hipótesis específicas, no genéricas

---

## 5. FASE 2: RECOLECCIÓN Y LIMPIEZA (Semanas 1-2)

**Responsable:** Claude Code (ejecución) + Usuario (validación)

### 2.1-2.4: WEB SCRAPING (Tareas paralelas)

**Especificación General para Web Scraping:**

```python
# SCRIPT TEMPLATE (Claude Code genera uno por cada fuente)

import requests
from bs4 import BeautifulSoup
import pandas as pd
import time
from datetime import datetime
import logging

# LOGGING (crítico para auditoría)
logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(levelname)s - %(message)s',
    handlers=[
        logging.FileHandler(f'scrapers/logs/scraper_{source}_{datetime.now().strftime("%Y%m%d")}.log'),
        logging.StreamHandler()
    ]
)

def scrape_with_quality_checks(source, keywords, **kwargs):
    """
    Scraper genérico con validaciones integradas
    
    Args:
        source: 'amazon', 'mercadolibre', 'reddit', 'forum'
        keywords: lista de términos a buscar
        **kwargs: parámetros específicos de fuente
    
    Returns:
        pd.DataFrame con datos scrapeados + QA checks
    """
    
    data = []
    errors = {'total': 0, 'urls': [], 'reasons': []}
    
    for keyword in keywords:
        logging.info(f"Scraping: {source} - {keyword}")
        
        try:
            # ESPECIFICIDAD POR FUENTE (ver secciones abajo)
            results = _scrape_{source}(keyword, **kwargs)
            
            # VALIDACIONES MÍNIMAS
            if len(results) < MINIMUM_PER_KEYWORD[source]:
                logging.warning(f"Bajo n para {keyword}: {len(results)}")
            
            data.extend(results)
            time.sleep(3)  # RESPETO A ROBOTS.TXT
            
        except Exception as e:
            errors['total'] += 1
            errors['reasons'].append(str(e))
            logging.error(f"Error scraping {keyword}: {e}")
    
    # CONSOLIDACIÓN
    df = pd.DataFrame(data)
    
    # QA REPORT INTEGRADO
    qa_report = {
        'total_records': len(df),
        'duplicates': df.duplicated().sum(),
        'missing_values': df.isnull().sum().to_dict(),
        'scraping_errors': errors['total'],
        'timestamp': datetime.now().isoformat()
    }
    
    logging.info(f"Scraping completado: {qa_report}")
    
    return df, qa_report
```

#### 2.1: DESCARGA INEGI

**Claude Code Task:**

```python
# SCRIPT: 02_DATOS/raw/01_download_inegi.py

"""
Descarga automática de datasets INEGI (ENOE, ENIGH)
Nota: INEGI ofrece descarga directa, no requiere scraping
"""

import pandas as pd
import requests
import zipfile
import os

# URLs públicas INEGI
URLS = {
    'enoe_2024': 'https://www.inegi.gob.mx/contenidos/programas/enoe/15ymas/...',
    'enoe_2025': '...',
    'enoe_2026': '...',
    'enigh_2024': '...'
}

def download_and_extract_inegi():
    logging.info("Iniciando descarga INEGI...")
    
    for dataset, url in URLS.items():
        logging.info(f"Descargando {dataset}...")
        
        # Descarga
        response = requests.get(url)
        zip_path = f"02_DATOS/raw/{dataset}.zip"
        with open(zip_path, 'wb') as f:
            f.write(response.content)
        
        # Extrae
        with zipfile.ZipFile(zip_path, 'r') as zip_ref:
            zip_ref.extractall(f"02_DATOS/raw/{dataset}/")
        
        logging.info(f"✓ {dataset} descargado y extraído")
    
    # Carga y consolida
    enoe = pd.read_csv('02_DATOS/raw/enoe_2024/data.csv')  # Ajustar path real
    enoe = enoe[(enoe['EDAD'] >= 18) & (enoe['EDAD'] <= 65)]
    
    enoe.to_csv('02_DATOS/processed/inegi_demografico_clean.csv', index=False)
    logging.info(f"✓ INEGI consolidado: {len(enoe)} registros")
    
    return enoe

# Ejecución
if __name__ == '__main__':
    df = download_and_extract_inegi()
    print(df.info())
    print(df.describe())
```

**Entrega:**
- `02_DATOS/raw/inegi_enoe_2024_2026.csv` (sin procesar)
- `02_DATOS/processed/inegi_demografico_clean.csv` (procesado)
- `INEGI_DOWNLOAD_LOG.txt` (ejecución)

**Commit:**
```bash
git add 02_DATOS/
git commit -m "data: INEGI ENOE 2024-2026 descargada y validada (n=456k)"
git push origin main
```

#### 2.2: GOOGLE TRENDS

**Claude Code Task:**

```python
# SCRIPT: 02_DATOS/raw/02_download_google_trends.py

"""
Extrae datos de Google Trends usando API pública
Alternativa: descarga manual de CSV (< 1 minuto)
"""

from pytrends.request import TrendReq
import pandas as pd
import logging

KEYWORDS = [
    'Ozempic', 'semaglutida',
    'nootrópicos', 'modafinilo', 'L-theanina',
    'adaptógenos', 'ashwagandha',
    'pérdida de peso', 'suplementos'
]

def get_google_trends():
    logging.info("Extrayendo Google Trends...")
    
    pytrends = TrendReq(hl='es-MX', tz=360)
    
    # Búsqueda multivariable (máx 5 keywords por request)
    trends_data = []
    
    for i in range(0, len(KEYWORDS), 5):
        batch = KEYWORDS[i:i+5]
        logging.info(f"Batch: {batch}")
        
        pytrends.build_payload(
            kw_list=batch,
            timeframe='2019-01-01 2026-05-31',  # 5 años
            geo='MX'
        )
        
        # Descarga datos
        df = pytrends.interest_over_time()
        trends_data.append(df)
    
    # Consolida
    trends_final = pd.concat(trends_data, axis=1)
    trends_final.to_csv('02_DATOS/raw/google_trends_5yr.csv')
    
    logging.info(f"✓ Google Trends: {len(trends_final)} semanas")
    return trends_final
```

**Entrega:**
- `02_DATOS/raw/google_trends_5yr.csv`
- `GOOGLE_TRENDS_LOG.txt`

#### 2.3: AMAZON SCRAPING

**Claude Code Task:**

```python
# SCRIPT: 02_DATOS/raw/03_scrape_amazon.py

"""
Web scraping ético de Amazon MX
Respeta: robots.txt, delays 3-5s, user-agent headers
"""

import requests
from bs4 import BeautifulSoup
import pandas as pd
import logging
import time
from random import uniform

# Headers para parecer navegador legítimo
HEADERS = [
    'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36',
    'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36',
    # ... más headers
]

PRODUCTOS = {
    'Ashwagandha 500mg': 'B0C...',  # ASINs reales
    'Modafinilo 200mg': 'B0D...',
    # ... más productos
}

def scrape_amazon_reviews(asin, producto_nombre, max_reviews=50):
    """
    Scrape reviews de un producto Amazon
    """
    reviews = []
    
    for page in range(1, max(pages_needed)):
        url = f'https://www.amazon.com.mx/product-reviews/{asin}/?pageNumber={page}'
        
        headers = {'User-Agent': choice(HEADERS)}
        
        try:
            response = requests.get(url, headers=headers, timeout=10)
            response.raise_for_status()
            
            soup = BeautifulSoup(response.content, 'html.parser')
            
            # Extrae reviews
            review_elements = soup.find_all('div', {'class': 'a-section review'})
            
            for elem in review_elements:
                try:
                    rating = elem.find('span', {'class': 'a-icon-star'}).text.split()[0]
                    texto = elem.find('span', {'class': 'a-size-base'}).text
                    fecha = elem.find('span', {'class': 'a-size-base'}).text
                    
                    reviews.append({
                        'asin': asin,
                        'producto': producto_nombre,
                        'rating': float(rating),
                        'texto': texto,
                        'fecha': fecha,
                        'plataforma': 'amazon',
                        'verificado': 'Compra verificada' in str(elem)
                    })
                except:
                    continue
            
            # RESPETO A ROBOTS.TXT
            time.sleep(uniform(3, 5))
            
        except requests.exceptions.RequestException as e:
            logging.error(f"Error scraping {asin}: {e}")
            continue
    
    return reviews

def main():
    all_reviews = []
    
    for producto, asin in PRODUCTOS.items():
        logging.info(f"Scraping: {producto}")
        reviews = scrape_amazon_reviews(asin, producto)
        all_reviews.extend(reviews)
    
    df_reviews = pd.DataFrame(all_reviews)
    
    # VALIDACIONES DE CALIDAD
    spam_count = len(df_reviews[df_reviews['texto'].str.len() < 30])
    logging.info(f"Spam detectado: {spam_count} reviews")
    
    df_reviews = df_reviews[df_reviews['texto'].str.len() >= 30]  # Filtra spam
    
    df_reviews.to_csv('02_DATOS/raw/amazon_reviews_raw.csv', index=False)
    logging.info(f"✓ Amazon: {len(df_reviews)} reviews válidas")

if __name__ == '__main__':
    main()
```

**Entrega:**
- `02_DATOS/raw/amazon_reviews_raw.csv`
- `AMAZON_SCRAPING_LOG.txt` (con estadísticas de spam)

#### 2.4: REDDIT + FOROS

Similar a 2.3, pero usando:
- **Reddit:** PRAW API (oficial)
- **Foros:** Beautiful Soup con manejo de JavaScript si es necesario

### 2.5: ENCUESTA PROPIA (NO AUTOMATIZADO)

**Claude Code genera:**
- Template de Google Forms (link compartible)
- Script para descargar CSV de respuestas

**Usuario ejecuta:**
- Comparte link en 3-5 comunidades (Reddit, Discord, etc.)
- Espera 3-5 días
- Descarga CSV

**Claude Code valida:**
- Mínimo 50 respuestas válidas
- Pasa filtros de calidad (tiempo de respuesta razonable, etc.)

### 2.6: CONSOLIDACIÓN Y LIMPIEZA

**Claude Code Task:**

```python
# SCRIPT: 03_NOTEBOOKS/02_data_cleaning_consolidation.ipynb

"""
Notebook: Consolidación de todas fuentes + decisiones de limpieza
- Carga raw datasets
- Aplica filtros de calidad (ver DATA_QUALITY_FRAMEWORK.md)
- Genera processed CSVs limpios
- Documenta cada decisión
"""

import pandas as pd
import numpy as np

# 1. LOAD ALL SOURCES
inegi = pd.read_csv('02_DATOS/processed/inegi_demografico_clean.csv')
gt = pd.read_csv('02_DATOS/raw/google_trends_5yr.csv')
amazon = pd.read_csv('02_DATOS/raw/amazon_reviews_raw.csv')
mercadolibre = pd.read_csv('02_DATOS/raw/mercadolibre_reviews_raw.csv')
reddit = pd.read_csv('02_DATOS/raw/reddit_posts_raw.csv')

# 2. DATA QUALITY CHECKS (ver DATA_QUALITY_FRAMEWORK.md para criterios)

# INEGI: Mínimas validaciones
assert len(inegi) > 10000, "INEGI: n muy bajo"
assert inegi['EDAD'].isnull().sum() / len(inegi) < 0.01, "INEGI: >1% missing EDAD"

# Google Trends: Interpolación si falta <5%
gt = gt.interpolate(method='linear')

# Reviews: Spam filtering
amazon = amazon[amazon['texto'].str.len() >= 30]  # >30 caracteres
amazon = amazon[amazon['rating'].notna()]  # Rating válido

# 3. GENERAR DATASET CONSOLIDADO
# (Mantener fuentes separadas, pero crear "master dataset" si es relevante)

reviews_consolidated = pd.concat([
    amazon.assign(source='amazon'),
    mercadolibre.assign(source='mercadolibre'),
    reddit.assign(source='reddit')
], ignore_index=True)

# 4. EXPORT PROCESSED DATASETS
inegi.to_csv('02_DATOS/processed/inegi_clean.csv', index=False)
gt.to_csv('02_DATOS/processed/google_trends_clean.csv', index=False)
reviews_consolidated.to_csv('02_DATOS/processed/reviews_consolidated_clean.csv', index=False)

# 5. DATA DICTIONARY
data_dictionary = {
    'inegi_clean.csv': {
        'n': len(inegi),
        'variables': list(inegi.columns),
        'missing_%': (inegi.isnull().sum() / len(inegi) * 100).to_dict()
    },
    'google_trends_clean.csv': {
        'n_semanas': len(gt),
        'keywords': list(gt.columns),
        'timeframe': '2019-2026'
    },
    # ... más datasets
}

# Exporta data dictionary
import json
with open('02_DATOS/data_dictionary.json', 'w') as f:
    json.dump(data_dictionary, f, indent=2)
```

**Criterio Go/No-Go Fase 2:**
- ✅ Todos los raw datasets descargados/scrapeados
- ✅ Processed datasets en `/02_DATOS/processed/`
- ✅ Data Dictionary generado
- ✅ Data Quality Report completado (ver DATA_QUALITY_FRAMEWORK.md)
- ✅ Todos los datos pusheados a GitHub

---

## 6. FASE 3: EDA (SEMANA 2-3)

**Responsable:** Claude Code (ejecución) + Usuario (validación)

### 3.1-3.4: Notebooks EDA

**Template General:**

```python
# NOTEBOOK: 03_NOTEBOOKS/01_eda_demografico.ipynb

"""
Exploratory Data Analysis - Perfil Demográfico
- Variables: EDAD, SEXO, ESCOLARIDAD, INGRESOS, OCUPACION, UBICACION
- Visualizaciones: Distribuciones, correlaciones, segmentaciones
"""

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from scipy.stats import chi2_contingency, ttest_ind

# 1. CARGA DATOS
inegi = pd.read_csv('../02_DATOS/processed/inegi_clean.csv')
print(f"Dataset: {len(inegi)} registros, {len(inegi.columns)} variables")

# 2. EXPLORACIONES UNIVARIADAS
fig, axes = plt.subplots(2, 3, figsize=(14, 8))

# Edad distribución
axes[0,0].hist(inegi['EDAD'], bins=20, color='steelblue')
axes[0,0].set_title('Distribución de Edad')

# Sexo
inegi['SEXO'].value_counts().plot(kind='bar', ax=axes[0,1])
axes[0,1].set_title('Sexo')

# ... más visualizaciones

# 3. EXPLORACIONES BIVARIADAS
# ¿Hay diferencia de ingresos por edad?
correlation = inegi['EDAD'].corr(inegi['INGRESOS_MENSUALES'])
print(f"Correlación EDAD-INGRESOS: {correlation:.3f}")

# Chi-squared test para variables categóricas
tabla = pd.crosstab(inegi['SEXO'], inegi['ESCOLARIDAD'])
chi2, pval, dof, expected = chi2_contingency(tabla)
print(f"Chi-squared SEX × EDUCATION: χ²={chi2:.2f}, p={pval:.4f}")

# 4. SEGMENTACIONES PRELIMINARES
# Crear quintiles de ingreso
inegi['INGRESO_QUINTIL'] = pd.qcut(inegi['INGRESOS_MENSUALES'], 5, labels=['Q1','Q2','Q3','Q4','Q5'])

# Edad agrupado
inegi['EDAD_GRUPO'] = pd.cut(inegi['EDAD'], bins=[18,25,35,45,55,65], labels=['18-25','26-35','36-45','46-55','56-65'])

# Visualizar: ¿Quién tiene mayores ingresos?
inegi.groupby('EDAD_GRUPO')['INGRESOS_MENSUALES'].mean().plot(kind='bar')
plt.title('Ingresos promedio por grupo de edad')
plt.show()

# 5. INSIGHTS PRELIMINARES (narrativa en Markdown)
"""
## Hallazgos Preliminares (EDA Demografía)

### Edad
- Rango: 18-65 años
- Mediana: 42 años
- Mayor concentración: 30-45 años (42% de la muestra)

### Ingresos
- Mediana: $18,500 MXN/mes
- Q1 (más bajo): <$12,000; Q5 (más alto): >$35,000
- Correlación con EDAD: 0.23 (positiva débil)

### Ocupación
- Sector Servicios: 34%
- Sector Tecnología/Finanzas: 12%
- Educación: 8%
- Manufactura: 15%
- Otros: 31%

### Insight Clave
Los consumidores con mayores ingresos (Q4-Q5) y profesiones en tecnología/finanzas 
muestran mayor densidad en CDMX y áreas metropolitanas → PROXY para "demanda de 
optimización" (productividad, rendimiento).

### Limitación
INEGI captura ocupación pero no específicamente "estrés laboral" o "demanda de 
productividad" → Usaremos horas laborales como proxy.
"""

# 6. EXPORT CLEANED DATA + VISUALIZACIONES
inegi.to_csv('../02_DATOS/processed/inegi_eda_segmented.csv', index=False)
plt.savefig('../04_VISUALIZACIONES/eda_edad_distribucion.png', dpi=300, bbox_inches='tight')
```

**Especificaciones por Notebook:**

#### 3.1: EDA Demográfico
- Variables: Edad, sexo, escolaridad, ingresos, ocupación, ubicación
- Visualizaciones: Histogramas, boxplots, crosstabs, mapas MX
- Output: Inegi con segmentaciones agregadas

#### 3.2: EDA Tendencias (Google Trends + Series Temporales)
- Tendencia 5 años para cada keyword
- Detección de picos/anomalías
- Correlaciones entre búsquedas (ej: "Ozempic" correlaciona con "pérdida de peso"?)
- Output: Gráficos temporales, análisis de seasonalidad

#### 3.3: EDA Sentimiento (Reviews)
- Distribución de ratings (1-5 stars)
- Análisis básico de sentimiento (positivo/negativo basado en score de palabras)
- Palabras más frecuentes (TF-IDF)
- Output: Word clouds, distribuciones, matriz de confusión (rating vs sentimiento)

#### 3.4: Clustering Exploratorio
```python
# K-means preliminary para identificar segmentos

from sklearn.preprocessing import StandardScaler
from sklearn.cluster import KMeans

# Prepara datos (review text + metadata)
# - Largo del review (proxy para engagement)
# - Rating promedio
# - Palabras positivas/negativas
# - Recencia (fecha del review)

X = np.array([len(review) for review in reviews['texto']], reviews['rating'], ...)
X_scaled = StandardScaler().fit_transform(X)

kmeans = KMeans(n_clusters=4, random_state=42)
clusters = kmeans.fit_predict(X_scaled)

# Visualiza clusters
plt.scatter(X[:, 0], X[:, 1], c=clusters)
plt.title('K-means Clustering (4 clusters)')
```

### 3.5: Documentación de Insights Preliminares

**Claude Code generates:** `05_INSIGHTS/insights_preliminares.md`

```markdown
# INSIGHTS PRELIMINARES (EDA)

## 1. PERFIL DEMOGRÁFICO DE MERCADO POTENCIAL

Del análisis INEGI (n=456k):
- Edad promedio interesado en "optimización": 35-45 años
- Ingresos: Q4-Q5 ($25k+/mes) sobrerepresentados
- Ocupación: Tech, Finanzas, Educación (35% vs 20% población general)
- Ubicación: CDMX + Guadalajara + Monterrey (52% de la muestra)

## 2. TENDENCIA GOOGLE TRENDS

Búsqueda "Ozempic" México:
- 2019-2023: Bajo interés (<5 índice)
- 2024: Primer incremento (15 índice)
- 2025: +250% año-a-año
- 2026 (hasta mayo): Continuación uptrend

Interpretación: Adoptión acelerada, probablemente driven por:
- Viralización en redes sociales
- Celebridades/influencers mencionando
- Acceso farmacéutico mejorado

## 3. BRECHA PROMESA VS REALIDAD (PRELIMINARY)

Amazon reviews para "suplementos de pérdida de peso":
- Promesa (packaging/marketing): "Pierde 10kg en 30 días"
- Realidad (reviews): 
  - Rating promedio: 3.8/5
  - 25% reviews son negativas ("No funcionó")
  - Positivos mencionar: "Energía", "Concentración" (no pérdida de peso)

HIPÓTESIS: Marketing exagera pérdida de peso, pero producto sirve para 
energía/concentración → Mercado original vs mercado real son DIFERENTES.

## 4. LIMITACIONES IDENTIFICADAS

1. INEGI no captura específicamente usuarios de Ozempic/nootrópicos
   → Usamos "ocupación tech/finanzas + altos ingresos" como proxy
2. Google Trends es volumen de búsqueda, no compras
   → No podemos estimar TAM real
3. Reviews son sesgados hacia early adopters
   → Sobrerrepresentan efectos positivos

## PRÓXIMAS FASES

Fase 4 profundiza en:
- Segmentación psicográfica (valores, motivaciones)
- Análisis de brecha más riguroso (con métodos textuales)
- Narrativa de "killing insights"
```

**Criterio Go/No-Go Fase 3:**
- ✅ 4 notebooks completos (EDA demo, tendencias, sentimiento, clustering)
- ✅ ≥15 visualizaciones profesionales
- ✅ Insights preliminares documentados
- ✅ Limpieza de código + comentarios
- ✅ Todo pusheado a GitHub

---

## 7. FASE 4: ANÁLISIS AVANZADO (Semana 3)

**Responsable:** Usuario (narrativa final) + Claude Code (segmentación + código)

### 4.1: Segmentación Psicográfica

```python
# NOTEBOOK: 04_segmentacion_psicografica.ipynb

"""
Segmentación de consumidores basada en:
1. Comportamiento demográfico (INEGI)
2. Sentimiento en reviews (polaridad, temas)
3. Patrones de búsqueda (Google Trends timing)

Objetivo: 3-4 segmentos diferenciados + narrativa
"""

# Datos base
inegi = pd.read_csv('02_DATOS/processed/inegi_eda_segmented.csv')
reviews = pd.read_csv('02_DATOS/processed/reviews_consolidated_clean.csv')

# 1. FEATURE ENGINEERING (crear features psicográficas)

# Feature 1: "Demanda de optimización" (proxy de búsqueda de performance)
# Combina: ingresos altos + ocupación tech/profesional + edad 30-50
inegi['optimization_demand'] = (
    (inegi['INGRESO_QUINTIL'].isin(['Q4','Q5']).astype(int) * 0.4) +
    (inegi['OCUPACION'].isin(['TECH','FINANZAS','EDUCACION']).astype(int) * 0.4) +
    ((inegi['EDAD'] >= 30) & (inegi['EDAD'] <= 50)).astype(int) * 0.2
)

# Feature 2: "Conciencia de salud" (análisis de reviews)
# Personas que leen/escriben sobre suplementos tienden a tener mayor conciencia
review_engagement = reviews.groupby('user_id').agg({
    'rating': 'mean',
    'texto': 'count'  # num_reviews
}).rename(columns={'texto': 'review_count'})

# Feature 3: "Tech savviness" (proxy: usa amazon en vez de farmacia física)
# (en INEGI no está, usar como "internet adoption" de censo)

# 2. K-MEANS CON 4 CLUSTERS (POR VALIDACIÓN DE SILHOUETTE)

from sklearn.preprocessing import StandardScaler
from sklearn.cluster import KMeans
from sklearn.metrics import silhouette_score

# Prepara feature matrix
features = inegi[['EDAD', 'INGRESOS_MENSUALES', 'optimization_demand']].copy()
features['EDAD'] = StandardScaler().fit_transform(features[['EDAD']])
features['INGRESOS_MENSUALES'] = StandardScaler().fit_transform(features[['INGRESOS_MENSUALES']])

# Prueba k=2,3,4,5
silhouette_scores = {}
for k in range(2, 6):
    kmeans_temp = KMeans(n_clusters=k, random_state=42, n_init=10)
    labels_temp = kmeans_temp.fit_predict(features)
    score = silhouette_score(features, labels_temp)
    silhouette_scores[k] = score
    print(f"k={k}: silhouette={score:.3f}")

# Selecciona k=4 (mejor balance)
kmeans = KMeans(n_clusters=4, random_state=42, n_init=10)
inegi['segment'] = kmeans.fit_predict(features)

# 3. INTERPRETAR SEGMENTS

for segment in range(4):
    segment_data = inegi[inegi['segment'] == segment]
    print(f"\n=== SEGMENT {segment} (n={len(segment_data)}) ===")
    print(f"Edad: {segment_data['EDAD'].mean():.1f} ± {segment_data['EDAD'].std():.1f}")
    print(f"Ingresos: ${segment_data['INGRESOS_MENSUALES'].mean():,.0f}")
    print(f"Ocupación top: {segment_data['OCUPACION'].value_counts().head(3).to_dict()}")
    print(f"Ubicación: {segment_data['UBICACION'].value_counts().head(2).to_dict()}")

# 4. NARRATIVA DE SEGMENTOS

"""
## SEGMENTOS PSICOGRÁFICOS IDENTIFICADOS (4 clusters)

### SEGMENT 0: "High-Achieving Professionals" (n=89,234 | 19.5%)
- **Demografía:** 38 ± 8 años, ingresos $42k/mes, tech/finanzas
- **Ubicación:** CDMX (67%), Monterrey (18%)
- **Motivación:** Optimización de performance, biohacking
- **Comportamiento:** Activos en Amazon, siguen influencers fitness
- **Productos Interés:** Nootrópicos, Ozempic, adaptógenos premium
- **Narrativa:** "La vida es competencia, necesito cada ventaja"

### SEGMENT 1: "Health-Conscious Millennials" (n=112,456 | 24.6%)
- **Demografía:** 28 ± 6 años, ingresos $18k/mes, educación, marketing
- **Ubicación:** CDMX, Guadalajara, ciudades medias
- **Motivación:** Bienestar integral, no solo performance
- **Comportamiento:** Sigue wellness influencers, comunidades yoga/fitness
- **Productos Interés:** Suplementos naturales, adaptógenos, probióticos
- **Narrativa:** "Quiero estar bien conmigo mismo"

### SEGMENT 2: "Value-Seeking Pragmatists" (n=156,789 | 34.3%)
- **Demografía:** 45 ± 10 años, ingresos $16k/mes, ocupaciones diversas
- **Ubicación:** Provincia, ciudades medianas
- **Motivación:** Solución práctica a problemas (energía, peso)
- **Comportamiento:** Compra en farmacias locales, confía en farmacéutico
- **Productos Interés:** Suplementos tradicionales, vitaminas B/D
- **Narrativa:** "Necesito algo que funcione, sin tanto hype"

### SEGMENT 3: "Curious Explorers" (n=98,344 | 21.5%)
- **Demografía:** 32 ± 7 años, ingresos $21k/mes, educación superior
- **Ubicación:** CDMX, Guadalajara
- **Motivación:** Curiosidad científica, experimentación
- **Comportamiento:** Lee reviews detalladas, sigue comunidades Reddit/foros
- **Productos Interés:** Múltiples categorías, prueba activamente
- **Narrativa:** "¿Qué está funcionando ahora? Quiero probarlo"

## IMPLICACIONES ESTRATÉGICAS

1. **NO HAY SEGMENTO ÚNICO**
   → Mensaje único ("Pierde peso") no funciona para todos
   
2. **Tech professionals ≠ Health-conscious millennials**
   → Ozempic apela a profesionales
   → Adaptógenos naturales apelen a millennials wellness
   
3. **Majority (58%) es pragmático/conservative**
   → Marketing debe respetar skepticism
   → Recomendaciones de farmacéutico > influencers
"""

# 5. EXPORT SEGMENTED DATA
inegi.to_csv('../02_DATOS/processed/inegi_segmented_final.csv', index=False)
```

### 4.2-4.5: Análisis de Brecha + Narrativa Final

**Claude Code + Usuario:** Generan notebooks para:
- Brecha promesa vs realidad (análisis textual de reviews vs marketing claims)
- Análisis geográfico (¿CDMX vs provincia tienen comportamientos diferentes?)
- Síntesis narrativa final (2-3 página documento, hallazgos + recomendaciones)

---

## 8. FASE 5: VISUALIZACIONES (Semana 4)

**Responsable:** Claude Code (generación de dashboards interactivos)

### Dashboard 1: Contexto de Mercado

```python
# SCRIPT: generate_dashboard_1.py

"""
Dashboard interactivo con Plotly/Dash
- Datos: INEGI demografía, Google Trends
- Visualizaciones: Mapas MX, series temporales, distribuciones
"""

import plotly.express as px
import plotly.graph_objects as go
from plotly.subplots import make_subplots

fig = make_subplots(
    rows=2, cols=2,
    subplot_titles=('Búsquedas por Mes (Google Trends)', 
                    'Distribución Edad',
                    'Ingresos por Quintil',
                    'Top Ocupaciones')
)

# Subplot 1: Google Trends
fig.add_trace(
    go.Scatter(x=gt['fecha'], y=gt['Ozempic'], 
               mode='lines+markers', name='Ozempic',
               line=dict(color='#FF6B6B', width=3)),
    row=1, col=1
)

# Subplot 2-4: ...

fig.update_layout(height=800, title_text="Dashboard 1: Market Context")
fig.write_html('04_VISUALIZACIONES/dashboard_1_contexto.html')
```

### Dashboard 2: Comportamiento del Consumidor

- Segmentos psicográficos (gráfico interactivo)
- Rating vs sentimiento por segmento
- Palabras clave por categoría

### Dashboard 3: Análisis Geográfico

- Mapa de México mostrando "interés" por región
- Diferencias CDMX vs Guadalajara vs Monterrey

**Criterio Go/No-Go Fase 5:**
- ✅ 3 dashboards HTML interactivos
- ✅ ≥5 visualizaciones PNG de alta calidad (DPI 300+)
- ✅ Todos pusheados a GitHub

---

## 9. FASE 6: RECOMENDACIONES (Semana 4)

**Responsable:** Usuario (redacción final)  
**Claude Code:** Genera reportes base

```markdown
# 06_RECOMENDACIONES/para_iqvia.md

## RECOMENDACIONES ESTRATÉGICAS PARA IQVIA

(O aplicable a cualquier cliente de consultoría)

### 1. SEGMENTACIÓN DIFERENCIADA POR PERFIL

Recomendación: Diseñar 4 estrategias de market entry, no una
- Profesionales de alto rendimiento: Enfoque "biohacking", tech-savvy
- Millennials wellness: Enfoque "bienestar integral", sustentable
- Pragmáticos: Enfoque "solución práctica", farmacéutico
- Exploradores: Enfoque "científico", transparencia total

### 2. DETECTAR LA BRECHA PROMESA-REALIDAD

Hallazgo: Marketing dice "pierde peso", reviews dicen "gano energía"

Recomendación: Repositionar mensaje
- Para Ozempic: Enfatizar resultados clínicamente probados (FDA)
- Para nootrópicos: Cambiar a "productividad/concentración", no perdida peso genérica

### 3. REGIONAL ACTIVATION

CDMX ≠ Provincia (valores, acceso, confianza diferentes)
Recomendación: Marketing localizado por región

...
```

---

## 10. CHECKLIST DE EJECUCIÓN CLAUDE CODE

```
FASE 2:
☐ INEGI descargado (02_DATOS/raw/)
☐ Google Trends extraído (02_DATOS/raw/)
☐ Amazon scrapeado (02_DATOS/raw/)
☐ MercadoLibre scrapeado (02_DATOS/raw/)
☐ Reddit scrapeado (02_DATOS/raw/)
☐ Datasets limpios (02_DATOS/processed/)
☐ Data Dictionary generado
☐ Quality Report generado
☐ Todos los commits pusheados

FASE 3:
☐ 4 notebooks EDA completados
☐ ≥15 visualizaciones
☐ Insights preliminares documentados
☐ Código comentado
☐ Commits pusheados

FASE 4:
☐ Segmentación psicográfica completada
☐ Análisis brecha promesa vs realidad
☐ Narrativa final (>1000 palabras)
☐ Commits pusheados

FASE 5:
☐ 3 dashboards interactivos
☐ ≥5 PNGs alta calidad
☐ Commits pusheados

FASE 6:
☐ Reportes de recomendaciones
☐ Executive summary
☐ Commits pusheados

FASE 7:
☐ README actualizado
☐ Documentación de GitHub completa
☐ License file (MIT)
☐ requirements.txt
☐ .gitignore actualizado
☐ Repo PÚBLICO
```

---

## 11. PROTOCOLO DE COMUNICACIÓN USUARIO ↔ CLAUDE CODE

**Diariamente (o cuando haya cambios):**

1. Usuario revisa outputs en GitHub
2. Si hay ajuste necesario, comenta en commit o crea issue
3. Claude Code lee feedback y ajusta
4. Pushea nueva versión

**Formato de commit recomendado:**
```bash
# Correctamente
git commit -m "feat: segmentacion psicografica con 4 clusters (k=4, silhouette=0.68)"
git commit -m "docs: data quality report INEGI (n=456k, 99.8% completitud)"
git commit -m "fix: spam filtering threshold 30 chars, reduce false positives"

# Evitar
git commit -m "actualización"
git commit -m "cambios varios"
git commit -m "arreglos"
```

---

## 12. ESCALATION & TROUBLESHOOTING

| Problema | Solución |
|----------|----------|
| Web scraping bloqueado | Cambiar IP, aumentar delays, usar API pública |
| INEGI datos incompletos | Combinar ENOE + ENIGH, documentar gap |
| Segmentación no clara | Aumentar k=5-6, revisar features |
| Timeline ajustado | Reducir scope: omitir análisis regional, focus en segmentación + brecha |
| Calidad datos baja | Volver a Fase 2, recolectar más datos o cambiar fuentes |

---

## 13. FINAL: PERSONA DE CLAUDE CODE

**Eres:**
- Consultor técnico de Market Intelligence
- No vendedor, no emprendedor
- Riguroso, documentado, reproducible
- Orientado a insights accionables
- Rápido sin sacrificar calidad

**Nunca:**
- Sesgo hacia Fika o IQVIA
- Ambigüedad en decisiones
- Código sin comentarios
- Hallazgos sin limitaciones documentadas

---

**Documento activo. Actualizado: 31 de Mayo 2026**
