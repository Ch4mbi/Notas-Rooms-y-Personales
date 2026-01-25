# Network Traffic Basics
https://tryhackme.com/room/networktrafficbasics

## Intro-Logs
Hoy en dia atacan a las redes de diferentes maneras, y analistas soc responsables de analizar tráfico sospechoso entrante a una red, se deben fijar en varias características de los logs:
- Query y tipo de query
- Revisión de dominios de los atacantes (Por ejemplo, usar Virustotal)
- Identificar el sistema que está “provocando” los logs via IP
- Identificar también si la ip del atacante está registrada en virus total
- Construir una línea de tiempo para sacar deducciones

Los logs DNS no muestran más información que esa, por lo que es complicado sacar mas información de ahí. Aun asi, se debe revisar el tráfico dns y las respuestas de dicho tráfico.

El tráfico de red se debe analizar por:
- Monitorear la red
- Encargarse de comportamiento anormal en la red/firewall
- Inspeccionar comunicaciones/conexiones sospechosas interna y externamente
- Detectar actividad sospechosa
- Reconstruir ataques en la fase de respuesta ante incidentes
- Verificar o validar alertas

## Grupos de red

### Categorías
Una red de una empresa normalmente tiene dos grupos/categorías de fuentes:

- Intermediarios  
  Dispositivos por los que el tráfico pasa, siendo el tráfico que generan dichos dispositivos menos al del endpoint. Aquí entran:
  - Firewalls
  - Switches
  - Proxies
  - IDS
  - IPS
  - Routers
  - Controladores LAN  
  El tráfico que estos dispositivos generan viene de servicios como protocolos de enrutamiento(EIGRP,BGP,...), controles de administración(PING), protocolos de logging(SYSLOG),...

- Endpoint  
  Dispositivos en los que el tráfico se origina y termina. Los dispositivos que entran en esta categoría son:
  - servidores
  - hosts
  - dispositivos iot
  - VM
  - Teléfonos/tablets,...

### Flujos
- Norte-sur  
  Tráfico que entra en la LAN pasando el firewall. Los servicios conocidos que entran en esta categoría son:
  - HTTPS
  - DNS
  - SSH
  - VPN
  - RDP  
  Cada uno de estos protocolos se clasifica en:
  - Entrada
  - Salida

- Este-oeste  
  Tráfico que se queda en la LAN. Un atacante normalmente intentará explotar diferentes servicios internos para lograr el movimiento lateral en la red. Hay varios servicios que entran en esta categoría:
  - Directorios, autenticación, y servicios de identidad
    - Queries de autenticación para activar el directorio
    - Controles de acceso de red
  - Compartimiento de archivos
    - Accesos a unidades de red(CIFS)
  - Router,switching, y servicios de infraestructura
    - Tráfico DHCP entre host y servidor
    - DNS internos
    - Mensajes de protocolos de enrutamiento
  - Comunicación con la aplicación
    - Conexiones SQL por TCP
    - Microservicios de las APIs
  - Backups y réplicas
    - Copias de archivos, entre centros de datos y servidores backup
    - Copias de bases de datos
  - Monitorización y administración
    - SNMP(Dispositivos de métricas de salud)
    - Syslog→ Logging centralizado

## Información obtenible de una red
En una red, se pueden obtener diferentes tipos de información:

- Logs  
  Primer vistazo de la adquisición de información de lo que pasa por la red. Cada sistema y protocolo genera logs. No hay un sistema universal en el que se asignen los loggins, como microsoft con windows event logs. Aun así, los que suministran dicho servicio no aportan todos los logs, solo aportan los que consideran o se consideran relevantes(IP de destino y salida)

- Captura de paquetes  
  Para capturar e inspeccionar paquetes, se tienen 2 opciones:
  - Network tap → Dispositivo físico que uno pone en su red. Crea copias de todo el tráfico de red, las cuales se mueren posteriormente a sistemas que usan el monitoreo de puertos, sin afectar al rendimiento de la misma.
  - Duplicado de puertos → Software que copia paquetes de un puerto en un dispositivo a otro conectado.  

  A la hora de llevar a cabo una captura completa de paquetes, se debe tener en cuenta:
  - Hay que posicionar a los programas/dispositivos correctamente dependiendo de los objetivos
  - Una captura de paquetes completa necesita mucho espacio, necesitando mucho del mismo
  - Los dispositivos físicos normalmente ofrecen menos problemas a la eficacia de la red, el duplicado puede afectar al rendimiento de la misma  

  Para analizar paquetes se usan herramientas como:
  - Wireshark
  - TCPdump
  - Snort

- Estadísticas de la red  
  Otra manera de encontrar anomalías en una red es reuniendo metadatos del tráfico, como contar el número de solicitudes DNS que un usuario manda. Los protocolos que facilitan dicha información son:
  - Netflow → Protocolo desarrollado por cisco que almacena metadatos del tráfico que fluye a través de la red, siendo así útil para detectar comunicaciones malas(C2(Servidor de atacantes–Sistema con malware)), exfiltración de datos, movimiento lateral,...
  - IPFIX → Conocido como el protocolo de exportación de información de flujo de internet(Internet Protocol Flow Information Export Protocol). Netflow fue creado solo para sistemas cisco. El IPFIX fue creado por el IETF,  y publicado como un estándar, siendo similar a netflow, pero más flexible.  

  Para implementar cualquiera de los dos no se necesita construir una nueva infraestructura desde cero ya que la mayoría de proveedores implementan dichos servicios de primeras. Solo se debe configurar el protocolo para que se suban los metadatos a un sitio determinado.
