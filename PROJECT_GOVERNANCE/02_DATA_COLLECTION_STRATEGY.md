# DATA COLLECTION STRATEGY
## Recolección Internet-First + Web Scraping para Market Intelligence

**Propósito:** Detallar cómo recopilar datos validados en <2 semanas sin encuestas formales  
**Timeline:** Semana 1 (planificación) + Semana 2 (ejecución)  
**Responsable:** Claude Code (ejecución) + Usuario (validación)

---

## 1. PRINCIPIOS RECTORES

### ✅ LO QUE SÍ HACEMOS:
- ✓ Datos públicos (INEGI, Google Trends, datos abiertos)
- ✓ Web scraping ético (respetando robots.txt, terms of service)
- ✓ Opiniones reales de consumidores (Amazon, MercadoLibre, Reddit, Trustpilot)
- ✓ Análisis de tendencias históricas (5 años)
- ✓ Documentación transparente de fuentes y limitaciones

### ❌ LO QUE NO HACEMOS:
- ❌ Encuestas formales (Survey Monkey, Typeform, Google Forms)
- ❌ Entrevistas en profundidad (ahorra 3 semanas)
- ❌ Trabajo de campo presencial
- ❌ Web scraping de datos protegidos o privados

---

## 2. MATRIZ DE FUENTES DE DATOS

| # | Fuente | Tipo | Datos | Método | Tiempo | Confiabilidad |
|---|--------|------|-------|--------|--------|-----------------|
| **F1** | INEGI (ENOE, ENIGH) | Público | Demográfica, socioeconómica | Descarga directa | 3-4 horas | ⭐⭐⭐⭐⭐ |
| **F2** | Google Trends | Público | Búsquedas, tendencias 5 años | API / descarga manual | 2-3 horas | ⭐⭐⭐⭐ |
| **F3** | Amazon MX | Web scraping | Reviews, ratings, productos | Beautiful Soup | 4-6 horas | ⭐⭐⭐⭐ |
| **F4** | MercadoLibre MX | Web scraping | Precios, comentarios, vendedores | Beautiful Soup/Selenium | 4-6 horas | ⭐⭐⭐⭐ |
| **F5** | Reddit (r/mexico, r/fitness, r/nootropics) | Web scraping | Opiniones auténticas, uso real | PRAW API / Beautiful Soup | 3-4 horas | ⭐⭐⭐ |
| **F6** | Foros de salud / bienestar | Web scraping | Testimonios, experiencias | Beautiful Soup | 2-3 horas | ⭐⭐⭐ |

**Total de tiempo de recolección: ~25-30 horas de ejecución Claude Code**

---

## 3. FUENTE-A-FUENTE: ESPECIFICACIONES

### **F1: INEGI (DATOS DEMOGRÁFICOS & SOCIOECONÓMICOS)**

**¿Qué es?**
- ENOE (Encuesta Nacional de Ocupación y Empleo)
- ENIGH (Encuesta Nacional de Ingresos y Gastos de los Hogares)

**¿Qué información captura?**
- Perfil demográfico por edad, género, educación, ingresos
- Ocupación, sector, horas laborales (proxy para "demanda de productividad")
- Gastos en salud y bienestar
- Ubicación geográfica (CDMX, Monterrey, Guadalajara, provincia)

**Cómo obtener:**
1. Ir a `inegi.gob.mx/programas/enoe/15ymas/default.html`
2. Descargar últimos 2 años (2024-2026)
3. Filtrar variables relevantes (edad, escolaridad, ingresos, ocupación)

**Variables clave a extraer:**
```
Demográfica:
- EDAD (18-65)
- SEXO (M/F)
- ESCOLARIDAD (primaria, secundaria, bachillerato, licenciatura, posgrado)
- ESTADO_CIVIL

Socioeconómica:
- INGRESOS_MENSUALES (en bandas)
- SECTOR_OCUPACION (tecnología, finanzas, salud, educación, retail)
- HORAS_SEMANALES (proxy para stress/demanda de productividad)

Geográfica:
- ENTIDAD_FEDERATIVA (CDMX, Jalisco, Nuevo León, etc.)
- TAMAÑO_LOCALIDAD (metropolitana, urbana, rural)

Salud:
- GASTOS_SALUD_ANUAL (si está disponible)
- AFILIACION_SEGURO (privado vs público)
```

**Tamaño esperado:** n=100,000+ registros (muestra ENOE oficial)  
**Calidad:** ⭐⭐⭐⭐⭐ (institución gubernamental, rigurosa)  
**Limitaciones:** No captura específicamente "usuarios de Ozempic/nootrópicos"

---

### **F2: GOOGLE TRENDS (BÚSQUEDAS E INTERÉS)**

**¿Qué es?**
- Agregación anónima de búsquedas en Google
- Datos históricos 5 años, por país/región

**¿Qué información captura?**
- Volumen relativo de búsquedas: "Ozempic", "semaglutida", "nootrópicos", "modafinilo"
- Tendencia temporal (crecimiento, estacionalidad, picos)
- Búsquedas relacionadas (co-ocurrencia de términos)
- Interés por región en México

**Cómo obtener:**
1. Ir a `trends.google.com`
2. Buscar keywords:
   ```
   Primarias:
   - "Ozempic"
   - "semaglutida"
   - "nootrópicos"
   - "modafinilo"
   - "suplementos de concentración"
   - "pérdida de peso"
   
   Secundarias (comparativas):
   - "vitaminas"
   - "fitness"
   - "bienestar"
   - "salud mental"
   ```
3. Filtrar: Últimos 5 años, México, todas las categorías
4. Exportar CSV

**Análisis esperado:**
- ¿Crecimiento de Ozempic vs otros?
- ¿Estacionalidad (enero = New Year's Resolution)?
- ¿Distribución geográfica intra-México?
- ¿Búsquedas relacionadas = intent signals?

**Tamaño esperado:** ~50-100 data points (uno por semana x 5 años)  
**Calidad:** ⭐⭐⭐⭐ (oficial Google, índices relativos)  
**Limitaciones:** Solo búsquedas, no compras; no captura dark web o farmacéuticas privadas

---

### **F3: AMAZON MÉXICO (REVIEWS & PRODUCTOS)**

**¿Qué es?**
- E-commerce con reviews públicos de suplementos
- Datos: nombre producto, precio, rating, # reviews, texto de reviews

**Productos clave a scrapear:**

```python
PRODUCTOS_TARGET = {
    'Ozempic/GLP-1': [
        'semaglutida inyectable',
        'tirzepatida',
        'analogos GLP-1'
    ],
    'Nootrópicos': [
        'modafinilo',
        'l-theanina',
        'cafeína + L-theanina',
        'alpha-GPC'
    ],
    'Adaptógenos': [
        'ashwagandha',
        'rhodiola',
        'magnesio',
        'maca'
    ],
    'Pérdida de Peso': [
        'garcinia cambogia',
        'CLA',
        'conjugated linoleic acid'
    ]
}
```

**Cómo scrapear:**
1. Usar `Beautiful Soup` para parsear HTML
2. Extraer por producto:
   - ASIN (product ID)
   - Título
   - Precio actual
   - Rating promedio (1-5 stars)
   - Número total de reviews
   - Primeros 20-30 reviews (texto, rating individual, fecha)

3. Delays entre requests (3-5 segundos) para respetar robots.txt
4. User-agent rotante para evitar bloqueos

**Variables a capturar:**
```python
{
    'product_id': 'ASIN',
    'titulo': 'Ashwagandha 500mg 60 cápsulas',
    'precio_mxn': 299.90,
    'rating_promedio': 4.2,
    'num_reviews_total': 1243,
    'categoria': 'Adaptógeno',
    'reviews': [
        {
            'texto': 'Excelente producto, me ayudó...',
            'rating': 5,
            'fecha': '2026-05-15',
            'verificado_compra': True
        },
        ...
    ]
}
```

**Tamaño esperado:** 5-7 productos × 20-30 reviews = ~150-200 reviews  
**Calidad:** ⭐⭐⭐⭐ (reviews reales de clientes)  
**Limitaciones:** Sesgo hacia early adopters / tech-savvy; no captura farmacéuticas privadas

---

### **F4: MERCADOLIBRE MÉXICO (ALTERNATIVA + VALIDACIÓN)**

**¿Qué es?**
- Marketplace masivo en Latinoamérica
- Datos: productos, precios, calificaciones, comentarios

**Productos:** Mismo listado que F3

**Cómo scrapear:**
1. Usar Selenium (si necesita JavaScript) o Beautiful Soup
2. Extraer:
   - ID de producto
   - Título
   - Precio
   - Calificación del vendedor
   - Número de vendedores
   - Comentarios públicos

3. Comparar precios vs Amazon (validación de brecha geográfica)

**Variables:**
```python
{
    'product_id': 'MLM...',
    'titulo': 'Nootrópicos Stack - Modafinilo + L-Theanina',
    'precio_mxn': 450,
    'calificacion_vendedor': 4.8,
    'num_vendedores': 5,
    'comentarios': [...]
}
```

**Tamaño esperado:** 5-7 productos × 15-20 comentarios = ~100-150 comentarios  
**Calidad:** ⭐⭐⭐⭐ (real marketplace, refleja precio local)  
**Limitaciones:** Menos reviews que Amazon; algunas productos no disponibles

---

### **F5: REDDIT (OPINIONES AUTÉNTICAS)**

**¿Qué es?**
- Plataforma de comunidades anónimas
- Datos: posts, comentarios, upvotes (proxy para relevancia)

**Subreddits objetivo:**
```
Primarios:
- r/mexico (65M+ personas, discusiones de consumo)
- r/fitness_mexico (comunidad fitness)
- r/Nootropics (global, pero usuarios MX)

Secundarios:
- r/HealthyFood
- r/mentalhealth
- r/productivity
```

**Cómo scrapear:**
1. Usar PRAW (Python Reddit API Wrapper)
   - Requiere credenciales OAuth (gratuito)
   - Legal y soportado por Reddit

2. Buscar keywords:
   ```
   "Ozempic", "semaglutida", "modafinilo", 
   "nootrópicos", "suplementos", "pérdida de peso"
   ```

3. Extraer:
   - ID del post
   - Título
   - Autor
   - Subreddit
   - Upvotes (relevancia)
   - Texto completo
   - Comentarios top (primeros 5-10)

**Variables:**
```python
{
    'post_id': 't3_xxxxx',
    'titulo': '¿Alguien en México ha usado Ozempic sin diabetes?',
    'subreddit': 'mexico',
    'upvotes': 342,
    'num_comentarios': 89,
    'texto': 'Estoy considerando usar Ozempic para bajar...',
    'comentarios': [
        {
            'texto': 'Yo llevo 3 meses y he bajado 8kg...',
            'upvotes': 156
        }
    ]
}
```

**Tamaño esperado:** 50-100 posts × 5 comentarios = ~300-500 comentarios auténticos  
**Calidad:** ⭐⭐⭐⭐⭐ (anónimo, auténtico, sin sesgo comercial)  
**Limitaciones:** Puede haber desinformación; muestra self-selected; requiere lectura atenta

---

### **F6: FOROS DE SALUD & BIENESTAR**

**¿Qué es?**
- Foros especializados (médicos, farmacéuticos, wellness)
- Datos: experiencias de usuarios, testimonios

**Sitios objetivo:**
```
Españoles/Mexicanos:
- forosperu.net (sección suplementos)
- foroactivo.com (comunidades de salud)
- reddit.com/r/mexico (ya cubierto)
- tusud.com (testimonios de salud)

Globales (en español):
- forum.xtendida.es
- comunidad.ivoox.com
```

**Cómo scrapear:**
1. Identificar threads sobre: Ozempic, nootrópicos, suplementos
2. Usar Beautiful Soup para extraer:
   - Título del thread
   - Posts/respuestas
   - Fecha
   - Autor (seudónimo)
   - "Likes" o "relevancia"

3. Consolidar en CSV

**Tamaño esperado:** 20-40 threads × 3-5 comentarios = ~100-200 testimonios  
**Calidad:** ⭐⭐⭐ (muy específicos pero baja n)  
**Limitaciones:** Pequeña muestra; sesgo hacia problemas/efectos secundarios

---

## 4. CRONOGRAMA DE RECOLECCIÓN (2 SEMANAS)

```
SEMANA 1 (2-4 Junio):

Lunes 2 Jun:
- ✓ Descarga INEGI (2-3 horas)
- ✓ Google Trends (2-3 horas)
- ✓ Setup web scraping (scripts base)

Miércoles 4 Jun:
- ✓ Web scraping Amazon (ejecución, 4-6 horas)
- ✓ Validación de datos

SEMANA 2 (5-11 Junio):

Lunes 5 Jun:
- ✓ Web scraping MercadoLibre (4-6 horas)
- ✓ Reddit + PRAW (3-4 horas)

Miércoles 7 Jun:
- ✓ Foros de salud (2-3 horas)
- ✓ Consolidación inicial

Viernes 9 Jun:
- ✓ Validación de calidad
- ✓ Data dictionary completado
```

---

## 5. TAMAÑO MÍNIMO DE MUESTRA RECOMENDADO

### Por Fuente:

| Fuente | Mínimo Absoluto | Óptimo | Lógica |
|--------|-----------------|--------|--------|
| **INEGI** | 10,000 registros | 100,000+ | Datos poblacionales, n grande válida |
| **Google Trends** | 52 semanas (1 año) | 260 semanas (5 años) | Detección de tendencias |
| **Amazon Reviews** | 50 reviews | 200+ reviews | Saturation point ~100 |
| **MercadoLibre** | 30 reviews | 100+ reviews | Validación regional |
| **Reddit Posts** | 30 posts + comentarios | 100+ posts | Discusiones auténticas |
| **Foros** | 20 testimonios | 50+ testimonios | Complemento, no primario |

### CRITERIO GO/NO-GO:

✅ **GO** si:
- INEGI: n > 5,000 registros ✓
- Google Trends: 3+ años de historia ✓
- Amazon + MercadoLibre: 100+ reviews combinados ✓
- Reddit: 50+ posts relevantes ✓

❌ **NO-GO** si:
- Cualquier fuente primaria con <30% del mínimo
- Más de 40% de datos con missing values críticos

---

## 6. DECISIONES DE LIMPIEZA PRELIMINAR

**Usuario toma decisiones finales, Claude Code propone:**

### INEGI:
```
Decisión 1: ¿Edades? → Filtrar 18-65 (población económicamente activa)
Decisión 2: ¿Missing? → Si >10% en variable, documentar y dropear variable
Decisión 3: ¿Outliers? → Conservar (p.ej. ingresos extremos pueden ser válidos)
```

### Google Trends:
```
Decisión 1: ¿Datos faltantes? → Si <2% omisiones por semana, interpolar
Decisión 2: ¿Normalización? → Escala 0-100 (estándar Google Trends)
```

### Reviews (Amazon, MercadoLibre, Reddit):
```
Decisión 1: Idioma → Solo en español (excluir posts en inglés)
Decisión 2: Spam → Eliminar reviews obvios de bots (texto genérico repetido)
Decisión 3: Relevancia → Si rating ≤2 y <50 palabras → revisar manualmente
Decisión 4: Deduplicación → Si mismo review en múltiples URLs, conservar 1 copia
```

---

## 7. DATA DICTIONARY ESPERADO

**Salida de esta fase (Fin Semana 2):**

```markdown
# DATA DICTIONARY

## DATASET: demographic_inegi.csv
- EDAD: int (18-65)
- SEXO: categorical (M/F)
- INGRESOS_MENSUALES: int (pesos MX)
- ... (20+ variables)

## DATASET: google_trends.csv
- FECHA: date (YYYY-MM-DD)
- OZEMPIC: int (0-100 index)
- NOTROPICOS: int (0-100 index)
- ... (10+ términos)

## DATASET: reviews_consolidated.csv
- PRODUCTO_ID: str
- TITULO_PRODUCTO: str
- PLATAFORMA: categorical (amazon, mercadolibre, reddit)
- RATING: int (1-5)
- TEXTO_REVIEW: str
- FECHA: date
- ... (15+ variables)
```

---

## 8. DOCUMENTACIÓN A GENERAR

Al fin de la recolección, Claude Code genera:

```
📁 02_DATOS/
├── DATA_COLLECTION_REPORT.md
│   ├── Descripción de cada fuente
│   ├── Metodología de scraping
│   ├── Tamaño final n
│   ├── Limitaciones identificadas
│   └── URLs + scripts usados
│
├── DATA_DICTIONARY.md
│   ├── Variable-by-variable
│   ├── Tipos de dato
│   ├── Rangos y distribuciones
│   └── Missing data patterns
│
├── raw/
│   ├── inegi_enoe_2024_2026.csv
│   ├── google_trends_5yr.csv
│   ├── amazon_reviews.csv
│   ├── mercadolibre_reviews.csv
│   ├── reddit_posts.csv
│   └── foros_testimonios.csv
│
└── DATA_QUALITY_REPORT.md (Fase 2)
```

---

## 9. MITIGACIÓN DE RIESGOS DE RECOLECCIÓN

| Riesgo | Probabilidad | Mitigación |
|--------|--------------|-----------|
| INEGI no disponible | Baja | Usar ENOE + ENIGH en paralelo |
| Amazon/MercadoLibre bloquean scraping | Media | Delays 3-5 seg, user-agent rotante, cambiar IPs si necesario |
| Reddit datos limitados | Media | Expandir a foros especializados de salud |
| Google Trends sin histórico completo | Baja | Usar 3 años si 5 no disponibles |
| Reviews spam/fake | Media | Verificación manual de 10% muestral |

---

## 10. AUTORIZACIÓN Y PRÓXIMOS PASOS

**Responsable ejecución:** Claude Code (semana 1-2)  
**Validador:** Usuario (diario)  
**Entregable:** Data Dictionary + Datasets limpios en `02_DATOS/`

**Siguiente documento:** DATA_QUALITY_FRAMEWORK.md (cómo validar)

---

**Documento activo. Próxima revisión: Semana 1 (4 Jun)**
