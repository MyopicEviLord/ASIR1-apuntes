# Gestión de Discos en Linux
## Conexión y activación de un disco nuevo

Luego de conectar el nuevo disco, ya sea físicamente o de manera virtual, iniciamos nuestro equipo. Una vez dentro de nuestra sesión, abrimos el terminal e introducimos el comando `fdisk -l`.

### Crear una partición

Para crear una partición en un disco, ejecutaremos el comando `fdisk /dev/sdb`. Una vez dentro de la interfaz, utilizaremos la tecla `n` para añadir una nueva partición en el disco sdb. Cuando el sistema nos pregunte qué tipo de partición queremos crear, extendida (`e`) o primaria (`p`), escogemos la segunda pulsando la tecla correspondiente y ejecutamos con la tecla `intro`.

>[!IMPORTANT]
>En este caso en el comando `fdisk /dev/sdb` nos referimos al disco sdb _(Sata Disk B)_ pero en el caso de que tengamos otros, el nombre cambiará. Por defecto Linux los denominará de manera consectiva como sda, sdb, sdc, etc.

A continuación el sistema nos preguntará qué número asignar a la partición. Indicamos el número `1` y a continuación pulsamos `intro`. El terminal nos pregunta en qué cilindro establecer nuestra partición, a lo que simplemente respondemos con `intro` para asignar su valor por defecto. Justo después podemos configurar, de manera opcional, el espacio que queremos asignar a la partición, por ejemplo escribiendo `+5000MB` para asignar 5GB. En este caso simplemente continuamos con `intro`, dándole todo el espacio disponible en el disco. Por último, estableceremos el sistema de archivos pulsando la tecla `t`. Para aplicar los cambios definitivamente, pulsaremos la tecla `w`.

Podemos comprobar el resultado de nuestro trabajo utilizando de nuevo el comando `fdisk -l`, que nos mostrará que el disco sdb ahora cuenta con una partición, denominada sdb1 y con las siguientes características:

| Device Boot | Start | End | Blocks | Id | System |
|-------------|-------|-----|--------|----|--------|
|/dev/sdb1 | 1 | 652 | 5237158+ | 83 | Linux |

>[!WARNING]
>La partición no se crea realmente hasta que no pulsamos la tecla `w` para guardar los cambios, por lo que si el proceso se interrumpe habrá que volver a empezar. Si en algún momento queremos salir sin guardar los cambios utilizaremos la tecla `q`.

### Crear el sistema de ficheros (filesystem)

Una vez creada la partición de nuestro disco, necesitamos crear el sistema de archivos, también llamado formatear o dar formato. Para lograrlo ejecutaremos el comando `Mkfs.ext4 /dev/sdb1` o bien `Mkfs -t ext4 /dev/sdb1`, estableciendo así un sistema de ficheros ext4 en la partición sdb1.

### Crear un punto de montaje

