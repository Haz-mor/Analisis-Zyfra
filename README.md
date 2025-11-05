# PROTOTIPO DE MODELO ML PARA LA RECUPERACIÓN DE ORO (ZYFRA) 🥇

> Desarrollo de un prototipo de modelo de Machine Learning para predecir la cantidad de oro recuperado (coeficiente de recuperación) del mineral, con el objetivo de optimizar la producción y eliminar parámetros no rentables para la empresa Zyfra.

---

## 📝 Descripción Breve del Proyecto y su Objetivo

Este proyecto aborda un problema de Machine Learning de regresión proporcionado por **Zyfra**. El objetivo es construir un modelo que pueda predecir el **coeficiente de recuperación** del oro en dos etapas clave del proceso de purificación:

1.  La recuperación del **concentrado rougher** (`rougher.output.recovery`).
2.  La recuperación **final** del concentrado (`final.output.recovery`).

El modelo entrenado busca ayudar a **optimizar la producción** y a **eliminar los parámetros no rentables** del proceso de extracción de la mina.

---

## 📂 Fuentes de Datos Utilizadas

Los datos provienen de la extracción y purificación del mineral de oro y están divididos en tres archivos, indexados por fecha y hora (`date`):

* `gold_recovery_train.csv`: Conjunto de datos de entrenamiento.
* `gold_recovery_test.csv`: Conjunto de datos de prueba (sin objetivos de recuperación).
* `gold_recovery_full.csv`: Dataset fuente que contiene ambos conjuntos con todas las características.

### Proceso Tecnológico (Flotación y Purificación)
El proceso incluye:
* **Flotación (`rougher`):** Tratamiento primario para obtener el concentrado *rougher*.
* **Purificación (`cleaner`):** Dos etapas de limpieza (primaria y secundaria) para obtener el concentrado final.

---

## ⚙️ Metodología o Pasos del Análisis

El proyecto siguió una metodología rigurosa en tres fases:

### 1. Preparación de Datos
* **Validación de Recuperación:** Se verificó la corrección de la recuperación calculada para la característica `rougher.output.recovery` usando la fórmula de recuperación y se confirmó un Error Absoluto Medio (EAM) insignificante, validando la integridad de los datos.
* **Análisis de Ausencias:** Se identificaron características ausentes en el set de prueba y se realizó la imputación de valores faltantes utilizando el último valor conocido.

### 2. Análisis de Datos (EDA)
* **Concentración de Metales (Au, Ag, Pb):** Se observó el cambio de concentración a través de las etapas del proceso.
* **Distribución de Partículas:** Se compararon las distribuciones del tamaño de las partículas de alimentación (*feed size*) entre los conjuntos de entrenamiento y prueba.
* **Análisis de Anomalías:** Se examinaron las concentraciones totales de sustancias en diferentes etapas para identificar y eliminar valores anómalos.

### 3. Construcción y Entrenamiento del Modelo
* **Métrica sMAPE:** Se implementó una función personalizada para calcular la métrica de evaluación clave.
* **Modelado y Cross-Validation:** Se entrenaron modelos de Regresión Lineal, Árbol de Decisión y Bosque Aleatorio, y se evaluaron con **validación cruzada (KFold)** para seleccionar el mejor.
* **Prueba Final:** El modelo campeón se probó utilizando la muestra de prueba para obtener el resultado final de la métrica.

---

## 🛠️ Herramientas y Librerías Empleadas

* **Lenguaje:** Python
* **Librerías Clave:**
    * `pandas`: Manipulación y análisis de datos.
    * `numpy`: Operaciones numéricas y vectoriales.
    * `matplotlib` / `seaborn`: Visualización de datos y EDA.
    * `sklearn` (Scikit-learn): Implementación de modelos (`RandomForestRegressor`, `LinearRegression`), escalado (`StandardScaler`) y métricas.

---

## 📈 Resultados y Conclusiones

### Hallazgos del Análisis de Datos (EDA)
1.  **Concentración de Metales:** La **concentración de oro (Au) aumenta consistentemente** en cada etapa, confirmando la eficacia del proceso de purificación. La concentración de plata (Ag) **disminuye drásticamente** después de la flotación.
2.  **Distribución de Partículas:** La distribución del tamaño de las partículas de alimentación (*feed size*) es **muy similar** entre los conjuntos de entrenamiento y prueba, lo cual asegura que la evaluación del modelo será **correcta** al ser representativas la una de la otra.

### Rendimiento del Modelo

La métrica final combina las predicciones de las dos etapas con diferentes pesos (sMAPE final $= 25\% \times sMAPE_{rougher} + 75\% \times sMAPE_{final}$).

* **Modelo Seleccionado:** **Bosque Aleatorio (RandomForestRegressor)**, el cual mostró el error porcentual más bajo en la validación cruzada.
* **sMAPE Obtenido en Cross-Validation (Línea Base):** **~6.11%** (significativamente mejor que la línea base de 10.03%).
* **sMAPE Final del Modelo (Prueba en Test):** **8.43%**

#### Conclusión
El prototipo de **RandomForest (sMAPE de 8.43%)** es un éxito, siendo significativamente superior a simplemente usar el promedio (sMAPE de 10.03%). El modelo está listo para ser presentado como un prototipo viable para Zyfra.

---

## 🖼️ Ejemplo de Ejecución / Visualizaciones

### 1. Concentración de Metales por Etapa

![Concentración promedio de metales por etapa](URL_A_TU_IMAGEN_DE_CONCENTRACION_DE_METALES)

* **Descripción:** Este gráfico muestra la **evolución de la concentración promedio de metales**. La concentración de oro (Au) aumenta en cada etapa, mientras que la plata (Ag) se reduce progresivamente, lo que valida la calidad de los datos y la lógica del proceso de purificación.

### 2. Distribución del Tamaño de Partículas

![Comparación de la distribución del tamaño de partículas](URL_A_TU_IMAGEN_DE_DISTRIBUCION_DE_PARTICULAS)

* **Descripción:** La superposición de las curvas de densidad Kernel (KDE) demuestra que la distribución del tamaño de las partículas de alimentación es **homogénea** entre los conjuntos de entrenamiento y prueba, lo que garantiza la fiabilidad del entrenamiento del modelo.
