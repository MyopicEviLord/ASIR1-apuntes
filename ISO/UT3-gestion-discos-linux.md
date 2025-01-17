# Gestión de Discos en Linux
## 1. Conexión y activación de un disco nuevo

Luego de conectar el nuevo disco, ya sea físicamente o de manera virtual, iniciamos nuestro equipo. Una vez dentro de nuestra sesión, abrimos el terminal e introducimos el comando `lsblk`, que mostrará en pantalla un listado de los discos presentes en nuestro dispositivo y sus correspondientes particiones. Si queremos evitar que Linux también nos muestre en el listado los discos loop del sistema, podemos agregar a nuestro comando los atributos `|grep -v loop` para simplificar la visualización.

### 1.1 Crear una partición
Para crear una partición en un disco, ejecutaremos el comando `fdisk /dev/sdb`. Una vez dentro de la interfaz, utilizaremos la tecla `g` para asignarle una etiqueta GPT al volumen y a continuación `n` para añadir una nueva partición en el disco sdb[^1].

>[!IMPORTANT]
>En nuestro comando utilizamos la ruta `/dev/sdb` para referirnos al disco sdb _(Sata Disk B)_ pero en el caso de que tengamos otros, el nombre cambiará. Por defecto Linux los denominará de manera consectiva como sda, sdb, sdc, etc.

A continuación el sistema nos preguntará qué número asignar a la partición. Indicamos el número `1` o pulsamos `intro` para que tome el valor 1 por defecto. El terminal nos pregunta en qué cilindro establecer nuestra partición, a lo que simplemente respondemos nuevamente con `intro` para asignar su valor por defecto. Justo después podemos configurar, de manera opcional, el espacio que queremos asignar a la partición, por ejemplo escribiendo `+5000MB` para asignar 5GB. En este caso simplemente continuamos con `intro`, dándole todo el espacio disponible en el disco. Por último, pulsaremos la tecla `w` para aplicar los cambios definitivamente.

Podemos comprobar el resultado de nuestro trabajo utilizando de nuevo el comando `fdisk -l` (opcionalmente también se le puede agregar los atributos `|grep -v loop` para simplificar la vista), que nos mostrará que el disco sdb ahora cuenta con una partición, denominada sdb1 y con las siguientes características:

| Dispositivo | Comienzo | Final | Sectores | Tamaño | Tipo |
|-------------|-------|-----|--------|----|--------|
|/dev/sdb1 | 2048 | 20971486 | 20969439 | 10G | Sistema de ficheros de Linux |

>[!WARNING]
>La partición no se crea realmente hasta que no pulsamos la tecla `w` para guardar los cambios, por lo que si el proceso se interrumpe habrá que volver a empezar. Si en algún momento queremos salir sin guardar los cambios utilizaremos la tecla `q`.

### 1.2 Crear el sistema de ficheros (filesystem)

Una vez creada la partición de nuestro disco, necesitamos crear el sistema de archivos, también llamado formatear o dar formato. Para lograrlo ejecutaremos el comando `Mkfs.ext4 /dev/sdb1` o bien `Mkfs -t ext4 /dev/sdb1`, estableciendo así un sistema de ficheros ext4 en la partición sdb1.

### 1.3 Crear un punto de montaje

[^1]: Si el sistema pregunta si queremos crear una partición extendida (`e`) o primaria (`p`) escogeremos la segunda opción.
