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