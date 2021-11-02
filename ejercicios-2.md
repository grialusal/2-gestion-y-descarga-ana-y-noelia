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

Con el comando `ls -li` podemos ver que se ha creado el enlace y que el inodo es el mismo que el fichero original. Además, se puede apreciar cómo el número de enlaces de referencia ha cambiado a 2.
![enlace duro](https://user-images.githubusercontent.com/92113066/138568836-1fbcbdba-83ef-4384-9141-be8338c1aede.png)


Ahora creamos un enlace blando en el directorio **pruebas-2**, con la orden `ln -s`:

`ln -s ../gtfscopy/Drosophila_melanogaster.BDGP6.28.102.gtf .`

Ejecutamos `ls -li` para comprobar que se ha creado correctamente y que el inodo es distinto del fichero original al que nos redirige.

![enlance blando4](https://user-images.githubusercontent.com/92113066/139146379-8bdcbb7b-dcd3-4c75-99b3-1cf496907c1c.png)

_**1.**_ Para borrar el archivo original utilizamos la orden `rm gtfscopy/Drosophila_melanogaster.BDGP6.28.102.gtf`. A continuación, accedemos a los directorios prueba con los enlaces creados.
Comprobamos así que, en la carpeta **pruebas-1** donde estaba el enlace duro, este sigue apareciendo. El inodo no ha cambiado pero el número de enlaces de referencia ya no es 2 sino 1. Comprobamos con `nano Drosophila_melanogaster.BDGP6.28.102.gtf` que se puede acceder igualmente al archivo.


![acceso enlace duro](https://user-images.githubusercontent.com/92091175/138285851-fe69cb81-4993-49b7-99b3-d749e6d7402e.png)

No ocurre así con el enlace blando, pues guarda la dirección original, a la cual no se puede acceder, y si se comprueba con `nano`, genera un archivo vacío.

![enlace blando del original borrado](https://user-images.githubusercontent.com/92091175/139207055-3d45db5b-216b-4e1d-a8c4-366784390881.png)


![nuevo archivo nano](https://user-images.githubusercontent.com/92091175/138286753-908b9d64-2bfe-4460-83a0-db87f0f79636.png)

_**2.**_ En el caso del enlace duro cuando vamos a la carpeta **gtfscopy** y ejecutamos el comando `ls -li` se observa como el fichero Drosophila_melanogaster.BDGP6.28.102.gtf tiene 2 enlaces de referencia. Si eliminamos el fichero de destino del enlace con la orden `rm pruebas-1/Drosophila_melanogaster.BDGP6.28.102.gtf` y ejecutamos el comando `ls -li` podemos ver como ese número ahora es 1. Además, al ejecutar `nano` también es posible acceder al fichero. 

![enlace duro 2](https://user-images.githubusercontent.com/92113066/138570675-374dd682-2b2f-4658-8d59-bc3104c414f3.png)

Cuando eliminamos el fichero de la carpeta **pruebas-2**, donde hemos creado el enlace blando, con la orden `rm pruebas-2/Drosophila_melanogaster.BDGP6.28.102.gtf` vemos que el fichero original se mantiene igual y se puede acceder al fichero con el editor `nano`.

![enlace blando 2](https://user-images.githubusercontent.com/92113066/138570938-9ff2db74-bd9e-4c44-898e-e0f553603455.png)


_**3.**_ Si se edita el fichero original, por ejemplo, con el editor `nano`, al consultar el enlace duro del directorio **pruebas-1** y ejecutar `nano`, podemos comprobar que aparece con la misma modificación:

![modificacion origen enlace duro](https://user-images.githubusercontent.com/92091175/138308707-8bce6b58-ebbb-44e0-874a-434e3ef7b3e0.png)

Al modificar el archivo de destino, en caso del enlace duro, también se modifica el fichero de origen.

Si accedemos al enlace blando habiendo previamente modificado el archivo original, este va a seguir mostrando la misma ruta, pues no se ha cambiado la ubicación del archivo. Al ejecutar el fichero con `nano` se observa que tambien presenta las modificaciones que le hemos hecho al fichero original. Si hubiéramos movido el fichero original, el enlace blando seguiría indicando la misma ruta, por lo que dejaría de ser válido.
El enlace blando, al no ser un fichero *per se*, siempre va a conservar la misma ruta, salvo que forcemos su modificación o creemos uno nuevo en otro directorio.

_**4.**_ Al copiar el enlace duro con la orden `cp Drosophila_melanogaster.BDGP6.28.102.gtf Drosophila.copia.enlace.duro.gtf` y ejecutar `ls` nos aparece que la copia que hemos hecho tiene un inodo distinto a la original:

![Copia enlace duro](https://user-images.githubusercontent.com/92091175/138264052-b989af1f-3412-4e45-b533-c9675e463e87.png)

Cuando copiamos el enlace blando con la orden `cp Drosophila_melanogaster.BDGP6.28.102.gtf Drosophila.copia.enlace.blando.gtf`y observar los ficheros con el comando `ls -li` podemos ver que se copia el fichero enlazado que tiene un inodo diferente al original y no es un enlace.

![enlace blando5](https://user-images.githubusercontent.com/92113066/139148805-6acaa62d-3430-45a2-aba5-fca2d3afda9f.png)

### Comentario: Todo correcto, muy bien :)
### Nota: 3.3/10

## Ejercicio 2

Usa la documentación de `find` para encontrar las opciones que permiten encontrar todos los notebook Jupyter (ficheros con extension ipynb) con fecha de última modificación 17 de Noviembre de 2020 que haya en el directorio /home/alejandro. Excluye todos aquellos que se encuentren dentro de directorios ocultos (aquellos que comienzan por un punto .).

### Respuesta ejercicio 2

Los ficheros de notebook Jupyter tienen la extensión .ipynb, por lo que podemos utilizar esa información como punto de partida para buscar en el directorio correspondiente los archivos deseados. Para ello, utilizamos la orden `find . -name "*.ipynb"`. 

![find ipynb](https://user-images.githubusercontent.com/92091175/138910635-0289b949-a546-465f-b949-0860629cfb08.png)

~~Consultamos en `man find` cómo añadir a la orden principal los criterios de búsqueda por fecha de última modificación, así como el criterio de exclusión de directorios ocultos, y encontramos la siguiente entrada:~~ 
```
       -mtime n
              File's  data was last modified less than, more than or
              exactly n*24 hours ago.  See the comments  for  -atime
              to  understand how rounding affects the interpretation
              of file modification times.
```

~~Tenemos entonces que calcular `n`, esto es, el número de días que han pasado desde el 17 de noviembre de 2020 hasta hoy. Una de las formas de hacerlo es definiendo variables (procedimiento consultado [aquí](https://www.linuxito.com/gnu-linux/nivel-basico/928-como-restar-fechas-en-bash)). Nombramos la fecha actual como `FECHA_FIN` y el 17 de noviembre de 2020 será `FECHA_INICIO` Con `+%s` le estamos dando instrucciones para pasar la fecha al número de segundos transcurridos desde el 1 de enero de 1970, para así restar la fecha final menos la de inicio. La cantidad resultante se divide entre 60 * 60 * 24 para transformar los segundos en días.~~

```
negido@cpg3:~$ FECHA_FIN=$(date +%s)
negido@cpg3:~$ FECHA_INICIO=$(date --date="2020-11-17" +%s)
negido@cpg3:~$ DIAS=$(( ($FECHA_FIN - $FECHA_INICIO) / (60*60*24) ))
negido@cpg3:~$ echo $DIAS
343
```
~~Ahora ya podemos añadir este criterio a la orden `find`: `find . -name "*.ipynb" -mtime 343`.
Nos devuelve un archivo:~~

![archivo ipynb](https://user-images.githubusercontent.com/92091175/138912394-ade8e403-3d6a-443a-8cfb-8d9bf57d9c71.png)


~~Por último, añadimos el criterio de exclusión de ficheros ocultos mediante `-not -path '*/\.*'`.Con esta orden le decimos a la shell que no incluya ficheros que tengan en su ruta `/.`. Nos quedaría algo así: `find . -not -path '*/\.*' -name "*.ipynb" -mtime 343`.~~

![excluir ficheros ocultos](https://user-images.githubusercontent.com/92091175/138913090-7f46de68-826f-4f61-8db0-2599b63fa288.png) 


Consultamos el manual de `find` y buscamos el comando que nos permita introducir la fecha de modificación del archivo que nos interesa, en este caso, 17 de noviembre de 2020. 

![find newermt](https://user-images.githubusercontent.com/92091175/139240213-56665503-29d1-4b42-a2f7-40e3c41ddb6c.png)

Añadimos también el criterio de exclusión de archivos ocultos mediante `-not -path '*/\.*'`.Con esta orden le decimos a la shell que no incluya ficheros que tengan en su ruta `/.`. Quedaría algo así:

`find . -not -path '*/\.*' -name "*.ipynb" -newermt "20201117"`

![find](https://user-images.githubusercontent.com/92091175/139240718-c0953297-9ada-4d87-8b63-a1b0faff6c10.png)

### Comentario: Todo correcto, muy bien :)
### Nota: 3.3/10

## Ejercicio 3
Descarga, empleando la orden oportuna, todos los ficheros [de esta URL](ftp://ftp.ensembl.org/pub/release-102/gtf/accipiter_nisus/). 
- Inspecciona el fichero CHECKSUMS y genera los checksums adecuados para asegurarte de que los datos son íntegros. 
- Genera un fichero SHA-1 para todos los ficheros .gz descargados y documenta en esta memoria el comando con el que te descargaste los datos, adjuntando aquí los checksums. 


### Respuesta ejercicio 3

Para descargar todos los ficheros ejecutamos el comando:

`wget -r ftp://ftp.ensembl.org/pub/release-102/gtf/accipiter_nisus/`

Posteriormente vamos al directorio donde se han descargado y comprobamos que están todos los ficheros

![ficheros](https://user-images.githubusercontent.com/92113066/138591151-7da69191-f4a1-4647-8715-85cdf50dcba6.png)


Inspeccionamos el fichero CHECKSUMS y vemos que estan los checksums de los ficheros .gz calculados con el algoritmo `CRC`.

![checksum](https://user-images.githubusercontent.com/92113066/138590344-f4395c7c-e190-4ff3-9548-caff01c0f778.png)

Generamos los checksums empleando el mismo algoritmo para asegurarnos de la integridad de los datos y comprobamos que coinciden. El comando empleado fue 

`sum Accipiter_nisus.Accipiter_nisus_ver1.0.102.abinitio.gtf.gz Accipiter_nisus.Accipiter_nisus_ver1.0.102.gtf.gz`

![checksum2](https://user-images.githubusercontent.com/92113066/138590434-7bd9be24-108c-47c1-ac17-b9efc7b70418.png)

Creamos un fichero SHA-1 para los ficheros .gz empleando la orden `shasum ./*.gz > gz_checksums.sha` y lo nombramos *gz_checksums*. Con el comando `cat gz_checksums.sha` podemos observar los checksums almacenados en el fichero *gz_checksums*.

![sha](https://user-images.githubusercontent.com/92113066/138591004-ed0e4b9a-946c-45fc-8c2b-04dc64aca4bf.png)

Finalmente, comprobamos los checksums con `-c`:

![comprobacion checksums](https://user-images.githubusercontent.com/92091175/138592913-6ad472c5-1406-49b1-9a39-a341ae5ce50c.png)



### Comentario: Simplemente os faltó añadir en el comando `wget -r ftp://ftp.ensembl.org/pub/release-102/gtf/accipiter_nisus/` las opciones `--no-parent` para que no descarge todo lo que hay en directorios que están por encima en el árbol de directorios y `--no-directories` para que no os descargue los datos con la misma estructura de directorios y os descargue simplemente los archivos, por lo demás todo correcto, muy bien :)
### Nota: 3.2/10






