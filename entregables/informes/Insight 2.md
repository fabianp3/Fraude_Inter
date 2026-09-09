# Insight 2

## En las variables numericas, el fraude se concentra mas en perfiles extremos e intensidades altas que en el cliente promedio

El hallazgo principal del analisis numerico es que las variables continuas no muestran un cambio abrupto y uniforme en la tasa de fraude entre todos los rangos, pero si dejan ver una senal consistente: el riesgo tiende a elevarse en perfiles economicos extremos y en operaciones de mayor intensidad transaccional.

En ingresos mensuales, la tasa de fraude general del conjunto de datos se mantiene cerca de 4,28%, pero aumenta hasta aproximadamente 6,64% en el rango de mas de 50 millones, aunque con bajo volumen de observaciones. En egresos mensuales se observa un comportamiento similar: el rango de 20M a 50M alcanza una tasa cercana a 5,66%, por encima del promedio general. Esto sugiere que los perfiles financieros mas altos, aunque menos frecuentes, concentran un riesgo relativo mayor.

La intensidad operativa tambien aporta senales de riesgo. El rango de 6 a 10 transacciones diarias presenta una tasa de fraude ligeramente superior al rango mas comun de 0 a 5 transacciones, lo que apunta a que un aumento en la frecuencia de operacion puede asociarse con mayor exposicion. En edad, las diferencias existen pero son moderadas, por lo que su capacidad explicativa es menor frente a otras variables numericas.

## Implicacion para deteccion de fraude

Las variables numericas parecen ser mas utiles para detectar fraude cuando se interpretan como senales de comportamiento extremo o intensidad operativa, y no como simples promedios. Por tanto, en la construccion de reglas o modelos conviene prestar especial atencion a rangos altos de ingresos, egresos y frecuencia transaccional, tratandolos como posibles indicadores de riesgo incremental.

## Observacion metodologica

La variable `NUM_INTENTOS_FALLIDOS` tiene potencial analitico, pero la discretizacion actual no permite diferenciar riesgo porque todos los valores quedaron agrupados en un solo rango. Antes de extraer conclusiones de negocio sobre esa variable, conviene redefinir sus intervalos de acuerdo con su distribucion real, que va de 0 a 6 intentos.