# Administracion de sistemas Unix/Linux 2025-1
# Practica 06

## Fail2ban

Realizamos la instalacion y configuracion de 

![fail2ban_01.png](img/fail2ban_01.png)

![fail2ban_02.png](img/fail2ban_02.png)

Editamos el archivo `jail.local`, estando en 
`/etc/fail2ban`

```bash
vim jail.local
```


![fail2ban_03.png](img/fail2ban_03.png)


Luego configuramos a fail2ban para que use
systemd para leer los registros del sistema 


![fail2ban_04.png](img/fail2ban_04.png)

![fail2ban_05.png](img/fail2ban_05.png)

Probamos banear y desbanear ips con fail2ban

![fail2ban_06.png](img/fail2ban_06.png)

## ClamAV
ClamAV es un antivirus de código abierto diseñado para detectar
virus, malware y otras amenazas en sistemas Linux

Instalamos ClamAV y su Daemon y ademas 
lo configuramos

![clamav_01](img/clamav_01.png)

![clamav_02](img/clamav_02.png)

![clamav_03](img/clamav_03.png)

![clamav_04](img/clamav_04.png)

![clamav_05](img/clamav_05.png)

![clamav_06](img/clamav_06.png)

![clamav_07](img/clamav_07.png)

![clamav_08](img/clamav_08.png)

![clamav_09](img/clamav_09.png)

![clamav_10](img/clamav_10.png)

![clamav_11](img/clamav_11.png)

![clamav_12](img/clamav_12.png)

![clamav_13](img/clamav_13.png)

![clamav_14](img/clamav_14.png)

![clamav_15](img/clamav_15.png)

![clamav_16](img/clamav_16.png)

![clamav_17](img/clamav_17.png)

![clamav_18](img/clamav_18.png)

![clamav_19](img/clamav_19.png)

![clamav_20](img/clamav_20.png)

![clamav_21](img/clamav_21.png)

![clamav_22](img/clamav_22.png)

![clamav_23](img/clamav_23.png)

Ahora verificamos el servicio freshclam

![clamav_24](img/clamav_24.png)

Actualizamos la base de datos de clamAV

![clamav_25](img/clamav_25.png)

Creamos un script `clamavscan.sh` en el directorio de
tareas diarias y lo probamos

![clamav_26](img/clamav_26.png)

![clamav_27](img/clamav_27.png)

## Postfix y Logwatch 

Postfix es un servidor de correo para enviar y recibir mensajes, y
Logwatch, una herramienta para generar y enviar informes de los registros del sistema.

Con estos dos en conjunto podemos recibir correos con
reportes del sistema.

Comenzamos con la instalacion

![postlog_01](img/postlog_01.png)

Y tambien realizamos la configuracion de postfix

![postlog_02](img/postlog_02.png)

![postlog_03](img/postlog_03.png)

![postlog_04](img/postlog_04.png)

![postlog_05](img/postlog_05.png)

![postlog_06](img/postlog_06.png)

![postlog_07](img/postlog_07.png)

![postlog_08](img/postlog_08.png)

![postlog_09](img/postlog_09.png)

![postlog_10](img/postlog_10.png)

Indicamos  a que direcciones deben
redirigirse los correos enviados a postmaster y root

![postlog_11](img/postlog_11.png)

Ahora, creamos el directorio cacheh para Logwatch y copiamos el archivo de
configuración principal de Logwatch (logwatch.conf) al directorio de configuración en `/etc`.

![postlog_12](img/postlog_12.png)

Abrimos el archivo logwatch.conf y cambiamos el método

de salida de los informes, para que ahora sea mail.

![postlog_13](img/postlog_13.png)


Probamos

![postlog_14](img/postlog_14.png)

