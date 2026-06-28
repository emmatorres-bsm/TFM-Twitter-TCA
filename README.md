\# Detección temprana de Trastornos de Conducta Alimentaria en Redes Sociales mediante Inteligencia Artificial



Este repositorio contiene el \*\*pipeline analítico completo y multicapa\*\* desarrollado para mi Trabajo de Fin de Máster (TFM). El proyecto tiene como objetivo la identificación proactiva y la caracterización sociolingüística de perfiles en riesgo de padecer Trastornos de la Conducta Alimentaria (TCA) en la red social X (anteriormente Twitter). 



Inspirado metodológicamente en el \*\*Proyecto STOP\*\* (\*Suicide prevenTion in sOcial Platforms\*) liderado por la Dra. Ana Freire, este trabajo expande el uso de modelos de aprendizaje profundo (\*Deep Learning\*) y Procesamiento del Lenguaje Natural (NLP) hacia la detección y mitigación de dinámicas patológicas de salud mental en entornos digitales.



\---



\##  Arquitectura del Pipeline (Fases de Datos)



El repositorio está estructurado en 7 módulos secuenciales en formato Jupyter Notebook (`.ipynb`), organizados de la siguiente manera:



\###  1. Adquisición y Unificación de Datos

\* \*\*`01a\_extraccion\_geolocalizada.ipynb`\*\*: Extracción dirigida de publicaciones mediante la API de Twitter utilizando geolocalización por coordenadas (GPS de España) y filtros lingüísticos preliminares para aislar ruido de otros dialectos.

\* \*\*`01b\_extraccion\_filtros\_linguisticos.ipynb`\*\*: Captura de datos basada en grupos de palabras clave (\*keywords\*), \*hashtags\* encubiertos y jerga propia de la comunidad. Excluye de forma agresiva \*slangs\* y nombres geográficos de Latinoamérica para centrar el estudio en España.

\* \*\*`02\_unificacion\_y\_limpieza.ipynb`\*\*: Módulo encargado de consolidar las diferentes fuentes de extracción, aplicar deduplicación estricta de registros por identificador único, eliminar celdas vacías y descartar publicaciones con longitudes de texto insuficientes para el análisis.



\###  2. Filtrado y Preparación del Dataset Maestro

\* \*\*`03\_filtro\_instituciones\_vs\_usuarios.ipynb`\*\*: Para evitar sesgos en el análisis discursivo, se implementa un clasificador BERT binario (`bert-base-spanish-wwm-cased`) apoyado por heurísticas avanzadas de filtrado semántico. Su objetivo es identificar y separar de forma automatizada las cuentas de instituciones, clínicas u ONGs de los perfiles de usuarios reales e individuales.

\* \*\*`04\_extraccion\_historial\_usuarios.ipynb`\*\*: Descarga de los últimos 50 tuits originales (excluyendo retuits y respuestas automáticas) de los usuarios reales identificados. Los textos individuales se agrupan cronológicamente para construir una cadena única de texto largo representativa del historial discursivo de cada perfil.

\* \*\*`05\_anonimizador\_usuarios.ipynb`\*\*: Módulo crítico de ética y privacidad. Implementa anonimización agresiva mediante reconocimiento de entidades nombradas (NER) con \*\*spaCy\*\* (`es\_core\_news\_md`), la librería \*\*scrubadub\*\* y expresiones regulares para sustituir nombres de personas, ubicaciones geográficas, enlaces URL, números y menciones por etiquetas genéricas (`{{PERSON}}`, `{{LOCATION}}`, etc.).



\###  3. Fine-Tuning de BERT y Minería de Texto

\* \*\*`06\_FineTuning\_BERT\_y\_Clasificacion.ipynb`\*\*: Carga el Gold Standard etiquetado manualmente y realiza el ajuste fino (\*fine-tuning\*) de \*\*BETO\*\* para clasificación multicatenaria. Introduce estratégicamente la clase \*\*"Dudoso"\*\* como un umbral de incertidumbre metodológica para aislar textos ambiguos y salvaguardar la pureza de las clases \*\*"Riesgo"\*\* y \*\*"Control"\*\*. Genera automáticamente la matriz de confusión y ejecuta la inferencia sobre la totalidad del corpus.

\* \*\*`07\_Analisis\_Exploratorio\_Resultados.ipynb`\*\*: Fase de minería de datos y NLP discursivo. Implementa un sistema de \*\*preprocesamiento semántico dual\*\*: una \*lista estricta\* de \*stopwords\* (que elimina la paja de internet y las etiquetas de anonimización) para gráficos de unigramas y nubes de palabras comparativas; y una \*lista relajada\* (que mantiene vivos los verbos y pronombres) para la extracción de bigramas, permitiendo capturar la intencionalidad conductual del discurso del grupo de riesgo frente al grupo de control.



\---



\##  Datasets Incluidos



Debido a estrictas políticas de privacidad y protección de datos sensibles de salud mental, la información personal se encuentra completamente cifrada y anonimizada. Se incluyen los siguientes archivos esenciales en formato tabular:



1\.  \*\*`SAMPLE\_USUARIOS\_RIESGO.csv`\*\*: El \*Gold Standard\* compuesto por una muestra aleatoria de 500 perfiles validados y etiquetados manualmente por el equipo de investigación para auditar, entrenar y evaluar el rendimiento predictivo del algoritmo de Deep Learning.

2\.  \*\*`USUARIOS\_CLASIFICADOS\_RIESGO.csv`\*\*: El dataset maestro definitivo que contiene los historiales anonimizados consolidados e incluye la etiqueta predictiva final generada por el modelo BETO (`LLM\_TCA\_Risk`: control, dudoso, riesgo), sirviendo de base para todo el análisis sociolingüístico y estadístico del TFM.



\---



\##  Instalación y Configuración



Para garantizar la correcta replicabilidad del entorno y la ejecución local o en servidor de todos los módulos del pipeline, sigue estos pasos:



1\.  \*\*Clonar el repositorio:\*\*

&#x20;   ```bash

&#x20;   git clone https://github.com/tu-usuario/tu-repositorio-tca.git

&#x20;   cd tu-repositorio-tca

&#x20;   ```



2\.  \*\*Instalar las dependencias de Python:\*\*

&#x20;   Asegúrate de tener un entorno limpio e instala el archivo de requisitos que consolida todo el stack tecnológico (`pandas`, `numpy`, `transformers`, `torch`, `scikit-learn`, `wordcloud`, etc.):

&#x20;   ```bash

&#x20;   pip install -r requirements.txt

&#x20;   ```



3\.  \*\*Descargar el modelo de lenguaje de spaCy:\*\*

&#x20;   Requisito indispensable para que funcione el módulo de anonimización avanzada (Fase 05):

&#x20;   ```bash

&#x20;   python -m spacy download es\_core\_news\_md

&#x20;   ```





\---



\##  Licencia y Propósito Ético



Este proyecto se ha desarrollado bajo la supervisión de un comité ético de investigación. Su fin es puramente preventivo, educativo y de salud pública. Las propuestas teóricas derivadas de este modelo buscan complementar plataformas de asistencia médica y psicológica (como el Proyecto STOP), rechazando de forma explícita cualquier mecanismo punitivo o de censura de contenidos en entornos digitales.

