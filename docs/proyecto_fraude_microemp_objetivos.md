# DISEÑO DEL PROYECTO

## Sistema de detección y priorización de fraude para emprendedores y microempresarios

### Contexto del negocio

Una empresa financiera ficticia, inspirada en el tipo de operación de organizaciones que apoyan financieramente a emprendedores, microempresarios y pequeños empresarios, dispone de información histórica sobre clientes y transacciones financieras.

La organización ha identificado operaciones potencialmente fraudulentas, pero actualmente necesita transformar los datos históricos en información que permita comprender el comportamiento del fraude, identificar perfiles y operaciones de mayor riesgo y construir una primera solución predictiva que apoye el monitoreo.

La base utilizada en este proyecto es **100 % sintética/dummy** y fue creada exclusivamente con fines académicos y de portafolio. No contiene información real de Interactuar ni de clientes reales.

La base contiene 60.000 transacciones y 26 variables relacionadas con características demográficas, financieras, transaccionales, geográficas y de seguridad. La variable objetivo es `FRAUDE`.

---

# PROBLEMA DE NEGOCIO

La empresa no conoce con suficiente precisión qué características y comportamientos están asociados a las operaciones fraudulentas. Esto dificulta:

- Identificar tempranamente transacciones potencialmente fraudulentas.
- Priorizar los casos que deben ser revisados por los analistas.
- Definir reglas de monitoreo basadas en evidencia.
- Optimizar los recursos destinados a investigación.
- Reducir el impacto económico potencial del fraude.

El reto consiste en transformar los datos históricos en conocimiento accionable y posteriormente desarrollar un modelo de Machine Learning que permita estimar la probabilidad de fraude de una nueva operación.

---

# OBJETIVO GENERAL

Desarrollar un proyecto de Data Science que permita identificar patrones asociados al fraude financiero en operaciones de emprendedores, microempresarios y pequeños empresarios, mediante técnicas de **Business Analytics, análisis exploratorio de datos y Machine Learning**, con el propósito de generar un mecanismo de priorización de operaciones de riesgo.

---

# OBJETIVOS ESPECÍFICOS

1. Comprender la estructura y calidad de los datos históricos.
2. Analizar el comportamiento de las operaciones fraudulentas y legítimas.
3. Identificar variables y perfiles asociados con una mayor incidencia de fraude.
4. Analizar el desbalance existente entre operaciones fraudulentas y no fraudulentas.
5. Realizar limpieza y preparación de los datos.
6. Construir variables derivadas que permitan representar mejor el comportamiento financiero y transaccional.
7. Entrenar diferentes modelos de clasificación.
8. Comparar los modelos utilizando métricas apropiadas para un problema de fraude.
9. Identificar las variables que más contribuyen a la predicción.
10. Construir un esquema de clasificación o scoring de riesgo.
11. Traducir los resultados técnicos en recomendaciones de negocio.
12. Documentar el proyecto utilizando un flujo reproducible en **Conda + VS Code + GitHub + GitHub Copilot**.

---

# METODOLOGÍA

El proyecto seguirá una combinación de **Discovery + Business Analytics + CRISP-DM**, adaptada a un proyecto de portafolio de Data Science.

## 1. Discovery

### Entendimiento del negocio

Preguntas principales:

- ¿Qué entendemos por una operación fraudulenta?
- ¿Cuál es el impacto de una operación fraudulenta?
- ¿Qué operaciones deberían ser priorizadas para revisión?
- ¿Qué costo tendría revisar demasiadas operaciones legítimas?
- ¿Es más importante detectar la mayor cantidad posible de fraudes o reducir las falsas alarmas?
- ¿Qué información debería recibir un analista de riesgos?

### Hipótesis iniciales

Se plantea explorar si el fraude presenta mayor incidencia cuando existen combinaciones como:

- Dispositivo nuevo.
- Cambio reciente de clave.
- Beneficiario nuevo.
- Múltiples intentos fallidos.
- Operaciones realizadas en horarios inusuales.
- Distancias geográficas elevadas respecto al comportamiento esperado.
- Montos significativamente superiores al promedio histórico.
- Clientes con poca antigüedad.
- Alta cantidad de transacciones en un mismo día.
- Determinados canales o tipos de transacción.

Estas hipótesis deben ser **validadas mediante los datos**, no asumidas como verdaderas.

---

# DATOS

## Archivo principal

`data/raw/base_fraude_interactuar_microempresarios_60000.csv`

La información es sintética y contiene:

- 60.000 registros.
- 26 variables.
- Variable objetivo: `FRAUDE`.
- Aproximadamente 4,28 % de operaciones fraudulentas.
- Valores nulos intencionales para practicar técnicas de tratamiento de datos.

---

# PREGUNTAS SEMILLA

## Business Analytics

1. ¿Cuál es la proporción de operaciones fraudulentas respecto al total?
2. ¿Qué segmentos presentan mayor tasa de fraude?
3. ¿Qué canales presentan mayor incidencia de fraude?
4. ¿Qué tipos de transacción presentan mayor riesgo?
5. ¿Existen diferencias de fraude entre ciudades?
6. ¿Existen diferencias según país de destino?
7. ¿Qué rangos de edad presentan mayor tasa de fraude?
8. ¿Qué rangos de ingresos presentan mayor tasa de fraude?
9. ¿Existe relación entre el monto de la operación y el fraude?
10. ¿Qué relación existe entre ingresos, egresos y monto transaccional?
11. ¿Las operaciones desde dispositivos nuevos presentan mayor riesgo?
12. ¿Los cambios recientes de clave están asociados al fraude?
13. ¿La distancia geográfica está relacionada con operaciones fraudulentas?
14. ¿Existe relación entre intentos fallidos y fraude?
15. ¿Los clientes nuevos presentan mayor incidencia?
16. ¿Las operaciones nocturnas presentan mayor riesgo?
17. ¿Los beneficiarios nuevos presentan mayor incidencia de fraude?
18. ¿Qué variables parecen tener mayor poder discriminativo?

---

# KPIs

| KPI                                     | Fórmula / definición                         | Utilidad                          |
| --------------------------------------- | ---------------------------------------------- | --------------------------------- |
| Total de transacciones                  | Conteo de operaciones                          | Dimensionar el problema           |
| Transacciones fraudulentas              | Conteo de`FRAUDE = 1`                        | Medir volumen de fraude           |
| Tasa de fraude                          | Fraude / Total × 100                          | Medir incidencia                  |
| Tasa de fraude por segmento             | Fraude del segmento / operaciones del segmento | Identificar perfiles críticos    |
| Tasa de fraude por canal                | Fraude del canal / operaciones del canal       | Fortalecer controles              |
| Tasa de fraude por tipo de transacción | Fraude / operaciones del tipo                  | Identificar operaciones críticas |
| Monto total asociado a fraude           | Suma de montos fraudulentos                    | Estimar exposición económica    |
| Monto promedio fraudulento              | Promedio de`MONTO_TRANSACCION` en fraude     | Caracterizar operaciones          |
| Tasa de fraude por dispositivo          | Fraude según`DISPOSITIVO_NUEVO`             | Evaluar señal de seguridad       |
| Tasa de fraude por horario              | Fraude según franjas horarias                 | Identificar horarios críticos    |
| Recall del modelo                       | TP / (TP + FN)                                 | Capacidad para detectar fraude    |
| Precision                               | TP / (TP + FP)                                 | Calidad de las alertas            |
| F1-score                                | Media armónica de Precision y Recall          | Balance entre ambas               |
| ROC-AUC                                 | Capacidad de discriminación                   | Comparar modelos                  |

---

# FASE 1 — ENTENDIMIENTO Y CALIDAD DE DATOS

Se documentarán las decisiones tomadas durante la limpieza.

---

# FASE 2 — EDA

## Análisis bivariado

Analizar la relación de cada variable relevante frente a `FRAUDE`.

## Análisis multivariado

Matriz de correlación.

- Asociación entre variables categóricas.
- Análisis de combinaciones de señales.
- Comparación de perfiles fraudulentos vs legítimos.

---

# FASE 3 — FEATURE ENGINEERING

### Ratio de endeudamiento/gasto

### Desviación del monto

### Horario de riesgo

Crear una variable categórica:

- Madrugada
- Mañana
- Tarde
- Noche

### Monto inusual

Indicador cuando:

`MONTO_TRANSACCION > MONTO_PROMEDIO_30D × 3`

### Perfil de operación sospechosa

Combinar señales como:

- Dispositivo nuevo.
- Beneficiario nuevo.
- Distancia elevada.
- Intentos fallidos.
- Cambio de clave.
- Monto inusual.


---

# FASE 4 — PREPARACIÓN PARA MACHINE LEARNING+

Se evitará utilizar información del conjunto de prueba durante el entrenamiento.

---

# FASE 5 — MODELAMIENTO

Se propone comparar inicialmente:

### Modelo 1 — Regresión Logística

### Modelo 2 — Random Forest

### Modelo 3 — XGBoost

Modelo principal candidato para capturar relaciones complejas y obtener un alto poder predictivo.

---

# FASE 6 — EVALUACIÓN

Debido al desbalance del problema.

Se evaluará:

- Confusion Matrix.
- Precision.
- Recall.
- F1-score.
- ROC-AUC.
- Precision-Recall.
- Umbral de clasificación.

### Prioridad de negocio

En fraude será especialmente importante analizar el **Recall**, porque un falso negativo representa una operación fraudulenta que el sistema no detectó.

Sin embargo, también se analizará Precision porque un exceso de falsos positivos puede generar demasiadas alertas y aumentar innecesariamente el trabajo de los analistas.

Por tanto, la selección final del modelo dependerá del equilibrio entre:

**Detección de fraude + cantidad de falsas alertas + impacto para el negocio.**

---

# FASE 7 — INTERPRETABILIDAD

Se analizará:

- Feature Importance.
- Coeficientes de Regresión Logística.
- Importancia de variables en Random Forest.
- Importancia de variables en XGBoost.
- SHAP, como etapa avanzada.

El objetivo será responder:

> ¿Por qué el modelo considera que una operación tiene mayor probabilidad de fraude?

---

# FASE 8 — SCORING DE RIESGO

La probabilidad generada por el modelo se podrá transformar en una clasificación:

| Score   | Nivel    | Acción sugerida                   |
| ------- | -------- | ---------------------------------- |
| 0–20   | Bajo     | Operación normal                  |
| 21–50  | Medio    | Monitoreo                          |
| 51–80  | Alto     | Revisión prioritaria              |
| 81–100 | Crítico | Investigación / control adicional |

Los rangos serán definidos y validados a partir de los resultados del modelo y del contexto del negocio, no solamente de manera arbitraria.

---

# FASE 9 — SOLUCIÓN DE NEGOCIO

El resultado final no será solamente un modelo de Machine Learning.

Se propondrá un flujo conceptual:

**Transacción → Preparación de datos → Feature Engineering → Modelo → Probabilidad de fraude → Score de riesgo → Priorización → Revisión del analista → Decisión**

La solución permitirá priorizar las operaciones con mayor probabilidad de fraude para que el equipo de riesgos concentre sus recursos en los casos más relevantes.

---

# ENTREGABLES

1. Base de datos sintética.
2. Notebook de análisis exploratorio.
3. Notebook de preparación y Feature Engineering.
4. Notebook de Machine Learning.
5. Comparación de modelos.
6. Matriz de confusión y métricas.
7. Análisis de variables importantes.
8. Scoring de riesgo.
9. Informe ejecutivo.
10. README profesional en GitHub.
11. Estructura reproducible del proyecto.
12. Conclusiones y recomendaciones de negocio.

---

# ESTRUCTURA PROPUESTA PARA GITHUB

```text
fraude-microempresarios/
│
├── data/
│   ├── raw/
│   │   └── base_fraude_interactuar_microempresarios_60000.csv
│   └── processed/
│
├── notebooks/
│   ├── 01_entendimiento_datos.ipynb
│   ├── 02_eda_fraude.ipynb
│   ├── 03_feature_engineering.ipynb
│   ├── 04_modelamiento.ipynb
│   └── 05_evaluacion_modelo.ipynb
│
├── src/
│   ├── data_processing.py
│   ├── feature_engineering.py
│   ├── modeling.py
│   └── evaluation.py
│
├── models/
│
├── reports/
│
├── .gitignore
├── requirements.txt
├── environment.yml
└── README.md
```

---

# TECNOLOGÍAS

El proyecto será desarrollado inicialmente en:

- Python
- Conda
- VS Code
- Git
- GitHub
- GitHub Copilot
- Jupyter Notebook
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-Learn
- XGBoost

Como evolución posterior:

- MLflow
- Databricks
- Lakeflow
- Power BI
- MLOps

---

# RESULTADO ESPERADO DEL PROYECTO

Al finalizar, se espera contar con una solución que permita responder:

> **¿Qué características están asociadas al fraude, cuáles son las operaciones de mayor riesgo y cómo podemos utilizar Machine Learning para priorizar su detección?**

El proyecto debe demostrar no solamente capacidad para entrenar un modelo, sino también capacidad para:

**entender el negocio → analizar los datos → generar hipótesis → preparar los datos → construir variables → entrenar modelos → evaluar resultados → interpretar el modelo → traducir resultados técnicos en decisiones de negocio.**

---

# NOTA SOBRE LOS DATOS

Esta base es completamente sintética y fue creada con fines educativos, de experimentación y construcción de portafolio. La empresa y el contexto utilizados son ficticios y no representan información real, confidencial o propietaria de Interactuar.
