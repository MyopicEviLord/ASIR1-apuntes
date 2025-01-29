# 1. Configuración del switch mediante consola

Primero que nada, debemos comprobar que nuestro switch y el PC que queremos configurar para acceder en modo consola están conectados. Si no lo están, los conectamos mediante el cable apropiado, partiendo del puerto **RS232** en el PC al puerto **Console** del switch).

Una vez conectados, procedemos a configurar el switch. Entramos al dispositivo y a continuación vamos a la pestaña CLI _(Command Line Interface)_. Aquí es donde iniciaremos la configuración de nuestro switch mediante comandos.

## 1.1 Vista general de la configuración de consola

```
Switch>enable
Switch#configure terminal
Switch(config)#enable secret asir
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#hostname asir1
asir1(config)#line console 0
asir1(config-line)#password consola
asir1(config-line)#login
asir1(config-line)#do write
Building configuration...
[OK]
asir1(config-line)#exit
asir1(config)#service password-encryption
asir1(config)#exit
asir1#
%SYS-5-CONFIG_I: Configured from console by console
```
A continuación se desglosan los diferentes comandos utilizados:

* `enable`: activa el modo privilegiado, que permite configurar el dispositivo.
* `configure terminal`: otorga acceso a la configuración del terminal.
* `enable password (contraseña)` o `enable secret (contraseña)`: establece una contraseña para entrar en modo privilegiado. La diferencia es el cifrado que la opción `secret` ofrece en contraste con `password`, que guarda las contraseñas en formato de texto plano. Por ello, siempre es mejor utilizar la opción `secret`.
* `hostname (nombre)`: asigna un nombre al dispositivo. Este paso es opcional, pero ayuda a identificar el dispositivo con un término fácil de recordar.
* `line console 0`: permite acceder a la interfaz de la consola, que siempre será la 0.
* `password (contraseña)`: establece una contraseña para utilizar la consola (nótese que al momento de definir la contraseña estamos dentro de la configuración de la línea 0, es decir, la consola).
* Utilizar el comando `login`.
* Finalmente utilizar `do write` para guardar los cambios.

Entrar al PC configurado con consola, abrir el terminal en el escritorio y comprobar que hay conexión.
