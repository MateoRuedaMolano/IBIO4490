LABORATORIO 1 Y 2
Computer Vision
Universidad de los Andes
Mateo Santiago Rueda Molano - 201517252

1) El comando grep se utiliza para buscar un patr�n de texto (especificado en la l�nea de comando) en un archivo, m�ltiples archivos
o en una entrada. Se busca l�nea por l�nea el patr�n y grep entrega la l�nea de texto completa donde se encuentra este patr�n.

Sintaxis.
grep [OPCIONES] PATR�N [ARCHIVO]

Esta informaci�n se bas� de https://www.computerhope.com/unix/ugrep.htm

2) #!/bin/python corresponde a un shebang, se conoce como shebang al conjunto de caracteres #! cuando est�n al principio de un archivo 
tipo texto. Este indica que el archivo corresponde a un script y qu� int�rprete ha de usarse para ejecutar el mismo (en este caso el
int�rprete es Python localizado en /bin).  Los sistemas operativos Linux (y otros sistemas Unix) soportan de forma nativa 
esta caracter�stica.

La informaci�n se bas� del siguiente link https://bash.cyberciti.biz/guide/Shebang.

3)  
COMANDOS:
El comando utilizado para descargar la base de datos fue:
wget http://www.eecs.berkeley.edu/Research/Projects/CS/vision/grouping/BSR/BSR_bsds500.tgz
Al descomprimir se utiliz�:
tar -xvf BSR_bsds500.tgz

4) 
COMANDOS:
vision@bcv002:~/ms.rueda10/BSR/BSDS500/data/images$ du BSR
vision@bcv002:~/ms.rueda10/BSR/BSDS500/data/images$ du -h BSR 

Se utiliz� el comando du en la carpeta descomprimida y se obtuvo que esta ocupa un espacio de 73Megabytes o 74128bits. 
El -h se utiliz� para saber el espacio que ocupaba en derivados del Byte (KiloByte, MegaByte...):

Como todas las im�genes son jpg, se determin� el n�mero de im�genes mediante la l�nea de c�digo

vision@bcv002:~/ms.rueda10/BSR/BSDS500/data/images$ find . -name "*.jpg" -exec identify {} \; | wc -l


Cualquier archivo con el nombre jpg, mediante un contador se almacena. El resultado fue de 500 im�genes.
Vale la pena aclarar que esta l�nea de c�digo se corri� en la carpeta images dentro del dataset.
 
5) 

FORMATO DE LOS ARCHIVOS DE LA CARPETA IMAGES:

vision@bcv002:~/ms.rueda10/BSR/BSDS500/data/images$ find ./* -type f | awk -F. '{print $NF}' | sort -u

Resultado:
db
jpg
 
Mediante el comando anterior, se encontr� la �ltima l�nea despu�s de un punto, que corresponde al formato de los archivos. 
Despu�s de obtener los formatos de cada imagen, se orden� con sort -u para obtener los tipos de formatos de las im�genes. 
Las im�genes as�, son jpg, aunque existe un archivo en el folder 
de formato db. 

El separador de punto en awk y el print de la �ltima l�nea se bas� en los ejemplos de los siguientes links.
https://www.computerhope.com/unix/usort.htm y https://stackoverflow.com/questions/4304917/how-to-print-last-two-columns-using awk.


RESOLUCI�N:

vision@bcv002:~/ms.rueda10/BSR/BSDS500/data/images$ identify $(find . -name "*.jpg") | awk '{print $3}' | sort -u

Resultado: 
321x481
481x321

Este comando se utiliz� para hallar las resoluciones de las im�genes. Mediante $(find . -name "*.jpg") se encontraban los nombres de las im�genes
y haciendo identify se obtienen elementos como el formato, la resoluci�n, los bits, si es RGB, etc. A partir de estos datos se busc� mediante awk 
la columna 3 correspondiente a la resoluci�n. Con sort -u se unificaron las resoluciones iguales obteniendo tan solo las resoluciones 481*321 
y 321*481.

FORMATO TAN SOLO DE LAS IM�GENES:

vision@bcv002:~/ms.rueda10/BSR/BSDS500/data/images$ identify $(find . -name "*.jpg") | awk '{print $2}' | sort -u

Resultado:
JPEG

Este comando es similar al anterior, solo que se obtiene en vez de las resoluciones, el formato de las im�genes.

La informaci�n de identify en imagemagick se obtuvo de:
https://superuser.com/questions/275502/how-to-get-information-about-an-image-picture-from-the-linux-command-line.

6) 


COMANDO ORIENTACI�N LANDSCAPE:
vision@bcv002:~/ms.rueda10/BSR/BSDS500/data/images$ identify $(find . -name "*.jpg") | awk '{print $3}' | grep -c "481x321"

Resultado:
348

COMANDO ORIENTACI�N PORTRAIT:
vision@bcv002:~/ms.rueda10/BSR/BSDS500/data/images$ identify $(find . -name "*.jpg") | awk '{print $3}' | grep -c "321x481"

Resultado:
152


El comando se bas� en el del numeral anterior en el que obten�amos la resoluci�n de cada imagen, sin embargo, se a�adi� una l�nea de grep -c "resoluci�n",
la cual me buscaba en cada una de las resoluciones de las im�genes alguna de las dos encontradas en el numeral anterior. Mediante un contador, se obtiene
finalmente el resultado de todas las im�genes con las resoluciones tipo landscape (en este caso 481x321) y portrait (321x481).



7) Para el punto 7, primero se comenz� creando los directorios correspondientes de la nueva carpeta

COMANDOS:
mkdir newFolder 
cd newFolder
mkdir train
mkdir test
mkdir val

En la carpeta newFolder se crearon estos sub directorios para guardar las im�genes modificadas en resoluci�n.

Posteriormente, se cambi� el size de cada imagen de cada una de las carpetas (train, val y test) a 256x256 y se pas� a las carpetas nuevas creadas. Se usaron
3 comandos para cada una de las carpetas, los cuales fueron:
TRAIN:
vision@bcv002:~/ms.rueda10/BSR/BSDS500/data/images$ for imagen in $(find train -type f -name "*.jpg"); do convert $imagen -resize 256x256\! newFolder/$imagen; done

VAL:
vision@bcv002:~/ms.rueda10/BSR/BSDS500/data/images$ for imagen in $(find val -type f -name "*.jpg"); do convert $imagen -resize 256x256\! newFolder/$imagen; done

TEST:
vision@bcv002:~/ms.rueda10/BSR/BSDS500/data/images$ for imagen in $(find test -type f -name "*.jpg"); do convert $imagen -resize 256x256\! newFolder/$imagen; done

As� pues, las im�genes quedaron con resoluci�n 256x256 y con el mismo nombre que las im�genes originales. Quedaron almacenadas en newFolder en cada una de las sub
carpetas asociadas (train,val, test).

El for, se bas� en la sintaxis presentada en la gu�a del laboratorio, mientras que el resize se encontr� de https://imagemagick.org/Usage/resize/






