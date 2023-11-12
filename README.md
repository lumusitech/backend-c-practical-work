# University practical work - C lang

## Introducción

En el siguiente informe, se describen los ejercicios propuestos
correspondientes al trabajo práctico 1 de Sistemas Operativos y Redes I.
El trabajo práctico se dividió en dos partes, determinado esto por el
lenguaje usado y los objetivos a alcanzar.
En la primera parte, debíamos desarrollar una serie de scripts en Bash
para luego integrarlos al super menú provisto. Esta primer tarea, nos sirvió para
familiarizarnos con la terminal de GNU Linux, como también en el uso de
repositorios del tipo github.
La segunda parte consistió en programar usando el lenguaje C. Se
implementó el uso de threads de manera tal que conozcamos su funcionamiento
básico y entendamos las ventajas que trae.
El trabajo fue tan desafiante, debido a la inexperiencia en el manejo de
las mayoría de las herramientas que debimos utilizar para su realización; como
educativo ya que, dadas las circunstancias, era necesario investigar por nuestra
cuenta y eso nos dió la oportunidad de expandir nuestros conocimientos.
Posiblemente, en un futuro cercano, surgirán nuevos lenguajes de programación
o procedimientos que deberemos asimilar para realizar nuestra labor. Por esto fue
muy certero dicho desafío ya que entendemos que será algo que deberemos
enfrentar repetidas veces en nuestras vidas profesionales.
A continuación se detalla la lógica detrás de cada código, además de las
dificultades encontradas en el proceso y su resolución.

## Primer parte

### Ejercicio 1

El ejercicio 1 demandaba establecer las bases para el posterior desarrollo
de funcionalidades y la creación de un acceso directo del Super menú con el
objeto de facilitar el acceso al mismo.

### Ejercicio 2

El segundo ejercicio consistía en crear scripts en bash para añadirlos en el
Super Menú.

- #### Punto a

    Este script debía recibir el nombre de un programa y devolver si ese
    programa está instalado o no. Para esto, primero pedimos al usuario que ingrese
    por teclado el nombre del programa, luego se pregunta si el programa se
    encuentra dentro de la lista de programas instalados, de estarlo imprime por
    pantalla ”programa instalado”, caso contrario imprime por pantalla “programa no
    instalado”.

- #### Punto b

    Este punto consistía en que el usuario debe ingresar el path de un
    directorio, la extensión de un archivo y su nombre de manera que el programa
    buscará en ese directorio todos los archivos con ese nombre y esa extensión. Para
    ello, primero nos movemos al directorio ingresado por el usuario y una vez allí
    se lista todo el contenido en ese directorio y se filtra aquellos archivos con el
    nombre y la extensión ingresados, de no encontrar nada imprime por consola que
    no pudo encontrarse ningún archivo con la extensión especificada.

- #### Punto c

    El punto c requería programar un script que pidiera al usuario un string y un archivo.
    Con esto, debería devolvernos el número de línea del string dentro del archivo.
    Para llevar a cabo esto, primero pedimos al usuario que ingrese el string que desea
    buscar y la ruta absoluta del archivo donde desea buscar, luego se lee el archivo, se filtra la
    cadena ingresada, se obtiene el número de línea y el resultado lo envía a un archivo llamado
    salida.out. De haberse ejecutado correctamente, muestra por pantalla el contenido de
    salida.out. En caso de no ejecutarse correctamente, imprime por pantalla un mensaje de error.

- #### Punto d

    El punto d requería capturar los cambios de estado de un proceso
    determinado utilizando el monitor de procesos top. Además, debíamos filtrar y
    mostrar mediante el comando grep solo la línea correspondiente al proceso
    monitoreado.
    Para cumplir con tal fin, se creó un script de tipo bash que cambiara de
    estados a partir de un bucle de escritura a un archivo de tipo txt que lo dejaba en
    estado de ejecución o R y del comando sleep, para pasarlo a estado de durmiendo
    o S. Para ejecutar en segundo plano y filtrar dichos estados se creó otro script
    que muestra en pantalla el estado actual del proceso, más otros datos como
    usuario, id del proceso, prioridad absoluta y relativa, uso de la memoria y cpu,
    etc.

## Segunda parte

### Ejercicio 1

#### Primer canto

El primer ejercicio consistía en utilizar hilos para que se imprima el canto
“MA” “RA” “DOOO…”, utilizando semáforos para sincronizarlo correctamente.
La resolución de este ejercicio fue simple y no tuvimos ningún inconveniente
particular, aunque sí tuvimos que investigar por nuestra cuenta para encontrar la
sintaxis correcta de las funciones que queríamos utilizar. Utilizamos 3 threads
(t1, t2, t3) y 3 semáforos (s1=1, s2=0 y s3=0) de manera que t1 consume s1,
imprime “MA” y levanta s2, t2 consume s2, imprime “RA” y levanta s3, t3
consume s3, imprime “DOOO…” y levanta s1 para así poder repetir el canto si
así se lo desea.

#### Segundo canto

El segundo ejercicio consistía en utilizar 5 hilos para imprimir el canto
“ole ole ole, ole ole ole ola, ole ole ole, cada día te quiero más, oooh Argentina,
es un sentimiento, no puedo parar...”, donde cada frase del canto es una función
diferente. Las principales diferencias con el ejercicio anterior son la cantidad de
threads a utilizar y último y más importante el hecho de que el canto no se
produce en un orden lineal si no que uno de los versos se repite en medio del
canto.
En primer lugar tratamos de resolverlo utilizando un semáforo por cada
thread pero no podíamos coordinar la repetición del primer thread correctamente
por lo que recurrimos a un sexto semáforo de manera que el código termina
funcionando de la siguiente manera. Existen 6 semáforos (s1, s2, s3, s4, s5 y s6),
los primeros dos se inicializan en 1 y el resto en 0, t1 consume s1, imprime lo
que debe y finalmente levanta s4, t2 consume s2 y s4 estrictamente en ese orden
de manera que no pueda consumir s4 (el cual es utilizado más adelante) a menos
que s2 no sea 0, imprime lo que debe y levanta s1 (para que se ejecute
nuevamente t1) y s3, se ejecuta nuevamente t1, t3 consume s3 (levantado en t2) y
s4 (levantado al repetirse t1) de esta manera t3 solo puede ejecutarse luego de t2
y t1 en ese orden y luego levanta s5, t4 consume s5, imprime lo que corresponde
y levanta s6, finalmente t5 consume s6 imprime lo que corresponde y luego
levanta s1 y s2 para así poder repetir el canto de ser deseado.

### Ejercicio 2

El propósito de este ejercicio fue convertir el simulador del mundial que
se ejecutaba de manera secuencial, en uno que lo hiciera de forma paralela
implementando threads. Al principio optamos por una implementación rústica,
donde declaramos y creamos los hilos de forma “manual” pero, a pesar de que
funcionaba, no nos gustaba la implementación ya que el código se volvería muy
difícil de depurar y mantener. De esta manera comenzamos pruebas para incluir
la inicialización y creación de los threads dentro de ciclos. Realizamos un primer
intento de una manera intuitiva, declarando N threads para luego crearlos en un
ciclo for pero, esta idea no llegó ni a compilarse ya que no encontramos la
manera de referenciar dinámicamente los threads declarados. Luego de muchos
intentos fallidos por la “violación de segmento” y una larga investigación
encontramos la manera de alojar en memoria N thread utilizando la función
“calloc”, el siguiente problema fue como darle a las funciones jugar_grupo(),
jugar_partido_octavos(), etc. un parámetro para que “sepan” que partido es el
que se debe jugar. La solución fue crear un puntero hacia el indice, referenciarlo
en la creación del thread y luego realizar un casteo dentro de la función que
ejecuta el thread. De esta manera se logró realizar exitosamente el simulador
paralelo.
En cuanto al tiempo de ejecución, el simulador paralelo le lleva más
tiempo completar la ejecución del programa. Esto creemos que se da ya que la
creación de un thread lleva tiempo, menos que la creación de un nuevo proceso
pero, aún así el tiempo que se gana en multiprocesamiento se lo pierde en la
creación de los hilos dado que las funciones que deben ejecutar no toman casi
nada de tiempo.

## Conclusión

Tras haber vivenciado el desarrollo de funcionalidades mediante scripts
de BASH, comprendimos la potencia de una terminal de GNU Linux y su uso
básico que nos permite seguir investigando y aprovechando sus virtudes en
futuros retos.
En cuanto a la implementación de Threads en el lenguaje C, entendimos
la aplicación de temas teóricos con más alcance. También, nos llevó a indagar en
su sintaxis y capacidades. Aunque todavía hay mucho por descubrir, este trabajo
fue un gran disparador para acercarnos a este gran, y todavía muy vigente,
lenguaje de programación.
