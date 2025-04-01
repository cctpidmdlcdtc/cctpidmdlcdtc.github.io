# Mundo ordinario

Tengo un dominio, un servidor, 20 años de experiencia administrando páginas web, pero no tengo una propia. Ejemplo de manual de en casa del herrero cuchillo de palo.




# Llamada a la aventura

Mi diógenes digital ya no es cosa de risa:
- Tengo más de 20 años de experiencia procrastinando cosas, como por ejemplo, un blog.
- Llevo más de 10 años acumulando contenido para mi publicar en mi web.
- Hace más de 5 años que ultimo los detalles para mi primer vídeo de Youtube.
- Desde hace 1 año estoy preparando el desembarco de mi marca en Instagram.

Por no hablar del sinfín de documentos, fotos, vídeos, código, juegos y sus partidas guardadas, pestañas abiertas, listas de tareas pendientes y demás contenido digital que tengo desperdigado por diferentes discos duros a lo largo de varios ordenadores.

El desorden digital empieza a tener un impacto muy presente en mi realidad diaria. Socorro.


# Rechazo de la llamada

¿Cuántas veces he intentado emprender? ¿Cuántas veces he empezado y me he cansado a los pocos días? ¿Por qué esta vez va a ser diferente? ¿Por qué merece la pena el esfuerzo?

Sé que soy un vago, muchas veces me lo han dicho. ¿O es al revés? Jaja no, soy la leche de vago, por eso lo automatizo todo.


# Encuentro con el mentor

Sin embargo, esta vez es distinto, y al mismo tiempo, puede que sea la última oportunidad. Ahora tengo a ChatGPT. ¡Viva el _vibe coding_!.


# Cruce del umbral

Entro en la nube de Hetzner y formateo mi servidor.


# Aliados

Al crear la nueva máquina con 2 CPUs, 4GB de RAM y 40GB de disco, he indicado una clave ssh para poder conectarme al servidor:

```shell
ceon@portatil:~$ ssh root@cctpidmdlcdtc.com
root@inductor:~# 
```

Antes de nada, voy a actualizar el sistema operativo:

```shell
root@inductor:~# apt update
root@inductor:~# apt dist-upgrade
```

Y creo un usuario sin contraseña para conectarme a la máquina, en vez de usar root:

```shell
root@inductor:~# useradd -m -s /bin/bash ceon
root@inductor:~# passwd -d ceon
passwd: password changed.
root@inductor:~# 
```

Le configuro la clave ssh que ya estoy usando para root:

```shell
root@inductor:~# mv .ssh/ /home/ceon/
root@inductor:~# chown -R ceon:ceon /home/ceon/.ssh
root@inductor:~# 
```

Le doy permisos de sudo para poder entrar como root:

```shell
root@inductor:~# cat /etc/sudoers.d/ceon
ceon ALL=(ALL) NOPASSWD:ALL
root@inductor:~# 
```

Y bloqueo por completo el acceso como root en _/etc/ssh/sshd_config_:

```shell
PermitRootLogin no
```

Como he quitado la clave ssh de root, en estos momentos ya no se puede hacer login por ssh usando root. Por eso, aprovechando la shell de root aún abierta en el servidor, me aseguro de que se puede hacer sudo desde el usuario recién creado:

```shell
root@inductor:~# su - ceon
$ sudo su -
root@inductor:~# 
logout
$ 
root@inductor:~# 
```

Hay que comprobar también que se puede entrar por ssh con el nuevo usuario: 

```shell
ceon@portatil:~$ ssh ceon@cctpidmdlcdtc.com
```

Si todo funciona como se espera, ya se puede reiniciar el servidor ssh:

```shell
root@inductor:~# systemctl restart sshd
```

Y aunque las trazas de este tipo no van a parar, estaré más tranquilo sabiendo que es imposible entrar como root de manera remota:

```shell
root@inductor:~# journalctl -x | grep ": authentication failure;" | tail
Jun 13 16:32:35 inductor sshd[5534]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=180.101.88.200  user=root
Jun 13 16:33:52 inductor sshd[5575]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=92.55.190.215  user=root
Jun 13 16:34:03 inductor sshd[5577]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=180.101.88.200  user=root
Jun 13 16:35:33 inductor sshd[5579]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=180.101.88.200  user=root
Jun 13 16:37:07 inductor sshd[5583]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=180.101.88.200  user=root
Jun 13 16:37:41 inductor sshd[5591]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=188.127.180.208  user=root
Jun 13 16:38:30 inductor sshd[5683]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=180.101.88.200  user=root
Jun 13 16:40:05 inductor sshd[5691]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=180.101.88.200  user=root
Jun 13 16:40:10 inductor sshd[5693]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=80.94.95.81  user=root
Jun 13 16:41:14 inductor sshd[5707]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=180.101.88.196  user=root
root@inductor:~# 
```






