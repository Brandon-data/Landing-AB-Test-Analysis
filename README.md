🧪 Análisis de un experimento A/B en una landing page

Abrir en Colab

Evaluación estadística de un experimento A/B realizado en la página de inicio de un ecommerce, para decidir qué versión implementar a partir de la tasa de conversión, el gasto promedio y el comportamiento por canal de tráfico y tipo de usuario.

English summary ↓

🎯 Objetivo

Responder con evidencia estadística a cinco preguntas de negocio:

¿Existe una diferencia significativa en el gasto promedio por usuario convertido entre las versiones A y B?
¿Qué versión genera mayor tasa de conversión?
¿La conversión depende de la fuente de tráfico?
¿El tipo de usuario (nuevo o recurrente) influye en la conversión?
¿Qué recomendaciones permiten optimizar la estrategia de marketing y el diseño de la página?
📁 Contenido del repositorio
Archivo	Descripción
landing_ab_test_analysis.ipynb	Notebook con el análisis completo, desde la validación de datos hasta el insight ejecutivo
README.md	Este documento

Sobre el dataset: landing_experiment.csv proviene del material de un curso y no se incluye en el repositorio. El notebook lo lee desde la ruta de la plataforma del curso (/datasets/landing_experiment.csv). Para ejecutarlo en otro entorno, coloca allí tu propio archivo con las mismas columnas y ajusta la ruta de lectura.

🗂️ Datos

40,000 usuarios expuestos a una de las dos versiones de la página durante 28 días (enero de 2026), con una fila por usuario.

Columna	Descripción
user_id	Identificador único del usuario
date	Fecha de exposición a la página
landing	Versión mostrada (A: control, B: prueba)
region	Norte, Centro, Sur, Occidente u Oriente
dispositivo	Mobile o Desktop
traffic_source	Organic, Ads, Email o Referral
user_type	Nuevo o Recurrente
converted	1 si el usuario compró, 0 si no
gasto	Monto gastado (0 si no convirtió)
🔍 Metodología
Validación de datos: sin nulos ni usuarios duplicados, date convertida a tipo fecha, consistencia entre converted y gasto y grupos A y B balanceados (49.96% y 50.04%).
Elección de la prueba según la pregunta:
Pregunta	Variable	Prueba
Gasto promedio A vs B (solo usuarios convertidos)	Numérica	Prueba t de Welch, tras verificar la igualdad de varianzas con Levene
Tasa de conversión A vs B	Binaria	Prueba z para proporciones
Conversión vs fuente de tráfico	Categórica	Chi-cuadrada de independencia + V de Cramér
Conversión vs tipo de usuario	Categórica	Chi-cuadrada de independencia + V de Cramér
Verificación de supuestos: independencia de las observaciones (un registro por usuario), varianzas (Levene) y frecuencias esperadas ≥ 5 en las tablas de contingencia.
Visualización: barras agrupadas (cantidades) y apiladas (proporciones) para complementar las pruebas con cada variable categórica.
Insight ejecutivo con decisión, recomendaciones y limitaciones.
📊 Hallazgos clave
La página B convierte más: 15.96% frente a 12.57% de la página A, una diferencia de 3.38 puntos porcentuales (aproximadamente 27% más conversiones en términos relativos). Prueba z: p ≈ 3.8 × 10⁻²².
Y cada cliente gasta más en B: 68.75 frente a 61.09 de promedio, una diferencia de 7.66 (aproximadamente 12.5% más). Prueba t de Welch: p ≈ 3.6 × 10⁻²¹.
Fuente de tráfico: hay asociación con la conversión, pero débil (p ≈ 0.034; V de Cramér ≈ 0.015). Email (15.0%) y Ads (14.7%) convierten ligeramente mejor que Referral (13.9%) y Organic (13.8%), aunque Organic aporta el mayor volumen de conversiones (2,480, cerca del 43% del total).
Tipo de usuario: sin evidencia de asociación (p ≈ 0.47; V de Cramér ≈ 0.004). Usuarios nuevos y recurrentes convierten prácticamente igual (14.4% y 14.1%).
✅ Recomendaciones
Implementar la página B para todos los usuarios: mejora la conversión y el gasto promedio por cliente.
Mantener el tráfico orgánico por su volumen y evaluar con datos de costo si conviene reforzar Email y Ads, ya que sus diferencias de tasa son pequeñas.
No segmentar la página por tipo de usuario, porque no se observaron diferencias en la conversión.
Dar seguimiento tras la implementación y revisar si el beneficio de B se mantiene por canal, dispositivo y región.
⚠️ Limitaciones
El análisis se basa en una muestra de 40,000 usuarios durante 28 días.
Muestra diferencias y asociaciones, pero no explica por qué la página B funciona mejor.
No incluye costos de implementación ni de cada canal de tráfico.
No se evaluó si el efecto de la página cambia según canal, dispositivo o región.
El gasto tiene una distribución sesgada a la derecha, con algunas compras altas que se conservaron por ser plausibles.
🛠️ Herramientas

Python · pandas · NumPy · SciPy (ttest_ind, levene, chi2_contingency) · statsmodels (proportions_ztest) · seaborn · matplotlib · Jupyter

🌎 English summary

A/B test analysis of an ecommerce landing page. I evaluated an experiment with 40,000 users (28 days, versions A and B) to recommend which page to ship, using statistical tests matched to each business question: Welch's t-test (average spend, after Levene's test for equal variances), a two-proportion z-test (conversion rate) and chi-square tests of independence with Cramér's V (traffic source and user type), checking each test's assumptions.

Key findings:

Page B converts better: 15.96% vs. 12.57% (+3.38 percentage points, ~27% relative lift), significant at α = 0.05.
Customers also spend more on B: 68.75 vs. 61.09 on average (+7.66, ~12.5%), significant at α = 0.05.
Traffic source: a statistically significant but weak association (V ≈ 0.015). Email and Ads convert slightly better, while Organic brings the most converted users by volume.
User type: no evidence of association with conversion (p ≈ 0.47).

Recommendation: ship page B, evaluate channel costs before reallocating budget, and monitor the effect by segment after rollout.

Limitations: 28-day sample, no cost data, no analysis of whether the page effect differs by segment, and associations are not causal explanations. The dataset comes from a course and is not included in this repository.
