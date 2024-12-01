# Administracion de sistemas Unix/Linux 2025-1 
# Practica 03 - Asegurando GRUB 

### Jesus Israel Gutierrez Elizalde


Agregamos la opcion `init=/bin/sh:` para que durante el arranque en lugar de iniciar
el sistema normalmente, cargue una shell /bin/sh.

![grub_00](img/grub_00.png).

Luego presionamos `ctrl+x` o `F10`, en mi caso lo que funciono fue `F10`

![grub_01](img/grub_01.png).

Ejecutamos `mount` para montar el sistema

![grub_02](img/grub_02.png).

Ejecutamos `mount -o remount,rw /` para volver a montar el sistema
pero ahora con permisos de lectura y escritura

![grub_03](img/grub_03.png).

Ahora cambiamos la contrase~na del root y reiniciamos el sistema

![grub_04](img/grub_04.png).

![grub_05](img/grub_05.png).

![grub_06](img/grub_06.png).

Como `root` ejecutamos

```
grub-mkpasswd-pbkdf2 | tee -a /etc/grub.d/40_custom
```

y luego terminamos de modificar `/etc/grub.d/40_custom`
con `vi`

![grub_07](img/grub_07.png).

![grub_08](img/grub_08.png).

Reiniciamos el sistema y vemos que en efecto ahora se pide contrase~na
para cualquier opcion de grub

![grub_09](img/grub_09.png).
