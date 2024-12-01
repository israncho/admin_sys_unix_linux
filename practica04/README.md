# Administracion de sistemas Unix/Linux 2025-1
# Practica 04 - Paquetes deb y Paginas de manual

### Jesus Israel Gutierrez Elizalde

## Creacion de nuestro paquet deb

Creamos nuestro propio paquete DEB usando  [`dpkg`](https://en.wikipedia.org/wiki/Dpkg),
el paquete es para realizar un respaldo del directorio `/home`
de un usuario dado, para crear el respaldo usamos [`tar`](https://en.wikipedia.org/wiki/Tar_(computing)),
a continuación mostramos todo el proceso de creación.

![paquete1](img/paquete1.png).

![paquete2](img/paquete2.png).

Ahora, para realizar bien todo el proceso de crear el manual,
ahora desinstalaremos el paquete que ya teniamos instalado

![paquete3](img/paquete_3.png).

Ahora creamos el manual para el paquete DEB

![paquete4](img/paquete_4.png).

![paquete5](img/paquete_5.png).

Volvemos a generar el .deb y lo instalamos

![paquete6](img/paquete_6.png).

Por ultimo para verificar ejecutamos

```bash
man mkbackup
```

![paquete7](img/paquete_7.png).
