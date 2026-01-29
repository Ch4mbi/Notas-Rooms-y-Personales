#Modelo osi

Ejemplos de ataques por capas:
## 7.Aplicación
- SQL Injection: No muy común a dia de hoy
- Cross-Site Scripting: Inyección de javascript en páginas para robar cookies o llevar a cabo acciones
- Dos lento: Enviar y mantener muchas conexiones con el servidor para ralentizarlo en la medida de lo posible
## 6.Presentación
- SSL Stripping: Degradar la conexión para que pase de https a http
## 5.Sesion
- Apropiación de una sesión en curso para suplantar la identidad
- SSL/TLS renegotiation attacks- Explorar la renegociación de la sesión para llevar a cabo ataques de inyección o manipulación
- Desincronización con la sesión- Provocar la desincronización de partes de la sesión para generar vulnerabilidades
## 4.Transporte
- Port scanning: Descubrir puertos abiertos en el servidor/servicio
- Predicción de números de secuencia TCP, predecir numeros de secuencia tcp para inyectar paquetes en una conexión legítima
## 3.Red
- IP Spoofing- Falsificación de la ip de origen de los paquetes
- MITM
- Cross layer attacks- Ataques que aprovechan correlaciones entre capas para romper la seguridad DNS
## 2.Data link
- ARP Spoofing- Asociar la dirección mac del atacante con la ip de otro equipo para interceptar el tráfico
- Mac spoofing- Falsificar la dirección mac para hacerse pasar por otro dispositivo
- Inundar un switch con muchas mac para forzarlo a funcionar en modo flood
## 1.Físico
- Ataques con usb
- Conexiones con cables
