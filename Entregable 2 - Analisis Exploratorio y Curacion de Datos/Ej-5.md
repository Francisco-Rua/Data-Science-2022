# Ejercicio 5: Documentación

Para obtención del conjunto de datos final se realizaron los siguientes pasos:

## Ejercicio N° 1: 

- El dataset del cual partimos en este entregable  es el denominado “merge_sales_df_tp1”.

- Ya que consideramos que sería de utilidad para el problema de la predicción de precios de venta de inmuebles de Melbourne, dicho archivo estaba compuesto por las siguientes variables a las que llegamos mediante un merge de datos entre los archivos melb_df y airbnb, propuestos inicialmente, luego de un proceso de limpieza de outliers y de eliminación de otras variables no pertinentes. De esta manera, nuestro punto de partida que Podemos definir como variables de interés, son:
“Suburb, Rooms,  Type, Price, Postcode, Landsize, BuldingArea, YearBuilt, CouncilArea, Lattitude, Longitude, Regionname, zipcode, airbnb_record_count, airbnb_price_mean”     

- Este dataframe posee la siguiente forma: 11041 filas por 15 columnas.

- Luego del análisis y para facilitar las tareas de encoding, eliminamos las filas  en cero  de la variable CouncilArea, reduciéndose el mismo a 9983 filas por 15 columnas.

- El punto 1 del ejercicio solicita que se eliminen las variables YearBuilt y BuildingArea.
En nuestro proceso de trabajo, para realizar el proceso de Encoding de variables categóricas 
    - categorical_cols: el que incluye las variables categóricas
        -'Type'
        - 'Suburb'
        - 'Regionname'
        - 'CouncilArea'
    - numerical_cols: el que incluye las variables numérica     
        - 'Rooms'
        - 'zipcode'
        - 'Price'
        - 'Landsize'
        - 'Lattitude'
        - 'Longtitude'
        - 'airbnb_record_count'
        - 'airbnb_price_mean'

- Luego visualizamos la cantidad de datos únicos que tenemos en las variables categóricas y apartamos Suburb, ya que dicha variable posee muchos valores únicos y nos podría generar problemas de dimensionalidad:

```
categorical_cols_filtered = ['Type','Regionname','CouncilArea']
```
- Nos pareció oportuno, dividir el dataset en dos dataframes menores a los que llamamos:

```
melb_df_cat = melb_df[categorical_cols_filtered]
melb_df_no_cat = melb_df[numerical_cols]
```
### Características seleccionadas para el OneHotEncoding.

Utilizamos las librerías

```
!pip install feature_engine
from feature_engine.encoding import OneHotEncoder
```
### Características categóricas:

  - `Type` Tipo de la propiedad. *house*, *unit*, y *townhouse*.
  - `Regionname` Región general de la propiedad.
  - `CouncilArea` Departamento 

Todas las variables categóricas fueron codificadas con un método `OneHotEncoder`.


### Características numéricas:

  - `Price` Precio de la propiedad.
  - `Rooms` Cantidad de habitaciones.
  - `Lattitude` Latitud de ubicación.
  - `Longtitude` Longitud de ubicación.
  - `BuildingArea` Tamaño de la edificación.
  - `YearBuilt` Año de construcción.
  - `Landsize` Tamaño del Terreno.
  - `zipcode` Código postal.
  Proviene del conjunto de datos de AirBnB luego de aplicar el método `merge`.
  - `bnb_price_mean` Precio promedio de la propiedad.
  Proviene del conjunto de datos de AirBnB luego de aplicar el método `merge`, y realizar la operación `mean`.

Nuestro dataset posee ahora: 9983 filas y 54 columnas. 
3 son por Type.
7 son por Regionname.
34 son por CouncilArea.
Y 10 restantes son variables numéricas.


## Ejercicio N° 2: 

Agregamos las variables numéricas de la siguiente manera:
```
relevant_cols = ['BuildingArea', 'YearBuilt']
join_df = encoded_df.join(melb_df[relevant_cols], how='left')
```
Luego procedemos a la estandarización utilizando:
```
from sklearn.preprocessing import StandardScaler
scaler = StandardScaler()
```
Obtenemos como resultado el dataframe: scaled_df. 

Imputación por KNN

Este último dataframe, con sus variables categóricas y numéricas estandarizadas mediante StandarScaler, se encuentra listo para realizar la imputación de valores faltantes mediante la utilización de las siguientes librerías.
```
from sklearn.experimental import enable_iterative_imputer
from sklearn.neighbors import KNeighborsRegressor
from sklearn.impute import IterativeImputer
```
Realizamos esta transformación para las variables 'BuildingArea' y 'YearBuilt' del df y luego para el dataframe completo

Incorporamos un gráfico comparando las distribuciones de datos obtenidas con cada método de imputación.


## Ejercicio N° 3- Reducción de dimensionalidad con PCA

### Datos aumentados

Se agregan los 20 primeros componentes principales, obtenidos al aplicar `PCA` sobre el conjunto de datos totalmente procesado.

Se muestra mediante un gráfico la varianza capturada de los n componentes principales.

Nuestro dataset final, luego de incorporar los 20 primeros componentes principales, posee ahora 9983 filas por 74 columnas.


