# Estructura de un experimento de medida del coste temporal

El análisis empírico se realiza para constatar los resultados del análisis teórico. Al diseñar un experimento para medir tiempos empíricos, se deben tomar en consideración los siguientes aspectos fundamentales:

* **La medida de tiempo debe hacerse para varias tallas**: el objetivo es obtener el perfil de una función empírica de coste temporal (tiempo en función de la talla). Por tanto, se deben medir los tiempos para diferentes tamaños del problema.
* **Las instancias significativas deben medirse de forma independiente**: los casos mejor, peor y promedio presentan habitualmente diferentes tasas de crecimiento. Deben evaluarse por separado, aportando al algoritmo los parámetros adecuados que permitan reproducir cada caso.
* **Tomar múltiples medidas (repeticiones) para reducir el "ruido"**: una única medida por talla no es estadísticamente significativa. El tiempo de ejecución de un programa se ve afectado por fluctuaciones del sistema (p.ej., otros procesos ejecutándose, el recolector de basura de Python, latencias de la memoria, etc.). Por tanto, se debe ejecutar el algoritmo decenas, cientos o incluso miles de veces para la misma talla, **promediando** finalmente los tiempos resultantes.

### Hacia una implementación genérica

Dado que a lo largo del curso hemos analizado multitud de algoritmos distintos (búsqueda, ordenación, cálculos matemáticos, etc.), no resulta práctico escribir un script de medición específico para cada uno de ellos. Nuestro objetivo es construir una **herramienta de análisis temporal genérica**.

Para lograr esta genericidad, nuestra herramienta tratará al algoritmo a evaluar como un parámetro más. En Python, las **funciones** son **objetos de primera clase**, lo que significa que **podemos pasar una función como argumento a otra función o clase**. De la misma forma, las funciones encargadas de generar las instancias significativas (las funciones generadoras) también se pasarán como parámetros. Así, nuestra herramienta solo tendrá que preocuparse de coordinar el cronómetro, ejecutar la función que reciba (sea cual sea) con los parámetros que devuelvan los generadores, y recolectar los tiempos resultantes.

### Midiendo tiempos en Python

El proceso básico para medir el tiempo que tarda un algoritmo se resume así:

1. Leer el valor de tiempo en el instante previo a ejecutar el algoritmo ($$t_0$$).
2. Ejecutar el algoritmo.
3. Leer el valor de tiempo en el instante inmediatamente posterior ($$t_1$$).
4. La diferencia ($$t_1 - t_0$$)​ es el coste temporal empírico del algoritmo para esa ejecución.

Para llevar a cabo esto de forma extremadamente precisa en Python, emplearemos la función [`perf_counter_ns()`](https://docs.python.org/es/3/library/time.html#time.perf_counter_ns) del módulo nativo [`time`](https://docs.python.org/es/3/library/time.html). Esta función proporciona un temporizador con la mayor resolución disponible en el sistema operativo, devolviendo valores en **nanosegundos**.

```python
import time

t0 = time.perf_counter_ns()
# ... (llamada a la función que se quiere medir) ...
t1 = time.perf_counter_ns()

tiempo_transcurrido = t1 - t0
```

### Repetición y generación de parámetros

Para calcular el promedio de ejecuciones como hemos comentado anteriormente, envolveremos la medición en un bucle de repeticiones. Es importante acumular el tiempo total de todas las repeticiones y, finalmente, dividir por el número de las mismas:

{% code title="" %}
```python
total_ns = 0
for _ in range(repeticiones):
    t0 = time.perf_counter_ns()
    algoritmo(*parametros)
    t1 = time.perf_counter_ns()
    total_ns += (t1 - t0)

tiempo_promedio = total_ns / repeticiones
```
{% endcode %}

{% hint style="info" icon="puzzle-piece" %}
**El algoritmo, los parámetros generados, y el operador de desempaquetado `*`**

En el fragmento anterior, observarás la llamada `algoritmo(*parametros)`. Esta revela la **genericidad de nuestra aproximación**:

* `algoritmo` es una variable que apunta al objeto función que implementa el algoritmo que estamos evaluando (sea el que sea).
* `parametros` es una tupla que contiene los parámetros generados previamente por una función generadora de parámetros para la función `algoritmo` y para un caso particular.

Como no se sabe de antemano cuántos argumentos necesita la función `algoritmo` (la búsqueda lineal necesita dos, pero otros podrían necesitar uno, o tres), y la función generadora de parámetros devuelve siempre una **tupla** con todos los argumentos necesarios, en la invocación de la función `algoritmo` aplicamos el operador asterisco `*` delante de la tupla `parametros`. De este modo, Python "desempaqueta" los elementos de la tupla y los pasa como argumentos individuales e independientes al algoritmo.

Así, si `parametros` es una tupla de dos componentes con, p.ej., `([1, 2, 3], 5)`, la llamada `algoritmo(*parametros)` se convierte internamente en la llamada `algoritmo([1, 2, 3], 5)`.
{% endhint %}

Ahora bien, un factor crítico en el diseño experimental es **cuándo generar los parámetros de la función del algoritmo**:

* **Casos deterministas (mejor y peor)**: La instancia que fuerza el caso mejor o peor para una talla concreta siempre es la misma. Por ejemplo, en la búsqueda lineal, el caso peor siempre es buscar un elemento que no existe. No tiene sentido volver a generar la misma lista una y otra vez dentro del bucle de repeticiones. Lo más eficiente (para no contaminar los tiempos con el coste de generación de memoria) es generar los parámetros **una sola vez**, _fuera_ del bucle de repeticiones.
* **Casos aleatorios (promedio o único)**: El caso promedio o el comportamiento general (para algoritmos sin diferentes instancias) implica ejecutar el algoritmo para una **instancia típica o escogida al azar**. Si generásemos la instancia fuera del bucle, estaríamos repitiendo miles de veces la ejecución _exactamente sobre la misma instancia_, lo que sesgaría el tiempo promedio y dejaría de ser representativo. Por tanto, para estos casos es obligatorio invocar a la función generadora de parámetros **dentro** del bucle de repeticiones, proveyendo una nueva muestra aleatoria en cada iteración.

{% hint style="warning" icon="hourglass-clock" %}
Notar que `perf_counter_ns` solo debe envolver al algoritmo; no debemos "cronometrar" el tiempo (coste) de la generación de parámetros.
{% endhint %}
