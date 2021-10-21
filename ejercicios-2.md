# Sesión II - Gestión avanzada y descarga de datos
Herramientas computacionales para bioinformática: UNIX, expresiones regulares y shell script

Edita esta plantilla en formato markdown [Guía aquí](https://guides.github.com/features/mastering-markdown/) como se pide en el guión. 
Cuando hayas acabado, haz un commit de tus cambios y súbelos al repositorio antes de la fecha de entrega señalada. 
Puedes usar el cliente 

======================================
Antes de comenzar, crea


## Ejercicio 1
Crea una copia de la carpeta gtfs. Luego crea enlaces duros y blandos a los ficheros .gtf y responde:

1. ¿Qué ocurre cuando se borra el origen y se intenta acceder al destino?
2. ¿Qué ocurre cuando se borra el destino y se intenta acceder al origen?
3. ¿Qué ocurre con la otra parte cuando se edita el destino o el origen del enlace?
4. ¿Qué ocurre cuando copiamos un enlace?


### Respuesta ejercicio 1

En primer lugar, para crear una copia de la carpeta gtfs nos situamos en nuestro repositorio y ejecutamos la orden `cp -r gtfs gtfscopy`. Comprobamos con `ls` que nuestra copia se ha creado:

![copy gtfs](https://user-images.githubusercontent.com/92091175/138259066-6afac656-4a67-43a8-9d52-7d40750aeb08.png)

A continuación, con la orden `mkdir` creamos dos directorios prueba en los que ubicaremos nuestros enlaces duros y blandos. 
En el directorio **pruebas-1** crearemos un enlace duro con la orden `ln`:

`ln gtfscopy/Drosophila_melanogaster.BDGP6.28.102.gtf pruebas-1/`

Con el comando `-i` podemos ver que se ha creado, y que el número de inodos ha subido a 2.

Ahora creamos un enlace blando en el directorio **pruebas-2**, con la orden `ln -s`:

`ln -s gtfscopy/Drosophila_melanogaster.BDGP6.28.102.gtf pruebas-2/`

Ejecutamos `ls -li` para comprobar que se ha creado correctamente y que el inodo es distinto del fichero original al que nos redirige.

![enlaces](https://user-images.githubusercontent.com/92091175/138259422-39cf14bb-1b13-4d35-bb3f-8beb71b11b97.png)

1. Para borrar el archivo original utilizamos la orden `rm gtfscopy/Drosophila_melanogaster.BDGP6.28.102.gtf`. A continuación, accedemos a los directorios prueba con los enlaces creados.
Comprobamos así que, en la carpeta **pruebas-1** donde estaba el enlace duro, este sigue apareciendo. El número de inodos ha cambiado a 1, pero comprobamos con `nano Drosophila_melanogaster.BDGP6.28.102.gtf` que se puede acceder igualmente al archivo.


![acceso enlace duro](https://user-images.githubusercontent.com/92091175/138285851-fe69cb81-4993-49b7-99b3-d749e6d7402e.png)

No ocurre así con el enlace blando del directorio **pruebas-2**, pues guarda la dirección original, a la cual no se puede acceder, y si se comprueba con `nano`, genera un archivo vacío.

![direccion enlace blando](https://user-images.githubusercontent.com/92091175/138286968-41fc59e7-bd40-405d-a733-0d123683904d.png)

![nuevo archivo nano](https://user-images.githubusercontent.com/92091175/138286753-908b9d64-2bfe-4460-83a0-db87f0f79636.png)


3. Si se edita el fichero original, por ejemplo, con el editor `nano`, al consultar el enlace duro del directorio **pruebas-1** y ejecutar `nano`, podemos comprobar que aparece con la misma modificación:

![modificacion origen enlace duro](https://user-images.githubusercontent.com/92091175/138308707-8bce6b58-ebbb-44e0-874a-434e3ef7b3e0.png)


4. Al copiar el enlace duro con la orden `cp Drosophila_melanogaster.BDGP6.28.102.gtf Drosophila.copia.enlace.duro.gtf` y ejecutar `ls` nos aparece que la copia que hemos hecho tiene un inodo distinto a la original:

![Copia enlace duro](https://user-images.githubusercontent.com/92091175/138264052-b989af1f-3412-4e45-b533-c9675e463e87.png)

Sin embargo, al intentar llevar a cabo la misma acción con nuestro enlace blando, nos aparece un error:

![error copia enlace blando](https://user-images.githubusercontent.com/92091175/138264633-8cf163f2-5e49-4076-a73b-2b80e9252f17.png)



## Ejercicio 2
Usa la documentación de `find` para encontrar todos los notebook Jupyter con fecha de última modificación 30 de Noviembre de 2020 que haya en tu directorio HOME. Excluye todos aquellos que se encuentren dentro de directorios ocultos (aquellos que comienzan por un punto `.`). 

### Respuesta ejercicio 2


## Ejercicio 3
Descarga, empleando la orden oportuna, todos los ficheros [de esta URL](ftp://ftp.ensembl.org/pub/release-102/gtf/accipiter_nisus/). 
- Inspecciona el fichero CHECKSUMS y genera los checksums adecuados para asegurarte de que los datos son íntegros. 
- Genera un fichero SHA-1 para todos los ficheros .gz descargados y documenta en esta memoria el comando con el que te descargaste los datos, adjuntando aquí los checksums. 


### Respuesta ejercicio 3
