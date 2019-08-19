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
