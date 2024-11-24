# Configuramos el hostname

```bash
vim /etc/hosts
cat /etc/hosts
```

```
127.0.0.1	    localhost
127.0.1.1   	ldap.phctxhfc.unam.mx ldap
.
.
```

```bash
hostnamectl set-hostname ldap.phctxhfc.unam.mx
hostname -d
```

```
phctxhfc.unam.mx
```

# Instalacion

```bash
apt update
apt install slapd ldap-utils -y
```


## Configuracion de ldap

```bash
dpkg-reconfigure -plow slapd
ldapsearch -x -LLL -s base -b "" namingContexts
```

```
dn:
namingContexts: dc=phctxhfc,dc=unam,dc=mx
```


# Habilitamos TLS/SSL

## Creamos llave privada para CA
```bash
apt install gnutls-bin ssl-cert
certtool --generate-privkey --bits 4096 --outfile /etc/ssl/private/mycakey.pem
vim /etc/ssl/ca.info
cat /etc/ssl/ca.info
```

```
cn = phctxhfc.unam.mx
ca
cert_signing_key
expiration_days = 3650
```

## Creamos certificado autofirmado y lo agregamos a los CAs de confianza
```bash
certtool --generate-self-signed \
--load-privkey /etc/ssl/private/mycakey.pem \
--template /etc/ssl/ca.info \
--outfile /usr/local/share/ca-certificates/mycacert.crt
```

```bash
update-ca-certificates
```

## Creamos llave privada para el servidor

```bash
certtool --generate-privkey \
--bits 4096 \
--outfile /etc/ldap/ldap.phctxhfc.unam.mx_slapd_key.pem
```

```bash
vim /etc/ssl/ldap.phctxhfc.unam.mx.info
cat /etc/ssl/ldap.phctxhfc.unam.mx.info
```

```bash
organization = phctxhfc.unam.mx
cn = ldap.phctxhfc.unam.mx
tls_www_server
encryption_key
signing_key
expiration_days = 365
```

## Creamos el certificado del servidor

```bash
certtool --generate-certificate \
--load-privkey /etc/ldap/ldap.phctxhfc.unam.mx_slapd_key.pem \
--load-ca-certificate /etc/ssl/certs/mycacert.pem \
--load-ca-privkey /etc/ssl/private/mycakey.pem \
--template /etc/ssl/ldap.phctxhfc.unam.mx.info \
--outfile /etc/ldap/ldap.phctxhfc.unam.mx_slapd_cert.pem
```

```bash
chgrp openldap /etc/ldap/ldap.phctxhfc.unam.mx_slapd_key.pem
chmod 0640 /etc/ldap/ldap.phctxhfc.unam.mx_slapd_key.pem
```

## Aceptamos la configuracion para TLS

```bash
vim certinfo.ldif
```

```bash
dn: cn=config
add: olcTLSCACertificateFile
olcTLSCACertificateFile: /etc/ssl/certs/mycacert.pem
-
add: olcTLSCertificateFile
olcTLSCertificateFile: /etc/ldap/ldap.phctxhfc.unam.mx_slapd_cert.pem
-
add: olcTLSCertificateKeyFile
olcTLSCertificateKeyFile: /etc/ldap/ldap.phctxhfc.unam.mx_slapd_key.pem
```

```bash
ldapmodify -Y EXTERNAL -H ldapi:/// -f certinfo.ldif
```

```bash
vim /etc/default/slapd
```

```bash
SLAPD_SERVICES="ldap:/// ldapi:/// ldaps:///"
```

```bash
systemctl restart slapd
```

```bash
ldapwhoami -x -ZZ -H ldap://ldap.phctxhfc.unam.mx
ldapwhoami -x -H ldaps://ldap.phctxhfc.unam.mx
```

## Ahora configuramos un cliente para poder conectarse 

### Dentro del cliente que usara el servidor ldap

```bash
sudo vim /etc/ldap/ldap.conf
```

Agregamos al final esta linea pues confiamos en el certificado
autofirmado que nosotros generamos

```
TLS_REQCERT never
```

```bash
ldapwhoami -x -ZZ -H ldap://<IP_del_servidor_ldap>
ldapwhoami -x -H ldaps://<IP_del_servidor_ldap>
```

---------------------------------------------------------------------------------

# Poblamos el directorio

```bash
vim add_groups.ldif
```

```
dn: ou=Users,dc=phctxhfc,dc=unam,dc=mx
objectClass: organizationalUnit
ou: Users

dn: ou=Groups,dc=phctxhfc,dc=unam,dc=mx
objectClass: organizationalUnit
ou: Groups

dn: cn=sftp,ou=Groups,dc=phctxhfc,dc=unam,dc=mx
objectClass: posixGroup
cn: sftp
gidNumber: 10000

dn: cn=hosting,ou=Groups,dc=phctxhfc,dc=unam,dc=mx
objectClass: posixGroup
cn: hosting
gidNumber: 10001
```

```bash
ldapadd -x -D "cn=admin,dc=phctxhfc,dc=unam,dc=mx" -W -f add_groups.ldif
```

```bash
vim add_user.ldif
```

```
dn: uid=testuser,ou=Users,dc=phctxhfc,dc=unam,dc=mx
objectClass: inetOrgPerson
objectClass: posixAccount
objectClass: shadowAccount
uid: testuser
sn: noob 
givenName: testuser
cn: testuser noob
displayName: testuser noob
uidNumber: 10000
gidNumber: 10001
userPassword: {SSHA}JGjZy07p+q2UnIHbVsBJF0onUADFMYEN
gecos: testuser noob
loginShell: /bin/bash
homeDirectory: /home/testuser
```

```bash
ldapadd -x -D "cn=admin,dc=phctxhfc,dc=unam,dc=mx" -W -f add_user.ldif
```

### Pruebas de consulta desde servidor ldap

```bash
ldapsearch -x -LLL -b dc=phctxhfc,dc=unam,dc=mx '(uid=testuser)' cn gidNumber
```

```
dn: uid=testuser,ou=Users,dc=phctxhfc,dc=unam,dc=mx
cn: testuser noob
gidNumber: 10001
```

## Pruebas desde el servidor ldap

```bash
ldapsearch -x -H ldap:/// -b "dc=phctxhfc,dc=unam,dc=mx" "(objectClass=*)"
ldapsearch -x -ZZ -H ldap:/// -b "dc=phctxhfc,dc=unam,dc=mx" "(objectClass=*)"
ldapsearch -x -H ldaps:/// -b "dc=phctxhfc,dc=unam,dc=mx" "(objectClass=*)"
```

## Pruebas desde el cliente que consulta ldap

```bash
ldapsearch -x -H ldap://<servidor_ip_o_hostname> -b "dc=phctxhfc,dc=unam,dc=mx" "(objectClass=*)"
ldapsearch -x -ZZ -H ldap://<servidor_ip_o_hostname> -b "dc=phctxhfc,dc=unam,dc=mx" "(objectClass=*)"
ldapsearch -x -H ldaps://<servidor_ip_o_hostname> -b "dc=phctxhfc,dc=unam,dc=mx" "(objectClass=*)"
```

```
# extended LDIF
#
# LDAPv3
# base <dc=phctxhfc,dc=unam,dc=mx> with scope subtree
# filter: (objectClass=*)
# requesting: ALL
#

# phctxhfc.unam.mx
dn: dc=phctxhfc,dc=unam,dc=mx
objectClass: top
objectClass: dcObject
objectClass: organization
o: phctxhfc.unam.mx
dc: phctxhfc

# Users, phctxhfc.unam.mx
dn: ou=Users,dc=phctxhfc,dc=unam,dc=mx
objectClass: organizationalUnit
ou: Users

# Groups, phctxhfc.unam.mx
dn: ou=Groups,dc=phctxhfc,dc=unam,dc=mx
objectClass: organizationalUnit
ou: Groups

# sftp, Groups, phctxhfc.unam.mx
dn: cn=sftp,ou=Groups,dc=phctxhfc,dc=unam,dc=mx
objectClass: posixGroup
cn: sftp
gidNumber: 10000

# hosting, Groups, phctxhfc.unam.mx
dn: cn=hosting,ou=Groups,dc=phctxhfc,dc=unam,dc=mx
objectClass: posixGroup
cn: hosting
gidNumber: 10001

# testuser, Users, phctxhfc.unam.mx
dn: uid=testuser,ou=Users,dc=phctxhfc,dc=unam,dc=mx
objectClass: inetOrgPerson
objectClass: posixAccount
objectClass: shadowAccount
uid: testuser
sn:: bm9vYiA=
givenName: testuser
cn: testuser noob
displayName: testuser noob
uidNumber: 10000
gidNumber: 10001
gecos: testuser noob
loginShell: /bin/bash
homeDirectory: /home/testuser

# search result
search: 2
result: 0 Success

# numResponses: 7
# numEntries: 6
```

# Mejorando la seguridad

## Deshabilitamos las conexiones anonimas

```bash
vim ldap_disable_bind_anon.ldif
```

agregamos

```
dn: cn=config
changetype: modify
add: olcDisallows
olcDisallows: bind_anon

dn: cn=config
changetype: modify
add: olcRequires
olcRequires: authc

dn: olcDatabase={-1}frontend,cn=config
changetype: modify
add: olcRequires
olcRequires: authc
```

Ejecutamos el siguiente comando con privilegios

```bash
ldapadd -Y EXTERNAL -H ldapi:/// -f ldap_disable_bind_anon.ldif
```

Ahora el comando:

```bash
ldapsearch -x -ZZ -H ldap:/// -b "dc=phctxhfc,dc=unam,dc=mx" "(objectClass=*)"
```
muestra
```
ldap_bind: Inappropriate authentication (48)
	additional info: anonymous bind disallowed
```

Ahora se debe ejecutar siempre con una autenticacion

```bash
vim add_readonly_user.ldif
```

```
dn: cn=readonly,dc=phctxhfc,dc=unam,dc=mx
objectClass: inetOrgPerson
cn: readonly
sn: readonly
givenName: readonlyuser
displayName: readonlyuser readonly
userPassword: {SSHA}q7SNPEOpo9GNgFjyTfeq09XKBfQNYMIR
```

```bash
ldapadd -D "cn=admin,dc=phctxhfc,dc=unam,dc=mx" -W -f add_readonly_user.ldif
```

# Configuracion de acceso

```bash
vim access_config.ldif
```

```
dn: olcDatabase={1}mdb,cn=config
changetype: modify
replace: olcAccess
olcAccess: {0}to attrs=userPassword by self write by anonymous auth by * none
olcAccess: {1}to attrs=shadowLastChange by self write by * read
olcAccess: {2}to * by dn="cn=admin,dc=phctxhfc,dc=unam,dc=mx" read by dn="cn=readonly,dc=phctxhfc,dc=unam,dc=mx" read by * none
```

```bash
ldapmodify -Y EXTERNAL -H ldapi:/// -f reglas.ldif
```

Ahora solo pueden leer el admin y readonly

# Configuracion con apache

```bash
sudo apt update
sudo apt install apache2 apache2-utils
sudo a2enmod ldap
sudo a2enmod authnz_ldap
sudo a2enmod headers
sudo systemctl restart apache2
cd /etc/apache2/sites-available
a2dissite 000-default.conf
```

en otro archivo


```bash
vim ldap-auth.conf
```

```
LDAPVerifyServerCert Off
<VirtualHost *:80>
    # Otros parámetros de configuración...


    # Activar autenticación LDAP para toda la página
    #LDAPTrustedGlobalCert CA_BASE64 "/dev/null"
    <Location />
        AuthType Basic
        AuthName "LDAP Authentication"
        AuthBasicProvider ldap
        AuthLDAPURL "ldap://192.168.1.134/dc=phctxhfc,dc=unam,dc=mx?uid?sub?(objectClass=person)" TLS
        AuthLDAPBindDN "cn=admin,dc=phctxhfc,dc=unam,dc=mx"
        AuthLDAPBindPassword "estoy cansado jefe 0117"
        Require valid-user

	# Evitar que las credenciales se guarden en la cache del navegador
    	Header always set Cache-Control "no-store"
    	Header always set Pragma "no-cache"
    	Header always set Expires 0
	Header always set Authentication-Info "required"
    </Location>
</VirtualHost>
```

```bash
a2ensite ldap-auth.conf
sudo systemctl restart apache2
```

