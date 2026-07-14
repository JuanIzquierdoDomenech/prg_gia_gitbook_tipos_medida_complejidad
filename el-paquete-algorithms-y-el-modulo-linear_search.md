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

Añade a tu módulo `linear_search.py` las tres funciones generadoras necesarias, las cuales devolverán una tupla `(lista_generada, elemento_a_buscar)`:

1. `linear_search_generate_best(n)`: Para forzar el **caso mejor**, genera una lista de tamaño `n` (p.ej., con los números del `0` al `n-1`) y devuelve como segundo parámetro el **primer elemento** de la lista.
2. `linear_search_generate_worst(n)`: Para forzar el **caso peor**, genera una lista de tamaño `n` y devuelve un elemento que **no esté en la lista** (p.ej., el `-1`).
3. `linear_search_generate_average(n)`: Para evaluar el **caso promedio**, genera la misma lista, pero devuelve un elemento escogido al **azar** (usando la función [`randint()`](https://docs.python.org/es/3/library/random.html#random.randint) del módulo `random` (que deberás importar) de Python) de entre los que contiene la lista.

{% hint style="warning" icon="square-code" %}
Notar que **las tres funciones devuelven una tupla con dos componentes**: una lista de enteros y un valor entero.

**El orden de las componentes importa**: es justamente el orden de los parámetros de entrada de la función `linear_search()`.

Recuerda que esas dos componentes serán los parámetros que se usarán, a posteriori, en un contexto de medición de tiempos, para invocar a `linear_search`.
{% endhint %}

***

## Actividad 4: Exportación del módulo

Para que el algoritmo y sus funciones generadoras de parámetros sean accesibles desde fuera del paquete `algorithms` de forma limpia (p.ej., `from algorithms import linear_search`), debemos exponerlos en el fichero `__init__.py` del paquete.

Abre el fichero `algorithms/__init__.py` que creaste en la primera actividad e importa las funciones desde el módulo `linear_search`. Finalmente, usa la variable `__all__` para definir explícitamente la API pública del paquete:

{% code title="" %}
```python
from .linear_search import (
    linear_search,
    linear_search_generate_best,
    linear_search_generate_worst,
    linear_search_generate_average,
)

__all__ = [
    'linear_search',
    'linear_search_generate_best',
    'linear_search_generate_worst',
    'linear_search_generate_average',
]
```
{% endcode %}

***

Actividad 5: Depuración

Añade el siguiente código **(SIN MODIFICAR)** al final del fichero `linear_search.py` para probar tu implementación.

{% hint style="danger" icon="skull-crossbones" %}
**NO MODIFICAR ESTE BLOQUE DE CÓDIGO PRINCIPAL**

Su salida se podrá usar el día del examen para evaluaros.
{% endhint %}

<details>

<summary>Código de test</summary>

{% code title="" %}
```python
if __name__ == "__main__":
    print("Probando linear_search...")
    
    talla = 10
    
    # Prueba caso mejor
    params_best = linear_search_generate_best(talla)
    idx_best = linear_search(*params_best) # equivalente a linear_search(params_best[0],params_best[1])
    print(f"Caso mejor (talla {talla})  -> params: {params_best}, idx: {idx_best}")

    # Prueba caso peor
    params_worst = linear_search_generate_worst(talla)
    idx_worst = linear_search(*params_worst)
    print(f"Caso peor (talla {talla})   -> params: {params_worst}, idx: {idx_worst}")

    # Prueba caso promedio
    params_avg = linear_search_generate_average(talla)
    idx_avg = linear_search(*params_avg)
    print(f"Caso promedio (talla {talla})-> params: {params_avg}, idx: {idx_avg}")
```
{% endcode %}

</details>

Finalmente, ubícate en la raíz de tu directorio p3 y ejecuta el módulo:

{% code title="" %}
```bash
python -m algorithms.linear_search
```
{% endcode %}

Deberías obtener una salida equivalente a esta:

<details>

<summary>Salida esperada</summary>

{% code title="" %}
```bash
Probando linear_search...
Caso mejor (talla 10)  -> params: ([0, 1, 2, 3, 4, 5, 6, 7, 8, 9], 0), idx: 0
Caso peor (talla 10)   -> params: ([0, 1, 2, 3, 4, 5, 6, 7, 8, 9], -1), idx: -1
Caso promedio (talla 10)-> params: ([0, 1, 2, 3, 4, 5, 6, 7, 8, 9], 4), idx: 4
```
{% endcode %}

</details>

{% hint style="info" %}
_(Nota: En el caso promedio, el número a buscar y su índice variarán aleatoriamente en cada ejecución; en cualquier caso deben coincidir el segundo parámetro y el valor de idx)._
{% endhint %}
