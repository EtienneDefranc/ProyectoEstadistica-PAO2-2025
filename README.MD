# Documentación del Proyecto: Análisis de Consumo de Energía en Vehículos Eléctricos

Este documento resume los hallazgos de la investigación estadística realizada sobre el dataset `EV_Energy_Consumption_Cleaned.csv`. El objetivo principal fue analizar los factores que influyen en el consumo de energía (`Energy_Consumption_kWh`) en vehículos eléctricos y construir un modelo predictivo.

---

## Estructura del Proyecto

| Notebook | Descripción |
|----------|-------------|
| `analisis-exploratorio-descriptivo.ipynb` | Análisis exploratorio de datos (EDA), estadísticos descriptivos y detección de valores atípicos. |
| `analisis-bivariado-inferencial.ipynb` | Análisis de correlaciones y pruebas de hipótesis (Pearson). |
| `construccion-modelo-predictivo.ipynb` | Construcción, evaluación y selección del modelo de regresión lineal múltiple. |
| `evaluacion-validacion.ipynb` | Validación cruzada y evaluación final del modelo. |

---

## 1. Análisis Exploratorio y Descriptivo

### 1.1 Estructura del Dataset
- El dataset contiene registros de viajes en vehículos eléctricos con variables como velocidad, distancia, estado de batería, temperatura, humedad, condiciones climáticas y de tráfico.
- **Variables Numéricas:** `Speed_kmh`, `Distance_Travelled_km`, `Battery_State_%`, `Temperature_C`, `Battery_Temperature_C`, `Humidity_%`, `Energy_Consumption_kWh`, entre otras.
- **Variables Categóricas:** `Driving_Mode`, `Road_Type`, `Traffic_Condition`, `Weather_Condition`.

### 1.2 Estadísticos Descriptivos
Se calcularon medidas de tendencia central (media, mediana) y dispersión (desviación estándar, rango intercuartílico) para todas las variables numéricas.

### 1.3 Distribuciones
- Se generaron **histogramas** para cada variable numérica, observando la forma de las distribuciones.
- Se identificó el **sesgo (skewness)** de las variables para determinar si requerían transformaciones (Log, Sqrt).

### 1.4 Detección de Valores Atípicos
- Se utilizaron **boxplots** para identificar valores extremos en cada variable.
- Variables como `Distance_Travelled_km` y `Energy_Consumption_kWh` mostraron presencia de outliers que fueron considerados en el análisis.

---

## 2. Análisis Bivariado e Inferencial

### 2.1 Matriz de Correlación
Se calculó la matriz de correlación de Pearson entre todas las variables numéricas.

**Hallazgos Clave:**
| Relación | Correlación (r) | Interpretación |
|----------|-----------------|----------------|
| `Speed_kmh` vs `Energy_Consumption_kWh` | **0.79** | Correlación positiva fuerte |
| `Distance_Travelled_km` vs `Energy_Consumption_kWh` | Positiva significativa | A mayor distancia, mayor consumo |
| `Temperature_C` vs `Battery_Temperature_C` | **-0.04** | Sin multicolinealidad |

### 2.2 Análisis Visual (Scatterplots con Regresión)
Se generaron diagramas de dispersión con línea de tendencia para las variables:
- `Battery_State_%` vs `Energy_Consumption_kWh`
- `Humidity_%` vs `Energy_Consumption_kWh`
- `Distance_Travelled_km` vs `Energy_Consumption_kWh`

### 2.3 Pruebas de Hipótesis (Correlación de Pearson)

Se evaluó la significancia estadística de las correlaciones con nivel α = 0.05.

**Hipótesis:**
- $H_0: \rho = 0$ (No hay correlación)
- $H_1: \rho \neq 0$ (Existe correlación significativa)

| Variable | Correlación (r) | P-valor | Resultado |
|----------|-----------------|---------|-----------|
| `Battery_State_%` | Significativa | < 0.05 | Rechazo $H_0$ |
| `Humidity_%` | Significativa | < 0.05 | Rechazo $H_0$ |
| `Distance_Travelled_km` | Significativa | < 0.05 | Rechazo $H_0$ |

**Conclusión:** Las tres variables muestran correlación estadísticamente significativa con el consumo de energía.

---

## 3. Construcción del Modelo Predictivo

### 3.1 Metodología
- **Tipo de Modelo:** Regresión Lineal Múltiple (OLS - Ordinary Least Squares)
- **Variable Objetivo:** `Energy_Consumption_kWh`
- **Variables Predictoras:** `Battery_State_%`, `Humidity_%`, `Distance_Travelled_km`
- **División de Datos:** 80% Entrenamiento / 20% Prueba (random_state=42)

### 3.2 Verificación de Supuestos

| Supuesto | Método de Verificación | Resultado |
|----------|------------------------|-----------|
| **Linealidad** | Gráfico Residuos vs Predichos | Patrón aleatorio alrededor de cero |
| **Homocedasticidad** | Gráfico Residuos vs Predichos | Varianza aproximadamente constante |
| **Normalidad de Residuos** | Histograma + Q-Q Plot + Test Shapiro-Wilk | Distribución aproximadamente normal |
| **No Multicolinealidad** | Factor de Inflación de Varianza (VIF) | VIF < 5 para todas las variables |

### 3.3 Selección del Mejor Modelo (Criterio AIC)

Se probaron **todas las combinaciones posibles** de las variables predictoras (7 modelos en total) y se compararon usando:
- **AIC (Criterio de Información de Akaike):** Menor es mejor
- **BIC (Criterio de Información Bayesiano):** Menor es mejor
- **R² Ajustado:** Mayor es mejor

**Resultado:** El modelo con las 3 variables (`Battery_State_%`, `Humidity_%`, `Distance_Travelled_km`) obtuvo el menor AIC, siendo seleccionado como el modelo óptimo.

### 3.4 Métricas de Evaluación (Test Set)

| Métrica | Valor | Interpretación |
|---------|-------|----------------|
| **R² (Coeficiente de Determinación)** | 0.3120 | El modelo explica el 31.20% de la variabilidad del consumo |
| **RMSE (Error Cuadrático Medio)** | 1.8266 kWh | Error promedio de predicción |
| **AIC** | 16222.61 | Criterio de selección de modelo |
| **BIC** | 16247.79 | Criterio de selección de modelo |

### 3.5 Interpretación de Coeficientes

| Variable | Coeficiente | Std Error | P-valor | Intervalo de Confianza (95%) |
|----------|-------------|-----------|---------|------------------------------|
| `const` (Intercepto) | 6.9162 | 0.123 | 0.000 ✓ | [6.676, 7.156] |
| `Battery_State_%` | -0.0213 | 0.001 | 0.000 ✓ | [-0.024, -0.019] |
| `Humidity_%` | 0.0184 | 0.001 | 0.000 ✓ | [0.016, 0.021] |
| `Distance_Travelled_km` | 0.0718 | 0.002 | 0.000 ✓ | [0.068, 0.076] |

**Interpretación de los coeficientes:**
- **Intercepto (6.92 kWh):** Consumo base estimado cuando todas las variables predictoras son 0.
- **Battery_State_% (-0.0213):** Por cada 1% de aumento en el estado de batería, el consumo **disminuye** en 0.0213 kWh (relación inversa).
- **Humidity_% (+0.0184):** Por cada 1% de aumento en humedad, el consumo **aumenta** en 0.0184 kWh.
- **Distance_Travelled_km (+0.0718):** Por cada km adicional recorrido, el consumo **aumenta** en 0.0718 kWh.

> **Nota sobre Matriz de Confusión:** Esta métrica no aplica para problemas de regresión (valores continuos). Es exclusiva para problemas de clasificación (categorías).

---

## 4. Limitaciones del Modelo

1. **Supuesto de Linealidad:** El modelo asume relaciones lineales. Relaciones curvas o complejas no son capturadas adecuadamente.

2. **Variables Omitidas:** El consumo real depende de factores no incluidos en el dataset:
   - Estilo de conducción del usuario
   - Uso de aire acondicionado/calefacción
   - Elevación del terreno
   - Presión de neumáticos
   - Carga del vehículo

3. **Rango de Datos (Extrapolación):** Las predicciones solo son válidas dentro de los rangos observados en entrenamiento. Valores extremos pueden generar errores significativos.

4. **Multicolinealidad Residual:** Aunque el VIF es aceptable, la correlación entre variables puede dificultar la interpretación aislada de cada coeficiente.

5. **Generalización:** Si los datos provienen de condiciones controladas o simulaciones, el modelo podría no generalizar perfectamente a condiciones reales de conducción.

---

## 5. Conclusiones

1. **Las variables `Battery_State_%`, `Humidity_%` y `Distance_Travelled_km` tienen una relación estadísticamente significativa** con el consumo de energía en vehículos eléctricos.

2. **El modelo de regresión lineal múltiple** permite predecir el consumo de energía con un nivel de precisión aceptable, capturando las tendencias generales de los datos.

3. **La distancia recorrida es el factor más influyente** en el consumo de energía, lo cual es consistente con la física del movimiento vehicular.

4. **Para mejorar la capacidad predictiva**, se recomienda:
   - Incluir variables adicionales (temperatura ambiente, velocidad promedio, tipo de ruta).
   - Explorar modelos no lineales (Random Forest, Gradient Boosting) si el R² del modelo lineal es insuficiente.
   - Recolectar datos en condiciones de conducción diversas ("in-the-wild").

---

## Referencias

- Dataset: `EV_Energy_Consumption_Cleaned.csv`
- Librerías utilizadas: `pandas`, `numpy`, `matplotlib`, `seaborn`, `statsmodels`, `scikit-learn`, `scipy`
- Metodología: Regresión Lineal Múltiple con selección de variables por AIC
