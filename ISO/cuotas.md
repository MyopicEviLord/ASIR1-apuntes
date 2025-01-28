apd update
apt install quota quotatool
nano /etc/fstab
justo despues de default poner `,usrquota,grpquota` en función de si vamos a poner quotas de usuario o grupo. Guardar y salir.
mount -o remount,rw /media/(disco)
quotacheck -cgu  /media/discologico (crear cuotas para grupos y usuarios) crea las cuotas en el punto de montaje.
ls -l /media/doscologico (deben aparecer archivos aquota.user y aquota.group
edquota -u (usuario)
quota -vs usuario para comprobar las cuotas de un usuario/grupo
quotaon /media/disco
