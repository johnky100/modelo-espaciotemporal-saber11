# Modelo Espaciotemporal GNN–TimeSeries para Predicción Saber 11 (2015–2026)

Este proyecto desarrolla un modelo espaciotemporal que combina Redes Neuronales de Grafos (GNN) y modelos avanzados de series temporales para analizar el rendimiento académico del examen Saber 11 en Colombia (2015–2022) y generar predicciones para los años 2023–2026.  
La solución integra imputación avanzada, construcción de grafos educativos, entrenamiento con GraphSAGE, evaluación con métricas especializadas y proyección multianual del puntaje global.

---

## Objetivo General

Construir un modelo unificado que utilice tanto la estructura relacional entre estudiantes, municipios y atributos (GNN) como la dinámica temporal del rendimiento (series temporales), con el fin de predecir el puntaje Saber 11 para los años 2023–2026.

---

## Metodología General (Fases del Proyecto)

### Fase 1. Adquisición y Preprocesamiento
- Se utiliza la base oficial IFS–AV11 del ICFES (2015–2022).
- Se realiza depuración, estandarización, eliminación de duplicados y selección de variables relevantes.
- Se conserva únicamente la información necesaria para la predicción del puntaje global.

### Fase 2. Imputación y Tratamiento de Valores Ausentes
Proceso realizado en R utilizando MICE.  
Se aplican métodos según el tipo de variable:

| Tipo de variable | Método MICE |
|------------------|-------------|
| Continuas        | pmm         |
| Ordinales        | polr        |
| Binarias         | logreg      |
| Numéricas        | pmm         |

Se genera una base imputada, completa y lista para modelamiento.

### Fase 3. Construcción del Grafo Educativo (2015–2022)
En Python, se construyen grafos anuales que representan relaciones entre municipios, departamentos y variables educativas.  
Se generan estructuras:

- `data.x`: matriz de atributos  
- `data.edge_index`: conectividad kNN  
- Grafos almacenados por año  

### Fase 4. Entrenamiento de la Arquitectura GNN
Modelo principal: GraphSAGE.  
Métricas de evaluación:

- Coeficiente de determinación (R²)  
- MAE  
- RMSE  
- MedAE  

Se evalúa el rendimiento anual y la capacidad del modelo para capturar patrones educativos estructurales.

### Fase 5. Modelos de Series Temporales (Predicción 2023–2026)
Se construyen series multianuales y se entrenan modelos de forecasting para proyectar el puntaje Saber 11.  
Modelos utilizados:

- DeepState  
- DeepFactor  
- Modelos TimeSeries en Python  

### Fase 6. Dashboard Final en Tableau
Se desarrollará un dashboard interactivo que incluirá:

- Tendencias históricas 2015–2022  
- Predicciones 2023–2026  
- Mapa departamental  
- Resultados GNN  
- Estructura del grafo educativo  

Este dashboard será publicado en las próximas horas.

---

## Dataset Utilizado

Nombre: Resultados únicos Saber 11  
Fuente: Instituto Colombiano para la Evaluación de la Educación – ICFES  
Años utilizados: 2015–2022  
Enlace oficial:  
https://www.datos.gov.co/Educaci-n/Resultados-nicos-Saber-11/kgxf-xxbe/about_data

---

## Tecnologías y Librerías Utilizadas

### Lenguaje R
- mice  
- tidyverse  
- data.table  
- janitor  

### Python (GNN y Forecasting)
- PyTorch  
- PyTorch Geometric (GraphSAGE)  
- NumPy  
- Pandas  
- Scikit-Learn  
- GluonTS o librerías equivalentes de series temporales  

### Visualización
- Tableau (dashboard final)

---

```bash
Saber_11_2025/
│
├── data/                                   # Datos usados en el pipeline completo
│   ├── 1_chunks/                           # Datos crudos descargados desde datos.gov.co
│   ├── 2_sinDuplicados/                    # Datos tras la auditoría inicial (sin duplicados)
│   ├── 3_DB_imp/                           # Bases imputadas (MICE: PMM, POLR, LORED) por año
│   ├── 4_modelado/                         # Datos modelados y consolidados (2015–2022)
│   └── 5_grafos/                           # Grafos PyTorch Geometric (.pt) generados en Notebook 1
│
├── notebook_R/                             # Procesamiento, limpieza y auditoría en R
│   ├── auditoria_imputacion_2015.Rmd
│   ├── auditoria_imputacion_2016.Rmd
│   ├── auditoria_imputacion_2017.Rmd
│   ├── auditoria_imputacion_2018.Rmd
│   ├── auditoria_imputacion_2019.Rmd
│   ├── auditoria_imputacion_2020.Rmd
│   ├── auditoria_imputacion_2021.Rmd
│   └── auditoria_imputacion_2022.Rmd
│
├── notebook_python/                         # Modelado en Python: GNN + series temporales
│   ├── Notebook_1_Grafos.ipynb              # Construcción de grafos por departamento (2015–2022)
│   ├── Notebook_2_GraphSAGE_Entrenamiento.ipynb   # Entrenamiento del modelo GraphSAGE global
│   ├── Notebook_3_LSTM_Temporal.ipynb              # Modelo temporal (LSTM) para predicción 2023–2026
│   └── Notebook_4_Decoder_Embeddings_Puntajes.ipynb # Conversión de embeddings → puntajes Saber 11
│
├── modelos/                                 # Modelos entrenados (PyTorch)
│   ├── model_sage.pt                        # Modelo GraphSAGE entrenado en 2015–2022
│   ├── model_lstm.pt                        # Modelo temporal que predice embeddings 2023–2026
│   └── decoder_embeddings_puntajes.pt       # Decoder final (embeddings → puntajes)
│
├── resultados/                              # Resultados y reportes finales
│   ├── embeddings_2015_2022.csv             # Embeddings históricos generados por GraphSAGE
│   ├── predicciones_2023_2026_embeddings.pt # Embeddings proyectados para 2023–2026
│   ├── puntajes_2023_2026.csv               # Puntajes Saber 11 predichos mediante el decoder
│   └── comparacion_reales_vs_pred.xlsx       # Comparación reales vs proyectados
│
├── dashboard/                               # Dashboard final para visualización (Tableau)
│   ├── dashboard_saber11_espaciotemporal.twbx
│   └── capturas/                            # Capturas e imágenes para documentación
│
└── README.md                                # Documentación general del proyecto

```


Esta estructura se completará a medida que se carguen los avances finales.

---

## Estado Actual del Proyecto

Este repositorio se encuentra en fase de consolidación.  
Los notebooks finales, el dashboard y la documentación extendida serán publicados en las próximas horas.

---

## Autor

John Prado  
Proyecto académico y científico basado en datos abiertos.

---

## Licencia

Este proyecto está bajo licencia MIT, que permite su uso, modificación y redistribución con la debida atribución.

---

## Cómo citar este trabajo

Prado, J. (2025). Modelo Espaciotemporal GNN–TimeSeries para Predicción Saber 11 (2015–2026). Datos Abiertos – GitHub.
