## Blog Sistemas Recomendadores

Autor: Benjamín Cárcamo

### Opiniones

## #1 Slope One Predictors for Online Rating-Based Collaborative Filtering.

Los algoritmos Slope One Utilizan las recomendaciones de otros usuarios y ven cuánto es la diferencia de sus ratings de distintas películas para dar la predicción a una que un usuario no ha hecho rating. La siguiente figura ejemplifica el caso más básico.

![Imgur](https://i.imgur.com/QhPSWpY.png)

En este ejemplo se tienen 2 usuarios A y B. Para poder predecirle el rating al que el usuario B le dará al Item J, el usuario A y B deben haberle dado rating a un Item común, en este caso es el Item I. Además, el usuario A debe haberle dado rating al Item J. Una vez cumplidos estos requisitos, se usa la diferencia de rating dados por el usuario A y se utiliza esa diferencia para obtener el rating que el usuario B le dará al Item J.

Hay 2 variantes analizadas de este algoritmo, la más precisa diferencia las diferencias cuando los usuarios les gustó (like) un Item o no les gustó (dislike), utilizando su rating promedio de referencia. Este algoritmo es competitivo con los de "memoria", que en este caso se utilizó de benchmark al Pearson normal.

Es interesante el paper porque su principal fortaleza es que la implementación práctica se hace más viable. Esto a través de un menor nivel de procesamiento requerido. Siempre al tratarse (teóricamente) de métodos de predicción se busca un óptimo pero en este caso se basa en una aproximación para evitar el cálculo extra, el cual no da tanta ventaja.

Un tema es que se compara sólo con el más simple de los Pearson, generalizando esa familia de RecSys siendo que el más simple de los Pearson se compara con el Bi-Polar Slope One.

Lo que encuentro que faltó fue una comparación en los tiempos de procesamiento y memoria utilizadas por los algoritmos. Esto porque se basa en que la implementación es más simple y que se gastan menos recursos que los otros algoritmos pero en ningún minuto se realiza un Benchmark considerando estos puntos.

## #2 Collaborative filtering recommender systems. In The adaptive web

Este paper describe de qué se tratan los sistemas CF, qué tipos de estos sistemas existían en su año (creo que es 2007), cuándo son útiles, sus desafíos y problemas, _tradeoffs_ de las distintas interfaces de ratings y el cómo se evalúa la calidad de las recomendaciones y predicciones de estos sistemas.

Encontré que realiza una explicación muy detallada de cómo funcionan los sistemas basados en usuarios e ítems, los cuales no me habían quedado tan claros de las clases. También explica otros CFs (probabilístico, no probabilístico, reglas de asociación, etc), pero muy superficialmente. 

Me gustó que nombrara los problemas prácticos de las implementaciones de los algoritmos. Por ejemplo, con el user-based da ideas de otros investigadores como el _clustering_ y el _subsampling_, de los cuales da una breve explicación.

Además, plantea los distintos problemas que comparten todos los CF. En este aspecto encuentro que faltó detalle de cuales técnicas para ajustar usuarios/items con pocos ratings es más efectiva ya que el autor nombra 3 (Descartarlas, ajustarlas según su número de ratings o incorporar un dato basado en una distribución) pero no da más información de cuál es la mejor según distintos autores.

Me encantó el hincapié que hizo con que el MAE tiene el problema de que no pondera el error por la “importancia” que se les había dado a las recomendaciones. Es muy cierto que mientras mayor sea la posición de una recomendación, su error debería ser castigado en una mayor medida ya que las recomendaciones más altas son las que más importan al fin y al cabo.

Por último, me pregunto porqué no se puede automatizar el _novelty_ y el _serendipity_. Esto porque se podría “encuestar” a los usuarios cuando se realizan estas recomendaciones, preguntando si ese dominio es nuevo para ellos o no (y si les gustó). Quizás es muy invasivo, pero en el paper se habla de que se usa a gente real (osea participantes en vivo) para poder saber si funcionó o no.
