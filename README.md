![HenryLogo](https://assets.soyhenry.com/henry-landing/assets/Henry/logo-white.png)

# Proyecto Individual 1: Machine Learning Operations (MLOps)

## Alumno: Hernandez Corona Yahir

## Introduccion

El proyecto individual 1 es una aplicación basada en una API que proporciona información y funcionalidades relacionadas con películas. El objetivo principal de esta aplicación es permitir a los usuarios obtener datos estadísticos, realizar consultas y recibir recomendaciones de películas con base en diversos criterios.

El proyecto cuenta con una serie de características principales, que incluyen la posibilidad de obtener la cantidad de filmaciones en un mes o día específico, conocer el score de una película, obtener la cantidad de votos y el promedio de votaciones, explorar información sobre actores y directores, así como recibir recomendaciones de películas similares a partir de un título específico.

El proyecto se ha desarrollado utilizando el framework FastAPI y se ha implementado un sistema de recomendación de películas basado en el cálculo de similitud de puntuaciones, similitud de generos y similitud en el nombre de las peliculas. También se ha realizado un análisis exploratorio de los datos para identificar relaciones entre variables y patrones interesantes.

Finalmente, la aplicación se ha desplegado utilizando el servicio de Render, lo que permite que la API sea consumida desde la web. Además, se ha creado un video que demuestra el funcionamiento de las consultas propuestas y del modelo de machine learning entrenado.

## Requisitos previos

Tener instalado Python y un editor de texto como Visual Studio Code.



El cual posee los archivos:
* README.md : Readme del proyecto.
* data_preparada_ML.csv : Dataset para ser consumido por el modelo de ML.
* montaje_API.py : Archivo python que contiene el codigo para montar la API con las funciones.
* requirements.txt : Archivo que contiene las librerias con la version necesaria de cada una para el proyecto.
* Link_data.txt : Archivo donde se encuentra el link para descargar los dataset necesarios para el proyecto.

## Instalación

Instalar dependencias ejecutando en una terminal: pip install -r requirements.txt

Asegúrate de ejecutar este comando desde la ubicación raíz del proyecto, donde se encuentra el archivo requirements.txt.

## Uso

Ejecutar el proyecto en el siguiente orden:

1° ejecutar: PI1_Preparacion_datos_José_final.ipynb

En este notebook se encuentra el proceso detallado de la preparacion de los datos, el proceso de ETL. El mismo crea los archivos "data_preparada_movies_parte1.csv", "data_preparada_movies_parte2" estos archivos estan por partes para poder subirlos al repositorio de github ya que el mismo tiene un limite de 25MB por archivo, y "data_preparada_ML" en el cual solo hay una parte del dataset (8000 registros) para minimizar el uso de memoria RAM, ya que render tiene un limite de 510mb en su version gratuita.

2° ejecutar: PI1_análisis_exploratorio_datos_José.ipynb

En este notebook se encuentra un analisis exploratorio de los datos (EDA) donde se pueden vizualizar graficos y una nube de palabras frecuentes de los titulos.

3° ejecutar: montaje_API.py

En este archivo de python se encuentra el codigo para crear la API de forma local utilizando fastAPI.



En este link se encuentran las 7 funciones desarrolladas.

## Funciones desarrolladas

Para ejecutar los links de cada funcion debe estar en funcionamiento la API desde el archivo montaje_API.py

### 1° Función: Cantidad de filmaciones estrenadas por mes.

La funcion recibe un mes en español y devuelve la cantidad de películas estrenadas en ese mes. Puedes acceder al endpoint usando el mes como un parámetro de la URL, por ejemplo: http://localhost:8000/cantidad_filmaciones_mes/{mes}

Resumen de funcionamiento:

La función recibe el mes como un parámetro de tipo str en español y lo convierte a minúsculas. Luego, mapea los nombres de los meses en español a los números de los meses en inglés y verifica si el mes ingresado es válido. A continuación, filtra las filas que corresponden al mes consultado y devuelve la cantidad de películas en ese mes en forma de un string formateado.

### 2° Función: Cantidad de filmaciones estrenadas por día de la semana.

La funcion recibe un día en español, por ejemplo "lunes", y devuelve la cantidad de películas estrenadas en ese día. Puedes acceder al endpoint usando el dia como un parámetro de la URL, reemplazando {dia} con el día en español: http://localhost:8000/cantidad_filmaciones_dia/{dia}

Resumen de funcionamiento:

Toma un parámetro dia que representa el día de la semana sobre el cual se desea obtener la cantidad de filmaciones. Este parámetro se obtiene de la URL de la solicitud.

Inicializa un contador en 0 para contar la cantidad de filmaciones.

Define un diccionario dias_semana que mapea los nombres de los días en español a los números de los días de la semana.

Convierte el día ingresado a minúsculas para asegurar la consistencia en la comparación.
Verifica si el día ingresado es válido, es decir, si está presente en el diccionario dias_semana. Si no es válido, se devuelve un mensaje indicando que el día es inválido.

Itera sobre las fechas de estreno en la columna "release_date" del DataFrame data.

Para cada fecha de estreno, se convierte la cadena de texto en un objeto de fecha y hora utilizando datetime.datetime.strptime. Esto permite obtener el día de la semana correspondiente a esa fecha.

Se compara el día de la semana obtenido con el valor correspondiente en el diccionario dias_semana. Si son iguales, significa que la filmación se estrenó en el día consultado, por lo que se incrementa el contador en 1.

Finalmente, se devuelve un mensaje indicando la cantidad de filmaciones que se estrenaron en el día consultado. El método capitalize() se utiliza para convertir la primera letra del día en mayúscula y el resto en minúscula.

### 3° Función: Puntaje de la filmación.

Se ingresa el título de una filmación esperando como respuesta el título, el año de estreno y el score. Puedes acceder al endpoint usando el nombre de la pelicula como un parámetro de la URL, reemplazando {titulo_de_la_filmación} con el nombre de la pelicula. http://localhost:8000/score_titulo/{titulo_de_la_filmación}

Resumen de funcionamiento:

Realiza la búsqueda de la película en el DataFrame data utilizando la condición data['title'] == titulo_de_la_filmacion. Esto devuelve un DataFrame que contiene las filas que cumplen con la condición de igualdad.

Verifica si el DataFrame pelicula está vacío utilizando el método empty. Si está vacío, significa que no se encontró la película en el DataFrame y se devuelve el mensaje "Película no encontrada".

Si se encontró la película, se procede a obtener los valores de título, año de estreno y score de la primera fila del DataFrame pelicula.

El título se obtiene utilizando pelicula['title'].iloc[0], accediendo al primer registro de la columna "title" del DataFrame.

El año de estreno se obtiene utilizando pelicula['release_year'].iloc[0], accediendo al primer registro de la columna "release_year" del DataFrame.

El score se obtiene utilizando pelicula['popularity'].iloc[0], accediendo al primer registro de la columna "popularity" del DataFrame.

Finalmente, se devuelve un mensaje formateado que incluye el título de la película, el año de estreno y el score.

### 4° Función: Cantidad de votos y valor promedio de las votaciones de la filmación.

Se ingresa el título de una filmación esperando como respuesta el título, la cantidad de votos y el valor promedio de las votaciones. La misma variable deberá de contar con al menos 2000 valoraciones, caso contrario no se devuelve ningún valor. Puedes acceder al endpoint usando el nombre de la pelicula como un parámetro de la URL, reemplazando {titulo_de_la_pelicula} con el nombre de la pelicula.
http://localhost:8000/votos_titulo/{titulo_de_la_pelicula}

Resumen de funcionamiento:

Realiza la búsqueda de la película en el DataFrame data utilizando la condición data['title'] == titulo_de_la_pelicula. Esto devuelve un DataFrame que contiene las filas que cumplen con la condición de igualdad.

Verifica si el DataFrame pelicula está vacío utilizando el método empty. Si está vacío, significa que no se encontró la película en el DataFrame y se devuelve el mensaje "La película no existe en el dataset".

Si se encontró la película, se procede a obtener los valores de título, cantidad de votos, valor promedio de las votaciones y año de estreno de la primera fila del DataFrame pelicula.

El título se obtiene utilizando pelicula['title'].iloc[0], accediendo al primer registro de la columna "title" del DataFrame.

La cantidad de votos se obtiene utilizando pelicula['vote_count'].iloc[0], accediendo al primer registro de la columna "vote_count" del DataFrame.

El valor promedio de las votaciones se obtiene utilizando pelicula['vote_average'].iloc[0], accediendo al primer registro de la columna "vote_average" del DataFrame.

El año de estreno se obtiene utilizando pelicula['release_year'].iloc[0], accediendo al primer registro de la columna "release_year" del DataFrame.

Se verifica si la película cumple con la condición de tener más de 2000 votos. Si la cantidad de votos es menor a 2000, se devuelve un mensaje indicando que la película no cumple con esa condición y se muestra la cantidad de votos obtenidos. Si la cantidad de votos es mayor o igual a 2000, se devuelve un mensaje indicando el título de la película, el año de estreno, la cantidad de votos y el valor promedio de las votaciones.

### 5° Función: Éxito de un actor.

Se ingresa el nombre de un actor que se encuentre dentro del dataset debiendo devolver el éxito del mismo medido a través del retorno, la cantidad de películas en las que ha participado y el promedio de retorno. Puedes acceder al endpoint usando el nombre del actor como un parámetro de la URL, reemplazando {nombre_actor} con el nombre del actor. http://localhost:8000/get_actor/{nombre_actor}

Resumen de funcionamiento:

Filtra las filas del DataFrame data que contienen al actor consultado. Esto se realiza utilizando la función apply en la columna 'names_actors' y aplicando una función lambda que verifica si el nombre del actor está presente en la lista de nombres de actores de cada película. El resultado es un nuevo DataFrame llamado peliculas_actor que contiene las filas que cumplen con la condición.

Verifica si el DataFrame peliculas_actor está vacío utilizando el método empty. Si está vacío, significa que no se encontraron películas para el actor consultado y se devuelve un mensaje indicando esto.

Si se encontraron películas para el actor, se procede a obtener la cantidad de películas en las que ha participado y el promedio de retorno.

La variable cantidad_peliculas se obtiene utilizando la función len(peliculas_actor), que devuelve la cantidad de filas en el DataFrame peliculas_actor.

El promedio de retorno se calcula utilizando peliculas_actor['return'].mean(), que calcula el promedio de los valores en la columna 'return' del DataFrame peliculas_actor.

El retorno total se calcula sumando todos los valores en la columna 'return' del DataFrame peliculas_actor utilizando sum(peliculas_actor['return']).

Finalmente, se devuelve un diccionario con los siguientes campos: 'actor' (nombre del actor), 'cantidad_filmaciones' (cantidad de películas en las que ha participado), 'retorno_total' (suma de los retornos de todas las películas) y 'retorno_promedio' (promedio de los retornos de las películas).

### 6° Función: Éxito de un director.

Se ingresa el nombre de un director que se encuentre dentro de un dataset debiendo devolver el éxito del mismo medido a través del retorno. Además, deberá devolver el nombre de cada película con la fecha de lanzamiento, retorno individual, costo y ganancia de la misma. Puedes acceder al endpoint usando el nombre del director como un parámetro de la URL, reemplazando {nombre_director} con el nombre del director. http://localhost:8000/get_director/{nombre_director}

Resumen de funcionamiento:

Filtra las filas del DataFrame data que contienen al director consultado utilizando la condición data['name_director'] == nombre_director. El resultado es un nuevo DataFrame llamado peliculas_director que contiene las filas que cumplen con la condición.

Verifica si el DataFrame peliculas_director está vacío utilizando el método empty. Si está vacío, significa que no se encontraron películas para el director consultado y se devuelve un mensaje indicando esto.

Si se encontraron películas para el director, se procede a calcular la suma del retorno de inversión total utilizando el método sum() en la columna 'return' del DataFrame peliculas_director.

Se crea una lista llamada peliculas_info para almacenar la información de cada película.

Se itera sobre cada fila del DataFrame peliculas_director utilizando iterrows(). Para cada película, se obtienen los valores de título, año de lanzamiento, retorno individual, costo y ganancia.

Se crea un diccionario llamado pelicula_info con la información de la película.
Se agrega el diccionario pelicula_info a la lista peliculas_info.

Una vez se han recorrido todas las películas, se crea el diccionario de respuesta llamado respuesta con la suma del retorno total y la lista de películas.

Se devuelve el diccionario respuesta.

### 7° Función: Recomendacion de peliculas.

La función recomendacion(titulo) tiene como objetivo recomendar películas similares a una película dada.Se ingresa el nombre de una película y te recomienda las similares en una lista de 5 valores. Puedes acceder al endpoint usando el nombre de la pelicula como un parámetro de la URL, reemplazando {titulo} con el nombre de la pelicula. http://localhost:8000/recomendacion/{titulo}

Resumen de funcionamiento:



## Créditos
El recurso externo que mayormente use para crear mi proyecto es chatGPT.

## Contacto
Telefono celular: +543875990930

Email: hernandezcoronayahir4@gmail.com


