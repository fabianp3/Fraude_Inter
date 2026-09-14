
# Detección de Fraude Bancario con Machine Learning

## Descripción del proyecto

Este proyecto aborda el problema de **detección de operaciones potencialmente fraudulentas** mediante técnicas de Machine Learning.

Se construyó un flujo completo de análisis que incluye exploración de datos, preparación y transformación de variables, construcción de modelos de clasificación, evaluación mediante métricas apropiadas para datos desbalanceados y análisis de umbrales de decisión.

El proyecto fue desarrollado con fines de **aprendizaje y construcción de portafolio en Data Science & Artificial Intelligence**.

> **Nota:** El dataset utilizado en este proyecto es sintético y tiene fines académicos. Los resultados no deben interpretarse como un sistema de detección de fraude listo para producción.

---

## Objetivo

Desarrollar y evaluar modelos de Machine Learning capaces de identificar operaciones fraudulentas, teniendo en cuenta el fuerte desbalance entre las operaciones legítimas y fraudulentas.

El proyecto busca:

- Explorar y comprender los datos.
- Identificar variables relevantes para la detección de fraude.
- Preparar los datos para el entrenamiento de modelos.
- Comparar diferentes algoritmos de clasificación.
- Evaluar el desempeño utilizando métricas adecuadas para problemas desbalanceados.
- Analizar el efecto del umbral de clasificación sobre Precision y Recall.
- Identificar las variables con mayor importancia para el modelo.
- Interpretar las limitaciones del modelo y sus posibilidades de mejora.

---

## Dataset

El proyecto utiliza un **dataset sintético de operaciones bancarias** creado para simular diferentes características relacionadas con transacciones y comportamiento de clientes.

La variable objetivo es:

- `FRAUDE = 0`: operación no fraudulenta.
- `FRAUDE = 1`: operación fraudulenta.

Entre las variables utilizadas se encuentran características relacionadas con:

- Información del cliente.
- Segmentación.
- Antigüedad.
- Ingresos y egresos.
- Monto de la transacción.
- Tipo de transacción.
- Canal utilizado.
- Ciudad.
- Hora de la transacción.
- Distancia respecto a la ubicación habitual.
- Uso de un dispositivo nuevo.
- Cambio reciente de clave.
- Número de transacciones realizadas.
- Número de intentos fallidos.
- Cliente nuevo.
- Beneficiario nuevo.
- País de destino.
- Fin de semana y mes.

> El dataset es sintético, por lo que los patrones observados no representan necesariamente el comportamiento de fraude de una entidad financiera real.

---

## Metodología

El proyecto se desarrolló siguiendo las siguientes etapas:

### 1. Análisis exploratorio de datos (EDA)

Se realizó un análisis inicial para conocer:

- Estructura del dataset.
- Tipos de variables.
- Valores faltantes.
- Distribución de las variables.
- Variables categóricas.
- Distribución de la variable objetivo.
- Posibles patrones asociados al fraude.

### 2. Preparación de los datos

Se realizaron actividades de:

- Tratamiento de valores faltantes.
- Transformación de variables.
- Conversión de variables categóricas.
- Preparación de variables numéricas.
- Creación de variables derivadas.
- Separación entre variables predictoras y variable objetivo.

### 3. División de los datos

Los datos fueron divididos en conjuntos de entrenamiento y prueba:

| Conjunto | Registros |
| -------- | --------: |
| Train    |    48.000 |
| Test     |    12.000 |

### 4. Preprocesamiento

Se utilizó un `Pipeline` de **Scikit-learn** para integrar el preprocesamiento y el modelo.

Para las variables categóricas se utilizó codificación mediante `OneHotEncoder`.

---

## Modelos utilizados

Se evaluaron dos algoritmos de clasificación:

### Decision Tree

Se utilizó como modelo base:

```python
DecisionTreeClassifier(
    max_depth=5,
    class_weight='balanced',
    random_state=42
)
```

El parámetro `class_weight='balanced'` permitió asignar mayor peso a la clase minoritaria durante el entrenamiento.

### XGBoost

Posteriormente se implementó un modelo **XGBoost** y una versión balanceada utilizando:

```python
scale_pos_weight
```

Este parámetro permitió compensar el desbalance entre las clases y otorgar mayor importancia a la clase fraudulenta durante el entrenamiento.

---

## Evaluación

Debido al fuerte desbalance entre las clases, **Accuracy no se utilizó como única métrica de evaluación**.

Se analizaron principalmente:

- **Precision:** proporción de las operaciones clasificadas como fraude que realmente fueron fraude.
- **Recall:** proporción de los fraudes reales que fueron detectados por el modelo.
- **F1-score:** equilibrio entre Precision y Recall.
- **ROC-AUC:** capacidad global del modelo para diferenciar entre las dos clases.
- **Average Precision:** desempeño global considerando la relación entre Precision y Recall.
- **Matriz de confusión:** permite observar verdaderos positivos, verdaderos negativos, falsos positivos y falsos negativos.
- **Curva Precision-Recall:** permite analizar el comportamiento del modelo ante diferentes umbrales.

Estas métricas permiten analizar el compromiso entre detectar una mayor cantidad de fraudes y generar falsas alarmas.

---

## Resultados

Los principales resultados obtenidos fueron:

| Modelo             |         ROC-AUC | Average Precision | Recall fraude | Precision fraude | F1 fraude |
| ------------------ | --------------: | ----------------: | ------------: | ---------------: | --------: |
| Decision Tree      |           0.653 |            0.0766 |         46.5% |             7.5% |     12.9% |
| XGBoost balanceado | **0.665** |  **0.0884** |         46.5% |             7.4% |     12.8% |

XGBoost balanceado presentó una mejor capacidad global de discriminación, reflejada en un **ROC-AUC** y un **Average Precision** ligeramente superiores al Decision Tree.

Sin embargo, la diferencia entre ambos modelos fue moderada.

Un aspecto importante es que ambos modelos presentaron una **Precision baja para la clase fraude**, lo que significa que una proporción importante de las operaciones marcadas como sospechosas correspondería a falsas alarmas.

---

## Análisis del umbral

Se analizaron diferentes umbrales de clasificación para observar el comportamiento de **Precision, Recall y F1-score**.

En el caso del XGBoost balanceado, se observó que reducir el umbral incrementa considerablemente el Recall, pero también aumenta el número de alertas.

Por ejemplo:

| Umbral | Precision | Recall |     F1 | Alertas |
| -----: | --------: | -----: | -----: | ------: |
|   0.50 |     7.42% | 46.52% | 12.79% |   3.155 |
|   0.40 |     6.15% | 72.56% | 11.34% |   5.935 |
|   0.30 |     4.86% | 89.46% |  9.21% |   9.264 |
|   0.20 |     4.28% | 99.01% |  8.21% |  11.629 |

Este análisis demuestra el **trade-off entre Precision y Recall**.

Reducir el umbral permite detectar una mayor cantidad de fraudes, pero genera un volumen mucho mayor de alertas falsas.

Entre los umbrales evaluados, **0.50 presentó el mejor F1-score**, aunque su Precision continuó siendo baja. Por esta razón, el umbral definitivo en un escenario real debería establecerse considerando los costos asociados a falsos positivos y falsos negativos.

---

## Importancia de variables

El análisis de importancia de variables del modelo XGBoost permitió identificar las principales características utilizadas por el modelo para realizar sus predicciones.

Las cuatro principales fueron:

| Variable                  | Importancia |
| ------------------------- | ----------: |
| `DISPOSITIVO_NUEVO`     |      17.87% |
| `BENEFICIARIO_NUEVO`    |      10.59% |
| `NUM_INTENTOS_FALLIDOS` |       6.08% |
| `CAMBIO_CLAVE_RECIENTE` |       5.90% |

Estas variables representan señales relacionadas principalmente con cambios en el comportamiento habitual de una transacción.

> **Nota:** La importancia de una variable representa su contribución al proceso predictivo del modelo; no implica necesariamente una relación causal con el fraude.

---

## Principales conclusiones

1. El problema presenta un **fuerte desbalance de clases**, por lo que Accuracy no es suficiente para evaluar el desempeño del modelo.
2. El uso de `class_weight='balanced'` en Decision Tree y `scale_pos_weight` en XGBoost permitió incorporar el desbalance de clases durante el entrenamiento.
3. **XGBoost balanceado** obtuvo el mejor resultado global según **ROC-AUC y Average Precision**, aunque la mejora respecto al Decision Tree fue moderada.
4. El análisis de umbrales mostró un fuerte compromiso entre **Recall y Precision**.
5. Las variables relacionadas con dispositivos, beneficiarios, intentos fallidos y cambios recientes de clave estuvieron entre las características más importantes para el modelo.
6. Los resultados muestran que el modelo todavía presenta una **Precision baja para la clase fraude**, por lo que no debería considerarse listo para una implementación productiva.

---

## Limitaciones

Este proyecto presenta algunas limitaciones importantes:

- El dataset utilizado es sintético.
- Los patrones de fraude no representan necesariamente el comportamiento de una entidad financiera real.
- La cantidad y calidad de las variables predictoras es limitada.
- No se realizó validación temporal.
- No se realizó una optimización exhaustiva de hiperparámetros.
- El umbral de clasificación requiere una definición basada en costos reales de negocio.
- Para una implementación productiva sería necesario utilizar datos reales, realizar una validación más rigurosa, implementar monitoreo del modelo y evaluar periódicamente su desempeño.

---

## Tecnologías utilizadas

- Python
- Pandas
- NumPy
- Scikit-learn
- XGBoost
- Matplotlib
- Jupyter Notebook
- Conda
- Visual Studio Code
- Git / GitHub

---

## Estructura del proyecto

```text
Fraude_Inter/
│
├── .github/
│
├── datos/
│   ├── brutos/
│   ├── intermedios/
│   └── procesados/
│
├── docs/
│
├── entregables/
│   ├── dashboards/
│   ├── graficos/
│   └── informes/
│       ├── Anexo Insights.md
│       ├── Insight 1.md
│       ├── Insight 2.md
│       └── Insight 3.md
│
├── funciones/
│
├── notebooks/
│   ├── Analisis_exploratorio.ipynb
│   ├── BA.ipynb
│   └── modelo.ipynb
│
├── .gitignore
│
└── README.md
```

### Descripción de las carpetas

- **`.github/`**: contiene configuraciones relacionadas con GitHub y el repositorio.
- **`datos/brutos/`**: contiene los datos originales utilizados como punto de partida.
- **`datos/intermedios/`**: contiene archivos generados durante las etapas de transformación y preparación de los datos.
- **`datos/procesados/`**: contiene los datasets preparados para análisis y modelamiento.
- **`docs/`**: contiene documentación complementaria del proyecto.
- **`entregables/dashboards/`**: contiene elementos relacionados con dashboards.
- **`entregables/graficos/`**: contiene gráficos generados durante el análisis.
- **`entregables/informes/`**: contiene informes e insights derivados del proyecto.
- **`funciones/`**: contiene funciones reutilizables desarrolladas para el procesamiento y análisis.
- **`notebooks/`**: contiene los notebooks utilizados durante las diferentes etapas del proyecto.
- **`.gitignore`**: especifica los archivos y carpetas que no deben incluirse en el repositorio.
- **`README.md`**: contiene la documentación principal del proyecto.

---

## Autor

**Moisés Fabián**

Proyecto desarrollado como parte del proceso de aprendizaje y construcción de portafolio en **Data Science & Artificial Intelligence**.
