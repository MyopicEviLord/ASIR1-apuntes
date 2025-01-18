# Gestión de Discos en Linux
## 1. Conexión y activación de un disco nuevo

Luego de conectar el nuevo disco, ya sea físicamente o de manera virtual, iniciamos nuestro equipo. Una vez dentro de nuestra sesión, abrimos el terminal e introducimos el comando `lsblk`, que mostrará en pantalla un listado de los discos presentes en nuestro dispositivo y sus correspondientes particiones. Si queremos evitar que Linux también nos muestre en el listado los discos loop del sistema, podemos agregar a nuestro comando los atributos `|grep -v loop` para simplificar la visualización.

### 1.1 Crear una partición
Para crear una partición en un disco, ejecutaremos el comando `fdisk /dev/sdb`. Una vez dentro de la interfaz, utilizaremos la tecla `g` para asignarle una etiqueta GPT al volumen y a continuación `n` para añadir una nueva partición en el disco sdb[^1].

>[!IMPORTANT]
>En nuestro comando utilizamos la ruta `/dev/sdb` para referirnos al disco sdb _(Sata Disk B)_ pero en el caso de que tengamos otros, el nombre cambiará. Por defecto Linux los denominará de manera consectiva como sda, sdb, sdc, etc.

A continuación el sistema nos preguntará qué número asignar a la partición. Indicamos el número `1` o pulsamos `intro` para que tome el valor 1 por defecto. El terminal nos pregunta en qué cilindro establecer nuestra partición, a lo que simplemente respondemos nuevamente con `intro` para asignar su valor por defecto. Justo después podemos configurar, de manera opcional, el espacio que queremos asignar a la partición, por ejemplo escribiendo `+5000MB` para asignar 5GB. En este caso simplemente continuamos con `intro`, dándole todo el espacio disponible en el disco. Por último, pulsaremos la tecla `w` para aplicar los cambios definitivamente.

>[!WARNING]
>La nueva partición no se crea realmente hasta que no pulsamos la tecla `w` para guardar los cambios, por lo que si el proceso se interrumpe habrá que volver a empezar. Si en algún momento queremos salir sin guardar los cambios utilizaremos la tecla `q`.

Podemos comprobar el resultado de nuestro trabajo utilizando el comando `fdisk -l` (opcionalmente también se le puede agregar los parámetros `|grep -v loop` para simplificar la vista), que nos mostrará que el disco sdb ahora cuenta con una partición, denominada sdb1 y con las siguientes características:

| Dispositivo | Comienzo | Final | Sectores | Tamaño | Tipo |
|-------------|-------|-----|--------|----|--------|
|/dev/sdb1 | 2048 | 20971486 | 20969439 | 10G | Sistema de ficheros de Linux |

### 1.2 Crear el sistema de ficheros (filesystem)

Una vez creada la partición de nuestro disco, necesitamos crear el sistema de archivos, también llamado formatear o dar formato. Para lograrlo ejecutaremos el comando `Mkfs.ext4 /dev/sdb1`, estableciendo así un sistema de ficheros ext4 en la partición sdb1.

### 1.3 Montar la unidad

Para poder utilizar nuestro nuevo disco primero debemos asignarle un punto demontaje, lo que se conoce como montar el disco. Linux lo hace automáticamente al iniciar el equipo, por lo que una de nuestras opciones es reiniciar el ordenador y dejar que el sistema se encargue. No obstante, si queremos montar nuestra unidad en el mismo momento sin la necesidad de un reinicio, podemos utilizar el comando `mount /dev/sdb1 /media/disco2`. Nuevamente con el parámetro `/dev/sdb1` nos referimos al disco en cuestión (recordemos que el nombre puede cambiar) y con `/media/disco2` indicamos la ruta del directorio en el cual se montará. Si bien un disco se puede montar en cualquier directorio de Linux, por convención siempre se montan en una carpeta propia dentro del directorio `/media`.

>[!TIP]
>Debemos tener en cuenta que la carpeta `disco2` que hemos creado en el directorio `/media` para montar nuestro disco ha sido creada por nosotros como usuarios `root`, por lo que solo nosotros tenemos acceso a ella y por tanto, al disco. Para que el resto de usuarios pueda acceder a él, tenemos que modificar los permisos correspondientes con el comando `chmod`.

### 1.4 Fijar el punto de montaje

Aunque ya se le ha asignado un punto de montaje al disco, en este momento no está fijado. Es decir, que una vez reiniciemos nuestro ordenador, Linux montará el disco en una ubicación del sistema a su elección y bajo un nuevo nombre, complicando su localización. Para evitar que esto ocurra y que el disco siempre se monte en la misma carpeta, debemos editar mediante el comando `nano /etc/fstab` las líneas de código de este archivo. En el último renglón escribiremos lo siguiente `/dev/sdb1 /media/disco2 ext4 defaults 0 0`, en el cual indicamos en orden el disco (`/dev/sdb1`), la ubicación en la que se debe montar (`/media/disco2`) y por último su sistema de archivos (`ext4`). La sintaxis `defaults 0 0` simplemente indica al sistema que utilice el resto de parámetros por defecto.

>[!TIP]
>El archivo `fstab` _(files system table)_ contiene un listado de todos los discos y particiones presentes en el sistema, y en él se indican cómo montar cada dispositivo y su configuración a utilizar a la hora de iniciar el sistema. Siempre se localiza en la carpeta `/etc`.

### Anexo: Comandos utilizados <!-- Pendiente de actualizar con otros útiles -->

| Comando | Sintaxis | Uso | Ejemplo |
|:---------|:----------|:-----|:------|
| `lsblk` | `lsblk [parámetros]` | Lista información sobre todos los dispositivos de bloque disponibles en el sistema. | `lsblk \|grep -v loop` |
| `fdisk` | `fdisk [parámetros] (/dev/disco)` | Utilizado para dividir de forma lógica un disco, es decir, crear particiones. | `fdisk /dev/sdb` |
| `Mkfs` | `Mkfs.(formato) (/dev/disco)` | Permite formatear un dispositivo con el sistema de archivos indicado. | `Mkfs.ext4 /dev/sdb1` |
| `mount` | `mount (/dev/disco) (/media/ruta de montaje)` | Monta el disco indicado en la ruta especificada. | `mount /dev/sdb1 /media/disco2` |
| `nano` | `nano (ruta de archivo)` | Permite editar un archivo mediante un editor de textos. | `nano /etc/fstab` |

[^1]: Si el sistema pregunta si queremos crear una partición extendida (`e`) o primaria (`p`) escogeremos la segunda opción.
