# Impacto de métodos de imputación de datos faltantes en el rendimiento de modelos declasificación supervisada : comparación entre KNN y métodos tradicionales

**Universidad Nacional de Ingeniería — Facultad de Ciencias**  
**Escuela Profesional de Ciencia de la Computación**

| | |
|---|---|
| **Autora** | Bianca Merchán Torres |
| **Asesor** | Américo Chulluncuy |
| **Dataset** | [Water Potability Dataset](https://www.kaggle.com/datasets/adityakadiwal/water-potability) — Kaggle |
| **Lenguaje** | Python 3.12 |
| **Reproducibilidad** | `random_seed = 42` en todo el pipeline |

---

## Descripción

Este repositorio contiene el código, datos y documento LaTeX de la tesis que evalúa el impacto de dos métodos de imputación de datos faltantes —**imputación por media** e **imputación KNN**— sobre el rendimiento de un modelo de clasificación supervisada (Regresión Logística), usando el *Water Potability Dataset* como banco de pruebas.

El experimento introduce un **10 % de valores faltantes de forma controlada** bajo el mecanismo **MCAR** (*Missing Completely At Random*) y compara las métricas **Accuracy** y **F1-score** entre ambos métodos de imputación.

---

## Estructura del proyecto

```
Tesis-1/
├── data/
│   ├── raw/                          # Dataset original sin modificar
│   │   └── water_potability.csv
│   └── processed/                    # Datasets imputados generados por el experimento
│       ├── water_potability_media.csv
│       └── water_potability_knn.csv
│
├── notebooks/
│   ├── 01_exploracion.ipynb          # Análisis exploratorio de datos (EDA)
│   └── 02_experimento.ipynb          # Pipeline completo: imputación → clasificación → evaluación
│
├── results/
│   ├── figures/                      # Gráficos generados automáticamente
│   │   ├── boxplots_potabilidad.png
│   │   ├── comparacion_metodos.png
│   │   ├── confusion_Imputación_KNN.png
│   │   ├── confusion_Imputación_por_Media.png
│   │   ├── correlacion.png
│   │   ├── distribucion_clases.png
│   │   ├── distribucion_potabilidad.png
│   │   ├── histogramas_variables.png
│   │   └── mapa_faltantes.png
│   └── tables/
│       └── comparacion_metodos.csv   # Accuracy y F1-score por método
│
├── tesis/
│   ├── main.tex                      # Documento principal LaTeX
│   ├── chapters/
│   │   ├── 01_introduccion.tex
│   │   ├── 02_marco_teorico.tex
│   │   └── 03_metodologia.tex
│   ├── frontmatter/
│   │   └── portada.tex
│   └── bibliography/
│       └── references.bib            # Referencias en formato BibTeX (APA via biblatex)
│
├── requirements.txt
└── README.md
```

---

## Dataset

El **Water Potability Dataset** contiene mediciones fisicoquímicas de muestras de agua y una etiqueta binaria de potabilidad.

| Atributo | Tipo | Descripción |
|---|---|---|
| `ph` | Numérico | Nivel de pH del agua |
| `Hardness` | Numérico | Dureza (capacidad de precipitar jabón) |
| `Solids` | Numérico | Sólidos disueltos totales (ppm) |
| `Chloramines` | Numérico | Cantidad de cloraminas (ppm) |
| `Sulfate` | Numérico | Cantidad de sulfatos disueltos (mg/L) |
| `Conductivity` | Numérico | Conductividad eléctrica (μS/cm) |
| `Organic_carbon` | Numérico | Carbono orgánico total (ppm) |
| `Trihalomethanes` | Numérico | Trihalometanos (μg/L) |
| `Turbidity` | Numérico | Turbidez (NTU) |
| `Potability` | Binario | **Variable objetivo**: 1 = potable, 0 = no potable |

> Las columnas `ph`, `Sulfate` y `Trihalomethanes` presentan valores faltantes en el dataset original.

### Distribución de clases

| Clase | Cantidad | Porcentaje |
|---|---|---|
| No potable (0) | 1 998 | 61.0 % |
| Potable (1) | 1 278 | 39.0 % |

El dataset presenta **desbalance de clases**. Se aplicó `class_weight='balanced'` en la Regresión Logística para compensarlo.

---

## Diseño experimental

El pipeline sigue cinco etapas secuenciales:

1. **Análisis exploratorio** — distribuciones, correlaciones, valores atípicos y valores faltantes preexistentes (`01_exploracion.ipynb`).
2. **Introducción controlada de faltantes** — se introduce un 10 % de valores faltantes de forma aleatoria (mecanismo MCAR) sobre la matriz de características, *después* del split para evitar data leakage.
3. **Imputación** — se aplican los dos métodos evaluados de forma independiente sobre train y test.
4. **Clasificación** — Regresión Logística con estandarización (`StandardScaler`) y `class_weight='balanced'`.
5. **Evaluación** — Accuracy, F1-score, reporte por clase y matriz de confusión.

### Parámetros clave

| Parámetro | Valor |
|---|---|
| Semilla aleatoria | `42` |
| Split train/test | 80 % / 20 % (estratificado) |
| Mecanismo de ausencia | MCAR |
| Proporción de faltantes introducidos | 10 % |
| Vecinos KNN (`k`) | 5 |
| Iteraciones máximas (Regresión Logística) | 1 000 |

---

## Métodos de imputación evaluados

### Imputación por media
Reemplaza cada valor faltante con la media aritmética del atributo calculada sobre el conjunto de entrenamiento. Eficiente computacionalmente, pero reduce la varianza y destruye correlaciones entre variables.

### Imputación KNN (k = 5)
Estima cada valor faltante como el promedio de los $k$ vecinos más cercanos (distancia euclidiana) que presentan ese atributo completo. Preserva la estructura local del dataset y las correlaciones entre variables, a mayor costo computacional.

---

## Resultados

### Métricas por método de imputación

| Método | Accuracy | F1-score |
|---|---|---|
| Imputación por Media | 0.6113 | 0.0078 |
| Imputación KNN | 0.6113 | 0.0078 |

### Análisis de resultados

Ambos métodos arrojaron resultados idénticos. Esto se explica por tres factores:

1. **Desbalance de clases** — con ~61 % de muestras no potables, el clasificador tendía a predecir sistemáticamente la clase mayoritaria, produciendo Accuracy alto pero F1-score cercano a 0.
2. **Inseparabilidad lineal** — las variables fisicoquímicas presentan distribuciones similares entre clases, lo que limita la capacidad discriminativa de la Regresión Logística.
3. **Bajo porcentaje de faltantes** — con solo un 10 % de valores faltantes introducidos, la diferencia entre métodos de imputación es mínima cuando el modelo base no es suficientemente sensible a pequeñas variaciones en los datos.

### Figuras generadas

| Archivo | Descripción |
|---|---|
| `comparacion_metodos.png` | Gráfico comparativo de Accuracy y F1-score |
| `distribucion_clases.png` | Distribución de la variable objetivo |
| `distribucion_potabilidad.png` | Distribución de potabilidad (EDA) |
| `histogramas_variables.png` | Histogramas de las 9 variables del dataset |
| `correlacion.png` | Mapa de calor de correlaciones |
| `boxplots_potabilidad.png` | Boxplots por clase para cada variable |
| `mapa_faltantes.png` | Mapa visual de valores faltantes |
| `confusion_Imputación_por_Media.png` | Matriz de confusión — Media |
| `confusion_Imputación_KNN.png` | Matriz de confusión — KNN |

---

## Documento LaTeX

La tesis está escrita en LaTeX con `biblatex` (estilo APA, backend `biber`) y está estructurada en:

| Archivo | Contenido |
|---|---|
| `tesis/main.tex` | Documento principal; ensambla todos los capítulos |
| `chapters/01_introduccion.tex` | Contexto, problema de investigación, objetivos e hipótesis |
| `chapters/02_marco_teorico.tex` | Datos faltantes, mecanismos MCAR/MAR/MNAR, imputación, Regresión Logística, métricas |
| `chapters/03_metodologia.tex` | Descripción del dataset, diseño experimental, métodos, clasificador y métricas |
| `frontmatter/portada.tex` | Portada institucional |
| `bibliography/references.bib` | Referencias bibliográficas (Bishop 2006, Little & Rubin 2019, Troyanskaya 2001, entre otras) |

Para compilar el documento LaTeX:

```bash
cd tesis
pdflatex main.tex
biber main
pdflatex main.tex
pdflatex main.tex
```

---

## Requisitos

- Python 3.12
- Jupyter Notebook

```bash
pip install -r requirements.txt
```

Dependencias principales: `pandas`, `numpy`, `scikit-learn`, `seaborn`, `matplotlib`, `scipy`.

---

## Cómo reproducir el experimento

### 1. Clonar el repositorio

```bash
git clone https://github.com/TU_USUARIO/Tesis-1.git
cd Tesis-1
```

### 2. Crear y activar el entorno virtual

```bash
python -m venv venv

# Windows
venv\Scripts\activate

# Mac / Linux
source venv/bin/activate
```

### 3. Instalar dependencias

```bash
pip install -r requirements.txt
```

### 4. Ejecutar los notebooks en orden

**Paso 1 — Análisis exploratorio:**
```bash
jupyter notebook notebooks/01_exploracion.ipynb
```

**Paso 2 — Experimento de imputación y evaluación:**
```bash
jupyter notebook notebooks/02_experimento.ipynb
```

> Ejecutar todas las celdas en orden con **Run All**.

Los resultados (figuras y tabla CSV) se generan automáticamente en `results/`.

---

## Notas importantes

- La imputación se realiza **después** del split train/test para evitar data leakage.
- El mecanismo de ausencia simulado es **MCAR** con un 10 % de valores faltantes.
- La semilla `random_seed = 42` se aplica en todo el pipeline para garantizar reproducibilidad total.
- Los trabajos futuros propuestos incluyen: evaluar mecanismos MAR y MNAR, comparar múltiples clasificadores, optimizar hiperparámetros y explorar métodos de imputación más sofisticados (imputación múltiple, modelos generativos).

---

## Referencias clave

- Little, R. J. A. & Rubin, D. B. (2019). *Statistical Analysis with Missing Data* (3.ª ed.). Wiley.
- Troyanskaya, O. et al. (2001). Missing value estimation methods for DNA microarrays. *Bioinformatics*, 17(6), 520–525.
- García, S., Luengo, J. & Herrera, F. (2017). *Data Preprocessing in Data Mining*. Springer.
- Pedregosa, F. et al. (2011). Scikit-learn: Machine Learning in Python. *JMLR*, 12, 2825–2830.
- Japkowicz, N. & Stephen, S. (2002). The class imbalance problem: A systematic study. *Intelligent Data Analysis*, 6(5), 429–449.
