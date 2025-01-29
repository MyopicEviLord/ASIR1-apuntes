# 1. Configuración del switch mediante consola

Primero que nada, debemos comprobar que nuestro switch y el PC que queremos configurar para acceder en modo consola están conectados. Si no lo están, los conectamos mediante el cable apropiado, partiendo del puerto **RS232** en el PC al puerto **Console** del switch).

Una vez conectados, procedemos a configurar el switch. Entramos al dispositivo y a continuación vamos a la pestaña CLI _(Command Line Interface)_. Aquí es donde iniciaremos la configuración de nuestro switch mediante comandos.

## 1.1 Vista general de la configuración de consola

```
Switch>
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
asir1(config)#exit
asir1#
%SYS-5-CONFIG_I: Configured from console by console
asir1#exit
asir1>
```
A continuación se desglosan los diferentes comandos utilizados:

* `enable`: activa el modo privilegiado, que permite configurar el dispositivo.
* `configure terminal`: otorga acceso a la configuración del terminal.
* `enable password (contraseña)` o `enable secret (contraseña)`: establece una contraseña para entrar en modo privilegiado. La diferencia es el cifrado que la opción `secret` ofrece en contraste con `password`, que guarda las contraseñas en formato de texto plano. Por ello, siempre es mejor utilizar la opción `secret`.
* `hostname (nombre)`: asigna un nombre al dispositivo. Este paso es opcional, pero ayuda a identificar el dispositivo con un término fácil de recordar.
* `line console 0`: permite acceder a la interfaz de la consola, que siempre será la 0.
* `password (contraseña)`: establece una contraseña para utilizar la consola (nótese que al momento de definir la contraseña estamos dentro de la configuración de la línea 0, es decir, la consola).
* `login`: fideliza las credenciales introducidas.
* `do write`: guarda los cambios realizados.
* `exit`: Sale del entorno de configuración en el que estamos localizados.

Una vez finalizado el proceso, debemos entrar al PC que hayamos conectado por consola, acceder a su escritorio abrir el `terminal`. La pantalla que se muestra debe ser un reflejo de la que hemos visto en el CLI del switch, verificando que hay conexión y podemos configurarlo desde este dispositivo.

## Configuración primaria de la vlan (común tanto a Telnet como a SSH)

```
Switch>
Switch>enable
Switch#configure terminal
Switch#(config)#vlan 200
Switch#(config-vlan)#interface vlan 200
ASIR(config-if)#
%LINK-5-CHANGED: Interface Vlan 200, changed state to up
ip address 192.168.27.1 255.255.255.0
Switch(config-if)#exit
Switch(config)#exit
Switch#exit
Switch>
```
A continuación se desglosan los diferentes comandos utilizados:

* `vlan 200`: Crea una nueva vlan llamada vlan 200.
* `interface vlan 200`: Permite configurar la vlan especificada, en este caso la vlan 200.
* `ip address 192.168.27.1 255.255.255.0`: Establece la dirección IP introducida como la IP de la vlan que estamos configurando.

## 1.2 Vista general de la configuración para telnet

```
ASIR1>enable
Password:
ASIR1#configure terminal
Enter configuration commands, one per line.  End with CNTL/Z.
ASIR1(config)#interface fa0/1
ASIR1(config-if)#switchport mode access
ASIR1(config-if)#switchport access vlan 200
ASIR1(config-if)#
%LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan200, changed state to up
exit
ASIR1(config)#interface fa0/2
ASIR1(config-if)#switchport mode access
ASIR1(config-if)#switchport access vlan 200
ASIR1(config-if)#exit
ASIR1(config)#line vty 0 1
ASIR1(config-line)#transport input telnet
ASIR1(config-line)#password telnet
ASIR1(config-line)#login
ASIR1(config-line)#do write
Building configuration...
[OK]
ASIR1(config-line)#exit
ASIR1(config)#exit
ASIR1#
```

A continuación se desglosan los diferentes comandos utilizados:

* `interface fa0/1`:
* `switchport mode access`:
* `switchport access vlan 200`:
* `line vty 0 1`:
* `transport input telnet`:

## 1.3 Vista general de la configuración via ssh

```
ASIR1#configure terminal
Enter configuration commands, one per line.  End with CNTL/Z.
ASIR1(config)#vlan 200
ASIR1(config-vlan)#name asir_ssh
ASIR1(config-vlan)#exit
ASIR1(config)#ip domain-name asir.com
ASIR1(config)#crypto key generate rsa
The name for the keys will be: ASIR1.asir.com
Choose the size of the key modulus in the range of 360 to 4096 for your
  General Purpose Keys. Choosing a key modulus greater than 512 may take
  a few minutes.

How many bits in the modulus [512]: 2048
% Generating 2048 bit RSA keys, keys will be non-exportable...[OK]

ASIR1(config)#interface fa0/3
*Mar 1 1:0:48.608: %SSH-5-ENABLED: SSH 1.99 has been enabled
ASIR1(config-if)#switchport mode access
ASIR1(config-if)#switchport access vlan 200
ASIR1(config-if)#interface fa0/4
ASIR1(config-if)#switchport mode access
ASIR1(config-if)#switchport access vlan 200
ASIR1(config-if)#exit
ASIR1(config)#line vty 2 3
ASIR1(config-line)#transport input ssh
ASIR1(config-line)#password ssh
ASIR1(config-line)#login
ASIR1(config-line)#do write
Building configuration...
[OK]
ASIR1(config-line)#exit
ASIR1(config)#exit
ASIR1#
```

A continuación se desglosan los diferentes comandos utilizados:

* `name asir_ssh`:
* `ip domain-name asir.com`:
* `crypto key generate rsa`:
* `transport input ssh`:
