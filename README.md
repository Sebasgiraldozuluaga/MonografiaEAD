# Predicción del precio de la energía eléctrica en Colombia con horizonte de corto plazo utilizando modelos de Deep Learning
 Este repositorio contiene los notebooks elaborados para la construcción un modelo de predicción de  predicción del precio de la energía eléctrica en Colombia con un horizonte de corto plazo haciendo  uso de modelos de Deep Learning. Presentado por **Sebatián Giraldo Zuluaga y Edwar Alejandro Ramírez Londoño** como requisito para obtener el título de Especialista en Analítica y Ciencia de Datos. El trabajo  fue desarrollado bajo la asepría de Ph.D Álvaro Jaramillo Duque y Ph.D Walter Mauricio Villa Acevedo.

### RESUMEN ###
Este trabajo tiene como objetivo la implementación de diferentes modelos de Deep Learning aplicados a la predicción del precio de bolsa en el mercado eléctrico colombiano con un horizonte de 24 horas (muy corto plazo). 
Se entrenaron y se ajustaron modelos como: DNN, LSTM, GRU, CNN_-LSTM, permitiendo hacer comparaciones entre los pronósticos usando métricas de desempeño como MAE, MAPE, SMAPE, RMSE. Los pronósticos de precio son importantes para los actores participantes en un mercado desregulado dada la alta volatilidad que presenta. Un buen pronóstico  del precio de la electricidad en la bolsa de energía eléctrica da la posibilidad a los agentes participantes del  mercado de ajustar sus ofertas para mitigar los riesgos y maximizar beneficios. 

Los modelos fueron entrenados a partir de la información recopilada en la base de datos de XM donde se obtuvieron las variables de: precio de bolsa, demanda de energía, volumen útil de embalses, Aportes hidrológicos, disponibilidad plantas térmicas, disponibilidad de las plantas no térmicas y precio de oferta de cada planta. 

### Distribución de los Datos ###
Los datos están disponibles en un solo archivo CSV.  Cada fila del archivo representa una día y hora específico, y las columnas representan las diferentes variables de manera horario. Los aportes, el volumen útil y la oferta de precio de las planas (única para los 24 periodos del día) originalmente se descargaron de manera diaria, sin embargo, estos datos se replicaron de manera horaria para completar el Dataset. 


Descripción de las Columnas: Las columnas en los datos son las siguientes: 

Date: 		Representa la fecha y hora de cada entrada de datos. 

PrecioB: 	Precio de energía eléctrica en bolsa dado en $COP/kWh.  

Demanda: 	Demanda real del sistema eléctrico colombiano kWh. 

DispNoTer: 	Disponibilidad de las plantas de generación no térmicas kWh. 

VolUtil: 	Volumen Útil diario Energía por Sistema kWh. 

Aportes: 	Aportes en energía de los ríos que aportan agua a algún embalse del sistema interconectado nacional kWh. 

DispTer: 	Disponibilidad de las plantas de generación térmicas kWh. 

PrecioO: 	Precio de oferta de las plantas en $COP/kWh. 
## ESTRUCTURA DE NOTEBOOKS 
-	Se presentan 3 carpetas :
`Scripts` Contiene todos los jupyter notebooks usados.
`Datasets` Contiene todos las bases de datos usadas.
`experimental_files` Contiene los parametros corridos en la hiperparametrización.

##Exploración de los datos 
- El notebook `AnalisisExploratorio_Dia.ipynb` muestra las estadísticas descriptivas y los gráficos por día de la semana y por mes.
- El notebook `AnalisisExploratorio_Hora.ipynb` muestra las estadísticas descriptivas y los gráficos por hora del día y por franja horaria.
- El notebook `ClusterData.ipynb` aplica un algoritmo de clustering para agrupar los puntos de medición según su patrón de tráfico.

##Extracción de los datos
- Ejecutar el archivo `DownloadData.ipynb` para descargar los datos de la fuente original
- Ejecutar el archivo `Generate_csv.ipynb` para transformar los datos en formato csv

##Análisis de ventanados 
-	Cada archivoexplica el análisis de los modelos GRU, LSTM, DNN, CNN-LSTM  que indica el tamaño de la ventana y el horizonte de predicción en horas, por ejemplo, DL_Modelo_Ventanado_96_24 significa que se usa una ventana de 96 horas para predecir 24 horas.
`DL_Modelo_Ventanado_96_24.ipynb`
`DL_Modelo_Ventanado_72_24.ipynb`
`DL_Modelo_Ventanado_48_24.ipynb`
`DL_Modelo_Ventanado_24_24.ipynb`

-	El siguiente bloque de notebooks explica el análisis de los modelos GRU, LSTM, DNN, CNN-LSTM para diferentes ventanados para una predicción de 24horas, con earlystopping y crossvalidation.
`DL_Modelo_Ventanado_96_24_sinOfe_earlystop_kfolds.ipynb`
`DL_Modelo_Ventanado_72_24_sinOfe_earlystop_kfolds.ipynb`
`DL_Modelo_Ventanado_48_24_sinOfe_earlystop_kfolds.ipynb`
`DL_Modelo_Ventanado_24_24_sinOfe_earlystop_kfolds.ipynb`

-	El siguiente notebook corre el mejor resultado de los 4 ventanados usados y con mejores resultados con el que se trabajará  la hiperparametrización. 
`DL_Modelo_Ventanado_72_24_sinOfe_earlystop_kfolds-1.ipynb`

## Hiperparametrización 
-	El siguiente notebook corre la hiperparametrización de parámetros para 72_24 con early stopping y cross validation.
`DL_Modelo_hyperparameter.ipynb`

## Mejores hiperparámetros
-	El siguiente notebook corre los mejores resultados encontrados en la hiperparametrización.
`DL_Modelo_Best_Hyper.ipynb`


### DATASET ###

Los datos están en formato de archivo CSV, y se obtuvieron por medio de SINERGOX una API proporcionada por XM la cual tiene conexión a través de Python y de la cual se pueden encontrar ejemplos de uso.

- Un archivo CSV llamado `Collections.csv` contiene los datos de las colecciones de variables presentes en la API XM.
- Un archivo PKL llamado `DataModel.pkl` guarda el modelo de aprendizaje automático.

- Un archivo CSV llamado `Dayslabels.csv` tiene las etiquetas de los días de la semana.

- Un archivo LSX llamado `TABLAS.lsx` muestra las tablas de resultados através de todo el trabajo.

- Un archivo CSV llamado `XM_D.csv` almacena los datos presentados de manera diaria. 

- Un archivo CSV llamado `XM_H.csv` almacena los datos presentados de manera horaria. 

- Un archivo CSV llamado `XM_H_sinOfec.csv` elimina los registros de manera horaria eliminando la variable precio de oferta dada la correlación con la variable Precio de Bolsa, esta conclusión a partir del notebook de análisis exploratorio `AnalisisExploratorio_Hora.ipynb`

Los datos utilizados en este proyecto fueron descargados de la base de datos de XM donde está disponible la información del mercado eléctrico de energía mayorista colombiano. Se trabajó con una ventana de tiempo que va desde el año 2013 hasta el 2018. Estos datos se obtienen a partir de una API proporcionada por la empresa XM llamada SINERGOX, que ofrece la información de consulta para extraer la información a partir de Python. Los datos descargados fueron precio de bolsa de energía, demanda de energía, volumen útil de los embalses, aportes hidrológicos, precio oferta de las plantas, disponibilidad de energía de las plantas térmicas y disponibilidad de energía de las plantas no térmicas. Estos datos son relevantes para analizar la dinámica del mercado eléctrico, los cuales se consideran que son los factores que influyen en la formación de precio de bolsa.


------------

#RESULTADOS

El pronóstico de precio de bolsa en el mercado eléctrico colombiano es una variable con una volatilidad importante que esta influenciada por varios aspectos como los cambios climáticos donde las temporadas secas generan picos en el precio dada la alta dependencia de la generación hidráulica. Adicionalmente al ser un mercado con poca oferta (generadores) y mucha demanda (comercializadore), la estrategia de los agentes que determinan el precio de bolsa a través de la oferta de precio y disponibilidad pueden impactar la manera en la que se forma el precio en el mercado. También las políticas energéticas impulsadas por el gobierno de turno afectan el comportamiento de esta variable.

El tipo de ventando que se utiliza en los modelos de serie de tiempo tanto en el número de retados como en el traslape que se utilice es un factor de mucha relevancia dado que puede influir en la estabilidad del modelo y en los tiempos de ejecución cuando se está entrenando y haciendo las validaciones de los parámetros.

El mejor modelo encontrado fue el LSTM con 72 retardos, en el cual se obtuvo valores en el MAPE y RMSE en pruebas de 6.93% y 11.87 $COP/kWh. Sin embargo, el modelo CNN-LSTM que había obtenido buenas métricas antes de la sintonización, arrojó hiperparámetros que dieron los valores de errores más altos en pruebas. Esto indica que en la hiper parametrización, el rango de variabilidad de cada parámetro debe ser más estrecho y próximo a los valores usados inicialmente.

Se tuvieron en cuenta inicialmente 8 variables para el pronóstico del precio de bolsa, el análisis de correlación permitió determinar que el precio de oferta estaba altamente correlacionado (>0.7) y que era una variable que podía ser eliminada. Esto pudo ser corroborado haciendo algunos pronósticos teniendo en cuente el precio de oferta en las variables de entrada y sin incluirlo, mostrando que las métricas de error no cambiaban considerablemente. Sin embargo, es importante mencionar que dada la complejidad de la variable a pronosticar es posible que otras variables pueda ser eliminadas por eventualmente presentar colinealidad, lo cual debe ser validado con otras técnicas de estadística para determinar si los pronósticos mejoran.

Para variables que presenten comportamientos cíclicos, por tipo de día o temporada como es el caso de la demanda de energía, es recomendable agregar variables dummy que permitan al modelo tener en cuenta este comportamiento estacional. Se podría hacer un análisis adicional con el caso de los aportes para determinar las temporadas secas y de lluvias y así validar si el modelo mejora.