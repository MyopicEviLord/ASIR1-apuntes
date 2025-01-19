# Instalación y desinstalación de RAIDS en Linux
## 1. Creación del RAID
### 1.1 Recomendaciones previas

Para poder proceder a la creación, instalación, manipulación y desinstalación de RAIDS en Linux utilizaremos la herramienta/comando `mdadm`. Antes de comenzar con el proceso sería conveniente que realizáramos un formateo de nivel medio _(zero-filling)_ a los discos que pretendemos utilizar mediante el comando `dd if=dev/zero of=/dev/sdc status=progress`. Esta instrucción permite "llenar" todo el espacio del disco con ceros, eliminando cualquier posible dato residual que hubiera en él y dejándolo listo para usar.

>[!NOTE]
>La herramienta `mdadm` _(multiple device administration)_ no viene instalada por defecto en algunas distribuciones, por lo que es posible que necesitemos instalarla mediante `apt install mdadm`.

### 1.2. Creación de un RAID mediante MDADM

Una vez tenemos los discos preparados, procedemos a la creación de nuestro primer RAID. Para ello abrimos la terminal y utilizamos el comando `mdadm --create /dev/md0 --level=raid1 --raid-devices=2 /dev/sdb /dev/sdc`, donde encontramos los siguientes parámetros:

* `--create`: Indica que se procederá a la creación de un RAID. También se puede sustituir por `-C`.
* `/dev/md0`: Asigna el nombre que indiquemos al RAID creado, en este caso será `md0`.
* `--level=raidX`: Se utiliza para establecer qué tipo de RAID se va a crear. Debe ir seguido inmediatamente del número, como en nuestro caso `--level=raid1`.
* `--raid-devices=X`: Indica cuántos discos conformarán el RAID. En el ejemplo dado, serán 2.
* `/dev/sdb /dev/sdc`: Son los discos que utilizaremos en la composición del RAID, en esta caso serán `sdb` y `sdc`. 

### Anexo: Comandos usados y otros útiles

| Comando | Sintaxis | Uso | Ejemplo |
|:--------|:---------|:----|:--------|
|`cat /proc/mdstat` | `cat /proc/mdstat` | Permite comprobar y monitorizar el estado de los RAID presentes en el equipo. | `cat /proc/mdstat` |
| `mdadm /dev/md0` | `mdadm /dev/dispositivo RAID` | Ofrece información más detallada sobre el volumen RAID que indiquemos. | `mdadm /dev/md0` |
