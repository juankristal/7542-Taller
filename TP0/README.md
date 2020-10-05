# Paso 0

a) 

![Test](imgs/paso0a.png?raw=true)

b) Valgrind sirve para detectar perdidas o errores de memoria en un programa. Se suelen usar flags como --leak-check=full (para ver en detalle todos los leaks de memoria), --show-leak-kinds=all (para que todos los tipos de leaks posibles sean reportados), --verbose (para detecetar comportamientos inusuales del programa) y --track-origins=yes (para ver la ubicacion de los errores)

c) sizeof() representa la cantidad de memoria (en bytes) alocada para una estructura o tipo de dato determinado recibido por parametro. Por lo tanto, el valor de salida de sizeof(int) sera 4 y el de sizeof(char) sera 1.

d) No necesariamente. Puede serlo pero no siempre es asi. Un struct que contenga un char y un int tendra un sizeof() 8 en lugar de 5 que seria la suma de ambos. Esto se debe a que la memoria se asigna en base al atributo del struct que tenga mayor peso y se le asigna el mismo espacio a cada uno de dichos atributos.

e) Los archivos estandar STDIN, STDOUT y STDERR corresponden a canales de comunicacion entre el programa y el ambiente de ejecucion. STDIN es el input o entrada, STDOUT el output o salida y STDERR la conexion de errores. Utilizando los caracteres > y < podemos redireccionar el output o el input a otro archivo. Usando > redirigimos el primer argumento al segundo y vice versa con <. Utilizando un pipe (|) podemos redireccionar el output de una aplicacion al input de otra.

# Paso 1

![Test](imgs/paso1.png?raw=true)

a) En la seccion de verificando el codigo se pueden observar los errores de estilo.
* Falta un espacio entre el while y los parentesis que encierran su condicion. 
* Sobra un espacio luego del parentesis de la condicion del if.
* La llave que cierra la condicion anterior deberia estar en la misma linea que el else y no en la anterior.
* De igual manera a lo anterior, si el else tiene solo una llave del lado derecho tiene que tenerla tambien del lado izquierdo cerrando.
* Falta un espacio entre el parentesis y el if.
* Hay un espacio de mas antes del punto y coma.
* Las lineas deberian tener menos de 80 caracteres.
* En lugar de usar strcpy deberiamos usar snprintf pues al no saber exactamente el size del buffer strcpy este puede escribir en un lugar que no le corresponde. snprintf sin embargo siempre devuelve escribe el mismo tamanio y no le ocurre.
* Las llaves cerradas deben estar en la misma linea que el else (aplica al siguiente error tambien)

b) En la seccion de desempaquetado y compilado de codigo se ven los errores de generacion del ejecutable.

* El primer error se trata de un error de compilacion. El typo de dato wordscounter_t no esta definido.

* Los demas errores son errores de linker pues son declaraciones implicitas de funciones.

c) El sistema no reporto warnings. A pesar que al correrlos localmente los errores de linker son considerados warnings, el sistema los marca como errores porque espera que nuestros programas no tengan warnings. Esto se observa al final del log de error "all warnings being treated as errors".


# Paso 2

a) Fueron arreglados los errores de estilo marcados anteriormente y se incluyo el archivo header del wordscounter. De esa forma nuestro archivo principal tiene acceso a las estructuras y funciones definidas en el mismo.

b)

![Test](imgs/paso2b.png?raw=true)

c)

![Test](imgs/paso2c.png?raw=true)

Los errores en este caso son todos de linkeo pues son definiciones implicitas de funciones. Dichas funciones o tipos estan definidos en bibliotecas como stddef, stdlib y stdio.

# Paso 3

a) Las correcciones realizadas fueron incluir las librerias que contienen las funciones utilizadas en la entrega anterior como malloc. Dichos headers son stdio, stdlib y stddef.

b)

![Test](imgs/paso3.png?raw=true)

Aca hubo nuevamente un error de linkeo pues la funcion wordscounter_destroy esta definida en el header pero no en el codigo fuente.

# Paso 4

a) Se definio la funcion de destruccion que faltaba pero no hace absolutamente nada. De esta forma la definicion de la funcion existe y por lo tanto compila y linkea exitosamente.

b)

![Test](imgs/paso4b.png?raw=true)

Los errores reportados son memoria perdida definitivamente y memoria aun alcanzable. La memoria perdida definitivamente se pierde al ejecutar wordscounter\_next\_state el cual asigna 7 bytes de memoria y en cada letra que avanza esos 7 bytes no son liberados y por lo tanto se pierden definitivamente. La memoria aun alcanzable son 472 bytes que se deben a que el archivo no fue cerrado al terminar la ejecucion del programa.

c)

![Test](imgs/paso4c.png?raw=true)

 En este caso ocurre un buffer overflow. Esto significa que estamos tratando de escribir algo en un espacio que no es suficiente. Esto se debe al uso de memcpy y el espacio que se le asigna al nombre del archivo. Solo se le asignan 30 bytes y el nombre del archivo lo supera, por lo que memcpy escribe en un lugar que no le corresponde.

d) No se solucionaria utilizando strncpy ya que esto unicamente copiaria los primeros 30 caracteres del nombre del archivo y por lo tanto no podria abrirlo de todas formas. En este caso devolveria el codigo de ERROR.

e) Un buffer overflow como fue explicado anteriormente es escribir en memoria que no corresponde al buffer asignado. Un segmentation fault ocurre al intentar acceder a memoria que no nos pertenece. Este ultimo podria ocurrrir durante un buffer overflow.


# Parte 5

a) No se pide mas memoria dinamicamente para los caracteres delimitadores, en su lugar de usa memoria estatica entonces no pierde memoria. Se cierra efectivamente el archivo en caso de que el input no venga de stdin. No se copia en un pedazo de memoria el nombre del archivo, se usa directamente desde argv.

b)

![Test](imgs/paso5b.png?raw=true)

Las pruebas fallan para archivo invalido porque lanza un codigo de error que no es el esperado. Espera que devuelva un codigo de error 1 y en su lugar esta enviando -1. Para una sola palabra esta esperando una salida por pantalla de 1 cuando en su lugar manda 0.

c)

![Test](imgs/paso5c.png?raw=true)

El ultimo caracter es la letra d.

d)

![Test](imgs/paso5d.png?raw=true)

Utilizamos el comando list para ver la funcion en particular y encontrar la parte del codigo que queremos debugear. Usando break 45 le indicamos a gdb que queremos detenernos en la ejecucion de dicha linea. Al correr con run la ejecucion vemos que no se detiene en la linea 45 por lo que entendemos que no esta entrando a dicha linea y por eso falla el programa. Al no avanzar el contador de palabras, devuelve 0 en lugar de 1.

# Paso 6

a) En lugar de tener una constante se define DELIM_WORDS al inicio del codigo para evitar redefinirlo en cada instancia de ejecucion. Se redefine correctamente el codigo de error esperado. Se aumenta en uno la palabra al llegar al EOF.

b)

![Test](imgs/submita.png?raw=true)

![Test](imgs/submitb.png?raw=true)

c)

![Test](imgs/paso6c.png?raw=true)
