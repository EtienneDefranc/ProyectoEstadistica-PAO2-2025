# Documentación del Proyecto: Análisis de Consumo de Energía en Vehículos Eléctricos

Este documento resume los hallazgos de la investigación temporal realizada en el notebook `matriz-de-correlacion.ipynb`. El objetivo principal fue analizar las correlaciones entre diversas variables y el consumo de energía (`Energy_Consumption_kWh`) en el dataset `EV_Energy_Consumption_Cleaned.csv`.

## Resumen de la Investigación

### 1. Análisis de Correlación
Se calculó la matriz de correlación para evaluar la relación entre las variables numéricas y detectar posible multicolinealidad.

**Hallazgos Clave:**
*   **Temperatura vs. Temperatura de Batería**: La correlación entre `Temperature_C` y `Battery_Temperature_C` es de **-0.04**. Al ser este valor bajo (y menor al umbral de 0.8), se concluye que no existe riesgo de multicolinealidad significativa.
    *   *Recomendación*: Mantener ambas variables en el modelo.
*   **Velocidad vs. Consumo**: Se observó una correlación positiva fuerte de **0.79** entre `Speed_kmh` y `Energy_Consumption_kWh`.
*   **Distancia vs. Consumo**: Se ha determinado una **correlación importante** (ligera relación lineal) entre `Distance_Travelled_km` y `Energy_Consumption_kWh`. A mayor distancia recorrida, se evidencia un aumento proporcional en el consumo de energía.

### 2. Análisis Visual (Scatterplots)

#### Velocidad vs. Consumo de Energía
Se generó un diagrama de dispersión para visualizar la relación entre la velocidad (`Speed_kmh`) y el consumo (`Energy_Consumption_kWh`). La fuerte correlación numérica sugiere que la velocidad es un factor determinante en el consumo energético.

#### Distancia Recorrida vs. Consumo de Energía
Se analizó la relación entre `Distance_Travelled_km` y `Energy_Consumption_kWh`.

**Hallazgos:**
*   Existe una **clara relación lineal positiva**. A medida que aumenta la distancia recorrida, el consumo de energía aumenta proporcionalmente.
*   Este comportamiento es esperado y lógico para el uso de vehículos eléctricos.
