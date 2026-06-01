# DATA QUALITY FRAMEWORK
## Validación Rigurosa de Datos por Fuente (Sin Ambigüedad)

**Propósito:** Establecer estándares de calidad no negociables, por fuente  
**Responsable:** Claude Code (ejecución) + Usuario (decisión final)  
**Timeline:** Paralelo a recolección + Fase 2 (limpieza)

---

## 1. PRINCIPIO RECTOR: CONFIANZA DIFERENCIADA

```
⭐⭐⭐⭐⭐ ALTO (INEGI)
  → Aceptamos datos como-están
  → Mínimas validaciones
  
⭐⭐⭐⭐ MEDIO (Google Trends, Amazon Reviews)
  → Validaciones estándar
  → Limpieza visible
  
⭐⭐⭐ BAJO (Reddit, Foros)
  → Validaciones exhaustivas
  → Documentar limitaciones
  
⭐⭐ MUY BAJO (Social media anónima, comentarios sin verificación)
  → NO USAR o usar solo como "señal complementaria"
```

**Tu intuición fue correcta:** INEGI confías ciegamente, redes sociales menos.

---

## 2. MATRIZ DE VALIDACIÓN POR FUENTE

### **FUENTE 1: INEGI (⭐⭐⭐⭐⭐ ALTO)**

#### 2.1.1 Validaciones Mínimas

| Validación | Criterio GO | Criterio NO-GO | Acción |
|------------|------------|----------------|--------|
| **Completitud** | ≥99% registros con todas variables | <95% | Investigar qué falta, contactar INEGI si crítico |
| **Fechas** | Datos 2024-2026 | Datos <2023 | Descargar versión más reciente |
| **Estructura** | n > 50,000 registros | n < 10,000 | Combinar ENOE + ENIGH |
| **Tipo de dato** | Edad = int, Ingresos = numeric | Conversión fallida | Revisar descarga |

#### 2.1.2 Procesamiento
```python
# Validación INEGI (Claude Code):
1. Cargar CSV
2. Verificar shape: (n_rows, n_cols)
3. dtypes por columna → int, float, object
4. Missing data por columna: % de NaNs
5. Si cualquier variable >5% missing → FLAG para usuario
6. Estadísticas descriptivas (min, max, mean, median)
7. Generar reporte de 1 página
```

#### 2.1.3 Documentación de Salida
```markdown
# INEGI Data Quality Report

✓ Completitud: 99.8% (1 variable con 0.2% missings)
✓ Timeframe: ENOE 2024-2026 (3 años)
✓ N = 456,823 registros (ENOE 2024-2026 combined)
✓ Variables clave: EDAD, SEXO, ESCOLARIDAD, INGRESOS, OCUPACION, ENTIDAD

⚠ Notas:
- ENIGH no incluye detalle de ocupación (usar solo ENOE para eso)
- Ingresos reportados en bandas, no montos exactos
```

---

### **FUENTE 2: GOOGLE TRENDS (⭐⭐⭐⭐ MEDIO-ALTO)**

#### 2.2.1 Validaciones Obligatorias

| Validación | Criterio GO | Criterio NO-GO | Acción |
|------------|------------|----------------|--------|
| **Timeframe** | ≥156 semanas (3 años) | <52 semanas (1 año) | Proceder con 3 años si 5 no disponible |
| **Keywords** | ≥8 términos clave | <5 términos | Buscar términos relacionados |
| **Completitud** | ≥95% semanas con dato | <85% (gaps >2 meses) | Interpolar datos faltantes |
| **Índice 0-100** | Valores numéricos en rango | Valores fuera rango | Reescalar o rechazar |
| **Co-ocurrencia** | ≥3 búsquedas relacionadas por término | 0 búsquedas relacionadas | Revisar manualmente relevancia |

#### 2.2.2 Procesamiento
```python
# Validación Google Trends (Claude Code):
1. Cargar CSV exportado de trends.google.com
2. Verificar que rango temporal = 5 años (260 semanas)
3. Para CADA keyword:
   a. Contar % semanas con NaN
   b. Si >5% NaN → Interpolar lineal (fill method)
   c. Verificar rango 0-100
4. Detectar picos anómalos (>50% cambio semana-a-semana)
   - Si existe, anotar como "evento" (ej: FDA approval, noticia viral)
5. Validar tendencia general (creciente vs decreciente)
6. Generar gráfico temporal por keyword
```

#### 2.2.3 Validación de Interpretación
```markdown
✓ Trend: "Ozempic" muestra crecimiento 150% (2024-2026)
✓ Seasonality: Pico enero (New Year's Resolution)
⚠ Anomaly: Pico inusual mayo-2025 → Posible noticia = investigar
✓ Co-searches: "pérdida de peso", "diabetes", "GLP-1" correlacionan
```

#### 2.2.4 Documentación de Salida
```
GOOGLE_TRENDS_QUALITY_REPORT:
✓ Completitud: 98.2% (2 semanas faltando de 260)
✓ Interpolación aplicada: Lineal (impacto mínimo)
✓ Picos anotados: 3 anomalías investigadas
✓ Rango validado: 0-100 en todas variables
```

---

### **FUENTE 3: AMAZON REVIEWS (⭐⭐⭐⭐ MEDIO)**

#### 2.3.1 Validaciones Obligatorias

| Validación | Criterio GO | Criterio NO-GO | Acción |
|------------|------------|----------------|--------|
| **N por producto** | ≥15 reviews/producto | <10 reviews | Expandir búsqueda o excluir producto |
| **Rating dist.** | ≥1 review de cada 1-5 stars | Solo ratings 4-5 (sesgo) | Revisar si es producto legítimo |
| **Texto length** | Mediana ≥50 caracteres | Mediana <30 chars (bots) | Flag para revisión manual |
| **Spam detection** | ≤5% reviews probablemente fake | >10% spam score | Excluir producto |
| **Fecha rango** | Reviews últimos 24 meses | Solo reviews antiguas >2 años | Revisar popularidad actual |
| **Verificación compra** | ≥60% "Compra verificada" | <40% verificadas | Flag pero no excluir |

#### 2.3.2 Spam Detection Algorithm
```python
# Detección de reviews fake (Claude Code):
SPAM_FLAGS = {
    'texto_generico': texto en ['Excelente', 'Muy bueno', 'Recomendado'] y len(texto) < 50,
    'emoji_excesivo': texto.count(['😍','⭐','✅']) > 5,
    'repeticion': mismo_texto_en_multiples_reviews == True,
    'sintaxis_rota': texto.count('..') > 2 or caracteres_raros > 20%,
    'link_externo': 'http' in texto or 'whatsapp' in texto.lower()
}

spam_score = sum(SPAM_FLAGS.values()) / len(SPAM_FLAGS)
if spam_score > 0.3:
    FLAG_REVIEW('Posible spam')
```

#### 2.3.3 Validación de Sentimiento
```python
# Validación coherencia rating vs texto:
COHERENCIA_CHECK = {
    'rating_5_stars': Análisis de sentimiento texto >= 0.7 (positivo),
    'rating_1_2_stars': Análisis de sentimiento texto <= 0.3 (negativo),
}

if coherencia < 0.8:
    FLAG_REVIEW('Incoherencia rating-texto: investigar')
```

#### 2.3.4 Documentación de Salida
```markdown
# AMAZON REVIEWS QUALITY REPORT

**Productos analizados:** 7
**Reviews totales:** 487
**Reviews después de limpieza:** 451 (92.6%)

Por producto:
- Ashwagandha 500mg: 68 reviews, rating promedio 4.3 ✓
- Modafinilo: 72 reviews, rating promedio 4.5 ✓
- Ozempic (genérico): 45 reviews, rating promedio 3.8 ⚠ (baja, investigar)

Spam detected & removed:
- 28 reviews con <30 caracteres
- 8 reviews con spam flags >0.3
- 0 reviews duplicadas

Temporal distribution:
- Últimos 6 meses: 62%
- 6-12 meses: 28%
- >12 meses: 10% ✓

⚠ NOTAS:
- Producto "Ozempic genérico" tiene ratings bajos + reviews negativas
  → Posible clon / falsificación detectada
  → ACCIÓN: Reportar para análisis profundo en Fase 3
```

---

### **FUENTE 4: MERCADOLIBRE REVIEWS (⭐⭐⭐⭐ MEDIO)**

#### 2.4.1 Validaciones (Similar a Amazon)

| Validación | Criterio GO | Criterio NO-GO | Acción |
|------------|------------|----------------|--------|
| **N por producto** | ≥10 comentarios | <5 comentarios | Excluir producto |
| **Rating vendedor** | ≥4.5 estrellas | <4.0 estrellas | Excluir (vendedor poco confiable) |
| **Texto calidad** | Mediana ≥40 caracteres | <20 caracteres | Spam probable |
| **Precio validation** | Dentro ±30% de Amazon | Diferencia >50% | Investigar: posible falsificación |

#### 2.4.2 Comparativa Amazon vs MercadoLibre
```python
# Validación de consistencia cross-platform (Claude Code):
COMPARATIVA = {
    'mismo_producto_ambas_plataformas': count(),
    'diferencia_precio_promedio': % diferencia,
    'diferencia_rating_promedio': points,
}

if diferencia_precio > 30%:
    FLAG_PRODUCTO('Inconsistencia precio detectada')
if diferencia_rating > 1.0:
    FLAG_PRODUCTO('Inconsistencia sentimiento detectada')
```

#### 2.4.3 Documentación de Salida
```markdown
# MERCADOLIBRE REVIEWS QUALITY REPORT

**Productos analizados:** 6
**Comentarios totales:** 287
**Comentarios después de limpieza:** 263 (91.6%)

Cross-platform consistency:
- 5 productos coinciden con Amazon ✓
- 1 producto solo en MercadoLibre (Nootrópicos Stack local)

Precio comparativa (Amazon vs MercadoLibre):
- Ashwagandha: $299 vs $289 (3.4% diferencia) ✓
- Modafinilo: $450 vs $520 (13.3% diferencia) ⚠

Rating comparativa:
- Ashwagandha: 4.3 (Amazon) vs 4.1 (ML) ✓
- Modafinilo: 4.5 (Amazon) vs 4.2 (ML) ✓
```

---

### **FUENTE 5: REDDIT (⭐⭐⭐ MEDIO-BAJO)**

#### 2.5.1 Validaciones Específicas

| Validación | Criterio GO | Criterio NO-GO | Acción |
|------------|------------|----------------|--------|
| **N de posts** | ≥50 posts relevantes | <30 posts | Expandir búsqueda o subreddits |
| **Relevancia manual** | ≥80% posts on-topic | <70% on-topic | Revisar query keywords |
| **Longitud promedio** | Mediana ≥100 caracteres | <50 caracteres | Flag: bajo signal |
| **Score (upvotes)** | Distribuido (algunos altos, algunos bajos) | Todos scores <10 (bajo interés) | Subreddit no relevante |
| **Verificación bot** | <5% posts de bots conocidos | >10% bots | Excluir subreddit |
| **Idioma** | ≥95% en español | <80% español | Mezclar con traducciones o excluir |

#### 2.5.2 Clasificación de Relevancia Manual
```
INSTRUCCIÓN PARA USUARIO (revisión 10% muestral):

Para cada 10 posts, revisar manualmente 1-2:
- ¿El post trata específicamente de Ozempic, nootrópicos o suplementos?
  → SÍ = RELEVANTE
  → NO = OFF-TOPIC (excluir)
  
- ¿El post contiene experiencia/opinion verificable?
  → SÍ = VÁLIDO
  → NO = Chisme/anécdota sin base (flag pero conservar)

Si <80% relevancia en muestra:
  → Revisar query keywords
  → Cambiar subreddits
```

#### 2.5.3 Documentación de Salida
```markdown
# REDDIT QUALITY REPORT

**Subreddits analizados:** 3 (r/mexico, r/fitness_mexico, r/Nootropics)
**Posts totales:** 127
**Posts después de filtros:** 94 (74%)

Relevancia por subreddit:
- r/mexico: 45 posts, 82% on-topic ✓
- r/fitness_mexico: 32 posts, 88% on-topic ✓
- r/Nootropics: 42 posts, 65% on-topic ⚠ (spam/off-topic)

Distribución de upvotes:
- Mediana: 156 upvotes ✓
- Q1: 24 upvotes, Q3: 487 upvotes ✓
- Rango: 0-3,421 upvotes ✓

Idioma: 94% español, 6% inglés (aceptable)

LIMITACIONES IDENTIFICADAS:
- Reddit es self-selected (usuarios tech-savvy, jóvenes)
- Alto sesgo hacia early adopters / gym enthusiasts
- Usuarios anónimos (no se puede validar demográfica)
- Posible presencia de brand advocates o bots (requiere validación manual)

⚠ RECOMENDACIÓN:
- Usar Reddit como "señal complementaria", no primaria
- Validar manualmente 20% de posts para sesgo
```

---

### **FUENTE 6: FOROS (⭐⭐⭐ BAJO-MEDIO)**

#### 2.6.1 Validaciones Específicas

| Validación | Criterio GO | Criterio NO-GO | Acción |
|------------|------------|----------------|--------|
| **N testimonios** | ≥20 testimonios relevantes | <15 testimonios | Solo usar complementario |
| **Verificación usuario** | Perfil público ≥3 meses antiguo | Cuentas nuevas/anónimas | Flag como "baja confianza" |
| **Contenido** | Testimonios detallados ≥200 chars | Recomendaciones genéricas | Excluir |
| **Sesgo medicalización** | Variedad (beneficios Y efectos secundarios) | Solo beneficios/advertencias | Flag: sesgo detectado |

#### 2.6.2 Documentación de Salida
```markdown
# FOROS QUALITY REPORT

**Foros analizados:** 3
**Testimonios totales:** 89
**Testimonios válidos:** 67 (75%)

Características:
- Mediana longitud: 280 caracteres ✓
- Perfiles verificables: 62% ✓
- Antigüedad promedio: 2.3 años ✓

⚠ LIMITACIONES:
- Muestra pequeña (n=67)
- Sesgo hacia problemas/efectos secundarios (users post cuando algo sale mal)
- No representativo de población general
- Solo mencionar como contexto, no como hallazgo principal
```

---

## 3. MATRIZ DE GO/NO-GO CONSOLIDADA

| Fuente | Tamaño Mín. | Calidad Mín. | Status |
|--------|------------|--------------|--------|
| INEGI | n > 10,000 | >99% completo | ✅ GO |
| Google Trends | 156 semanas | >95% completo | ✅ GO |
| Amazon | 100 reviews | >85% válidas | ✅ GO (si <100, revisar spam) |
| MercadoLibre | 50 reviews | >80% válidas | ✅ GO (complementario) |
| Reddit | 50 posts | >70% on-topic | ⚠ CONDICIONAL (señal secundaria) |
| Foros | 30 testimonios | >60% válidos | ⚠ CONTEXTO SOLO (no primario) |

**Criterio GO global:** ≥3 fuentes primarias en GO

---

## 4. LÍMITES DE CONFIANZA POR FUENTE

En Fase 3 (EDA), reportaremos así:

```markdown
### HALLAZGO PRELIMINAR: Ozempic búsquedas crecen 300% (2024-2026)

Confianza POR FUENTE:

✓ Google Trends (⭐⭐⭐⭐⭐): 
   "Datos oficiales Google, índice 0-100 normalizado"

✓ INEGI Ocupación / Ingresos (⭐⭐⭐⭐⭐):
   "Fuente gubernamental, muestra n>400k"

✓ Amazon Reviews (⭐⭐⭐⭐):
   "72 reviews verificadas, 4.5 rating promedio"
   ⚠ LIMITACIÓN: Sesgo tech-savvy, e-commerce users

⚠ Reddit (⭐⭐⭐):
   "94 posts relevantes, sentimiento positivo 73%"
   ⚠ LIMITACIÓN: Self-selected, anónimo, posible sesgo early adopters
   → USAR COMO "señal complementaria", no evidencia primaria

❌ Foros (⭐⭐):
   "67 testimonios, pero muestra pequeña"
   ⚠ USAR SOLO para contexto cualitativo
```

---

## 5. DOCUMENTO DE AUDITORÍA DIARIO

Claude Code genera **daily quality report** durante recolección:

```markdown
# DAILY QUALITY REPORT - Día 4 (Miércoles 4 Jun)

Recolección del día:
✓ Amazon scraping: 73 reviews recolectadas
  - Spam detected: 3 (4.1%)
  - Valid: 70
  
✓ MercadoLibre: 45 comentarios recolectados
  - Spam: 2 (4.4%)
  - Valid: 43

⚠ Google Trends: 1 semana con dato faltante (interpolada)

RUNNING TOTALS:
- Total reviews: 188/300 target (62%)
- Total posts: 34/100 target (34%)
- Estimated completion: 11 Jun (on track) ✓
```

---

## 6. MATRIZ DE DECISIÓN: ¿QUÉ HACER SI FALLA?

| Escenario | Probabilidad | Decisión |
|-----------|--------------|----------|
| Amazon scraping bloqueado | Media | Usar API públicos o cambiar a librería (Selenium) |
| Google Trends: <3 años historia | Baja | Proceder con 3 años, documentar limitación |
| Reddit: <50 posts relevantes | Media | Expandir a subreddits relacionados (ej: r/health) |
| INEGI datos incompletos | Muy baja | Usar ENOE + ENIGH combinados, documentar |
| >30% reviews son spam | Baja | Ser más agresivo en spam filtering, revisar manual |

---

## 7. REPORTE FINAL DE CALIDAD

**Al fin de Fase 2, Claude Code genera:**

```
📊 DATA_QUALITY_FINAL_REPORT.md:

RESUMEN EJECUTIVO:
✓ 5 fuentes validadas
✓ ~1,200 data points recolectados
✓ 92.1% completitud después de limpieza
✓ 0 bloqueadores críticos

POR FUENTE (DETALLADO):
- INEGI: ⭐⭐⭐⭐⭐ 456k registros, 100% válidos
- Google Trends: ⭐⭐⭐⭐ 260 semanas, 98% completo
- Amazon: ⭐⭐⭐⭐ 450 reviews, 92.6% válidas
- MercadoLibre: ⭐⭐⭐⭐ 263 reviews, 91.6% válidas
- Reddit: ⭐⭐⭐ 94 posts, 70% on-topic
- Foros: ⭐⭐⭐ 67 testimonios, contexto complementario

LIMITACIONES DOCUMENTADAS:
1. Reviews sesgadas hacia early adopters (solución: combinar con INEGI)
2. Reddit representa usuarios tech-savvy (solución: usar como señal, no evidencia)
3. Pequeña muestra de foros (solución: usar contextualmente)

CONFIANZA GLOBAL: ⭐⭐⭐⭐ (Alto)
- Primarias (INEGI + Google Trends + Amazon): Altamente confiables
- Secundarias (MercadoLibre + Reddit): Confirmatorias
- Complementarias (Foros): Contexto cualitativo
```

---

## 8. AUTORIZACIÓN Y PRÓXIMOS PASOS

**Responsable validación:** Claude Code (diario) + Usuario (revisión)  
**Entregable:** Data Quality Final Report  
**Siguiente fase:** Fase 3 EDA (Semana 3)

---

**Documento activo. Próxima revisión: Semana 2 (11 Jun)**
