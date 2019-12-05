# Prediciendo-ME

## Objetivo: 	
El proyecto está orientado en el tipeo veloz de un usuario, con su dialecto, su redacción e inclusive su propia forma de escribir.
Para ello, se debe ingresar en una base de datos el "corpus" del usuario (es decir, una recompilacion de texto plano). En dicha bd se tendrá guardadas en 3-gramas las frases u oraciones descriptos por el mismo. Más adelante se explica cómo.

El programa tiene como función dar en tiempo pequeño el tipeo requerido por el usuario, a base de un corpus procesado en una base de datos de 3-gramas.

# requerimientos:
- pressagio
- jupyter notebook
- python 3.7
- sqlite3

Cuyo objetivo descrito anteriormente es hacer un sistema de texto predictivo que sea personalizable.

Al empezar el proyecto tardé en hallar modelos que respalden tareas tales como: clasificar las acciones observadas de acuerdo con sus tipos, predecir hacia adelante y completar las acciones (predecir y autocompletar).

Un enfoque particular radica en comprender cómo aprender gradualmente las acciones de manipulación de solo unos pocos ejemplos y cómo permitir que los modelos clasifiquen las acciones que aún están en progreso. 
Comparamos dos amplias clases de modelos, lo cual son tecnologias bases para predecir:
- modelos de secuencia generativa (HMM en particular este modelo desarrolla presage, consecuentemente pressagio) 
- y redes neuronales recurrentes de entrenadas de forma discriminatoria (redes recurrentes LSTM):

## HMM 

Los modelos de Markov (HMM) son mucho más simples que las redes neuronales recurrentes (RNN) y se basan en suposiciones fuertes que pueden no ser siempre ciertas. 
- Integración de datos: cuando se debe integrar una nueva secuencia en un modelo dado
construimos una ruta única entre el estado inicial y final del modelo donde cada símbolo de la secuencia corresponde a un estado nuevo en la nueva ruta. Cada uno de estos estados emite su símbolo respectivo en la secuencia subyacente y simplemente pasa al siguiente estado con probabilidad 1, produciendo una ruta secundaria en el modelo que reproduce exactamente la secuencia correspondiente.
- Evaluación del modelo: Evaluamos los modelos que resultan en el proceso de merging usando una mezcla entre un modelo estructural previo P (M) y la probabilidad del modelo dependiente de datos P (X | M)
![Texto alternativo](/imagen/2.png)

La siguiente figura muestra una secuencia de modelos obtenidos mediante mergings de muestras de un lenguaje ejemplar:
(ab)+ y posteriormente mergings los estados resaltados.
![Texto alternativo](/imagen/3.png)

## LSTM

La red recurrente discriminativa consiste en una capa de memoria a largo plazo (LSTM) con 128 bloques de memoria seguida de una capa lineal completamente conectada con normalización softmax y una unidad de salida por clase objetivo. La normalización softmax permite que la salida de la red se interprete como probabilidades de clase. A diferencia del enfoque de HMM donde entrenamos un HMM separado para cada clase, esta red aprende una representación conjunta entre las secuencias de las 4 clases.

![Texto alternativo](/imagen/4.png)


## Introduccion a la herramienta usada:

Pressagio es una libreria de python desarrollada por Peter Bouda (https://github.com/Poio-NLP/pressagio).
Pressagio te predice la siguiente palabra,  como asi la palabra a autocompletar, siguiendo un modelo de n-gram. Y siendo de la familia HMM, pressagio es fácil de usar.
Esta libreria trabaja con presage, es decir, presage es el motor de pressagio. Además de estar en continuo desarrollo. 


Unos de los proyectos que está involucrado pressagio, es en el proyecto POIO. (https://www.poio.eu)  
El proyecto Poio desarrolla tecnologías lingüísticas para apoyar la comunicación en idiomas menos utilizados y con pocos recursos y con dispositivos electrónicos. Dentro del proyecto Poio, desarrollan servicios de entrada de texto con predicción de texto (acá entra pressagio). Además desarrollan transliteración para dispositivos móviles y usuarios de escritorio para permitir la conversación en cientos de idiomas entre individuos y comunidades en línea.

# Desarrollo:

El objetivo, dicho anteriormente está orientado al desarrollo de una aplicación pesonal. Para la misma se guardo una recompilación de 9 archivos de texto plano, realizadas para la comunicacion debida de ciertos días. Llevando a dichos textos a un solo archivo. 

![Texto alternativo](/imagen/5.png)

### Implementacion:
Se separó en dos partes el programa:
- en uno obtener la base de datos con un corpus desarrollado por el usuario.
- En el otro se agarra el corpus de la wikipedia y se lo pasa a una bd,  a partir de ella predecir. (dicha bd, por ser grande se omitio en el proyecto).

Los textos planos, tenían caracteres iso-8859-1. Lo cual a pasar n-gramas tuvo grandes consecuencias. 
Para ello se suavizo a utf8 con el siguiente comando:

```sh iconv -f ISO-8859-1 -t UTF-8//TRANSLIT raw.es/spanishText_10000_15000.txt -o file-utf8.txt```

Teniendo los textos en formato utf8. Lo próximo a realizar fue: guardar en una base de datos los n-gramas. (se trabajo con n=3, osea con una ventana igual a 3). El siguiente comando, usando el archivo : "text2ngram.py",  generó una base de datos con 3 tablas, una para 1-gramas, otra para 2-gramas, y finalmente la de 3-gramas.

```sh python text2ngram.py -n 1 -o db.sqlite wiki.txt```
```sh python text2ngram.py -n 2 -o db.sqlite wiki.txt```
```sh python text2ngram.py -n 3 -o db.sqlite wiki.txt```

Finalmente se uso un scrip, propuesto en cada notebook de cada directorio propiamente llamados.

En la siguiente imagen se obtiene una captura de la potente herramienta. Para este ejemplo, se uso los textos planos desarrollados y no de la wiki. El cual toma entrada con el comando input.
Cabe aclarar que el programa te toma espacios para diferenciar entre palabra completa y palabra a predecir. En el caso que no haya un espacio entonces se autocompleta palabra.

![Texto alternativo](/imagen/7.png)

Siguiendo a un etorno más amigable, se trabajo con widgets para mayor comodidad del usuario.

![Texto alternativo](/imagen/8.png)


## Problematicas:

-Una de las problematicas está en que el texto predictivo se basa en un archivo con errores de tipeo, y/o acentos. 
Una posible solución futura será tratar el texto acumulado por el autor pasandolo por un script que haga correcciones en base a un diccionario de la Real Academia Española (RAE), obteniendo de esa forma un corpus más elegante semánticamente. 

# Tarea a futuro:
En el desarrollo del trabajo se intentó hacer un editor de texto y correr pressagio en el mismo. Este poceso queda como meta futura.

# Bibliografia
https://pub.uni-bielefeld.de/download/2903474/2907910/MOD2016.pdf
