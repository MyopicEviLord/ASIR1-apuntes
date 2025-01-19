# Instalación y desinstalación de RAIDS en Linux
## 1. Creación del RAID
### 1.1 Recomendaciones previas

Para poder proceder a la creación, instalación, manipulación y desinstalación de RAIDS en Linux utilizaremos la herramienta/comando `mdadm`. Antes de comenzar con el proceso sería conveniente que realizáramos un formateo de nivel medio _(zero-filling)_ a los discos que pretendemos utilizar mediante el comando `dd if=dev/zero of=/dev/sdc status=progress`. Esta instrucción permite "llenar" todo el espacio del disco con ceros, eliminando cualquier posible dato residual que hubiera en él y dejándolo listo para usar.

>[!NOTE]
>La herramienta `mdadm` _(multiple device administration)_ no viene instalada por defecto en algunas distribuciones, por lo que es posible que necesitemos instalarla mediante `apt install mdadm`.

### 1.2. Creación de un RAID mediante MDADM

Una vez tenemos los discos preparados, procedemos a la creación de nuestro primer RAID. Para ello abrimos la terminal y utilizamos el comando `mdadm --create /dev/md0 --level=raid1 --raid-devices=2 /dev/sdb /dev/sdc`, donde encontramos los siguientes parámetros:

* `--create`: Indica que se procederá a la creación de un RAID. También se puede sustituir por `-C`.
* `/dev/md0`: Asigna el nombre que indiquemos al RAID creado, en este caso el escogido es `md0`.
* `--level=raid1`: La cifra final establece el nivel de RAID se va a crear. Como podemos ver, se trata de un RAID1.
* `--raid-devices=2`: Indica cuántos discos conformarán el RAID. En el ejemplo dado, serán 2.
* `/dev/sdb /dev/sdc`: Son los discos que utilizaremos en la composición del RAID, para nuestro ejercicio serán `sdb` y `sdc`.

Una vez ejecutemos el comando y se haya creado el RAID5, podemos monitorizar su estado, funcionamiento y componentes (y el del resto de RAID si los hay) mediante el comando `cat /proc/mdstat`. Para obtener información más detallada, también tenemos el comando `mdadm /dev/md0`. Como podemos ver, en este último se debe indicar el nombre  al RAID que nos interesa.

>[!TIP]
>El archivo `mdstat` _(multiple device status)_ se encuentra en la carpeta `/proc` y contiene información sobre las configuraciones de disco múltiples presentes en el sistema, es decir, de los RAID.

### Simular un fallo de disco

Dado que la principal función de los RAID es la de ofrecer una manera de salvaguardar los datos ante posibles accidentes, vamos a simular la rotura de un disco. Para lograrlo recurrimos al comando `mdadm-f /dev/md0 /dev/sdc`, donde el parámetro `-f` indica que simularemos un fallo, `/dev/md0` se refiere al RAID y `/dev/sdc` al disco que queremos romper. Una vez ejecutado, si utilizamos nuevamente `cat /proc/mdstat` comprobaremos que junto al disco `sdc` aparece ahora la indicación `(F)` que nos dice que efectivamente, nuestro disco tiene un fallo. También podemos ver que el disco restante continúa funcionando sin problemas.

Un disco con fallos no es útil para nuestro RAID, por lo que procederemos a retirarlo del mismo mediante el comando `mdadm --remove /dev/md0 /dev/sdc`. Si queremos agregar un nuevo disco el comando es casi idéntico: `mdadm --add /dev/md0 /dev/sdd`. En función del tamaño del disco y la información almacenada, el proceso de adición del nuevo disco puede llevar un tiempo.

### Anexo: Comandos usados y otros útiles

| Comando | Sintaxis | Uso | Ejemplo |
|:--------|:---------|:----|:--------|
|`cat` | `cat [parámetros] (archivo)` | Muestra en la terminal el contenido de un archivo. | `cat /proc/mdstat` |
| `mdadm /dev/md0` | `mdadm /dev/dispositivo RAID` | Ofrece información más detallada sobre el volumen RAID que indiquemos. | `mdadm /dev/md0` |
