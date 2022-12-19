# Privilege Escalation

Para escalar privilegios se necesita primero que nada enumerar como se encuenta el sistema para lo cual realice esta especie de check list.

## List Current Process

En este caso los procesos de root

```
ps aux | grep root # pero puede ser de cualquier usuario

```

## Users Home Directory .bash_history  /.ssh history(command)

Usar .bash_history para ver si no hay informacion como llaves ssh

```
ls /home

```

```

ls -l ~/.ssh

```

Sirve para ver lo que se ha tecleado ya sabes.

```
history

```

## Sudo - List User's Privileges

Puedes checar que comando pueden ser corridos con tu user actual ya sea sin password NOPASSWD ( la cual es la directiva mas peligrosa ) y que otros pueden ser corridos como otro usuario con "sudo -u deploy" por ejemplo especificaste que correr el comando como el usuario deploy.


```
sudo -u user_example
sudo -l
```

## Buscar archivos interesantes ( configuration files, Readable Shadow files, password hashes in /etc/passwd )

Por ejemplo un hash dentro del archivo passwd 

```
cat /etc/passwd 

logger:x:1004:1004::/home/logger:
shared:x:1005:1005::/home/shared:
stacey.jenkins:x:1006:1006::/home/stacey.jenkins:
sysadm:$6$vdH7vuQIv6anIBWg$Ysk.UZzI7WxYUBYt8WRIWF0EzWlksOElDE0HLYinee38QI1A.0HW7WZCrUhZ9wwDz13bPpkTjNuRoUGYhwFE11:1007:1007::/home/sysadm:

```

## Cron Jobs


Cron jobs on Linux systems are similar to Windows scheduled tasks.

```
ls -la /etc/cron.daily/


```

## Unmounted File Systems and Additional Drives

Si descubre y puede montar una unidad adicional o un sistema de archivos desmontado, puede encontrar archivos confidenciales, contraseñas o copias de seguridad que se pueden aprovechar para escalar privilegios.

El comando lsblk nos muestra información de todos los dispositivos de bloque (discos duros, SSD, memorias flash, CD-ROM…) que forman parte del hardware de nuestro ordenador.

```
lsblk

```

## SETUID and SETGID Permissions and Writeable Directories

> Binaries are set with these permissions to allow a user to run a command as root, without having to grand root-level access to the user. Many binaries contain functionality that can be exploited to get a root shell.

### Cron y los directorios con permiso de escritura

> Puede descubrir un directorio grabable donde un trabajo cron coloca archivos, lo que proporciona una idea de la frecuencia con la que se ejecuta el trabajo cron y podría usarse para elevar los privilegios si el script que ejecuta el trabajo cron también se puede escribir.

### Find

Usa el comando find lo mas especifico posible para encontrar archivos intenresantes aqui algunas configuracion


#### Encontrar carpetas con permiso de escritura para otros usuarios que no sean el que estamos en ese momento


```
find / -path /proc -prune -o -type d -perm -o+w 2>/dev/null
```

#### Encontrar archuvos con el permiso de escritura

```
find / -path /proc -prune -o -type f -perm -o+w 2>/dev/null
```
