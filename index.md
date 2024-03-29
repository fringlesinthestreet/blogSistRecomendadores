## Blog Sistemas Recomendadores

Autor: Benjamín Cárcamo

[Entregas atrasadas al final](https://github.com/fringlesinthestreet/blogSistRecomendadores/blob/master/index.md#entregas-atrasadas)

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

# #6 Content-based artwork recommendation 

El paper se basa en detallar un algoritmo basado en contenido para recomendar piezas de arte física (pueden ser cuadros, esculturas). Se utilizó un algoritmo basado en contenido debido a que en el mundo del arte hay piezas que tienen un solo dueño, dificultando el trabajo de un algoritmo basado en CF.

Para esto se utilizó metadata precomputada de los cuadros (color, tema, estilo, etc) y un algoritmo visual (que normalmente es usado para videos o ropa). Se utilizó la similitud de coseno para obtener los scores. 

Entre los resultados, se obtiene que FA es un buen algoritmo para realizar predicciones (simple y buen desempeño). Me encantó el detalle que dieron que igual puede no ser confiable debido a que si un usuario ha comprado todos las las obras de su FA, entonces resulta difícil obtener un nuevo FA. Además, comprueban esta sospecha utilizando distintos perfiles de usuarios y comprobando que a medida que crece la cantidad de items estos algoritmos decrecen en desempeño.

Me gustó la conclusión de que los basados en reconocimiento de imágenes se mantenían más estables, pero encuentro que faltó considerar los recursos que se deben utilizar para realizar el reconocimiento de una escultura. Al comienzo se hace hincapié en que las ventas físicas incluían tanto cuadros como esculturas de artistas, pero luego en el análisis pareciera que sólo hablan de esculturas al extraer las _features_ que dictan. Una imagen de una escultura se ve muy influenciada en cómo se saca su foto (versus un cuadro que es 2D nomás).

## #7 Document Clustering Based On NMF

En este paper se propone una forma de realizar clustering de documentos, el cual se basa en abstraer los documentos en una adición combinada de distintos ejes (tópicos bases). La idea después es clusterear utilizando el eje (tópico base) con mayor valor de proyección.

Algo interesante es que se considera (muy inteligentemente) que los tópicos (ejes) pueden estar relacionados entre sí, por lo que no son necesariamente ortogonales, como uno puede asumir en una solución más _naive_.

Como bien dijo el profe, sí se siguen usando técnicas clásicas, por ejemplo stemming, para hacer un preprocesamiento de las palabras del texto y tener un mejor _recall_.

La verdad me encantó que siempre le dieran un significado a sus resultados. Por ejemplo, al concluir que al no forzar ortogonalidad se llega a mejores resultados, los autores hacen hincapié en su significado “humano” y es que no tiene sentido forzar esa distancia ya que en la vida real hay tópicos más relacionados con otros y la separación no debe ser siempre “cuadrada”.

Lo que quizás faltó que pocos papers encuentro que lo agregan, es como escala este approach. Puede ser que entregue un mejor resultado pero quizás se pierde en aspectos de eficiencia o recursos. Me hubiese gustado al menos una comparación empírica de la complejidad de ejecutar este algoritmo en comparación a los del estado del arte.

## #8 Content-Based Recommendation System 

Me encantó la introducción que tuvo. Era súper amigable al leer (a veces innecesariamente simple) y presentaba bien el paper. Además, realizaba una introducción a distintos tipos de sistemas recomendadores.

Me fijé que era un libro y que este capítulo se encargaba de explicar los principales tipos de sistemas de recomendación basados en contenido. Cómo está el estado del arte y principales ventajas/desventajas de las técnicas descritas.

Me interesó que nombra que se puede combinar content-based y CF para poder transformar los ratings a algo más basado en el contenido al cuál el usuario le dió like/dislike. Esta combinación es algo como lo que estamos usando en la tarea y hace mucho sentido ahora que lo pienso.

Eso de tratar a las queries pasadas de los usuarios como documentos “que le gustan” creo que es parte de lo que usa youtube para recomendar. Lo interesante es que se puede “deshabilitar” este seguimiento por parte de youtube y se nota el cambio en las recomendaciones (al fin no me salen puras cosas de lol jeje)

Siendo completamente sincero me decepcioné cuando me dí cuenta que era un libro, ya que estaba bien básica la introducción y pensé que en algún momento iba a tomar vuelo en algo interesante. No es que no fuera interesante pero faltaron esos "tips" que dan a veces los papers.

## #9 BPR: Bayesian Personalized Ranking from Implicit Feedback 

El paper se propone un método para realizar recomendaciones de _implicit feedback_. Los autores proponen el framework BPR con un criterio particular para optimizar y uno para realizar el _learning_ de BPR, comparándolo con los criterios actuales utilizados al combinar BPR con algoritmos recomendadores de la época como MF y KNN. 

El criterio optimizado por el algoritmo propuesto en el paper, es poder entregar un ranking personalizado (a través de ordenamiento de pares). Este es distinto al que el autor llama “típico” de los algoritmos de MF y KNN, los cuales optimizan sus parámetros para poder saber si un item es seleccionado por un usuario o no.

En este paper se utiliza el ordenar pares de items en vez de reemplazar los valores faltantes por valores negativos. El criterio es simple: siempre se prefiere al item que el usuario ha visto por sobre otro que no. Si se tiene un par de items I y J, si el usuario ha visto ambos o ninguno, no se puede saber la preferencia. 

Me gustó que explicara el aporte de performance que da el sampling de los pares en comparación a recorrer todos los pares del dataset. Además, el hecho de que se dieran cuanta que no era necesario completar el ciclo para obtener convergencia, sino que con una fracción de este puede bastar. 

Me hubiese gustado que no solo hayan intentado de hacer sampling con distribución uniforme, sino que buscar alguna manera de sacar las muestras inteligentemente. No digo que es malo, ya que cumple con el objetivo de evitar que se saque el mismo orden de forma consecutiva, pero sería interesante quizás forzar distintos órdenes, por ejemplo.

## #10 Deep Neural Networks for YouTube Recommendations 

Este paper trata de manera muy general los desafíos de recomendación de youtube (muchos datos, se deben recomendar videos nuevos, mucho ruido) y cómo utilizando redes neuronales profundas lo resuelven de manera eficiente. El problema de recomendación se separa en dos sub-problemas: generación de candidatos y ranking.

Se utilizan los candidatos seleccionados por la red neuronal para ser utilizados para la recomendación (dado que si se usan todos, serían demasiados videos por usuario) y luego se utiliza la red de ranking para obtener la lista de recomendación final.

En el paper se realizan comparaciones de precisión utilizando solo la información histórica de los videos vistos, utilizando también el historial de búsquedas e información contextual (edad, género, región, etc) normalizada. Se mostró que mientras más información utiliza la red, mejor es la predicción.

Además, en el paper se toca el tema de la estructura de la red neuronal (cuántos niveles y cuántas neuronas por nivel), pero encuentro que se ve súper poco y sólo se muestra una tabla resumen para ver las precisiones de las distintas configuraciones. Lo que rescato es el cómo muestran los resultados del cruce entre: los distintos tamaños de la red y la información utilizada. El gráfico lo deja súper claro.

Me pareció muy curioso eso de que tratan al historial de búsqueda como “una bolsa de tokens” y de hacer un rollback a un momento de su historia y aprovechar esas vistas en vez de siempre utilizar las más recientes. Personalmente, hoy en día he visto muchos videos de #Teloresumoasínomás y youtube no me recomienda puras cosas de ese canal (hay algunas, pero si se basara fuertemente en los últimos videos vistos, serían solo de esas).

La crítica más grande que tengo es que no dan muchos detalles de cómo lo hicieron. Entiendo que es una empresa y su valor está en su tecnología, pero por último tener una referencia de algo más simple pero práctico. Por esto mismo encuentro que es difícil refutar lo que youtube dice en el paper. Si no se puede reproducir, no se puede comprobar que lo que dicen es verdad.

## #11 Ensemble Recommendations via Thompson Sampling: an Experimental Study within e-Commerce

En este paper se explica una extensión de _Thompson Sampling_. En su forma más pura es heurística busca maximizar la ganancia (o minimizar la pérdida) basado en elegir la ‘acción’ (en este caso la recomendación) que maximice la ganancia esperada. La idea es utilizar un ensemble de recomendadores para “precargar” un set de acciones disponibles.

Esta extensión se implementa en el contexto del e-commerce y se busca que sea los más amigable posible para ser integrado en las aplicaciones existentes, ya que se basa en componentes que se van agregando para que el Thompson Sampling tenga mayor cantidad de opciones.

Mis únicos comentarios son que, si bien muestran un análisis de los _datasets_ utilizados, no dicen cómo separaron el dataset entre entrenamiento, validación y test. Además, tampoco explicitan el porqué usaron N=5, puede que las métricas cambien si agrandan el N a 10 por ejemplo.

## #12 The Effect of Explanations and Algorithmic Accuracy on Visual Recommender Systems of Artistic Images

En este paper se ven dos algoritmos para recomendar piezas de arte: DNN (Deep Neuronal Networks) y AVF (Attractiveness Visual Features). Cada uno tiene sus ventajas y desventajas con respecto al otro. Por ejemplo, DNN es mejor con respecto a la precisión pero AVF le gana en transparencia, es decir, poder explicar porqué se está recomendando algo. Esto es importante, porque al parecer el poder explicar las recomendaciones aumenta la satisfacción de los usuarios.

Encontré que adecuaron algunas de las decisiones de diseño tomadas. Por ejemplo, el hecho de que se trata de obras y que cuando una se vende ya no está disponible “para que otro usuario pueda comprarlo”, hace que Collaborative Filtering pierda sentido y por eso ellos utilizan _content-based recomendations_.

Es interesante que con DNN igual se puede saber “porqué” el algoritmo recomienda una pieza de arte. Eso sí, se basa en los rasgos o _features_ obtenidos por una red convolucional, y el problema recae en que estos rasgos o _features_ pueden ser perfectos para relacionar las piezas de arte pero no son ni el brillo, ni ninguna característica “típica” para las personas. 

Es por esto que encontré que fue una buena decisión presentar la explicación de la recomendación a nivel _black-box_, es decir, dado que se recomienda una obra porque se parece a otra (en base a estos rasgos o _features_ no entendibles), ellos no entran en los detalles de porqué esa pieza de arte se parece a la otra ( _white-box_ ), pero informan de que se está recomendando porque se parece a la otra ( _black-box_ ). Esta es una diferencia sutil pero hace que se pueda explicar, a grandes rasgos, porqué se está recomendando la imagen.

La interfaz utilizada ayuda muchísimo a visualizar las recomendaciones a nivel _white-box_ o _black-box_. Me quedó la duda: ¿de dónde salió? ¿Las hicieron ellos? quizás se me pasó pero no se habla mucho de su origen (poca relevancia con el estudio)

Otra duda que me queda es que como DNN genera miles de dimensiones, se podría hacer una relación lineal entre estas dimensiones y las utilizadas por el algoritmo AVF? Si se puede, se podría utilizar la interfaz que proporciona mayor información a los usuarios (mejor transparencia) y utilizar DNN (mejor precisión).

-----

# Entregas Atrasadas

## #13 Evaluating Collaborative Filtering Recommender Systems 

Hoy en día existen distintos tipos de algoritmos para realizar CF. A estos algoritmos se les hacen distintas evaluaciones para identificar cuál realiza mejores recomendaciones que otros. Esto es especialmente difícil porque existen muchos factores que influyen en la evaluación.

Esto hace que sea difícil comparar distintos estudios de sistemas recomendadores basados en CF, ya que cada uno prueba sus algoritmos utilizando (muchas veces) distintos datasets, distintas métricas e incluso puede ser que el fin u objetivo de cada sistema recomendador sea distinto.

Por ejemplo, las características del dataset influyen ya que algunos algoritmos están diseñados para ser mejores con ciertas condiciones (cantidad de usuarios > cantidad de items, o viceversa). Por otro lado, puede ser que un sistema se enfoque en realizar recomendaciones para que los usuarios descubran nuevos items, entonces los usuarios pueden quedar felices si hay 1 item “nuevo y correcto” en el ranking. En contraste, puede ser que el enfoque sea en que se maximice la cantidad de items que le acierta el sistema para que el usuario compre más.

Es muy interesante cuando habla de las tareas que no habían sido estudiadas en la literatura. Por ejemplo, el de encontrar **todos** los “buenos items” a priori suena mala idea, pero dado el contexto que se presenta (abogados) hace mucho sentido. Otro muy interesante es el _Just browsing_, el cual yo lo he hecho y probablemente (sin saberlo) fui “descalibrando” el sistema recomendador que me estaba asistiendo en mi búsqueda y yo no terminaba comprando las cosas que sí me gustan (y que en ese instante no tenía intención de comprar nomás).

El último de estas tareas, el _Find Credible Recommender_, lo hablamos en clases y al parecer sí se han hecho estudios sobre esto. Quizás no sobre el contexto de Sistemas Recomendadores pero puede ser que sea sobre comportamiento de las personas. El profe contaba que a veces era útil poner en la lista de recomendación una película que el usuario ya había visto (con alto rating del usuario) para que “sienta que la recomendación estaba bien”.

Algo que siempre encuentro que falta en los papers que presentan un nuevo sistema recomendador, es que se no se fijan en el manejo de recursos que toma el algoritmo en comparación a sus baselines. En esta lectura no se ve nada de eso tampoco y encuentro que es un punto de comparación fuerte para que, una empresa por ejemplo, adopte un algoritmo que aparece en una publicación y pueda integrarlo a su sistema productivo.

Otro tema que explicitan que no se ve en la lectura es la robustez, la cual no sabía a qué se refería en el contexto de los sistemas recomendadores y es todo un desafío para poder sanitizar los datos recolectados por los sistemas recomendadores y evitar que a través de _spam_ se pueda privilegiar a ciertos productos, películas, etc.

## #14 Evaluating Recommendation Systems 

En esta lectura también se enfrentan al problema de cómo evaluar distintos sistemas recomendadores. Se da un enfoque basado en que los sistemas recomendadores tienen propiedades propias y que, dependiendo de esas propiedades, se deben realizar los experimentos de un modo u otro (offline, grupo de usuarios u online). Luego, se rankean los algoritmos recomendadores según estas propiedades.

El estudio no termina ahí, sino que ayuda a mejorar la calidad de las conclusiones y decisiones que se pueden tomar luego de realizar los distintos tipos de experimentos. Por ejemplo, es muy deseable poder generalizar los resultados de los experimentos, pero para esto se debe tener cierto criterio para poder realizarlo. Se debe probar sobre varios datasets, por ejemplo. Se debe entender bien las propiedades de los datasets utilizados, ya que los resultados pueden potenciarse por sus características.

Un tema que se habla es que el fin de las pruebas offline es poder dejar fuera la mayor cantidad de candidatos de algoritmos para evitar elevar costos al realizar estudios posteriores (_user studies_ y online). Sin embargo, muchas veces en los paper sólo se realizan este tipo de pruebas debido a que se usan datasets públicos.

Igual se habla de que si vienen _timestamps_ en los datos, se puede “simular” una prueba online tomando algunos usuarios y “parándose en cierto punto del tiempo”, y así simular cómo estaría entrenado el algoritmo para ese punto y ver cómo responde a la situación.

Un dato interesante es que en los estudios de usuarios, según el paper, las personas tienden a sastisfacer a la compañía/investigadores, incluso más aún si estos fueron pagados para realizar las pruebas. El consejo que dan en este sentido es que no se debe hacer explicito el objetivo del experimento a las personas del estudio.

En términos prácticos se habla de tests estadísticos como herramientas para realizar mejores conclusiones. Me gustó la métrica del _sign test_ ya que como las pruebas se iban haciendo por usuario, se ve si un algoritmo recomienda mejor **para la mayor cantidad de usuarios**.

En este paper también se habla de _coverage_ y me gustó el hincapié que hace con los _cold starts_. Se habla de que puede ser bueno (se debe evaluar) recomendar estos items fríos a costa de algunos calientes, por lo que disminuye probablemente el _Accuracy_, pero podría potencialmente aumentar el _novelty_ y _serendipity_.

En este paper se habla del _trust_ de los usuarios, que aumenta al encontrar recomendaciones inútiles pero acertadas. Dice que otra forma de saber la confianza del sistema es preguntarle a los usuarios en el _user study_. Encuentro que igual se contradice porque es probable que, como se dijo antes, el usuario trate de “hacernos sentir bien” y las respuestas pueden estar contaminadas.

Siendo sincero, al principio del curso no me gustaban este tipo de publicaciones. Pero ahora que ha pasado un buen tiempo desde que leía un paper, está bueno para recordar cosas y aprender.
