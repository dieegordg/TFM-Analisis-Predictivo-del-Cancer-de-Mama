# Análisis predictivo del cáncer de mama: predicción de la supervivencia a cinco años mediante aprendizaje automático con datos SEER

Trabajo Fin de Máster del Máster Universitario en Big Data Science de la Universidad de Navarra.

## Descripción

Este repositorio contiene el código desarrollado para el Trabajo Fin de Máster:

**«Análisis Predictivo del Cáncer de Mama utilizando Técnicas de Big Data y Aprendizaje Automático»**

El objetivo del proyecto es desarrollar y evaluar modelos de aprendizaje automático capaces de predecir la supervivencia global a cinco años en casos de cáncer de mama, utilizando información clínico-epidemiológica procedente del programa **Surveillance, Epidemiology, and End Results (SEER)**.

El problema se formula como una tarea de clasificación binaria supervisada:

- `1`: supervivencia igual o superior a 60 meses.
- `0`: fallecimiento antes de alcanzar los 60 meses.

Los casos vivos con menos de 60 meses de seguimiento se consideran observaciones censuradas y se excluyen del conjunto utilizado para el modelado.

## Objetivos

Los principales objetivos del proyecto son:

- Analizar y preparar datos clínico-epidemiológicos de cáncer de mama.
- Construir una variable objetivo de supervivencia global a cinco años.
- Evitar fugas de información y sesgos asociados a la censura.
- Comparar modelos de regresión logística, Random Forest y XGBoost.
- Optimizar los hiperparámetros del modelo XGBoost.
- Evaluar el rendimiento mediante métricas de clasificación.
- Analizar la importancia de las variables y los errores del modelo.
- Valorar las limitaciones metodológicas y la posible utilidad clínica de los resultados.

## Conjunto de datos

El conjunto de datos original contiene **1.365.329 registros** correspondientes a diagnósticos realizados entre los años 2000 y 2022.

Después de aplicar los criterios de limpieza, excluir los registros con supervivencia desconocida, tratar la censura y limitar el análisis principal al periodo 2000–2017, el conjunto final de modelado contiene:

- **996.916 registros**
- **12 variables predictoras**
- **797.532 registros de entrenamiento**
- **199.384 registros de prueba**

La unidad de análisis es el caso tumoral registrado por SEER, por lo que cada fila representa un registro oncológico y no necesariamente una persona única.

### Disponibilidad de los datos

Los datos originales de SEER no se incluyen en este repositorio.

El acceso a estos datos está sujeto a las condiciones establecidas por el National Cancer Institute y debe realizarse a través de los canales oficiales del programa SEER.

El notebook requiere que el usuario disponga previamente del archivo de datos correspondiente y adapte la ruta de carga a su entorno local.

## Metodología

El flujo de trabajo desarrollado incluye las siguientes etapas:

1. Carga y exploración inicial de los datos.
2. Revisión de tipos de variables y valores especiales.
3. Conversión de la variable de supervivencia.
4. Eliminación de variables sin variabilidad.
5. Tratamiento de observaciones censuradas.
6. Construcción de la variable objetivo `survived_5_years`.
7. Exclusión de variables de desenlace para evitar fuga de información.
8. Análisis exploratorio de datos.
9. División estratificada en entrenamiento y prueba.
10. Imputación, codificación y escalado mediante pipelines.
11. Entrenamiento y comparación de modelos.
12. Optimización de hiperparámetros mediante validación cruzada.
13. Ajuste del umbral de decisión.
14. Interpretación de variables y análisis de errores.

## Modelos evaluados

Se comparan los siguientes modelos:

- Regresión logística.
- Random Forest.
- XGBoost.
- XGBoost optimizado mediante `RandomizedSearchCV`.

El preprocesamiento se implementa mediante herramientas de `scikit-learn`, utilizando objetos `Pipeline` y `ColumnTransformer`.

Para abordar el desbalance entre clases se emplean ponderaciones de clase en la regresión logística y Random Forest, y el parámetro `scale_pos_weight` en XGBoost.

## Resultados principales

Los resultados obtenidos sobre el conjunto de prueba fueron los siguientes:

| Modelo | Exactitud | Precisión | Sensibilidad | F1 | AUC-ROC |
|---|---:|---:|---:|---:|---:|
| Regresión logística | 0,7808 | 0,46 | 0,70 | 0,55 | 0,8274 |
| Random Forest | 0,7745 | 0,45 | 0,70 | 0,54 | 0,8219 |
| XGBoost | 0,7783 | 0,45 | 0,70 | 0,55 | 0,8282 |
| XGBoost optimizado | 0,7747 | 0,45 | 0,71 | 0,55 | 0,8316 |

La precisión, la sensibilidad y el F1 de la tabla corresponden a la clase de no supervivencia a cinco años.

El modelo XGBoost optimizado obtuvo el mayor AUC-ROC, aunque la diferencia respecto a la regresión logística fue reducida.

También se evaluó un umbral alternativo de decisión de `0,38`, seleccionado mediante predicciones *out-of-fold* sobre el conjunto de entrenamiento. Este umbral mejoró la precisión y el F1 de la clase de no supervivencia, a costa de reducir su sensibilidad.

## Variables más relevantes

Las variables con mayor relevancia predictiva fueron:

- Estadio tumoral.
- Edad.
- Cirugía sobre el sitio primario.
- Estado civil.
- Algunas categorías histológicas.

Estas relaciones deben interpretarse como asociaciones predictivas y no como efectos causales.

Las variables de tratamiento proceden de un registro observacional y pueden estar condicionadas por la gravedad de la enfermedad, las decisiones clínicas y otros factores de confusión.

## Contenido del repositorio

```text
.
├── Notebook_TFM_DiegoRodrigo_VersionFinal.ipynb
└── README.md
````

El notebook contiene:

* Preparación y limpieza de datos.
* Construcción de la variable objetivo.
* Análisis exploratorio.
* Preprocesamiento.
* Entrenamiento de modelos.
* Optimización de hiperparámetros.
* Evaluación de resultados.
* Interpretación del modelo.
* Análisis de errores.

## Requisitos

El proyecto ha sido desarrollado en Python y utiliza principalmente las siguientes librerías:

* `pandas`
* `numpy`
* `matplotlib`
* `scipy`
* `scikit-learn`
* `xgboost`
* `jupyter`

Las versiones exactas pueden depender del entorno de ejecución utilizado.

## Ejecución

1. Clonar el repositorio:

```bash
git clone URL_DEL_REPOSITORIO
```

2. Acceder al directorio:

```bash
cd NOMBRE_DEL_REPOSITORIO
```

3. Crear y activar un entorno virtual:

```bash
python -m venv .venv
```

En Windows:

```bash
.venv\Scripts\activate
```

En Linux o macOS:

```bash
source .venv/bin/activate
```

4. Instalar las dependencias necesarias:

```bash
pip install pandas numpy matplotlib scipy scikit-learn xgboost jupyter
```

5. Iniciar Jupyter Notebook:

```bash
jupyter notebook
```

6. Abrir el archivo:

```text
Notebook_TFM_DiegoRodrigo_VersionFinal.ipynb
```

7. Adaptar en el notebook la ruta del archivo de datos SEER al entorno local.

## Reproducibilidad

El conjunto de prueba se mantiene separado durante el entrenamiento y la optimización de los modelos.

Las transformaciones de preprocesamiento se ajustan exclusivamente sobre los datos de entrenamiento mediante pipelines, con el objetivo de reducir el riesgo de fuga de información.

Cuando se utilizan operaciones aleatorias, el notebook establece semillas mediante el parámetro `random_state` para favorecer la reproducibilidad de los resultados.

Los resultados pueden presentar pequeñas variaciones dependiendo de las versiones de las librerías, el sistema operativo y el hardware utilizado.

## Limitaciones

Entre las principales limitaciones del proyecto se encuentran:

* La formulación binaria simplifica la naturaleza temporal de la supervivencia.
* No se realiza una validación externa.
* La evaluación principal se basa en una partición aleatoria y no temporal.
* Algunas variables presentan una proporción elevada de información no registrada.
* Las puntuaciones de los modelos no fueron evaluadas mediante técnicas de calibración.
* Las variables de tratamiento no permiten realizar interpretaciones causales.
* Los datos de los años 2018–2022 no se incorporan al modelo principal debido al seguimiento insuficiente para evaluar la supervivencia a cinco años.

## Autor

**Diego Rodrigo Aguilar**

Trabajo Fin de Máster
Máster Universitario en Big Data Science
Universidad de Navarra
Curso académico 2025–2026

## Tutor académico

**José González Gomariz**

## Uso académico

Este repositorio se publica como material complementario del Trabajo Fin de Máster y tiene como finalidad facilitar la revisión, trazabilidad y comprensión del análisis desarrollado.

Los datos originales no forman parte del repositorio y deben obtenerse directamente a través del programa SEER, respetando sus condiciones de acceso y utilización.
