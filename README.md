# Tesis: Impacto de Métodos de Imputación de Datos Faltantes sobre el Rendimiento de Modelos de Clasificación Supervisada

**Universidad Nacional de Ingeniería**  
**Autor:** Bianca Merchán Torres
**Asesor:**  Americo Chucunlluy

---

````
##  Estructura del proyecto
Tesis-1/
├── data/
│   ├── raw/                  # Dataset original sin modificar
│   └── processed/            # Datasets imputados generados por el experimento
├── notebooks/
│   ├── 01_exploracion.ipynb  # Análisis exploratorio del dataset
│   └── 02_experimento.ipynb  # Experimento de imputación y evaluación
├── results/
│   ├── figures/              # Gráficos generados por el código
│   └── tables/               # Tablas de resultados en CSV
├── thesis/
│   ├── main.tex              # Documento principal LaTeX
│   ├── chapters/             # Capítulos de la tesis
│   ├── frontmatter/          # Portada
│   └── bibliography/         # Referencias en formato BibTeX
├── requirements.txt          # Dependencias del proyecto
└── README.md                 # Este archivo

````

---

##  Requisitos

- Python 3.12
- Las dependencias están listadas en `requirements.txt`

---

##  Cómo reproducir el experimento

### 1. Clonar el repositorio
```bash
git clone https://github.com/TU_USUARIO/Tesis-1.git
cd Tesis-1
```

### 2. Crear y activar el entorno virtual
```bash
# Crear entorno
python -m venv venv

# Activar en Windows
venv\Scripts\activate

# Activar en Mac/Linux
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

**Paso 2 — Experimento de imputación:**
```bash
jupyter notebook notebooks/02_experimento.ipynb
```

> Ejecutar todas las celdas en orden con **Run All**.

### 5. Resultados generados automáticamente

| Archivo | Descripción |
|---|---|
| `results/figures/comparacion_metodos.png` | Gráfico comparativo de métricas |
| `results/figures/distribucion_potabilidad.png` | Distribución de la variable objetivo |
| `results/figures/histogramas_variables.png` | Histogramas de todas las variables |
| `results/figures/correlacion.png` | Mapa de calor de correlaciones |
| `results/tables/comparacion_metodos.csv` | Tabla con Accuracy y F1-score por método |

---

##  Resultados
### Distribución de clases
| Clase | Cantidad | Porcentaje |
|---|---|---|
| No potable (0) | 1998 | 61.0% |
| Potable (1) | 1278 | 39.0% |

>  El dataset presenta desbalance de clases. Se aplicó `class_weight='balanced'`
> en la Regresión Logística para que el modelo no ignore la clase minoritaria.

### Métricas por método de imputación
| Método | Accuracy | F1-score |
|---|---|---|
| Imputación por Media | 0.6113 | 0.0078 |
| Imputación KNN | 0.6113 | 0.0078 |

### Análisis de resultados
Ambos métodos arrojaron resultados idénticos. Esto se explica por tres razones:

1. **Desbalance de clases:** con ~61% de muestras no potables, el modelo
   tendía a predecir siempre la clase mayoritaria, produciendo Accuracy
   alto pero F1-score cercano a 0.
2. **Naturaleza del dataset:** las variables fisicoquímicas tienen
   distribuciones similares entre clases, lo que dificulta la separación
   lineal con Regresión Logística.
3. **Impacto de la imputación:** con solo un 10% de valores faltantes,
   la diferencia entre métodos puede ser mínima cuando el modelo base
   no es suficientemente sensible a pequeñas variaciones en los datos.

### Figuras generadas
| Archivo | Descripción |
|---|---|
| `results/figures/comparacion_metodos.png` | Gráfico comparativo Accuracy y F1-score |
| `results/figures/distribucion_clases.png` | Distribución de clases del dataset |
| `results/figures/confusion_Imputación_por_Media.png` | Matriz de confusión — Media |
| `results/figures/confusion_Imputación_KNN.png` | Matriz de confusión — KNN |
| `results/figures/distribucion_potabilidad.png` | Distribución variable objetivo |
| `results/figures/histogramas_variables.png` | Histogramas de todas las variables |
| `results/figures/correlacion.png` | Mapa de calor de correlaciones |

---

##  Notas importantes

- La semilla de aleatoriedad usada es `random_seed = 42` para garantizar reproducibilidad.
- El split train/test es 80/20 con estratificación.
- El mecanismo de ausencia simulado es **MCAR** con un 10% de valores faltantes.
- La imputación se realiza **después** del split para evitar data leakage.
