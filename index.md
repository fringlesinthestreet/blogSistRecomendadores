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


## #3 Scalable Collaborative Filtering Approaches for Large Recommender Systems

Me encantó el problema analizado, ya que encuentro que es una de las falencias en muchas implementaciones de ML. Muchas veces, se utilizan algoritmos existentes en librerías, pero no se toma en cuenta cómo se va a comportar ese servicio a medida que el sistema escala.

En especial MF, que para entrenar el modelo se requiere de mucho procesamiento, a medida que crece el dataset se vuelve muy caro realizarlo al pie de la letra.

Encontré muy inteligente como repartieron los datos que tenían, ya que esto puede cambiar enormemente cómo se evalúa a un RS, lo cual lo explicitan en una parte favoreciendo al set ordenado.

Si bien pusieron los tiempos de entrenamiento de sus modelos v/s la precisión de estos (RMSE), encuentro que faltó poner los recursos (RAM, núcleos, etc) que fueron utilizados. En cada uno de esos ejes, también se puede mover la precisión y tiempo por lo que sería interesante ver la importancia del contexto en donde se corren estas pruebas.

Una crítica que tengo sobre este paper es que mezclan MF y NB dando a conocer las debilidades de ambos métodos (muy global y muy localizado respectivamente), pero no encontré en ningún lado si su combinación lograba solucionar o mitigar sus deficiencias. Personalmente lo asumí hasta que me surgió la duda.

## #4 Collaborative Filtering for Implicit Feedback Datasets

Me gustó que el autor fuese sincero en que su estudio se basó en *implicit feedback* porque no tenían el setup para obtener *explicit feedback*. Estas decisiones pasan más seguido de lo que uno cree.

Me sorprendió que el autor recalca que en implicit feedback no sirve ajustar los valores (con el promedio de los items o de los usuarios), algo que siempre asumía que mejoraba las predicciones. Obviamente esto sí sirve en explicit feedback.

El supuesto de que si el usuario ha consumido el producto, le gusta y si no; no, lo encuentro brutal ya que me sorprende de que funcione. Eso sí, me gustaría ver qué tanto afecta al _novelty_ y _serendipity_, ya que (según yo) se ven afectados por este supuesto, porque estos buscan asociar usuarios a nuevos productos que no han consumido (por lo que en este _approach_ se asume que no les gusta)

Encontré interesante que incluso con implicit feedback, uno es capaz de dar una algo de transparencia de las recomendaciones. Más aún cuando se utilizan factores latentes ya que se vuelven una “abstracción” de los gustos del usuario.

Si bien el paper era interesante, encontré que muchas veces las decisiones de diseño de sus algoritmos recaían mucho en el dominio que se estaba analizando. Me hubiese gustado que fuese más general.

Un detalle (muy menor pero me molestó) es que comienza el paper haciendo gancho con e-commerce, pero luego salta a todo el análisis de las películas.

## #5 Performance of Recommender Algorithms

El paper se basa en que criterios clásicos de error (MAE y RMSE) no son buenos para evaluar recomendadores top-N. Para estas prácticas es mejor utilizar formas "alternativas" de evaluación (en su año ~2010) basadas en _accuracy_,  como el _recall_ y _precision_.

Para evaluar esto se utilizaron varios recomendarores del estado del arte de la época (MovieAvg, TopPop, AsymSVD, pureSVD, etc) y se utilizaron evaluaciones basadas en _accuracy_ para evaluarlos.

Encontré interesante que en el test set solo dejaran las evaluaciones de 5 estrellas. Es muy ingenioso ya que así se aseguran de que cuando la lista top-N contenga una película del test set es un _hit_ y si no, un _miss_. Esto eso sí, disminuía considerablemente el tamaño del test set.

Hubiese sido interesante que utilizaran para comparar otras métricas (como MAP y DCG) para ver el efecto tanto en el posicionamiento y no sólo la presencia del item en la lista entregada como recomendación.
