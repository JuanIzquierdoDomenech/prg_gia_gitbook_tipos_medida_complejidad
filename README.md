# Introducción

<figure><img src=".gitbook/assets/logos.png" alt=""><figcaption></figcaption></figure>

En la Práctica 3 trabajaremos de forma práctica los **conceptos del análisis de la complejidad temporal de algoritmos.**&#x20;

Los objetivos son:

* Introducir el análisis de algoritmos en el laboratorio, usando un entorno real de programación: **análisis empírico**.
* Mostrar tablas de tiempos con [**pandas**](https://pandas.pydata.org/).
* Representar gráficamente con [**matplotlib**](https://matplotlib.org/) la evolución de las medidas del tiempo de ejecución de algoritmos para contrastar con los resultados teóricos.

{% hint style="info" %}
Si no usas el escritorio DSIC-LINUX, y estás usando tu propia instalación local de Llinux, deberás instalar **matplotlib y pandas**.

Si estás usando el entorno físico global de Python en sistemas basados en Debian/Ubuntu:

```bash
sudo apt update
sudo apt install python3-matplotlib python3-pandas
```

Si por contra, estás usando un entorno virtual de Python (recomendado), entonces instala las librerías con pip:

```bash
pip install matplotlib pandas
```
{% endhint %}
