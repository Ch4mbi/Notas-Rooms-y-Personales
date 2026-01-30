# XDR: Introduction

https://tryhackme.com/room/xdrintroduction

XDR: Extended Detection and Response

El XDR es una “solución” que unifica, añade y analiza datos de una organización en una única plataforma(incluyendo datos de endpoints, email, redes, identidades, entornos cloud,...). Está diseñado para el antes y el después de brechas de seguridad o escenarios similares. Integra:

- Prevención
- Detección
- Investigación 
- Respuestas 

XDR sirve para asistir a equipos de seguridad a prevenir e identificar riesgos aprovechando datos de otras plataformas de seguridad o apps de seguridad de Microsoft.
El XDR de Microsoft se diferencia de otros XDR en que los otros XDR a pesar de abarcar un rango más grande, necesitan ser configurados previamente. El de Microsoft se hizo con la intención de que lo usaran organizaciones con productos de Microsoft.

A diferencia de otras herramientas de seguridad, XDR puede ayudar con:
- Detección de amenazas
- Investigación
- Respuesta ante incidentes
- Tiempo de respuesta ante incidentes
- Amplia visión de incidentes de seguridad
- Visibilidad de amenazas
- Investigaciones fuentes
- Threat hunting

Desde una misma consola central.

# Diferencias XDR(Extended detection and response) y EDR(Endpoint detection and response)

## XDR

- Alcance/rango: Va más allá de los endpoint. Incluye email, identidades, redes, servicios cloud,...

- Fuente de datos: Añade y administra los datos en diferentes capas de seguridad y plataformas

- Visibilidad: Da visibilidad completa a la infraestructura

- Detección: Detecta diferentes amenazas en base a patrones de comportamiento y análisis

- Respuesta: Automatiza respuestas en varias capas de seguridad, como aislar sistemas atacados,bloquear correos, quitar accesos,...

- Casos de uso: Se usa en detección de amenazas, respuesta ante phishing, ataques a la cadena de suministro y riesgos a la misma, ataques a la nube,...

- Administración: Unifica una misma consola para la detección de amenazas, investigaciones, y respuestas ante incidentes

## EDR

- Alcance/rango: Se centra normalmente en portátiles, servidores, ordenadores de sobremesa,... Es decir, solo en dispositivos endpoint(Conectados a una red)

- Fuente de datos: Ya que se centra en dispositivos de una red, recoge datos solo de ellos

- Visibilidad: Se limita a actividades de cada dispositivo conectado 

- Detección: Poca/limitada detección de amenazas, basadas en comportamientos anómalos del endpoint

- Respuesta: Ya que hay poca detección, la respuesta ante las amenazas detectadas también será pobre

- Casos de uso: Se usa en la detección de malware, respuesta ante ransomwares, o amenazas internas en el endpoint

- Administración: Mayoritariamente requiere herramientas de seguridad orientadas al endpoint

Ya que el XDR de Microsoft está basado en cloud, combina datos de ciberataques con identidades, endpoints, emails, apps cloud,... en una sola plataforma. Usa la automatización de tareas y la IA para responder y prevenir amenazas automáticamente, frenando diversos ataques. El XDR de microsoft puede acceder al listado completo de amenazas y efectos de estos “productos”:

- Microsoft Defender (Endpoint)
Tiene EDR(Endpoint detection and response) integrado, junto con antivirus, y gestión de vulnerabilidades. Guarda señales de la red de la organizacion y las manda al xdr de microsoft, el cual devuelve protección para los dispositivos

- Microsoft Defender (Office 365)
Protege a las organizaciones de amenazas relacionadas con correos electrónicos ,links o herramientas ”grupales”. Transmite señales de dichas amenazas al XDR. El EOP(Exchange online protection(Protección del intercambio online) está presente e integrado para asegurar protección frente a emails y sus respectivos adjuntos(links, documentos, imágenes,...)

- Microsoft Defender (Identidad) 
Almacena señales de servicios de dominio de directorios activos(Active directory domain services(AD DS)), servicios de “federación” de directorios activos(Active directory federation services(AD FS)), servicios de certificación de directorios activos(Active directory certificate  services(AD CS)),... Estas señales aseguran un entorno híbrido, incluyendo defensas frente a atacantes que intentan explotar cuentas comprometidas para lograr el movimiento lateral

- Microsoft Defender (Apps cloud) 
Capta señales de los servicios en la nube que usa la organización, asegurando la transmisión de información segura entre la infraestructura IT y dichos servicios cloud

- Microsoft Defender (Administración de vulnerabilidades)
- Microsoft Defender (La nube)
- Microsoft ID Protection 
Evalúa riesgos de eventos de muchos intentos casi simultáneos de ,por ejemplo, logins. Microsoft entra ID , en base a las políticas establecidas de acceso, evalúa automáticamente los comportamientos para dar acceso o no a dichos logins. Entra ID actúa independientemente del XDR pero comparte señales con él.

# Cómo funciona el XDR de microsoft defender

-  Recolección de datos e integración: El defender de XDR reúne y consolida datos de diferentes fuentes, como endpoints, identidad, correos, y apps cloud. Dichos datos que le llegan se analizan en tiempo real,  permitiendo la priorización de los riesgos a equipos de seguridad
- Detección de amenazas y análisis: El defender de xdr también lleva a cabo analiticas , ejecuta modelos de machine learning, y usa también la ciberinteligencia frente a amenazas para detectar amenazas emergentes, indicadores de compromiso, notificaciones raras del sistema ya sean comportamientos del usuario extraños o alertas del sistema
- Respuesta ante incidentes automatizada: También, el cdr de defensa permite una respuesta más inmediata frente a ataques, conteniendolos y remediandolos. También evita el movimiento lateral o la ejecución de ransomware
- Caza de amenazas: Con xdr, los equipos de seguridad pueden usar kql para encontrar amenazas usando una sola interfaz unificada
- Organización:  XDR se puede integrar con herramientas como sentinel para organizarse mejor a la hora de sufrir ataques.

# Ataque y como XDR lo defendería

Un ataque normalmente iría:
1.Email faudulento/phising
2.El usuario abre el adjunto/link del correo
3.Se instala el malware
4.El atacante obtiene datos de la víctima como su identidad
5.Usa dichos datos de su víctima para poder moverse lateralmente
6.El atacante consigue exfiltrar datos

En el xdr, en la página de ajustes, se puede configurar:
- Notificaciones de correo electrónico: Permite a los equipos de seguridad establecer notificaciones email para incidentes, acciones de respuesta y reportes de análisis de amenazas
- Alertas de ajustes de servicios:  Permite administrar que alertas aparecerán en la sección de alertas
- Permisos/roles: Se pueden configurar ,de manera centralizada, permisos o accesos
- API en directo/streaming: Los equipos de seguridad pueden analizar eventos de azure(microsoft) event hub en base a visualización de servicios, motores de procesamiento de datos, soluciones SIEM de terceros,...
- Administración de normas: Permite crear normas para diferentes secciones y decidir qué acciones se aplicarán sobre ellos
- Ajuste de alertas: Permite a los equipos de seguridad administrar las alertas con anticipación
- Administración de activos críticos: Se puede configurar para gestionar como de críticos son los activos de la organización
- Respuesta automatizada de identidad: Se usa para quitar a usuarios de acciones de respuestas automáticas
