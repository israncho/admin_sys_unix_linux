# Administracion de sistemas Unix/Linux 2025-1
# Practica 05 - Compilar un kernel

### Jesus Israel Gutierrez Elizalde

El compilar el kernel es un proceso pesado
por lo que le daremos mas recursos a la maquina virtual
donde realizaremos esto.


![performance_00](img/performance_00.png)

![performance_01](img/performance_01.png)


Seleccionamos una vm con suficientes recursos 


![seleccionVM_00](img/seleccionVM_00.png)

Instalamos todo lo necesario para poder compilar el kernel

![req_00](img/req_00.png)

Vemos el kernel actual, vemos el espacio disponible y por
ultimo descargamos otra version del kernel.

En mi caso use `curl ` porque por alguna razon `wget`
se quedaba esperando sin descargar nada.

![kernel_00](img/kernel_00.png)

Lo descomprimimos

![kernel_01](img/kernel_01.png)

![kernel_02](img/kernel_02.png)

Copiamos la configuracion que ya estaba en uso
y ejecutamos 

```
make menuconfig
```

![kernel_03](img/kernel_03.png)

Seleccionamos `load`
![kernel_04](img/kernel_04.png)

![kernel_05](img/kernel_05.png)

Ahora seleccionamos `save`

![kernel_06](img/kernel_06.png)

![kernel_07](img/kernel_07.png)

![kernel_08](img/kernel_08.png)

Compilamos el kernel ejecutanto

```
make -j $(nproc)
```

![kernel_09](img/kernel_09.png)


![kernel_10](img/kernel_10.png)

Luego compilamos los modulos necesarios
que fueron seleccionados en la configuracion.

![kernel_11](img/kernel_11.png)

Preparamos nuestro kernel para usarlo

![kernel_12](img/kernel_12.png)

Hubo un error pues nos quedamos sin espacio en `/var`
![kernel_13](img/kernel_13.png)

![kernel_14](img/kernel_14.png)

Le di mas espacio a `/var`, pero al parecer
tambien hay poco espacio en `/boot`, por lo que
ahora agrandare este

![kernel_15](img/kernel_15.png)

Hubo muchos problemas con el espacio por lo 
que se decidio usar otra virtual con mas espacio.

Se retoma la evidencia para la practica desde de donde
se dejo pero ejecutandose desde otra vm.

![kernel_16](img/kernel_16.png)

Despues de actualizar el grub, reiniciamos y verificamos el cambio de
kernel.

![kernel_17](img/kernel_17.png)
