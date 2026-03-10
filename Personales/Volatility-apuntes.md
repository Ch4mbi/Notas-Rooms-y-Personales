# Comandos basicos Volatility
Lista de comabdos básicos útiles para el uso de volatility
- windows.info
- linux.info
- windows.pslist: Enumera procesos activos de la lista “doblemente linkeada” que almacena información de procesos en la memoria(Como el administrador de tareas). La salida/respuesta de esta entrada serán todos los procesos en ejecución y terminados y sus horas
- windows.pstree: Lista todos los procesos en base a su PID(Parent ID), usando los mismos metoos que pslist
- windows.psscan: Tecnica de listado que detecta procesos encontrando estructuras de datos que concuerdan con _EPROCESS. Aunque puede dar falsos positivos a veces
- windows.handles: Se usa para examinar detalles e identificadores de archivos y threads de un usuario/host
- windows.netstat: Intenta identificar todas las estructuras de memoria que tengan una conexión a la red
- windows.netscan: Se pueden adquirir tomas de red de la memoria. Recuperará conexiones abiertas y cerradas de TCP/UDP, IDs de procesos relacionados, puertos locales y remotos e IPs
- windows.dlllist: Lista toda las DLLs asociadas con procesos al momento de la extracción
- windows.ssdt: Se usa para detectar cambios en el SSDT ya que el kernel de windows lo usa para “resolver” las direcciones de las llamadas al sistema, algo que los rootkits de adversarios usan para redirigir las llamadas legítimas a las contrapartes maliciosas
- windows.modules: Lista drivers y módulos kernel de windows que se hayan cargado en la memoria(Incluyendo ruta de acceso, tamaño, y otros datos de cada archivo relacionado). Aunque puede pasar por encima de aquellos que están ocultos o sin ruta de acceso
- windows.driverscan: A diferencia de windows.modules, el cual se puede saltar drivers/módulos ocultos, .driverscan escanea la memoria raw buscando estructuras driver que podrían haberse desvinculado de las listas originales

Por ejemplo, en un comando de linux: python3 vol.py -f “ruta del archivo” windows.pstree , en windows se debe aplicar de manera "similar" pero con la ruta de archivos propia de windows
