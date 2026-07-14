# El paquete algorithms y el módulo linear\_search

## Paquete algorithms

En esta práctica vamos a construir un paquete llamado `algorithms` que ofrecerá un conjunto de módulos que implementan algoritmos de cualquier tipo. Lo inicializaremos con el módulo `linear_search` que implementará el algoritmo de búsqueda lineal.

***

## Actividad 1: Inicalización del directorio de trabajo y el paquete `algorithms`

Abre una terminal Bash de Linux y sitúate en el directorio donde quieras mantener organizadas las prácticas de la asignatura. A continuación, crea el directorio de la práctica actual y sitúate en él:

```bash
mkdir p3
cd p3
```

{% hint style="warning" icon="eyes" %}
Tened en cuenta que `p3` será nuestro directorio de trabajo de partida durante toda la práctica. Todos los paquetes y scripts de prueba se colocarán directamente aquí o colgarán de este directorio.
{% endhint %}

Ahora vamos a inicializar el paquete `algorithms`, donde guardaremos las implementaciones de los algoritmos que queremos analizar. Crea el subdirectorio del paquete, e incluye en él un fichero `__init__.py` vacío con el comando `touch` para que Python lo reconozca como un paquete importable:

```bash
mkdir algorithms
touch algorithms/__init__.py
```

Dentro de este paquete, vamos a crear un módulo que contendrá la implementación de la búsqueda lineal y las funciones que nos ayudarán a evaluar su rendimiento.

***

## Actividad 2: Implementar la búsqueda lineal

Crea un fichero llamado `linear_search.py` dentro de la carpeta `algorithms`. En él, implementa la función de búsqueda lineal basándote en el pseudocódigo que hemos estudiado en la sección anterior.

**Perfil**:

`linear_search(seq, elem)`

* donde:
  * `seq` es la secuencia (p.e. lista) donde buscar,
  * `elem` el elemento a buscar.

**Valor de retorno:**

* **la posición/índice** de la primera ocurrencia de `elem` en `seq`, o `-1` si no lo encuentra.

### Funciones generadoras de parámetros del algoritmo

Como sabemos, el análisis empírico requiere ejecutar el algoritmo para diversas tallas (i.e., tamaños del problema). Además, para la búsqueda lineal existen **instancias significativas**: el caso mejor, el caso promedio y el caso peor. Para medir el tiempo que tarda el algoritmo en cada uno de estos casos, necesitamos proporcionarle los datos de entrada adecuados.

Es por ello que, junto al algoritmo, implementaremos **funciones generadoras de los parámetros que necesita la función del algoritmo para ser invocado**, para cada uno de los tres casos. Estas funciones recibirán un único entero `n` (la talla del problema) y devolverán una **tupla** con los parámetros exactos que necesita la función de nuestro algoritmo. En el caso de `linear_search`: una lista de tamaño `n` y el elemento a buscar.

{% hint style="info" icon="fingerprint" %}
Para algoritmos que **no** presenten instancias diferentes (es decir, donde cualquier instancia de tamaño `n` tarda lo mismo), solo será necesario definir una única función generadora del caso único.
{% endhint %}

***

## Actividad 3: Funciones generadoras de parámetros de la búsqueda lineal

Añade a tu módulo `linear_search.py` las tres funciones generadoras necesarias:

1. `linear_search_generate_best(n)`: Para forzar el **caso mejor**, genera una lista de tamaño `n` (p.ej., con los números del `0` al `n-1`) y devuelve como segundo parámetro el **primer elemento** de la lista.
2. `linear_search_generate_worst(n)`: Para forzar el **caso peor**, genera una lista de tamaño `n` y devuelve un elemento que **no esté en la lista** (p.ej., el `-1`).
3. `linear_search_generate_average(n)`: Para evaluar el **caso promedio**, genera la misma lista, pero devuelve un elemento escogido al **azar** (usando la función [`randint()`](https://docs.python.org/es/3/library/random.html#random.randint) del módulo `random` de Python) de entre los que contiene la lista.

{% hint style="warning" icon="square-code" %}
Notar que **las tres funciones devuelven una tupla con dos componentes**: una lista de enteros y un valor entero.

**El orden de las componentes importa**: es justamente el orden de los parámetros de entrada de la función `linear_search()`.

Recuerda que esas dos componentes serán los parámetros que se usarán, a posteriori, en un contexto de medición de tiempos, para invocar a `linear_search`.
{% endhint %}

***

## Actividad 4: Exportación del módulo
