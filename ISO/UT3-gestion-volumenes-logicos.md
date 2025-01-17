# Gestión de volumenes lógicos
1. pvcreate /dev/sdb1 /dev/sdc1: crea los discos logicos (si no acepta la orden, apt update y apt install lvm2 -y). Se pueden
2. pvs: muestra los discos disponibles para crear volúmenes lógicos, recién creados.
3. vgcreate grupo1 /dev/sdb1 /dev/sdc1: agrupa los discos físicos que hayamos especificado y les asigna el nombre.
4. vgs: muestra los grupos de discos creados.
5. lvcreate grupo1: crea un volumen lógico utilizando el grupo1 recién creado. Si se agrega el atributo -L se puede especificar el tamaño que se quiere coger del columen.Con el atributo -n se puede asignar un nombre a este volumen lógico, en este caso discologico.
6. lvs: lista los volúmenes lógicos presentes en el sistema y otra información.
7. mkfs.ext4 /dev/grupo1/discologico: formatea el disco
8. mkdir /media/discologico: Crea un directorio de montaje en la carpeta /media. Cambiar permisos con chmod 777 /media/discologico para dar acceso a todos.
9. mount /dev/grupo1/discologico /media/discologico
10. comprobar con ls -l /media/discologico o en explorador de archivos.
11. nano /etc/fstab

## Agregar al grupo    
 1. pvcrate /dev/sdd1
 2. vgextend grupo1 /dev/sdd1: Extiende el grupo1 con el disco sdd1
 3. lvresize -L +25 /dev/grupo1/discologico
 ## Reto
 Crear un disco RAID desde un volumen lógico.
