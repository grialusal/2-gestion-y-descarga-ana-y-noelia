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

Con el comando `ls -li` podemos ver que se ha creado el enlace y que el inodo es el mismo que el fichero original. Además, se puede apreciar cómo el número de enlaces de referencia al archivo ha cambiado a 2.
![enlace duro](https://user-images.githubusercontent.com/92113066/138568836-1fbcbdba-83ef-4384-9141-be8338c1aede.png)


Ahora creamos un enlace blando en el directorio **pruebas-2**, con la orden `ln -s`:

`ln -s gtfscopy/Drosophila_melanogaster.BDGP6.28.102.gtf pruebas-2/`

Ejecutamos `ls -li` para comprobar que se ha creado correctamente y que el inodo es distinto del fichero original al que nos redirige.

![enlaces](https://user-images.githubusercontent.com/92091175/138259422-39cf14bb-1b13-4d35-bb3f-8beb71b11b97.png)

_**1.**_ Para borrar el archivo original utilizamos la orden `rm gtfscopy/Drosophila_melanogaster.BDGP6.28.102.gtf`. A continuación, accedemos a los directorios prueba con los enlaces creados.
Comprobamos así que, en la carpeta **pruebas-1** donde estaba el enlace duro, este sigue apareciendo. El inodo no ha cambiado pero el número de enlaces de referencia al archivo no es 2 sino 1. Comprobamos con `nano Drosophila_melanogaster.BDGP6.28.102.gtf` que se puede acceder igualmente al archivo.


![acceso enlace duro](https://user-images.githubusercontent.com/92091175/138285851-fe69cb81-4993-49b7-99b3-d749e6d7402e.png)

No ocurre así con el enlace blando del directorio **pruebas-2**, pues guarda la dirección original, a la cual no se puede acceder, y si se comprueba con `nano`, genera un archivo vacío.

![direccion enlace blando](https://user-images.githubusercontent.com/92091175/138286968-41fc59e7-bd40-405d-a733-0d123683904d.png)

![nuevo archivo nano](https://user-images.githubusercontent.com/92091175/138286753-908b9d64-2bfe-4460-83a0-db87f0f79636.png)

_**2.**_ En el caso del enlace duro, cuando vamos a la carpeta **gtfscopy** y ejecutamos el comando `ls -li` se observa como el fichero Drosophila_melanogaster.BDGP6.28.102.gtf presenta 2 enlaces de referencia. Si eliminamos el fichero de destino del enlace con la orden `rm pruebas-1/Drosophila_melanogaster.BDGP6.28.102.gtf` y ejecutamos el comando `ls -li` podemos ver como ese numero ahora es 1. Además, al ejecutar `nano` es posible acceder al fichero. 

![enlace duro 2](https://user-images.githubusercontent.com/92113066/138570675-374dd682-2b2f-4658-8d59-bc3104c414f3.png)

Cuando eliminamos el fichero de la carpeta **pruebas-2**, donde hemos creado el enlace blando, con la orden `rm pruebas-2/Drosophila_melanogaster.BDGP6.28.102.gtf` vemos que el fichero original se mantiene igual y se puede acceder al fichero con el editor `nano`.

![enlace blando 2](https://user-images.githubusercontent.com/92113066/138570938-9ff2db74-bd9e-4c44-898e-e0f553603455.png)


_**3.**_ Si se edita el fichero original, por ejemplo, con el editor `nano`, al consultar el enlace duro del directorio **pruebas-1** y ejecutar `nano`, podemos comprobar que aparece con la misma modificación:

![modificacion origen enlace duro](https://user-images.githubusercontent.com/92091175/138308707-8bce6b58-ebbb-44e0-874a-434e3ef7b3e0.png)

Sin embargo, al ejecutar `nano` en la carpeta **pruebas-2**, donde hemos creado el enlace blando, se observa vacio.

![enlace blando 3](https://user-images.githubusercontent.com/92113066/138571392-525b7110-1b12-4804-9e28-522bfcd100e0.png)

Al modificar el archivo de destino, en caso del enlace duro, tambien se modifica el fichero de origen. 

_**4.**_ Al copiar el enlace duro con la orden `cp Drosophila_melanogaster.BDGP6.28.102.gtf Drosophila.copia.enlace.duro.gtf` y ejecutar `ls` nos aparece que la copia que hemos hecho tiene un inodo distinto a la original:

![Copia enlace duro](https://user-images.githubusercontent.com/92091175/138264052-b989af1f-3412-4e45-b533-c9675e463e87.png)

Sin embargo, al intentar llevar a cabo la misma acción con nuestro enlace blando, nos aparece un error:

![error copia enlace blando](https://user-images.githubusercontent.com/92091175/138264633-8cf163f2-5e49-4076-a73b-2b80e9252f17.png)



## Ejercicio 2
Usa la documentación de `find` para encontrar todos los notebook Jupyter con fecha de última modificación 30 de Noviembre de 2020 que haya en tu directorio HOME. Excluye todos aquellos que se encuentren dentro de directorios ocultos (aquellos que comienzan por un punto `.`). 

### Respuesta ejercicio 2

Los ficheros de notebook Jupyter tienen la extensión .json, por lo que podemos utilizar esa información como punto de partida para buscar en nuestro directorio los archivos deseados. Para ello, utilizamos la orden `find . -name "*.json"`. Nos devuelve un solo archivo:

![find json](https://user-images.githubusercontent.com/92091175/138553224-476a6df0-5ff1-4420-9e57-75808190fce2.png)

Consultamos en `man find` cómo añadir a la orden principal los criterios de búsqueda por fecha de última modificación, así como el criterio de exclusión de directorios ocultos, y encontramos la siguiente entrada:
```
       -mtime n
              File's  data was last modified less than, more than or
              exactly n*24 hours ago.  See the comments  for  -atime
              to  understand how rounding affects the interpretation
              of file modification times.
```

Tenemos entonces que calcular `n`, esto es, el número de días que han pasado desde el 30 de noviembre de 2020 hasta hoy. 

***Se me ocurren dos formas:
-La rápida: Buscar en Google 'days since 30 nov 2020'
-La no-tan-rápida: definiendo variables (procedimiento consultado [aquí](https://www.linuxito.com/gnu-linux/nivel-basico/928-como-restar-fechas-en-bash))***

```
negido@cpg3:~$ FECHA_FIN=$(date +%s)
negido@cpg3:~$ FECHA_INICIO=$(date --date="2020-11-30" +%s)
negido@cpg3:~$ DIAS=$(( ($FECHA_FIN - $FECHA_INICIO) / (60*60*24) ))
negido@cpg3:~$ echo $DIAS
327
```
Ahora ya podemos añadir este criterio a la orden `find`: `find . -name "*.json" -mtime 327`.
Pero no devuelve ningún resultado.
Al ser un único archivo, podemos consultar la fecha de su última modificación accediendo al directorio donde se halla y ejecutar `date --reference=ARCHIVO`(orden encontrada en `man date`).

```
negido@cpg3:~/.local/share/jupyter/runtime$ date --reference=nbserver-144266.json 
mar 28 sep 2021 12:44:49 CEST
```
Comprobamos que la orden que hemos utilizado antes para buscar el archivo añadiendo la información sobre la fecha de última modificación funciona, ejecutando esta vez `find . -name "*.json" -mtime 25`

![find date](https://user-images.githubusercontent.com/92091175/138554875-bb6cc766-b0db-4491-95b3-a9ceece6ca9e.png)

## Ejercicio 3
Descarga, empleando la orden oportuna, todos los ficheros [de esta URL](ftp://ftp.ensembl.org/pub/release-102/gtf/accipiter_nisus/). 
- Inspecciona el fichero CHECKSUMS y genera los checksums adecuados para asegurarte de que los datos son íntegros. 
- Genera un fichero SHA-1 para todos los ficheros .gz descargados y documenta en esta memoria el comando con el que te descargaste los datos, adjuntando aquí los checksums. 


### Respuesta ejercicio 3
