# Insight 1

## La señal más fuerte de fraude está en la novedad operativa y en los eventos recientes de seguridad

El hallazgo más relevante del análisis bivariado es que el fraude aumenta de forma clara cuando la transacción involucra condiciones de mayor novedad o cambios recientes en la operación del cliente. En particular, las operaciones con `BENEFICIARIO_NUEVO = 1` presentan una tasa de fraude aproximada de 7,28%, frente a 3,79% cuando el beneficiario no es nuevo. De forma similar, las operaciones con `CAMBIO_CLAVE_RECIENTE = 1` alcanzan una tasa cercana a 7,45%, frente a 4,01% en los casos sin cambio reciente de clave.

Este patrón es especialmente importante porque no describe solo diferencias demográficas o de volumen, sino señales de comportamiento directamente relacionadas con riesgo transaccional. En contraste, variables como `SEXO` muestran tasas muy similares entre categorías, por lo que su poder explicativo es bajo para la detección de fraude.

Como conclusión de negocio, las operaciones con beneficiarios nuevos y cambios recientes de clave deben considerarse señales prioritarias de alerta temprana. Estas variables deberían tener un peso alto en reglas de monitoreo, segmentación de riesgo y en la futura construcción del modelo predictivo.

## Hallazgos complementarios

- Las transacciones internacionales también muestran mayor riesgo relativo, especialmente hacia `MX` y `US`, con tasas superiores al promedio general del conjunto de datos.
- El canal `App` registra una tasa de fraude superior a otros canales, lo que refuerza la importancia del contexto digital en la detección.
- Las diferencias por sexo, ciudad o tipo de cliente existen, pero son menos contundentes que las señales operativas y de seguridad.