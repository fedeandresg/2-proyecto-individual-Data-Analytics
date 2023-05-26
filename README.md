![Logo](https://blog.soyhenry.com/content/images/2021/02/HEADER-BLOG-NEGRO-01.jpg)

# DATA SCIENCE - PROYECTO INDIVIDUAL Nº2
# Data Analytics

Este proyecto abarca una serie de pasos para desarrollar un proceso de **`Data Analytics`** sobre un dataset de accidentes aéreos, para posteriormente disponibilizar un **`Dashboard Interactivo`** utilizando **`Power BI`**.

![Logo](https://github.com/fedeandresg/2-proyecto-individual-Data-Analytics/blob/main/src/avion.jpg?raw=true)

## Contexto

Se plantea la necesidad de analizar datos históricos de **`accidentes aéreos`** a los efectos de identificar patrones, tendencias y factores que ayuden a mejorar la seguridad en la aviación civil. 

## Dataset

El [dataset](https://github.com/fedeandresg/2-proyecto-individual-Data-Analytics/blob/main/AccidentesAviones.csv) en cuestión posee información acerca de vuelos que han sufrido un siniestro a lo largo de todo el mundo desde 1908 hasta 2021. El mismo cuenta con 5008 filas (representando cada fila un accidente aéreo) y 18 columnas (atributos de cada vuelo).

## Objetivo

El propósito del trabajo fue describir las características de vuelos siniestrados a los efectos de analizar los siguientes **`KPI's`** propuestos:

- Reducción en un 5% de la **`tasa anual de mortalidad`** (personas fallecidas/personas a bordo);
- Aumento en la **`tasa anual de supervivencia`** (personas sobrevivientes / personas a bordo) para aerolíneas con gran cantidad de accidentes históricos ;
- Evolución de accidentes aéreos y comparación con métricas para definir **`tendencia`**;
- Disminución de la **`cantidad de accidentes`** para el país con mayor cantidad de siniestros.

## ETL  

Para realizar los procesos de ETL se utilizó **`Python`** con librerías de Numpy, Pandas, Matplotlib y Seaborn, entre otras.
Se pueden visualizar las transformaciones y los análisis realizados en el siguiente
[archivo](https://github.com/fedeandresg/2-proyecto-individual-Data-Analytics/blob/main/accidentes_aereos.ipynb)

## Exportación a MySQL

Una vez finalizadas las transformaciones necesarias sobre el archivo csv, se procedió a ingestar el dataframe resultante en MySQL a través de python estableciendo la conexión correspondiente con la librería **`mysql-connector-python`**.
A través del código en cuestión, se creó la base de datos **`air_flights`** y la tabla contenedora del dataframe **`air_accidents`**

![Logo](https://github.com/fedeandresg/2-proyecto-individual-Data-Analytics/blob/main/src/info%201%20air_accidents%20mysql.PNG?raw=true)

![Logo](https://github.com/fedeandresg/2-proyecto-individual-Data-Analytics/blob/main/src/info%20air_accidents%20mysql.PNG?raw=true)

Se puede visualizar el código empleado en la última sección del siguiente
[notebook](https://github.com/fedeandresg/2-proyecto-individual-Data-Analytics/blob/main/accidentes_aereos.ipynb)

## Importación a Power BI y Modelado de datos

Una vez importada la tabla air_accidents de MySQL, se optó por dividir el dataset en 1 tabla de transacciones y 4 tablas de dimensiones según las siguientes variables elegidas del dataset:
- Marcas [brands]: identifcando la marca de la aeronave que tuvo el accidente;
- Categorías [categories]: utilizando la categoría de vuelos militares (military) y no militares (non_military);
- Países [countries]: colocando los países en los que tuvo lugar el accidente;
- Superficies [surfaces]: clasificando según la superficie de impacto del accidente haya sido tierra (ground) o agua (water);
- Accidentes aéreos [air_accidents]: tabla de transacciones que muestra por cada registro un (1) accidente aéreo con los atributos descriptos anteriormente y otros adicionales. Se procedió a crear una columna de índices a los fines de ser utilizada como clave primaria.

Una vez efectuadas las creaciones de tablas y algunas transformaciones mínimas en **`Power Query`**, procedimos a unir las tablas en la vista "Modelo" mediante la clave primaria de cada tabla con la tabla de transacciones creando un esquema estrella. 

A su vez creamos la tabla calendario [calendar] para poder ser utilizada en funciones de inteligencia de tiempo y mayor nivel de agregación de datos.
El modelo quedó de la siguiente forma:

![Logo](https://github.com/fedeandresg/2-proyecto-individual-Data-Analytics/blob/main/src/modelo.PNG?raw=true)


## Análisis exploratorio de datos

A los efectos de poder entender los datos presentados, se realizaron una serie de análisis y estudios sobre las variables del dataset a los efectos de poder encontrar relaciones entre los datos y comprender la relevancia de los mismos.
Dentro de los análisis efectuados se encuentran: 
- Nubes de palabras para identificar causas comunes en las descripciones de los accidentes;
- Distribuciones de frecuencias y estadísticas de las variables numéricas;
- Análisis e identificación de outliers en variables de interés (total_aboard y total_fatalities);
- Identificación de variables categóricas y sus valores; 
- Análisis de accidentes, tasa de mortalidad, tasa de supervivencia y seguridad en vuelos por: país, aerolínea, aeronave, marca de aeronave y categoría de vuelo;
- Análisis temporales por hora, día, mes y año.

Se pueden visualizar las transformaciones y los análisis realizados en el siguiente
[archivo](https://github.com/fedeandresg/2-proyecto-individual-Data-Analytics/blob/main/accidentes_aereos.ipynb)

## Dashboard e insights

El [dashboard](https://github.com/fedeandresg/2-proyecto-individual-Data-Analytics/blob/main/dashboard_air_accidents.pbix) consta de 1 portada y 4 páginas navegables a través de una botonera de navegación.

Se destaca que dentro de cada página del mismo, en la esquina superior derecha, se puede encontrar el botón de información que redirecciona a este repositorio de Github.

![Logo](https://github.com/fedeandresg/2-proyecto-individual-Data-Analytics/blob/main/src/portada.PNG?raw=true)

### País

En la primer página denominada **`País`**, se pueden observar los accidentes por país pudiendo filtrar a su vez por año, categoría del vuelo y superfice de impacto. Tenemos información acerca de pasajeros a bordo, fatalidades y tasa de mortalidad (TM) junto con un gráfico de su evolución a lo largo del tiempo.
Podemos observar a `Estados Unidos` como el país con mayor cantidad de accidentes a lo largo de la historia y Rusia como el segundo.
Por último podemos visualizar una tarjeta de KPI que muestra la tasa de mortalidad por año y su comparación con el **`target`** planteado (tasa del año anterior reducida en un 5% como mínimo). Si la tasa de mortalidad del año en cuestión es menor al target la tarjeta mostrará la tasa en color verde (véase 2020). Caso contrario mostrará la tasa en rojo (véase 2021). La tabla de análisis por año ayuda a clarificar sobre este KPI.

![Logo](https://github.com/fedeandresg/2-proyecto-individual-Data-Analytics/blob/main/src/pais.PNG?raw=true)


### Operador

En la segunda página denominada **`Operador`**, se puede observar la cantidad de operadores analizados pudiendo filtrar a su vez por año, País y superfice de impacto. Tenemos información acerca de pasajeros a bordo, sobrevivientes y tasa de supervivencia (TS) junto con un gráfico de su evolución a lo largo del tiempo.
Podemos observar a `Aeroflot` como la aerolínea con mayor cantidad de accidentes históricos seguido por la fuerza aérea de Estados Unidos.
Por último podemos visualizar un KPI para la tasa de supervivencia estableciendo un **`target`** de un 50% para la misma. Observamos que la tasa de supervivencia varía de operador en operador, pero que un objetivo es mantener la misma por encima de un 50% para mejorar la seguridad en los vuelos (observar tendencia histórica).

![Logo](https://github.com/fedeandresg/2-proyecto-individual-Data-Analytics/blob/main/src/operador.PNG?raw=true)


### Aeronave

En la tercer página denominada **`Aeronave`**, se puede observar la cantidad de aeronaves y marcas involucradas en el dataset pudiendo filtrar especificamente por tipo de aeronave, marca de aeronave, operador y año. Tenemos medidores de tasa de mortalidad y tasa de supervivencia, un gráfico de barras con las principales marcas y sus aeronaves y un gráfico de composición de las aeronaves según sea la categoría (vuelo militar o vuelo no militar)
Podemos observar al `Douglas DC-3` como el avión con mayor cantidad de accidentes a lo largo de la historia. El mismo perteneció a la compañía Douglas que con el tiempo mutó a MCDonell Douglas para finalmente convertirse en Boeing.


![Logo](https://github.com/fedeandresg/2-proyecto-individual-Data-Analytics/blob/main/src/aeronave.PNG?raw=true)


### Tendencia

La última página del dashboard se denomina **`Tendencia`** y se encarga de analizar los accidentes y la cantidad de pasajeros con sus respectivas tendencias a lo largo del tiempo.
Podemos destacar que no siempre existe una relación directa entre la tendencia de accidentes por período y la cantidad de pasajeros (véase en tendencia mensual).
Por otro lado se pueden analizar la cantidad de accidentes a lo largo de los años con la media móvil de 10 períodos, lo cual nos permite concluír que los años donde la cantidad de accidentes se ha encontrado por debajo de la media móvil, han resultado años con baja siniestralidad, mientras que aquellos años donde la cantidad ha estado por encima han sido años con una gran cantidad de accidentes (años entre 1946 y 2004). Dicha media móvil puede ser ajustada en función de las circunstancias del futuro. Tal es así que podemos inferir que a nivel mundial estamos en una tendencia a la baja en cantidad de accidentes desde 1989 (año en que comienza a descender la cantidad de accidentes) y por debajo del promedio móvil de 10 años en los años siguientes por lo que resulta interesante mantenerse por debajo de dicho patrón para los próximos años.
Por último analizamos la variación interanual de accidentes y podemos filtrar por países como Estados Unidos para observar cómo se ha comportado en los últimos años pese a tener la mayor cantidad de accidentes y pasajeros históricos.

![Logo](https://github.com/fedeandresg/2-proyecto-individual-Data-Analytics/blob/main/src/tendencia.PNG?raw=true)

## Conclusiones

- La Tasa anual de mortalidad, tal como puede visualizarse en la página 'País', oscila entre el 50% y el 100% para la mayoría de los años. La misma si bien presenta tendencia en algunos períodos, no podemos decir que tenga una marcada tendencia a la baja. Es importante en su lugar, estar por debajo del target de tasa conforme al KPI indicado por lo que se sugiere revisarlo año a año;
- La tasa anual de supervivencia, tal como puede visualizarse en la página 'Operador', en contraposición a la tasa anual de mortalidad, oscila entre el 0% y y el 50% para la mayoría de los años. Es deseable para la mayoría de las aerolíneas poder mantenerse por encima del 50%. Se alienta a trabajar con las aerolíneas que no alcanzan el 50% y revisar sus protocolos de seguridad;
- La tendencia anual de accidentes a lo largo de la historia comenzó una gran tendencia alcista desde los días de los primeros vuelos hasta 1945. La tendencia pudo consolidarse entre 1946 y 2004 para finalmente romper la zona de acumulación en 2005 (año en que confirmamos la tendencia a la baja comenzada en 1989). La media móvil de 10 años nos ayuda a visualizar años de baja siniestralidad, por lo que se alienta a mantenerse por debajo de la misma y activar alertas en el momento en que la tendencia anual se aproxime a la misma (situación que ha acontecido numerosas veces a lo largo de la historia). Es positivo a su vez, poder afirmar que la aviación ha alcanzado un nivel de seguridad que permite estar en los niveles de accidentes más bajos de la historia, considerando la cantidad de vuelos, personas transportadas y avances tecnológicos en contraposición a los primeros días;
- Por último es destacable la tendencia anual de accidentes para los países como Estados Unidos y Rusia, los cuales pese a tener la mayor cantidad de accidentes históricos, mantienen marcadas tendencias a la baja en los últimos años, en concordancia con la tendencia mundial. 