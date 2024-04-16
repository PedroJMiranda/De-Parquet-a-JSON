# De-Parquet-a-JSON
### Tutorial para convertir un archivo Parquet en un archivo JSON, mediante código Python.

## 1.- Importa la biblioteca que usaremos que usaremos:
```
import pandas as pd
```
> pandas: para trabajar con DataFrames de Python.
```
import pyarrow as py
```
> pyarrow: para trabajar con archivos Parquet.
```
import json
```
> json: para trabajar con archivos JSON.

## 2.- Definiremos las rutas y los nombres de los archivos:
```
in_path = "C:\\Users\\ianus\\OneDrive\\Escritorio\\Henry\\M4\\Clase 04\\Homework\\Proyecto\\input"
```
> in_path: Ruta del directorio de entrada (donde se encuentra el archivo Parquet, en este ejemplo te muestro la de mi ordenador).
```
out_path = "C:\\Users\\ianus\\OneDrive\\Escritorio\\Henry\\M4\\Clase 04\\Homework\\Proyecto\\output"
```
> out_path: Ruta del directorio de salida (donde se guardará el archivo JSON, nuevamente te muestro el de mi ordenador).
```
in_file = 'business.parquet'
```
> inp_file: Nombre del archivo Parquet.
```
out_file = 'business.json'
```
> out_file: Nombre del archivo JSON.

## 3.- Abrimos el archivo JSON:
```
archivo_json = open( out_path + out_file, 'w')
```
> Se abre el archivo JSON en modo de escritura utilizando la función open().
## 4.- Definir una función clave_valor:
```
def convertir_clave_valor(tupla):
    clave, valor = tupla
    return f"'{clave}',' {valor}'"
```
> Esta función toma una tupla como entrada y devuelve una cadena con la clave y el valor de la tupla formateados como un par clave-valor JSON.
## 5.- Leer el archivo Parquet y convertirlo en un DataFrame:
```
df = pd.read_parquet(in_path + in_file)
```
> Se utiliza la función pd.read_parquet() para leer el archivo Parquet y convertirlo en un DataFrame de Pandas.
## 6.- Obtener las columnas del DataFrame:
```
columnas = df.columns
```
> Se obtiene una lista de las columnas del DataFrame utilizando la propiedad columns.

## 7.- Construir una lista de colecciones a partir del DataFrame:
```
colecciones = list()
```
> Se crea una lista vacía llamada colecciones.
```
for file in range (len(df)):
```
> Se recorre cada fila del DataFrame y se crea una lista de pares clave-valor para cada columna.
```
colecciones.append([convertir_clave_valor((columnas[columna],str(df.iloc[fila,columna])))for columna in range (len(columnas))])
```
> Se agrega la lista de pares clave-valor a la lista colecciones.
## 8.- Construir el archivo JSON:
```
archivo_json.write("[ \r")

for indice, coleccion in enumerate(colecciones):
    archivo_json.write(" {\r")
    for columna in range(len(coleccion)):
        if columna == len(coleccion) - 1:
            archivo_json.write(coleccion[columna] + " \r ")
        else:
            archivo_json.write(coleccion[columna] + ",\r")
    if indice == len(colecciones) - 1:
        archivo_json.write("} \r ")
    else:
        archivo_json.write("}, \r ")

archivo_json.write ("] \r ")
archivo_json.close ()
```
> Este script asume que colecciones es una lista de listas, donde cada lista interna representa una colección de elementos.
Los tipos de datos de los elementos en colecciones son adecuados para la serialización JSON (cadenas, números, booleanos, etc.).










