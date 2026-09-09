# Anexo de Insights

Este documento consolida los hallazgos principales obtenidos en el analisis exploratorio de fraude.

---

# Insight 1

## La señal más fuerte de fraude está en la novedad operativa y en los eventos recientes de seguridad

El hallazgo más relevante del análisis bivariado es que el fraude aumenta de forma clara cuando la transacción involucra condiciones de mayor novedad o cambios recientes en la operación del cliente. En particular, las operaciones con `BENEFICIARIO_NUEVO = 1` presentan una tasa de fraude aproximada de 7,28%, frente a 3,79% cuando el beneficiario no es nuevo. De forma similar, las operaciones con `CAMBIO_CLAVE_RECIENTE = 1` alcanzan una tasa cercana a 7,45%, frente a 4,01% en los casos sin cambio reciente de clave.

Este patrón es especialmente importante porque no describe solo diferencias demográficas o de volumen, sino señales de comportamiento directamente relacionadas con riesgo transaccional. En contraste, variables como `SEXO` muestran tasas muy similares entre categorías, por lo que su poder explicativo es bajo para la detección de fraude.

Como conclusión de negocio, las operaciones con beneficiarios nuevos y cambios recientes de clave deben considerarse señales prioritarias de alerta temprana. Estas variables deberían tener un peso alto en reglas de monitoreo, segmentación de riesgo y en la futura construcción del modelo predictivo.

## Hallazgos complementarios

- Las transacciones internacionales también muestran mayor riesgo relativo, especialmente hacia `MX` y `US`, con tasas superiores al promedio general del conjunto de datos.
- El canal `App` registra una tasa de fraude superior a otros canales, lo que refuerza la importancia del contexto digital en la detección.
- Las diferencias por sexo, ciudad o tipo de cliente existen, pero son menos contundentes que las señales operativas y de seguridad.

---

# Insight 2

## En las variables numericas, el fraude se concentra mas en perfiles extremos e intensidades altas que en el cliente promedio

El hallazgo principal del analisis numerico es que las variables continuas no muestran un cambio abrupto y uniforme en la tasa de fraude entre todos los rangos, pero si dejan ver una senal consistente: el riesgo tiende a elevarse en perfiles economicos extremos y en operaciones de mayor intensidad transaccional.

En ingresos mensuales, la tasa de fraude general del conjunto de datos se mantiene cerca de 4,28%, pero aumenta hasta aproximadamente 6,64% en el rango de mas de 50 millones, aunque con bajo volumen de observaciones. En egresos mensuales se observa un comportamiento similar: el rango de 20M a 50M alcanza una tasa cercana a 5,66%, por encima del promedio general. Esto sugiere que los perfiles financieros mas altos, aunque menos frecuentes, concentran un riesgo relativo mayor.

La intensidad operativa tambien aporta senales de riesgo. El rango de 6 a 10 transacciones diarias presenta una tasa de fraude ligeramente superior al rango mas comun de 0 a 5 transacciones, lo que apunta a que un aumento en la frecuencia de operacion puede asociarse con mayor exposicion. En edad, las diferencias existen pero son moderadas, por lo que su capacidad explicativa es menor frente a otras variables numericas.

## Implicacion para deteccion de fraude

Las variables numericas parecen ser mas utiles para detectar fraude cuando se interpretan como senales de comportamiento extremo o intensidad operativa, y no como simples promedios. Por tanto, en la construccion de reglas o modelos conviene prestar especial atencion a rangos altos de ingresos, egresos y frecuencia transaccional, tratandolos como posibles indicadores de riesgo incremental.

## Observacion metodologica

La variable `NUM_INTENTOS_FALLIDOS` tiene potencial analitico, pero la discretizacion actual no permite diferenciar riesgo porque todos los valores quedaron agrupados en un solo rango. Antes de extraer conclusiones de negocio sobre esa variable, conviene redefinir sus intervalos de acuerdo con su distribucion real, que va de 0 a 6 intentos.

---

# Insight 3

## Validacion de los hallazgos previos: el fraude se explica mejor por senales operativas y de seguridad que por variables numericas tradicionales

Los resultados de asociacion confirman de forma clara el contenido de `Insight 1` y matizan `Insight 2`. En las variables categoricas, el estadistico de Cramer muestra que las senales mas relacionadas con fraude son `DISPOSITIVO_NUEVO` (0.0904), `BENEFICIARIO_NUEVO` (0.0599) y `CAMBIO_CLAVE_RECIENTE` (0.0456). Esto valida que la novedad operativa y los eventos recientes de seguridad son las senales categoricas mas utiles para identificar riesgo, muy por encima de variables demograficas como `SEXO` (0.0021) o `TIPO_CLIENTE` (0.0041), cuyo poder explicativo es practicamente nulo.

Tambien se confirma que `CANAL` y `PAIS_DESTINO` aportan informacion relevante, pero con una fuerza menor que las senales de seguridad. Sus valores de Cramer (`CANAL` = 0.0317 y `PAIS_DESTINO` = 0.0297) respaldan que el contexto de la transaccion importa, aunque no tanto como los indicadores de novedad y cambio reciente.

En cuanto a las variables numericas, los resultados obligan a refinar el `Insight 2`. La correlacion lineal con fraude es baja en casi todas las variables, lo que indica que su capacidad predictiva aislada es limitada. La excepcion principal es `NUM_INTENTOS_FALLIDOS` con una correlacion de 0.0608, claramente superior al resto. En contraste, variables como `MONTO_TRANSACCION` (0.0141), `DISTANCIA_UBICACION_KM` (0.0141), `INGRESOS_MENSUALES` (0.0071) y `NUM_TRANSACCIONES_DIA` (0.0008) muestran una relacion debil cuando se analizan de forma global.

## Conclusion de negocio

La evidencia consolidada indica que el fraude en este conjunto de datos no esta dominado por el perfil demografico ni por magnitudes financieras promedio, sino por senales de comportamiento operativo reciente. Para monitoreo, reglas y modelado, las variables que deben priorizarse son `DISPOSITIVO_NUEVO`, `BENEFICIARIO_NUEVO`, `CAMBIO_CLAVE_RECIENTE` y `NUM_INTENTOS_FALLIDOS`, seguidas por `CANAL` y `PAIS_DESTINO` como factores contextuales complementarios.

## Confirmacion de hallazgos anteriores

- `Insight 1` queda confirmado: las variables operativas y de seguridad son las mas relevantes para detectar fraude.
- `Insight 2` queda parcialmente confirmado: existen senales numericas, pero su fuerza general es baja y la unica variable con relacion destacable es `NUM_INTENTOS_FALLIDOS`.
- La observacion metodologica de `Insight 2` tambien se confirma: conviene redisenar la discretizacion de `NUM_INTENTOS_FALLIDOS`, ya que la correlacion sugiere que esta variable puede aportar mucho mas valor del que refleja su agrupacion actual.