# Data Science III - Proyecto Final

## Clasificación automática de noticias mediante NLP y Deep Learning

Desarrollo de un modelo de clasificación multiclase para la categorización automática de noticias empresariales, económicas y regulatorias.

Proyecto realizado como parte de la **Diplomatura en Data Science III**.

---

## Descripción del proyecto

El crecimiento de la información digital ha generado la necesidad de contar con herramientas capaces de procesar y organizar grandes volúmenes de contenido textual.

En este proyecto se desarrolla un modelo capaz de analizar automáticamente el contenido textual de una noticia y asignarle una categoría temática.

El problema corresponde a un escenario de **aprendizaje supervisado de clasificación multiclase**, utilizando técnicas de **Procesamiento de Lenguaje Natural (NLP)** y **Deep Learning**.

---

## Objetivo

Desarrollar y evaluar un modelo de Deep Learning capaz de clasificar automáticamente noticias en diferentes categorías temáticas utilizando únicamente el contenido textual de las mismas.

La variable predictora utilizada es:

* `news`: contenido textual de la noticia.

La variable objetivo es:

* `Type`: categoría temática de la noticia.

La variable `url` se utiliza únicamente como referencia y no forma parte de las características utilizadas por el modelo.

---

## Fuente de los datos

El dataset utilizado en este proyecto fue obtenido de **Kaggle**, a partir del conjunto de datos:

**Spanish News Classification**

El dataset contiene noticias en español clasificadas en diferentes categorías temáticas y fue seleccionado para abordar un problema de clasificación multiclase mediante técnicas de NLP y Deep Learning.

### Dataset original

* **Nombre:** Spanish News Classification
* **Plataforma:** Kaggle
* **Fuente:** [Spanish News Classification — Kaggle](https://www.kaggle.com/datasets/kevinmorgado/spanish-news-classification)

A partir del dataset original se realizó un proceso de análisis exploratorio, limpieza, normalización y preparación de los datos antes de utilizarlos para el entrenamiento de los modelos.

Para facilitar la reproducibilidad del proyecto, se incorporó una copia del dataset utilizado al repositorio de GitHub.

---

## Categorías

El dataset utilizado contiene noticias clasificadas en siete categorías:

* Macroeconomia
* Alianzas
* Innovacion
* Regulaciones
* Sostenibilidad
* Otra
* Reputacion

---

## Dataset utilizado

El conjunto de datos utilizado inicialmente contiene **1.217 registros y 3 variables**:

| Variable | Descripción                      |
| -------- | -------------------------------- |
| `url`    | URL o referencia de la noticia   |
| `news`   | Contenido textual de la noticia  |
| `Type`   | Categoría temática de la noticia |

Durante el proceso de limpieza se realizaron diferentes tareas de preparación de los datos.

Entre ellas:

* eliminación de registros duplicados exactos;
* eliminación de registros con contenido textual vacío;
* tratamiento de URLs presentes en los textos;
* conversión del texto a minúsculas;
* eliminación de espacios innecesarios;
* eliminación de caracteres especiales;
* verificación de valores nulos;
* verificación de inconsistencias en las categorías.

Luego del proceso de limpieza, el dataset quedó conformado por **1.140 noticias**.

---

## Metodología

El proyecto se desarrolló siguiendo las siguientes etapas:

1. Análisis exploratorio de los datos.
2. Identificación de valores nulos y duplicados.
3. Análisis de la distribución de las categorías.
4. Análisis de la longitud de los textos.
5. Limpieza y normalización del contenido textual.
6. Separación de las variables predictoras y objetivo.
7. Codificación numérica de las categorías.
8. División de los datos en conjuntos de entrenamiento y prueba.
9. Tokenización del texto.
10. Conversión de los textos en secuencias numéricas.
11. Aplicación de padding.
12. Construcción del modelo base.
13. Entrenamiento y evaluación del modelo.
14. Análisis mediante métricas de clasificación y matriz de confusión.
15. Desarrollo de un segundo modelo con una arquitectura más compleja.
16. Comparación de los resultados obtenidos.

Para favorecer la reproducibilidad de los resultados se estableció una configuración fija de los procesos aleatorios utilizados durante el entrenamiento.

---

## Modelo de Deep Learning

### Modelo base

La arquitectura del modelo base está compuesta por:

```text
Embedding
    ↓
GlobalAveragePooling1D
    ↓
Dense(64, activation="relu")
    ↓
Dropout(0.5)
    ↓
Dense(7, activation="softmax")
```

El modelo utiliza:

* `Embedding` para representar las palabras mediante vectores numéricos.
* `GlobalAveragePooling1D` para reducir la representación de las secuencias.
* Una capa `Dense` de 64 neuronas.
* `Dropout` para ayudar a reducir el sobreajuste.
* Una capa final `Softmax` para realizar la clasificación entre las siete categorías.

---

### Modelo mejorado

Como parte de la profundización del proyecto se desarrolló una segunda arquitectura con mayor capacidad de representación:

```text
Embedding
    ↓
GlobalAveragePooling1D
    ↓
Dense(128, activation="relu")
    ↓
Dropout(0.4)
    ↓
Dense(64, activation="relu")
    ↓
Dropout(0.3)
    ↓
Dense(7, activation="softmax")
```

El objetivo fue evaluar experimentalmente si una arquitectura más compleja podía mejorar el desempeño de clasificación.

Para ambos modelos se utilizó:

* Optimizador: `Adam`
* Función de pérdida: `sparse_categorical_crossentropy`
* Métrica principal: `Accuracy`
* `EarlyStopping` para controlar el entrenamiento.

---

## Evaluación

Los modelos fueron evaluados utilizando el conjunto de prueba, compuesto por **228 noticias**.

Las principales métricas utilizadas fueron:

* Accuracy
* Precision
* Recall
* F1-score

También se utilizaron matrices de confusión para analizar el comportamiento de los modelos en cada categoría.

---

## Resultados

Los resultados obtenidos fueron:

| Modelo          |   Accuracy |
| --------------- | ---------: |
| Modelo base     | **66,67%** |
| Modelo mejorado | **64,47%** |

El modelo base obtuvo el mejor Accuracy general.

La modificación de la arquitectura produjo una disminución de **2,20 puntos porcentuales** en el Accuracy.

Sin embargo, el análisis de las métricas por categoría mostró que la modificación de la arquitectura produjo cambios diferentes según la clase.

El modelo base obtuvo los siguientes resultados principales:

| Categoría      | F1-score |
| -------------- | -------: |
| Alianzas       |     0,62 |
| Innovacion     |     0,79 |
| Macroeconomia  |     0,87 |
| Otra           |     0,00 |
| Regulaciones   |     0,55 |
| Reputacion     |     0,00 |
| Sostenibilidad |     0,65 |

La categoría **Macroeconomia** presentó el mejor desempeño, mientras que **Otra** y **Reputacion** presentaron importantes dificultades de clasificación.

En particular, `Reputacion` contó solamente con **5 registros en el conjunto de prueba**, lo que representa una limitación importante para el aprendizaje de patrones representativos de dicha categoría.

---

## Conclusiones

Los resultados obtenidos muestran que es posible utilizar técnicas de **NLP y Deep Learning** para clasificar automáticamente noticias según su contenido textual.

El modelo base alcanzó un Accuracy de **66,67%**, mientras que el modelo mejorado obtuvo un **64,47%**.

Por lo tanto, en este experimento el incremento de la complejidad de la arquitectura no produjo una mejora en el desempeño general.

Este resultado demuestra que aumentar la cantidad de capas o neuronas de una red neuronal no garantiza necesariamente una mejor capacidad de generalización.

Las principales dificultades identificadas están relacionadas no solamente con la arquitectura del modelo, sino también con las características del dataset, especialmente:

* desbalance entre categorías;
* cantidad reducida de ejemplos en determinadas clases;
* diferencias en la dificultad de identificación de cada categoría.

---

## Limitaciones y trabajos futuros

Entre las principales limitaciones del proyecto se encuentran:

* tamaño relativamente reducido del dataset;
* desbalance entre las categorías;
* baja cantidad de ejemplos disponibles para algunas clases;
* diferencias importantes en el desempeño entre categorías.

Como posibles líneas futuras de trabajo se plantea:

* aumentar la cantidad de noticias disponibles;
* balancear las categorías;
* utilizar técnicas de aumento de datos;
* experimentar con diferentes arquitecturas de redes neuronales;
* ajustar hiperparámetros;
* utilizar técnicas de NLP más avanzadas;
* comparar con otros modelos de clasificación;
* evaluar modelos específicos para procesamiento de textos en español.

---

## Tecnologías utilizadas

* **Python**
* **Pandas**
* **NumPy**
* **Matplotlib**
* **Seaborn**
* **Scikit-learn**
* **TensorFlow**
* **Keras**
* **NLP**
* **Deep Learning**

---

## Estructura del repositorio

```text
proyecto_final_data_science_III/
│
├── proyecto_final_data_science_lll_diego_godoy.ipynb
├── df_total.csv
├── README.md
└── requirements.txt
```

---

## Instalación

Clonar el repositorio:

```bash
git clone https://github.com/Diegoagodoy/proyecto_final_data_science_III.git
```

Ingresar al directorio:

```bash
cd proyecto_final_data_science_III
```

Instalar las dependencias:

```bash
pip install -r requirements.txt
```

Para ejecutar el proyecto se puede abrir el notebook:

```bash
jupyter notebook
```

---

## Autor

**Diego Adolfo Godoy**

Proyecto realizado como parte de la:

**Diplomatura en Data Science III - CODERHOUSE - https://app.coderhouse.com/

---

## Repositorio

El código, notebook y dataset utilizado para el proyecto se encuentran disponibles en:

https://github.com/Diegoagodoy/proyecto_final_data_science_III
