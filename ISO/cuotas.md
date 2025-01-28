1. apd update
2. apt install quota quotatool
3. nano /etc/fstab
4. justo despues de default poner `,usrquota,grpquota` en función de si vamos a poner quotas de usuario o grupo. Guardar y salir.
5. mount -o remount,rw /media/(disco)
6. quotacheck -cgu  /media/discologico (crear cuotas para grupos y usuarios) crea las cuotas en el punto de montaje.
7. ls -l /media/doscologico (deben aparecer archivos aquota.user y aquota.group
8. edquota -u (usuario)
9. quota -vs usuario para comprobar las cuotas de un usuario/grupo
10. quotaon /media/disco
