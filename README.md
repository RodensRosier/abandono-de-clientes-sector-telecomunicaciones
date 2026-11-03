# Abandono de Clientes (Telecomunicaciones)

## Objetivo del Proyecto
Este proyecto está enfocado en identificar y predecir qué clientes tienen mayor probabilidad de cancelar su servicio de telefonía móvil (*Churn*), analizando los factores claves asociados a la fuga para permitir la planificación oportuna de estrategias proactivas de retención.

---

## Problema de Negocio
En el sector de las telecomunicaciones, el costo de adquisición de un nuevo cliente es significativamente más elevado que el costo de mantener a uno existente. Retener a los clientes en riesgo mediante campañas personalizadas requiere identificar preventivamente quiénes se irán, entender los motivos subyacentes y priorizar los recursos en aquellos clientes con mayor riesgo y valor financiero.

---

## Fuente de Datos
- **Dataset obtenido de Kaggle:** Dataset de telecomunicaciones Cell2Cell (`https://www.kaggle.com/datasets/carlosolmosfuentes/cell2cell-churn-xgboost-optuna`).
- **Volumen:** 51,047 registros de clientes y 58 variables descriptivas.
- **Variable Objetivo:** `Churn` (Distribución desbalanceada: 71.18% Retención / 28.82% Abandono).

---

## Stack Tecnológico
- **Lenguaje:** Python 3.15.2
- **Procesamiento de Datos:** Pandas, NumPy
- **Visualización:** Matplotlib, Seaborn
- **Modelado & Machine Learning:** Scikit-Learn (Logistic Regression, Random Forest Classifier)
- **Evaluación & Métrica:** ROC-AUC Score, Classification Report, Confusion Matrix
- **Persistencia de Modelos:** Joblib

---

## Metodología y Enfoque

### Análisis Exploratorio de Datos (EDA) y Limpieza (`01_eda_y_limpieza.ipynb`)
- **Auditoría de Datos:** Inspección de estructura, tipos de datos y porcentaje exacto de nulos por columna.
- **Tratamiento de Nulos y Duplicados:** Imputación de variables numéricas utilizando la **mediana** y variables categóricas mediante la **moda**. Verificación de ausencia de registros duplicados.
- **Análisis Descriptivo:** Evaluación del desbalance de clases de la variable `Churn` e inspección visual de ingresos, antigüedad y minutos consumidos.
- **Exportación:** Generación del dataset limpio (`cell2cell_churn_clean.csv`).

### Ingeniería de Características y Modelado (`02_ingenieria_de_caracteristicas.ipynb.ipynb`)
- **Transformación de Variables:** Conversión de la variable `Churn` a formato binario (1/0) y aplicación de *One-Hot Encoding* para variables categóricas.
- **Limpieza de Identificadores:** Exclusión de columnas de alta cardinalidad o no informativas como `CustomerID` y `ServiceArea`.
- **Partición Estratificada:** División de los datos en 80% entrenamiento y 20% prueba manteniendo la proporción de clases (`stratify=y`).
- **Entrenamiento y Manejo de Desbalance:**
  - **Línea base:** Regresión Logística estándar.
  - **Regresión Logística Balanceada:** Uso de `class_weight='balanced'`.
  - **Modelo Final:** **Random Forest Classifier** ajustado con `class_weight='balanced'`.
- **Interpretabilidad:** Evaluación del *Feature Importance* para determinar los principales inductores de abandono (`CurrentEquipmentDays`, `MonthsInService`, consumo mensual, entre otros).
- **Exportación:** Guardado del modelo entrenado en `modelo_churn_random_forest.pkl`.

---

## Resultados e Impacto de Negocio
- **Capacidad Predictiva:** El modelo final de Random Forest con balanceo de clases logró un **ROC-AUC Score sólido**, superando la línea base y demostrando alta sensibilidad para detectar la clase minoritaria (`Churn = 1`).
- **Factores Clave de Riesgo:** Se identificó que la antigüedad del equipo actual (`CurrentEquipmentDays`) y los meses totales en el servicio (`MonthsInService`) son los factores más determinantes en la probabilidad de fuga.
- **Aplicación Práctica:** La salida del modelo permite segmentar a los clientes según su nivel de riesgo y exportar las predicciones para activar campañas proactivas antes de que se efectúe la cancelación del servicio.

---

## Estructura del Repositorio
```text
abandono-de-clientes-sector-telecomunicaciones/
│
├── dashboard/
│   ├── screenshot/           # Capturas de pantalla de KPIs y el dashboard de Tableau
│
├── data/
│   ├── raw/                  # cell2celltrain.csv (dataset original)
│   └── processed/            # cell2cell_churn_clean.csv (dataset limpio)
│
├── notebooks/
│   ├── 01_eda_and_cleaning_3.ipynb         # Auditoría, limpieza y EDA
│   └── 02_feature_engineering_3.ipynb      # Ingeniería de características, entrenamiento/pruebas y Machine Learning
│
├── .gitignore
├── LICENSE                    # Licencia del proyecto
├── README.md                  # Documentación principal
└── requirements.txt           # Dependencias de Python necesarias
```
---

## Cómo Ejecutar el Proyecto
1. Clonar el repositorio:
   ```bash
   git clone https://github.com/rodensrosier/abandono-de-clientes-sector-telecomunicaciones.git
   cd abandono-de-clientes-sector-telecomunicaciones
   ```
2. Instalar las dependencias:
   ```bash
   pip install -r requirements.txt
   ```
3. Ejecutar los notebooks en la carpeta `notebooks/` para revisar el flujo completo de análisis y modelado.
