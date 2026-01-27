# Hydra

https://tryhackme.com/room/hydra

# Introducción
Hydra es un “cracker” de contraseñas que usa la fuerza bruta para bypassear loggins, aunque hay que tener en cuenta que la fuerza bruta sirve en sitios no protegidos.
Acorde con el repositorio de hydra(https://github.com/vanhauser-thc/thc-hydra), puede atacar con fuerza bruta a estos protocolos:
Asterisk
AFP
Cisco AAA
Cisco auth
Cisco enable
CVS
Firebird
FTP
HTTP-FORM-GET
HTTP-FORM-POST
HTTP-GET
HTTP-HEAD
HTTP-POST
HTTP-PROXY
HTTPS-FORM-GET
HTTPS-FORM-POST
HTTPS-GET
HTTPS-HEAD
HTTPS-POST
HTTP-Proxy
ICQ
IMAP
IRC
LDAP
MEMCACHED
MONGODB
MS-SQL
MYSQL
NCP
NNTP
Oracle Listener
Oracle SID
Oracle
PC-Anywhere
PCNFS
POP3
POSTGRES
Radmin
RDP
Rexec
Rlogin
Rsh
RTSP
SAP/R3
SIP
SMB
SMTP
SMTP Enum
SNMP v1+v2+v3
SOCKS5
SSH (v1 y 2)
SSHKEY
Subversion
TeamSpeak 
Telnet
VMware-Auth
VNC
XMPP
https://en.kali.tools/?p=220

# Comandos básicos
Hay que tener en cuenta que en base a los protocolos que se pretendan atacar, los comandos deben cambiar conforme a ellos.
También hay que tener en cuenta que otros ataques de fuerza bruta de código directo, usan: 
- Las respuestas del servidor como indicadores de que un carácter de la contraseña o usuario está bien
- Un wordlist predefinido(Por ejemplo rocky.txt) 
## SSH
El comando a usar, en linux , que es la plataforma en la que normalmente se usa esta herramienta es:
hydra -l “nombre de usuario” -P rockyou.txt ftp://”Dirección a la que se quiere atacar”
El .txt no tiene por qué ser rockyou
-l : especifica el nombre de usuario para hacer el login
-P : indica la lista(.txt o similar) que se va a usar en el ataque de fuerza bruta
 -t : Establece el número de procesos a generar

## Post Web Form
También se puede usar hydra para hacer login en páginas web(poco seguras). El comando de ataque sería:
sudo hydra <username> <wordlist> machine_ip http-post-form"<path>:<login_credentials>:<invalid_response>"
-l: El nombre de usuario del login
-P: La lista de contraseñas a usar
http-post-form: Los métodos post son bastante usados
<path>: La url del login
<login_credentials>: Las credenciales nombre y contraseña de usuario, va en un formato “username=^USER^&password=^PASS^”
<invalid_response>: Parte de la respuesta cuando el login falla
-V: Salida detallada de cada intento
Siendo un ejemplo:
hydra -l <nombre de usuario> -P <wordlist> machne_ip http-post-form “/:username=^USER^&password=^PASS^:F=incorrect“ -V
El <nombre de usuario> es donde iría el campo en el que se inserta el usuario
El usuario especificado se reemplaza en ^User^
password es el campo donde se introduce la contraseña
La contraseña que le dé reemplazará el ^Pass^
F=incorrect es un string que aparecerá como respuesta del servidor cuando el login falla
