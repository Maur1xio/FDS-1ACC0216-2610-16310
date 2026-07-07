# Trending YouTube Video Statistics (México) - Fundamentos de Data Science

## Objetivo del trabajo
Realizar un análisis exploratorio y de calidad de datos (EDA) sobre videos en tendencia de YouTube en México, con el fin de comprender patrones de engagement, preparar el conjunto de datos mediante técnicas de limpieza, integración y transformación, y extraer conclusiones iniciales que sirvan como base para un modelado predictivo de visualizaciones, utilizando Python como herramienta de software.

## Alumnos participantes
- Belledonne Espinoza, Claudia Valeria
- Carrasco Betancourt, Chris
- Rivera Hernandez, Gabriel Omar
- Elera Rodríguez, Mauricio Elera

## Breve descripción del dataset
El conjunto de datos utilizado se denomina **"Trending YouTube Video Statistics"**, correspondiente al país asignado: **México**. Consiste en registros diarios de videos que alcanzaron la sección de tendencias en la plataforma, enriquecidos con coordenadas geográficas `(lat, lon)`, división administrativa `(state)` y geometría `(geometry)`.

Los archivos empleados son:
- `MXvideos_cc50_202101.csv` (CSV, ~45.7 MB): dataset transaccional principal.
- `MX_category_id.json` (JSON, ~9 KB): metadatos de categorías de YouTube.

El dataset original comprende **44,043 registros** y **20 variables**, e incluye información detallada como:
- Identificación del video y metadatos (`video_id`, `title`, `channel_title`, `tags`, `description`).
- Fechas de publicación y de tendencia (`publish_time`, `trending_date`).
- Métricas de interacción (`views`, `likes`, `dislikes`, `comment_count`).
- Configuración del video (`comments_disabled`, `ratings_disabled`, `video_error_or_removed`).
- Ubicación geográfica asociada (`state`, `lat`, `lon`, `geometry`).

Tras las fases de limpieza y depuración, el conjunto quedó en **40,401 registros** válidos, listos para análisis descriptivo y modelado predictivo.

*Nota: Para fines académicos, el dataset presenta inconsistencias reales que fueron tratadas durante el proyecto, como identificadores corruptos (`#NAME?`), registros duplicados, valores nulos estructurales y valores atípicos en las métricas de engagement.*

## Conclusiones

- **Volumen y representatividad:** El dataset ofrece escala suficiente (más de 40,000 registros y ~34,250 videos únicos) para analizar de forma robusta el comportamiento de tendencias en YouTube México.
- **Estructura semántica:** La combinación de variables categóricas (título, canal, categoría, estado) y numéricas (vistas, likes, dislikes, comentarios) permite caracterizar tanto el contenido como el nivel de interacción de cada video.
- **Asimetría en el engagement:** Las métricas de interacción presentan alta dispersión y sesgo positivo (media de vistas ~342,000 vs. mediana ~57,000), reflejando la presencia de videos virales con alcance masivo frente a la mayoría de contenidos con tracción moderada.
- **Categorías predominantes:** Las categorías más frecuentes incluyen **Entertainment**, **News & Politics** y **People & Blogs**, lo que indica una mezcla de entretenimiento, actualidad y contenido de creadores individuales dentro del ecosistema de tendencias mexicano.
- **Cobertura geográfica:** Los registros abarcan **32 entidades federativas**; **Morelos** concentra la mayor frecuencia de apariciones, lo que sugiere un posible sesgo geográfico en la asignación o enriquecimiento de coordenadas del dataset.
- **Calidad e integridad de datos:** Se corrigieron identificadores erróneos en `video_id`, se eliminaron duplicados y registros con más del 50% de campos vacíos, y se depuraron filas sin métricas esenciales, logrando un 0% de nulos en variables críticas como `views`.
- **Tratamiento de outliers:** En lugar de eliminar videos virales, se aplicó transformación logarítmica (`log1p`) sobre `views`, `likes`, `dislikes` y `comment_count`, preservando la información de alto impacto y estabilizando las distribuciones para modelos lineales.
- **Integración y enriquecimiento:** El mapeo de `category_id` con el archivo JSON permitió obtener nombres legibles de categorías (`category_name`), facilitando la interpretación de resultados en etapas posteriores.
- **Preparación para modelado:** Se generaron variables derivadas (días hasta tendencia, ratios de polaridad y discusión), se codificaron variables categóricas (One-Hot Encoding), se abordó la multicolinealidad entre métricas de interacción y se estandarizaron los datos, dejando el conjunto en condiciones óptimas para la fase de modelado predictivo.

## Licencia
Este proyecto ha sido desarrollado con fines estrictamente académicos para el curso 1ACC0216 - Fundamentos de Data Science en la Universidad Peruana de Ciencias Aplicadas (UPC). El contenido, código y análisis presentados en este repositorio son propiedad de los autores mencionados en la sección de participantes. Se permite su uso y consulta exclusivamente con fines educativos y de referencia, siempre que se otorgue el crédito correspondiente a los autores originales y a la institución académica.
