# Tesis: Impacto de Métodos de Imputación de Datos Faltantes sobre el Rendimiento de Modelos de Clasificación Supervisada

**Universidad Nacional de Ingeniería**  
**Autor:** Bianca Merchán Torres
**Asesor:**  Americo Chucunlluy

---

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

| Método | Accuracy | F1-score |
|---|---|---|
| Imputación por Media | 0.6113 | 0.0078 |
| Imputación KNN | 0.6113 | 0.0078 |

---

##  Notas importantes

- La semilla de aleatoriedad usada es `random_seed = 42` para garantizar reproducibilidad.
- El split train/test es 80/20 con estratificación.
- El mecanismo de ausencia simulado es **MCAR** con un 10% de valores faltantes.
- La imputación se realiza **después** del split para evitar data leakage.
