---
layout:
  width: wide
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
  metadata:
    visible: true
  tags:
    visible: true
  actions:
    visible: true
---

# Análisis teórico del coste temporal del algoritmo de búsqueda lineal

## Definición del problema

El problema de la búsqueda lineal consiste en, dada una secuencia `A` de elementos de un cierto conjunto (p.ej., los números enteros) y dado un elemento arbitrario `x` de ese conjunto (p.ej., un número entero concreto), devolver la posición de la secuencia que contenga la primera ocurrencia del elemento `x`. Si `x` no se encuentra en `A`, se devuelve una posición inválida (por ejemplo, -1), a fin de señalar que el elemento no se ha encontrado.

La siguiente función, descrita en **pseudo-código**, resuelve este problema:

```
Algoritmo BusquedaLineal(A, x)
    Entrada: Secuencia A de tamaño N, elemento x a buscar
    Salida: Índice de la primera ocurrencia de x en A, o -1 si no se encuentra

    i <- 0
    mientras i < N y A[i] != x hacer
        i <- i + 1
    fin mientras

    si i < N entonces
        devolver i
    sino
        devolver -1
    fin si
Fin Algoritmo
```

***

## Análisis teórico del coste temporal <a href="#analisis-teorico-del-coste-temporal" id="analisis-teorico-del-coste-temporal"></a>

La talla del problema es el tamaño del array, ya que determina el número máximo de iteraciones del bucle (por su condición `i < N`).

Además de esto, se debe estudiar si el algoritmo presenta **instancias significativas** o no. La búsqueda lineal presenta instancias significativas, es decir, **casos mejor y peor**. Estos casos se definen en base a la segunda condición del bucle (`A[i] != x`):

* **Caso mejor**: cuando el elemento `x` se encuentra en la primera posición de `A` (es decir, `A[0] == x`), ya que en ese caso el bucle no ejecutaría ninguna de sus iteraciones previstas.
* **Caso peor**: cuando el elemento `x` no se encuentra en `A`, ya que para verificar ese hecho se debe explorar completamente todo el array.

Con este análisis previo, se puede concluir que el coste temporal de la búsqueda lineal es <mark style="background-color:green;">**constante en el caso mejor**</mark> y <mark style="background-color:red;">**lineal en el caso peor**</mark>.  Tomando como tamaño del problema $$n =$$ `N`, se tiene $$T_{\text{mejor}}(n) \in \Theta(1)$$ y $$T_{\text{peor}}(n) \in \Theta(n)$$.&#x20;

El caso mejor se produce cuando el elemento `x` ocupa la primera posición del array, mientras que el caso peor ocurre cuando `x` no se encuentra en el array o aparece en su última posición. Por tanto, la complejidad temporal del algoritmo en el caso peor es $$\Theta(n)$$.

### Caso promedio

Cuando se realiza un estudio empírico de un algoritmo que presenta instancias diferentes, no solo se miden tiempos para el caso mejor y peor. También se considera el estudio del **caso promedio:** cómo se comporta en general el algoritmo. Esto lo podemos simular mediante la generación de muchos casos aleatorios (cuantos más mejor), promediando los tiempos obtenidos. Para ello asumiremos ciertas simplificaciones como que el elemento a buscar siempre está en el array, y que la probabilidad de encontrar el elemento en cualquier posición es la misma.
