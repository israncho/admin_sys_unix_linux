# Administracion de sistemas Unix/Linux 2025-1 
# Practica 01 - Instalación de VMware y Linux

**Nota:** Tuve demasiados problemas para instalar `VMware` en mi sistema.

```bash
->  ~ lsb_release -a
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 24.04.1 LTS
Release:	24.04
Codename:	noble
```

Y por cuestiones de timpo decidi usar `VirtualBox` que funciona bien para el curso.

De igual forma se presenta el proceso de instalacion de `VirtualBox`.


## Instalacion de virtualBox

```bash
sudo apt update
```

```bash
sudo apt install virtualbox
```


![Descarga de virtualbox](./img/DescargaVbox.png)

Luego de ejecutar el comando, la aplicacion
de virtual box se debe encontrar disponible.

![Vbox_instalado](./img/App_instalada.png)

Luego damos click a la aplicacion o presionamos enter.

![Vbox_iniciado](./img/App_iniciada.png)

**Nota:** Ya hay varias maquinas virtuales, pues para cuando
se realizo este reporte ya se habian trabajado en el curso.

Luego damos click en **New**

![Creando_VM_NoLVM_01](./img/CreacionVM_NoLVM_01.png)

Lo cual mostrara la siguiente ventana. Le damos nombre a la 
vm y tambien seleccionamos la ISO.

![Creando_VM_NoLVM_02](./img/CreacionVM_NoLVM_02.png)

Ahora, le asignamos recursos a la vm. 

![Creando_VM_NoLVM_03](./img/CreacionVM_NoLVM_03.png)

**Nota:** El usar la opcion `Enable EFI(special OSes only)`
dio problemas por lo que se realizo la creacion de otra vm
sin esta opcion.

![Creando_VM_NoLVM_04](./img/CreacionVM_NoLVM_04.png)

**Nota:** El usar la opcion `2GB`
dio problemas por lo que se realizo la creacion de otra vm
con `20GB`.

Confirmamos la configuracion de instalacion

![Creando_VM_NoLVM_05](./img/CreacionVM_NoLVM_05.png)

Seleccionamos la vm recien creada y hacemos click en
`start`

## Instalacion de Linux sin LVM y particiones ad-hoc

![Creando_VM_NoLVM_06](./img/CreacionVM_NoLVM_06.png)


Luego aparecera esta ventana con las opciones para
instalar `Debian12`

![Creando_VM_NoLVM_07](./img/CreacionVM_NoLVM_07.png)


Ahora seguimos las instrucciones para la instalacion.


![Creando_VM_NoLVM_08](./img/CreacionVM_NoLVM_08.png)

![Creando_VM_NoLVM_09](./img/CreacionVM_NoLVM_09.png)

![Creando_VM_NoLVM_10](./img/CreacionVM_NoLVM_10.png)

![Creando_VM_NoLVM_11](./img/CreacionVM_NoLVM_11.png)

![Creando_VM_NoLVM_12](./img/CreacionVM_NoLVM_12.png)

![Creando_VM_NoLVM_13](./img/CreacionVM_NoLVM_13.png)

![Creando_VM_NoLVM_14](./img/CreacionVM_NoLVM_14.png)

![Creando_VM_NoLVM_15](./img/CreacionVM_NoLVM_15.png)

![Creando_VM_NoLVM_16](./img/CreacionVM_NoLVM_16.png)

![Creando_VM_NoLVM_17](./img/CreacionVM_NoLVM_17.png)

![Creando_VM_NoLVM_18](./img/CreacionVM_NoLVM_18.png)

![Creando_VM_NoLVM_19](./img/CreacionVM_NoLVM_19.png)

![Creando_VM_NoLVM_20](./img/CreacionVM_NoLVM_20.png)

![Creando_VM_NoLVM_21](./img/CreacionVM_NoLVM_21.png)

![Creando_VM_NoLVM_22](./img/CreacionVM_NoLVM_22.png)

![Creando_VM_NoLVM_23](./img/CreacionVM_NoLVM_23.png)

![Creando_VM_NoLVM_24](./img/CreacionVM_NoLVM_24.png)

![Creando_VM_NoLVM_25](./img/CreacionVM_NoLVM_25.png)

![Creando_VM_NoLVM_26](./img/CreacionVM_NoLVM_26.png)

![Creando_VM_NoLVM_27](./img/CreacionVM_NoLVM_27.png)

![Creando_VM_NoLVM_28](./img/CreacionVM_NoLVM_28.png)

![Creando_VM_NoLVM_29](./img/CreacionVM_NoLVM_29.png)

![Creando_VM_NoLVM_30](./img/CreacionVM_NoLVM_30.png)

![Creando_VM_NoLVM_31](./img/CreacionVM_NoLVM_31.png)

![Creando_VM_NoLVM_32](./img/CreacionVM_NoLVM_32.png)

![Creando_VM_NoLVM_33](./img/CreacionVM_NoLVM_33.png)

![Creando_VM_NoLVM_34](./img/CreacionVM_NoLVM_34.png)

![Creando_VM_NoLVM_35](./img/CreacionVM_NoLVM_35.png)

![Creando_VM_NoLVM_36](./img/CreacionVM_NoLVM_36.png)

![Creando_VM_NoLVM_37](./img/CreacionVM_NoLVM_37.png)

![Creando_VM_NoLVM_38](./img/CreacionVM_NoLVM_38.png)

## Instalacion de Linux con LVM y particiones ad-hoc

La configuración de la máquina al momento de crearla es la misma.
La instalación también es la misma solo hasta esta parte:

![Creando_VM_LVM_01](./img/CreacionVM_LVM_01.png)

![Creando_VM_LVM_02](./img/CreacionVM_LVM_02.png)

![Creando_VM_LVM_03](./img/CreacionVM_LVM_03.png)

![Creando_VM_LVM_04](./img/CreacionVM_LVM_04.png)

![Creando_VM_LVM_05](./img/CreacionVM_LVM_05.png)

![Creando_VM_LVM_06](./img/CreacionVM_LVM_06.png)

![Creando_VM_LVM_07](./img/CreacionVM_LVM_07.png)

![Creando_VM_LVM_08](./img/CreacionVM_LVM_08.png)

![Creando_VM_LVM_09](./img/CreacionVM_LVM_09.png)

![Creando_VM_LVM_10](./img/CreacionVM_LVM_10.png)

![Creando_VM_LVM_11](./img/CreacionVM_LVM_11.png)

![Creando_VM_LVM_12](./img/CreacionVM_LVM_12.png)

![Creando_VM_LVM_13](./img/CreacionVM_LVM_13.png)

![Creando_VM_LVM_14](./img/CreacionVM_LVM_14.png)

![Creando_VM_LVM_15](./img/CreacionVM_LVM_15.png)

![Creando_VM_LVM_16](./img/CreacionVM_LVM_16.png)

![Creando_VM_LVM_17](./img/CreacionVM_LVM_17.png)
