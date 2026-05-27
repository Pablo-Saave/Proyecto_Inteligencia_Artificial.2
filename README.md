# Evaluación 2 — Modelos de Regresión
**Asignatura:** Inteligencia Artificial  
**Carrera:** Ingeniería Civil en Informática  
**Universidad:** Universidad del Bío-Bío, sede Concepción  

## Integrantes

| Leandro Flores | leandro.flores2201@alumnos.ubiobio.cl |
| Pablo Águila | pablo.aguila1901@alumnos.ubiobio.cl |
| Pablo Saavedra | pablo.saavedra2201@alumnos.ubiobio.cl |

---

## Descripción

Este proyecto desarrolla modelos de regresión para predecir las **ventas semanales** de una cadena nacional de tiendas retail. El objetivo es apoyar la toma de decisiones operativas como optimización de inventario, planificación de campañas promocionales y ajuste de recursos según demanda esperada.

---

## Estructura del repositorio

```
Proyecto_Inteligencia_Artificial.2/
├── README.md
├── data/
│   ├── ingestion/
│   │   └── retail_ventas.csv       # Dataset original
│   └── cleaned/                    # Datos limpios y transformados
└── notebooks/
    └── E2-Regresion.ipynb          # Notebook principal de la evaluación
```

---

## Dataset

El archivo `retail_ventas.csv` contiene información semanal de múltiples tiendas con las siguientes variables:

| Variable | Descripción |
|---|---|
| `id_tienda` | Identificador único de la tienda |
| `tamano_tienda` | Tamaño en metros cuadrados |
| `antiguedad_tienda` | Años desde su apertura |
| `temperatura` | Temperatura promedio semanal (°C) |
| `precio_combustible` | Precio promedio del combustible |
| `indice_economico` | Indicador económico general (similar al CPI) |
| `semana` | Número de semana del año (1–52) |
| `es_feriado` | Si la semana contiene feriado (1 = sí, 0 = no) |
| `clientes_estimados` | Número estimado de clientes en la semana |
| `tipo_tienda` | Categoría: A (grande), B (mediana), C (pequeña) |
| `ventas_semanales` | **Variable objetivo** a predecir |

---

## Desarrollo del notebook

### 1. Importación de librerías
Carga de pandas, numpy, matplotlib, seaborn y módulos de scikit-learn.

### 2. Carga de datos
El dataset se descarga directamente desde el repositorio con `wget` para garantizar reproducibilidad en cualquier entorno.

### 3. Exploración del dataset
- Revisión de dimensiones, tipos de variables y estadísticas descriptivas
- Detección de valores nulos y distribución de variables
- Separación de variables predictoras (`X`) y variable objetivo (`y`)
- División en conjuntos de entrenamiento (80%) y prueba (20%)

### 4. Pipeline de preprocesamiento
Se construye un `ColumnTransformer` con tratamiento diferenciado:
- **Variables numéricas:** imputación con mediana + `StandardScaler`
- **Variable categórica (`tipo_tienda`):** imputación con moda + `OneHotEncoder`

El preprocesamiento se aprende solo con datos de entrenamiento para evitar *data leakage*.

### 5. Modelamiento
Se entrenan dos modelos mediante pipelines completos:
- **LinearRegression:** regresión lineal sin regularización
- **Ridge:** regresión con regularización L2, probando alphas = 0.1, 1.0 y 10.0

### 6. Evaluación de modelos
Comparación con métricas **MAE** y **R²**, incluyendo visualización de valores reales vs. predichos.

### 7. Interpretación de resultados
Análisis de coeficientes del modelo para identificar las variables más influyentes (`tamano_tienda`, `clientes_estimados`, `indice_economico`) e interpretación del impacto de `tipo_tienda`.

### 8. Predicción con nuevos registros
Aplicación del mejor modelo a tres escenarios hipotéticos: tienda tipo A (grande), B (mediana en feriado) y C (pequeña).

### 9. Pregunta de reflexión
Justificación de cuándo preferir Ridge sobre LinearRegression considerando multicolinealidad, estabilidad y complejidad del modelo.

---

## Requisitos

```
pandas >= 1.5.0
numpy >= 1.23.0
matplotlib >= 3.6.0
seaborn >= 0.12.0
scikit-learn >= 1.2.0
```

Desarrollado con **Python 3.12**.