# Detección de Fraude en Tarjetas de Crédito
Este proyecto aborda el problema de detección de transacciones fraudulentas en tarjetas de crédito, utilizando modelos de Machine Learning y técnicas de balanceo de clases. 
Se emplean métodos de detección de anomalías y estrategias de muestreo para mejorar la identificación de fraudes en un conjunto de datos altamente desbalanceado.

## 1. Contexto y Objetivo
El conjunto de datos utilizado proviene de [Kaggle - Credit Card Fraud Detection](https://www.kaggle.com/mlg-ulb/creditcardfraud), y contiene **284,807 transacciones**, de las cuales solo **492** son fraudulentas (**0.17% del total**). La naturaleza del problema implica los siguientes desafíos:

- **Desbalance de clases extremo:** Un modelo convencional tiende a sesgarse hacia la clase mayoritaria.
- **Minimización de falsos negativos:** En aplicaciones reales, es preferible un modelo que **maximice el recall**, ya que un fraude no detectado representa un costo alto.
- **Control de falsos positivos:** Un modelo que clasifica erróneamente demasiadas transacciones legítimas como fraudulentas es inviable en producción.

Este proyecto de exploración busca desarrollar un modelo que **maximice la detección de fraudes sin comprometer la precisión operativa**.

---
## 2. Metodología

### 2.1. Preprocesamiento
- Eliminación de valores nulos y variables irrelevantes.
- Normalización con `StandardScaler` para mejorar el rendimiento del modelo.
- División en conjunto de **entrenamiento (70%)** y **prueba (30%)**.

### 2.2. Modelos Evaluados
Se compararon diferentes enfoques para abordar el problema:

1. **Regresión Logística Baseline**  
   - Modelo inicial sin ajuste para el desbalance de clases.  
   - **Problema:** Alto accuracy pero baja capacidad de detección de fraudes.

2. **Detección de Anomalías**  
   - **Minimum Covariance Determinant (MCD)**  
   - **Isolation Forest**  
   - Enfoque centrado en identificar transacciones atípicas en los datos.

3. **Estrategias de Balanceo de Clases** 
   - **Cost-Sensitive Training:** Baja precisión, por ende muchas transacciones legítimas son clasificadas como fraude.   
   - **Undersampling:** Reducción de la clase mayoritaria.  
   - **Oversampling:** Aumento de la clase minoritaria.  
   - **SMOTE:** Generación de datos sintéticos para mejorar la representación de la clase fraudulenta.  

Cada técnica se evaluó en términos de su impacto en la capacidad del modelo para detectar fraudes.
---

## 3. Evaluación y Resultados

| Modelo | Accuracy | Recall | Precision | F1-score | Especificidad |
|--------|---------|--------|-----------|---------|---------|
| Regresión Logística | 99.92% | 62.5% | 87.6% | 72.9% | 99.9% |
| MCD | 99.93% | 80.88% | 80.3% | 80.58% | 99.9% |
| Isolation Forest | 99.83% | 80.88% | 72.3% | 76.3% | 99.95% |
| Cost-Sensitive | 97.42% | 92.64% | 5.43% | 10.27% | 97.43% |
| Undersamplig | 96.07% | 93.38% | 3.66% | 7.04% | 96.08% |
| Oversampling | 97.40% | 92.64% | 5.39% | 10.19% | 97.4% |
| SMOTE  | 97.25% | 92.64% | 5.12% | 9.71% | 97.26% |

**Análisis de resultados:**
- **La Regresión Logística convencional es inadecuada** debido a su bajo recall (62.5%), lo que implica una alta tasa de fraudes no detectados.
- **El modelo basado en MCD obtiene un mejor equilibrio**, con un recall del **80.88%**, lo que representa una mejora significativa en la detección de fraudes.
- **SMOTE + Regresión Logística es la mejor alternativa**, con un **recall del 87.5%**, aunque con un ligero aumento en los falsos positivos.

Dado el contexto del problema, donde la prioridad es minimizar los fraudes no detectados, **el modelo basado en SMOTE y Regresión Logística se considera la mejor opción**.

---
## 4. Conclusiones

- **El desbalance de clases afecta significativamente la capacidad del modelo** para identificar fraudes, lo que justifica el uso de técnicas de muestreo.
- **El uso de detección de anomalías (MCD) es efectivo**, pero estrategias basadas en balanceo de clases (SMOTE) logran un mejor desempeño.
- **El modelo debe ser validado en datos de producción**, considerando métricas adicionales como la tasa de falsos positivos y su impacto en la operativa de un sistema financiero.

---

## 5. Requerimientos y Ejecución
Este proyecto se desarrolló en Python y requiere las siguientes bibliotecas:
- pandas
- numpy
- scikit-learn
- matplotlib
- seaborn
