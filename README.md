# PROYECTO 1.
# Análisis del proyecto: Predicción de trastornos de sueño

## 1. Descripción y contexto del problema

Este proyecto aborda un problema de clasificación binaria en el dominio de la salud del sueño. El objetivo es predecir si un individuo se siente descansado tras dormir (`felt_rested = 1`) o no descansado (`felt_rested = 0`) a partir de variables fisiológicas, conductuales y ambientales.

Datos principales:
- Dataset de aproximadamente 100,000 muestras.
- Variables incluidas: duración y calidad del sueño, estrés, latencia del sueño, etapa REM, sueño profundo, consumo de cafeína y alcohol, tiempo de pantalla, actividad física, temperatura ambiental, cronotipo, condición de salud mental, frecuencia cardíaca, turnos de trabajo y más.
- Problema de negocio: identificar preventivamente a personas con riesgo de mal descanso para permitir intervenciones tempranas en contextos clínicos o de salud móvil.

Contexto científico:
- La variable objetivo es altamente relevante para la medicina del sueño.
- El enfoque prioriza minimizar falsos negativos, ya que no detectar un mal descanso puede tener un alto costo clínico.
- Benchmarking referenciado: accuracy 85-95% en datasets similares de Kaggle/UCI; AUC-ROC 0.92-0.97 en literatura especializada.

## 2. Métodos de trabajo

### 2.1 Preprocesamiento y manejo de datos

- Carga del dataset y revisión básica de estructura.
- Detección de valores nulos en el dataset.
- Eliminación de variables irrelevantes o de identificación, como `person_id`.
- Codificación de variables categóricas con `LabelEncoder`.
- Selección manual de features: se descartan columnas como `country`, `occupation`, `day_type`, `season`, `caffeine_mg_before_bed`, `alcohol_units_before_bed`, `chronotype`, `mental_health_condition`, `gender`, `sleep_aid_used`, `exercise_day` y `shift_work`.

### 2.2 División de datos y balanceo

- División de datos en entrenamiento y prueba con `train_test_split(test_size=0.2, stratify=y, random_state=42)`.
- Uso de SMOTE solo en el conjunto de entrenamiento para corregir el desbalance de clases.
- Normalización de features con `StandardScaler` entrenado únicamente sobre el conjunto de entrenamiento.

### 2.3 Modelado y evaluación

Se entrenaron múltiples modelos con una evaluación uniforme:
- Regresión Logística
- K-Nearest Neighbors (KNN)
- Naive Bayes Gaussiano
- Árbol de Decisión
- SVM (muestra de 15,000 registros por costo computacional)
- Random Forest
- XGBoost
- AdaBoost
- Gradient Boosting
- Extra Trees
- Bagging
- Stacking Classifier con regresión logística como meta-modelo

La evaluación se basa en métricas clave:
- Accuracy (train y test)
- Precision
- Recall
- F1-Score
- AUC-ROC
- Gap de overfitting (`Accuracy_Train - Accuracy_Test`)

### 2.4 Validación y umbral de decisión

- Validación cruzada estratificada `k=10` para evaluar estabilidad de modelos.
- Ajuste explícito del umbral de decisión para priorizar `Recall`, buscando reducir falsos negativos.
- Interpretabilidad con SHAP sobre el mejor modelo basado en árbol.

## 3. Resultados y respaldo gráfico

### 3.1 Balance de clases y análisis exploratorio

- El dataset presenta un desbalance moderado: aproximadamente 60.9% de clase 0 y 39.1% de clase 1.
- Gráfico de pastel de proporción de clases respalda el análisis de desbalance.
- Histogramas por clase para variables numéricas principales muestran diferencias de distribución relevantes en `sleep_duration_hrs`, `sleep_quality_score`, `stress_score`, entre otras.
- Boxplots revelan outliers significativos en variables como consumo de cafeína, latencia de sueño y duración del sueño.

### 3.2 Correlación y características clave

- El análisis de correlación mostró que las variables más asociadas con `felt_rested` son:
  - `sleep_quality_score`
  - `cognitive_performance_score`
  - `stress_score`
  - `work_hours_that_day`
  - `sleep_disorder_risk`
  - `rem_percentage`
  - `steps_that_day`
- La matriz de correlación presenta un triángulo inferior para evitar redundancia visual.
- Este análisis respalda la selección de features y la importancia fisiológica de las variables.

### 3.3 Desempeño de modelos

- El notebook compara modelos en una tabla con métricas de test ordenadas por F1-Score.
- Las métricas claves de comparación incluyen Accuracy_Test, Precision, Recall y F1-Score.
- Las gráficas de barras representando métricas comparativas permiten interpretar qué modelos equilibran mejor precisión y sensibilidad.

### 3.4 Overfitting y estabilidad

- La diferencia entre `Accuracy_Train` y `Accuracy_Test` se utiliza para detectar overfitting.
- Se generan curvas de aprendizaje para Random Forest y XGBoost, mostrando la convergencia entre desempeño de entrenamiento y validación.
- La validación cruzada k=10 con modelos clave demuestra la estabilidad de métricas de F1 y accuracy.

### 3.5 Umbral de decisión

- La búsqueda de umbral muestra cómo varían Accuracy, Precision, Recall y F1 con valores entre 0.20 y 0.65.
- El umbral óptimo elegido fue `0.35`, priorizando Recall sobre precisión, lo cual es consistente con el objetivo clínico.

### 3.6 Interpretabilidad con SHAP

- SHAP se utiliza para explicar el mejor modelo basado en árbol (XGBoost).
- Los plots de resumen y de barras respaldan la importancia global de features.
- Las dependencias de las tres variables top (`sleep_quality_score`, `cognitive_performance_score`, `stress_score`) confirman su influencia sobre la predicción.


## 4. Conclusión
Las gráficas respaldan los resultados clave:
- Desbalance de clases y su corrección mediante SMOTE;
- Distribuciones y outliers importantes en variables de sueño;
- Correlaciones con la variable objetivo;
- Desempeño comparativo de algoritmos;
- Curvas de aprendizaje y estabilidad en validación cruzada;
- Ajuste de umbral con énfasis en recall;
- Interpretabilidad con SHAP de las características más influyentes.
