# Trending YouTube Video Statistics (México) - Fundamentos de Data Science

## Objetivo del trabajo
Desarrollar un proyecto integral de ciencia de datos sobre videos en tendencia de YouTube en México, siguiendo las fases de comprensión, preparación y modelado: recolección e inventario de datos, descripción estructural, exploración univariada y bivariada, auditoría de calidad, limpieza e integración, y construcción de modelos de regresión lineal para predecir el volumen de visualizaciones (`views_log`), utilizando Python (Pandas, Scikit-learn, Seaborn, Plotly) como herramienta principal.

## Alumnos participantes
- Belledonne Espinoza, Claudia Valeria
- Carrasco Betancourt, Chris
- Rivera Hernandez, Gabriel Omar
- Elera Rodríguez, Mauricio Elera

## Estructura del repositorio

| Carpeta / archivo | Contenido |
|---|---|
| `code/Reporte_de_recolección_de_datos.ipynb` | Fase de recolección e ingesta de datos |
| `code/Reporte_de_descripción_de_los_datos.ipynb` | Inspección estructural y diccionario de variables |
| `code/Reporte_de_exploración_de_los_datos.ipynb` | Análisis exploratorio univariado y bivariado |
| `code/Reporte_de_calidad_de_los_datos.ipynb` | Auditoría de calidad (duplicados, nulos, outliers, consistencia lógica) |
| `code/Reporte_de_calidad_de_datos_pt_2.ipynb` | Limpieza, integración, transformación y preparación para modelado |
| `code/Data_Science (2).ipynb` | Notebook integrado con Fases 2, 3 y 4 (comprensión, preparación y modelado) |
| `data/df_analisis.csv` | Dataset limpio completo para análisis descriptivo (40,401 × 23) |
| `data/df_prediccion.csv` | Subconjunto predictivo con transformaciones logarítmicas (40,401 × 17) |

## Breve descripción del dataset
El conjunto de datos utilizado se denomina **"Trending YouTube Video Statistics"**, correspondiente al país asignado: **México**. Consiste en registros diarios de videos que alcanzaron la sección de tendencias en la plataforma, enriquecidos con coordenadas geográficas `(lat, lon)`, división administrativa `(state)` y geometría `(geometry)`.

Los archivos fuente empleados son:
- `MXvideos_cc50_202101.csv` (CSV, ~45.7 MB): dataset transaccional principal.
- `MX_category_id.json` (JSON, ~9 KB): metadatos de categorías de YouTube.

El dataset original comprende **44,043 registros** y **20 variables**, e incluye información detallada como:
- Identificación del video y metadatos (`video_id`, `title`, `channel_title`, `tags`, `description`).
- Fechas de publicación y de tendencia (`publish_time`, `trending_date`).
- Métricas de interacción (`views`, `likes`, `dislikes`, `comment_count`).
- Configuración del video (`comments_disabled`, `ratings_disabled`, `video_error_or_removed`).
- Ubicación geográfica asociada (`state`, `lat`, `lon`, `geometry`).

Tras las fases de limpieza y depuración, el conjunto quedó en **40,401 registros** válidos con **33,953 videos únicos**. Los datasets exportados en `data/` conservan dos vistas del mismo universo limpio:
- **`df_analisis.csv`**: réplica íntegra con 23 columnas (incluye `category_name`, `trending_date_dt` y campos descriptivos).
- **`df_prediccion.csv`**: subconjunto de 17 columnas orientado al modelado (incluye `views_log`, `likes_log`, `dislikes_log`, `comment_count_log`).

*Nota: Para fines académicos, el dataset presenta inconsistencias reales que fueron tratadas durante el proyecto: identificadores corruptos (`#NAME?`, 452 registros), duplicados (3,495), bloques de filas con ~75% de campos vacíos por error de parseo, valores nulos en `description` y valores atípicos en las métricas de engagement.*

## Conclusiones

### Exploración de datos
- **Distribución de engagement:** Las cuatro métricas numéricas (`views`, `likes`, `dislikes`, `comment_count`) presentan sesgo positivo y cola larga. La mayor concentración de videos trending se ubica en el rango de **10K–100K vistas**; los likes se concentran en **100–10K**; los dislikes en **0–50**; y los comentarios en **0–500**.
- **Correlación entre métricas:** Existe correlación positiva fuerte (**> 0.6**) entre `views` y las interacciones (`likes`, `comment_count`), confirmando que el alcance es el motor principal del engagement. Se detecta multicolinealidad entre `likes` y `comment_count`, relevante para el modelado.
- **Categorías de contenido:** Predominan **Entertainment** y **People & Blogs**. Las categorías con medianas de vistas superiores corresponden a los IDs **10, 20 y 24** (Music, Gaming y Entertainment).
- **Distribución geográfica:** La frecuencia de registros por estado es relativamente equilibrada entre las **32 entidades federativas**. En términos de volumen de vistas, no se observa un estado que domine sistemáticamente sobre los demás.
- **Restricciones de interacción:** Los videos con comentarios habilitados (`comments_disabled = FALSO`) presentan mediana de vistas mayor que aquellos con comentarios desactivados.
- **Estacionalidad:** La evolución mensual muestra picos de actividad no lineales, sugiriendo sensibilidad a factores temporales y de coyuntura.

### Calidad de datos
- **Duplicados e identificadores:** Se detectaron **3,495 duplicados** por la llave `(video_id, trending_date)` y **452 registros con `#NAME?`** en `video_id`. Tras corrección y depuración, el dataset quedó libre de duplicados e identificadores corruptos.
- **Valores faltantes:** `description` concentra el mayor porcentaje de nulos (**18.65%** en crudo; **~10.4%** tras limpieza), comportamiento esperado por ser un campo opcional. Un bloque de **~3,453 filas** (~8.85%) presentaba ausencia sistémica de métricas por error de parseo en `video_id`.
- **Consistencia lógica:** No se encontraron inconsistencias cruzadas (likes/dislikes/comentarios superiores a vistas, fechas de publicación posteriores a la de tendencia, etc.). La categoría **43** es histórica (anterior a la fecha máxima de tendencia: **2018-05-19**).
- **Outliers:** Los valores extremos son reales (videos virales), con máximos de **100.91M vistas**, **4.47M likes** y **1.35M dislikes**. Se optó por conservarlos y aplicar transformación logarítmica en lugar de eliminarlos.

### Preparación de datos
- **Limpieza final:** De 44,043 registros originales se llegó a **40,401** válidos mediante corrección de IDs, eliminación de duplicados, filtro por umbral de completitud (50%) y depuración de filas sin `views`.
- **Transformaciones:** Se aplicó `log1p` sobre métricas de engagement, se integraron categorías desde JSON (`category_name`), se calculó `days_until_trend`, se codificaron variables categóricas (One-Hot Encoding) y se generaron ratios de **polaridad** y **discusión** para mitigar multicolinealidad.
- **Estandarización:** División train/test (80/20), escalamiento con `StandardScaler` y reducción dimensional con PCA (2 componentes, **95.40%** de varianza retenida).

### Modelado predictivo
Se entrenaron cuatro modelos de regresión lineal sobre `views_log` (escalado), con los siguientes resultados en el set de prueba:

| Modelo | R² | RMSE |
|---|---:|---:|
| Baseline | 0.8287 | 0.7229 |
| Feature Engineering | **0.8305** | **0.7191** |
| PCA | 0.8280 | 0.7243 |
| Ridge Regression | **0.8305** | **0.7191** |

Las variables con mayor peso en el modelo baseline son `dislikes_log`, `likes_log` y `ratings_disabled`. El escenario con Feature Engineering (ratios de polaridad y discusión) obtuvo el mejor desempeño, confirmando que las métricas de interacción son los predictores más relevantes del volumen de visualizaciones en videos trending.

## Licencia
Este proyecto ha sido desarrollado con fines estrictamente académicos para el curso 1ACC0216 - Fundamentos de Data Science en la Universidad Peruana de Ciencias Aplicadas (UPC). El contenido, código y análisis presentados en este repositorio son propiedad de los autores mencionados en la sección de participantes. Se permite su uso y consulta exclusivamente con fines educativos y de referencia, siempre que se otorgue el crédito correspondiente a los autores originales y a la institución académica.
