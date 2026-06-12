### Evaluación 3 · Inteligencia Artificial · Ingeniería Civil en Informática
**Universidad del Bío-Bío — Sede Concepción**

---

## Integrantes

| Leandro Flores | leandro.flores2201@alumnos.ubiobio.cl |
| Pablo Águila | pablo.aguila1901@alumnos.ubiobio.cl |
| Pablo Saavedra | pablo.saavedra2201@alumnos.ubiobio.cl |

---

## Estructura del repositorio

```
620454/
│
├── README.md                        # Identificación del equipo y descripción del repositorio
│
├── data/
│   ├── ingestion/                   # Datos crudos originales
│   │   └── data_fraude.csv          # Dataset principal de transacciones
│   └── cleaned/                     # Datos limpios y transformados (generados por el notebook)
│
└── notebooks/
    ├── Proyecto_Inteligencia_Artificial,_leandro_F,Pablo_S,Pablo_Á   # Notebook de la evaluación anterior 
    └── E3-Clasificacion.ipynb       # Notebook principal de esta evaluación
```

> El notebook carga el dataset directamente desde la carpeta `data/ingestion/`.

---

## Dataset

El dataset contiene registros de transacciones financieras con las siguientes variables:

**Variables numéricas continuas**
- `transaction_amount` — monto de la transacción
- `account_balance` — saldo disponible en la cuenta
- `transaction_time_seconds` — hora del día expresada en segundos
- `avg_transaction_amount_7d` — promedio de transacciones de los últimos 7 días
- `std_transaction_amount_7d` — variabilidad reciente del monto transaccionado

**Variables numéricas discretas**
- `transactions_last_1h` — número de transacciones en la última hora
- `transactions_last_24h` — número de transacciones en las últimas 24 horas
- `failed_attempts` — intentos fallidos previos de autenticación
- `num_devices_used` — cantidad de dispositivos distintos utilizados

**Variables categóricas**
- `transaction_type` — tipo de transacción (`payment`, `transfer`, `withdrawal`, `purchase`)
- `device_type` — dispositivo utilizado (`mobile`, `desktop`, `tablet`)
- `location_region` — región geográfica (`urban`, `suburban`, `rural`)
- `is_foreign_transaction` — indica si la transacción es extranjera (0/1)
- `is_high_risk_country` — indica si el país de origen es de alto riesgo (0/1)

**Variable objetivo**
- `is_fraud` — `1` si la transacción es fraudulenta, `0` si es legítima

---

## Metodología

### Parte 1 — Preprocesamiento
- Identificación y separación de variables numéricas y categóricas
- Detección y corrección de valores inconsistentes (negativos, fuera de rango)
- Verificación de nulos y duplicados
- Tratamiento de valores atípicos mediante **Winsorización** (transformador personalizado)
- Encoding de variables categóricas con `OrdinalEncoder` y `OneHotEncoder`
- Escalamiento con `StandardScaler`
- Filtro de colinealidad mediante transformador personalizado `CorrelationFilter`
- División en conjuntos de entrenamiento y prueba (`train_test_split` con estratificación)

### Parte 2 — Entrenamiento y optimización
- Entrenamiento de `LogisticRegression` y `DecisionTreeClassifier` dentro de pipelines de scikit-learn
- Optimización de hiperparámetros con `GridSearchCV` y validación cruzada estratificada (`StratifiedKFold`, 5 folds)
- Métrica de optimización: **F1-score** (apropiada para datos desbalanceados)
- Comparación de rendimiento entre modelos

### Parte 3 — Evaluación base
- Predicciones con umbral por defecto (`threshold = 0.5`)
- Cálculo de Accuracy, Precision, Recall, F1-score y ROC-AUC
- Matriz de confusión e interpretación en contexto de fraude

### Parte 4 — Ajuste de umbral
- Evaluación del modelo con umbrales `0.3`, `0.5` y `0.7` usando `predict_proba`
- Análisis comparativo del impacto en falsos positivos, falsos negativos, precisión y recall
- Generación de archivos CSV con predicciones y probabilidades para cada umbral
- Decisión de negocio justificada en función del riesgo de fraude y el impacto en clientes

### Parte 5 — Conclusiones
- Identificación del modelo con mejor desempeño
- Análisis de variables más influyentes
- Propuestas de mejora al sistema de detección

---

## Requisitos de software

El proyecto fue desarrollado con **Python 3.12**. Las bibliotecas necesarias son:

```
pandas >= 1.5.0
numpy >= 1.23.0
matplotlib >= 3.6.0
seaborn >= 0.12.0
scikit-learn >= 1.2.0
```

Para verificar la versión de una librería instalada:

```python
import pandas as pd
print(pd.__version__)
```

---

## Cómo ejecutar el notebook

1. Clonar el repositorio:
   ```bash
   git clone <url-del-repositorio>
   cd 620454
   ```

2. Instalar las dependencias:
   ```bash
   pip install pandas numpy matplotlib seaborn scikit-learn
   ```

3. Abrir el notebook principal:
   ```bash
   jupyter notebook notebooks/E3-Clasificacion.ipynb
   ```

4. Ejecutar todas las celdas en orden. El dataset se carga automáticamente desde `data/ingestion/data_fraude.csv`.

---

## Archivos de salida generados

El notebook genera automáticamente los siguientes archivos CSV con las predicciones del mejor modelo:

| Archivo | Descripción |
|---|---|
| `predicciones_threshold_0.3.csv` | Predicciones y probabilidades con umbral 0.3 |
| `predicciones_threshold_0.5.csv` | Predicciones y probabilidades con umbral 0.5 |
| `predicciones_threshold_0.7.csv` | Predicciones y probabilidades con umbral 0.7 |

---

## Notas

- El desbalance de clases fue uno de los principales desafíos del proyecto. Se utilizó `class_weight='balanced'` para mitigar su efecto en el entrenamiento.
- La selección de variables predictoras se basó en la correlación con la variable objetivo, usando un umbral de correlación de `0.08` dado el bajo poder predictivo individual de las features disponibles.
- Se implementaron dos transformadores personalizados (`Winsorizer` y `CorrelationFilter`) compatibles con las interfaces de scikit-learn para su integración dentro de pipelines reproducibles.

---

*Fecha de creación: Junio 2026 — Versión 1.1*
