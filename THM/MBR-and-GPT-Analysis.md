# MBR and GPT Analysis

https://tryhackme.com/room/mbrandgptanalysis

Los datos en un disco duro se almacenan en formato de 0 y 1, la cual es la manera de que el ordenador lo entienda. Si los datos no están bien organizados,(0 y 1) arruinan el ordenador o dispositivo. Para organizarlos se dividen los archivos en particiones en las que cada una almacena datos diferentes. Por ejemplo en windows normalmente es C, y si hay otra partición D o E
Existen esquemas de partición que sirven como para para el ordenador para entender las propias particiones del disco:
- MBR(Master Boot Record)
- GPT(GUID Partition Table)
Ambos esquemas son diferentes respecto a las propiedades e infraestructura de los mismos

El proceso de arranque(boot process) enciende todo el sistema siguiendo estos pasos en orden:
1. Dar energía al sistema
2. Revisar que funciona correctamente
3. Localizar el dispositivo de arranque
4. Leer y seguir el MBR o GPT
5. Cargar el gestor de arranque
6. Cargar el sistema operativo para que el usuario pueda interactuar 

## Analizar MBR
Actualmente, MBR ha sido reemplazado por GPT, pero se sigue usando en diversos OS.
Para analizar el MBR se pueden usar herramientas como HxD, la cual es un editor hexadecimal. Se debe abrir el archivo MBR para poder analizarlo en la app.  Al abrir mbr, lo abrirá en un formato hexadecimal

- Ocupa 512 bytes de espacio en todo primer sector del disco
- Cada hexadecimal de 2 dígitos juntos es 1 byte, y una vez que se terminen los 512 bytes, terminará el esquema
- La firma de MBR es 55 AA al final ,marcando el final del código mbr
- 16 columnas x 32 filas
- 4 es el número máximo de particiones MBR

Hay 3 porciones en el código hexadecimal:
- La de la izquierda es el desplazamiento de bytes
- La central representa los bytes hexadecimales
- La de la derecha representa los bytes hexadecimales convirtiéndolos en ASCII(American Standard Code for Information Interchange)

### Estructura MBR

#### Código de arranque(446 bytes(0-445))

Es el primer componente del MBR, cubriendo 446 bytes de los 512 que ocupa. Es la zona central. Esta sección almacena el bootloader inicial cuya tarea es encontrar la partición correcta de la tabla de particiones del MBR

##### Tabla de particiones(64 bytes(446-509))

Es el segundo componente del MBR. Contiene detalles de las particiones presentes en el disco en formato MBR. El bootloader encuentra la primera partición de la tabla de particiones , la booteable, para después cargar desde ella el segundo bootloader, siendo este segundo el responsable de cargar el kernel. 
El mbr contiene en total:
- Partición 1(16 bytes)
- Partición 2(16 bytes)
- Partición 3(16 bytes)
- Partición 4(16 bytes)

La partición puede resultar útil para análisis forenses ya que los hexadecimales de las particiones pueden aportar mucha información en base a la posición y al “contenido”:
1. Boot indicator(Indicador de arranque)
Indica si la particion es arrancable/booteable. El indicador de boot es:
- 80: Indica que si es booteable
- 00: Indica que la partición no es booteable
2. Inicio de CHS(Cylinder Head Section) 
Indican desde donde inicia la partición en el disco, aportando:
- Cilindro
- Cabeza
- Número de sector
3. Tipo de partición
Indica el sistema de archivos de la partición(NTFS,FAT32,...). El byte indica el tipo de sistema de archivos, y cada sistema de archivos tiene su propio byte identificador, por ejemplo, 07 es de NTFS
4. Fin de CHS
Los 3 últimos bytes al final de la dirección CHS la ubicación física en la que la partición termina
5. Inicio dirección del bloque lógico(LBA)
Indica el comienzo de la partición, la dirección en la que comienza y también la física. Da más claramente la dirección lógica antes que la física. En MBR, una referencia es el comienzo 00 80 00 00
6. Número de sectores
Los últimos 4 bytes de la partición indican el número de sectores en la partición 

| Posición  | Longitud | Nombre del campo | 
|-----------|-----------|-----------|
| 0 | 1 | Indicador de arranque(Comienzo) |
|-----------|-----------|-----------|
| 1-3 | 3 | CHS (Método (antiguo) de localización de discos duros)|
|-----------|-----------|-----------|
| 4 | 1 | Tipo de partición |
|-----------|-----------|-----------|
| 5-7 | 3 | Fin de CHS |
|-----------|-----------|-----------|
| 8-11 | 4 | Dirección del bloque lógico(LBA Address) |
|-----------|-----------|-----------|
| 12-15 | 4 | Número de sectores|
|-----------|-----------|-----------|

##### Localizar la partición

Hay que buscar la parte de la partición que contenga 00 80 00 00. Están en un formato que el byte menos importante va primero, y el más, el último, teniendo que revertirlos(00 00 08 00). Después, hay que convertirlos a formato decimal, pudiendo usar herramientas online y/o HxD por ejemplo. En HxD se seleccionan los bytes y al lado derecho, en la fila de Int32 aparece el formato decimal(2048). Luego, el número de bytes hay que multiplicarlo por el tamaño del sector (512 bytes en total), lo que daria 1048576 siendo ese el valor real en la que la partición se encuentra, el cual se puede buscar directamente en HxD, lo que llevará al inicio de la partición

##### Tamaño de la partición

Para calcular el tamaño de la partición, hay que fijarse en los últimos 4 bytes. En el MBR, son 00 B0 23 03 , y se deben volver a dar la vuelta , 03 23 B0 00 ya que mantienen la “regla” de orden-importancia mencionada. En HxD se puede hallar el valor decimal de esa sección. Una vez hallado, se debe multiplicar por 512(el tamaño) otra vez para sacar el tamaño de la partición

Aclaración: La regla consiste no solo en ordenar como tal. La “inversión” consiste en:
- 1. El fragmento de bytes se divide en grupos de 4 bytes y de 2 bytes y 8 bytes. La división en grupos consiste
- 2. Se invierten los primeros 4 bytes
- 3. Se mantienen los 2 bytes en su lugar
- 4. Se mantienen los ultimos 8 bytes en su lugar
- 5. Se combina todo
Por ejemplo, si se tiene E3 C9 E3 16 0B 5C 4D B8 81 7D F9 2D F0 02 15 AE , se divide asi: E3 C9 E3 16 / 0B 5C / 4D B8 81 7D F9 2D F0 02 15 AE. Los 4 primeros bytes se invierten: 16 E3 C9 E3. Después , se deja 0B 5C igual , al igual que 4D B8 81 7D F9 2D F0 02 15 AE. Después se junta todo.
En formato GUID, los grupos de bytes van asi: 8-4-4-8
#### Firma MBR(2 bytes)
Es relativamente pequeño, pero su alteración tiene un gran impacto. En MBR es la ultima sección 55 AA, indican que el MBR ha terminado. Existen malware que pueden alterarlos para corromper todo
### Malware de MBR
A pesar de que MBR ha sido reemplazado por GPT, se sigue usando en algunos discos. MBR toma lugar para arrancar el OS, siendo esa la razón para que los atacante lo tengan como objetivo ya que si alteran los 512 bytes de alguna manera, pueden comprometer todo el sistema. Algunos de los malware usados son:
- Bootkits
Este tipo de malware se integra en el esquema MBR para pasar los mecanismos de protección del mecanismo, logrando así una técnica de persistencia en el sistema. Incluso si se formatea el dispositivo, el malware seguirá ahí
- Ransomware
En lugar de encriptar los archivos “visibles” del sistema, los atacantes usan ransomware para modificar el MBR e interrumpir el proceso de arranque y mostrar sus “exigencias” en pantalla. Algunos ejemplos son Petya o Bad Rabbit
- Malware que formatea/limpia(Wiper malware)
Este malware corrompe el MBR para que sea imposible arrancar el OS ya que cualquier cambio en el esquema da lugar a diversos fallos. 

## Analizar GPT
Se cambió MBR por GPT por limitaciones que tenía el propio esquema de MBR(MBR solo soporta 2 TB y 4 particiones mientras que GPT soporta 9 ZB y 128 particiones)
GPT es compatible con sistemas UEFI(Unified Extensible Firmware Interface)
1. Protective MBR
2. Encabezado primario de GPT
3. Array de entrada de partición
4. Backup del encabezado primario de GPT
5. Backup del array de entrada de partición
### Protective MBR
Los discos con GPT están particionados, y el sistema usa UEFI para administrar dichas particiones, aunque algunos sistemas siguen usando BIOS con GPT. Hay que tener en cuenta que:
- BIOS esta diseñado para trabajar con MBR
- UEFI esta diseñado para trabajar con GPT
El uso de “protective MBR” es para indicar a la BIOS que el sistema está usando GPT. Se lo indica en el esquema en 3 secciones:
- Código bootloader
Siendo el de GPT diferente al de MBR, no lleva a cabo ninguna función durante el proceso de arranque. Solo está presente para parecer que es el mismo “estándar” que el mbr. A pesar de que solo está de “decoración”, a menudo varía por temas de compatibilidad
- Tabla de particiones
- Firma MBR
La firma es la misma que el estandar MBR ,55 AA, que marca el final de protective MBR


### Encabezado GPT Primario

El encabezado primario GPT empieza después del fin de protective MBR(55 AA). Todos los bytes en el encabezado GPT tienen propósito. Ocupa 92 bytes de los 512 que tiene. Después de esos 92 bytes, serían todos 00 para hacer relleno para completar el sector en el que están.

| Posicion de los bytes | Longitud de los bytes | Nombre del campo |
|-----------|-----------|-----------|
| 0-7 | 8 | Firma |
|-----------|-----------|-----------|
| 8-11 | 4 | Revisión |
|-----------|-----------|-----------|
| 12-15 | 4 | Tamaño del encabezado|
|-----------|-----------|-----------|
| 16-19 | 4 | CRC32 del encabezado |
|-----------|-----------|-----------|
| 20-23 | 4 | Dado la vuelta |
|-----------|-----------|-----------|
| 24-31 | 8 | LBA actual |
|-----------|-----------|-----------|
| 32-39 | 8 | Backup del LBA |
|-----------|-----------|-----------|
| 40-47 | 8 | Primer LBA usable  |
|-----------|-----------|-----------|
| 48-55 | 8 | Ultimo LBA usable |
|-----------|-----------|-----------|
| 56-71 | 16 | GUID del disco |
|-----------|-----------|-----------|
| 72-79 | 8 | Array de entrada a la partición LBA |
|-----------|-----------|-----------|
| 80-83 | 4 | Numero de entradas a la partición |
|-----------|-----------|-----------|
| 84-87 | 4 | Tamaño de cada entrada a la partición |
|-----------|-----------|-----------|
| 88-91 | 4 | CRC32 del array de partición |
|-----------|-----------|-----------|

- Firma
Reconoce al campo como un encabezado GPT ,estando siempre al principio(45 46 49 20 50 41 52 54)
- Revisión
Representa la versión del gpt, siendo normalmente 00 00 01 00
- Tamaño del encabezado
Representa el tamaño del encabezado, normalmente siendo 5C 00 00 00, que convertido a decimal, son 92 bytes
- CRC32 del encabezado
Checksum del encabezado  CRC32, si cambia indica que se ha corrompido el gpt
- LBA actual 
Indica la localización del encabezado gpt. Se puede obtener convirtiendo los bytes a decimal(01 00 00 00 00 00 00)
- Backup del LBA
Indica el backup del LBA del gpt
- Primer LBA usable
Esta dirección Lba indica donde comienza la partición en el disco
- Ultimo LBA usable
Indica la ultima dirección en la que la partición se puede usar
- GUID del disco
Distingue el disco de otros discos que se conecten al sistema. En gpt son 16 bytes
- Array de entrada a la partición LBA
Esta dirección lba indica el inicio del array de entrada a la partición
- Numero de entradas a la partición
Indica el numero de particiones en el disco, siendo así que gpt puede tolerar hasta 128 particiones. El indicador es 80 00 00 00 que a decimal es 128
- Tamaño de cada entrada a la partición
Indica el tamaño de cada partición de las 128. No es el tamaño de la partición ,es el tamaño de la entrada a la partición
- CRC32 del array de partición
Es el checksum de todo el array de entrada a la partición

### Array de entrada a la partición

Los arrays de entrada a la partición almacenan información de cada una de las 128 particiones que tiene gpt. Los bytes de cada partición indican diferentes características:
| Posicion de los bytes | Tamaño de los bytes | Campo |
|-----------|-----------|-----------|
| 0-15 | 16 | Tipo de partición |
|-----------|-----------|-----------|
| 16-31 | 16 | GUID único de partición |
|-----------|-----------|-----------|
| 32-39 | 8 | Inicio LBA |
|-----------|-----------|-----------|
| 40-47 | 8 | Fin LBA |
|-----------|-----------|-----------|
| 48-55 | 8 | Atributos |
|-----------|-----------|-----------|
| 56-127  | 72 | Nombre de la partición |
|-----------|-----------|-----------|

- GUID único de la partición
Se usa para distinguir particiones, siendo único en cada una. Para pasarlo a decimal se le debe aplicar el estándar internacional para pasarlo a GUID
- Inicio de LBA
Indica el área en el que inicia dicha partición
- Fin LBA
Indica el área en la que la partición termina
- Atributos
Campo en el que se almacenan características de la partición
- Nombre de la partición

### Encabezado Backup de gpt

Simplemente es una de las razones por las que GPT reemplazó a MBR, los backups, por si hay secciones corruptas. En MBR era casi imposible recuperar los bytes corruptos. Contiene la misma información que el encabezado GPT

### Backup del array de entrada a la partición

Al igual que el encabezado de la entrada a la partición, existe otro backup para el array de entrada a la misma, conteniendo la misma información actual que el array de entrada a la partición original

### Malware GPT
- Bootkits
El GPT tiene el bootloader dentro del sistema de partición EFI(esp) y es manejado por uefi. Los atacantes pueden tener como objetivo el propio archivo .efi y reemplazarlos o alterarlos con su propio código malicioso o bootkits. Esto les permite saltarse el nivel de seguridad de inicio del sistema ya que ya están funcionando antes de que el sistema arranque totalmente
- Ransomware
Los atacantes usan ransomware para alterar el proceso de arranque, aunque es relativamente más difícil que con MBR ya que los backups lo prevén. En caso de que se consiga, alteran el proceso de arranque
- Wiper malware
Existen malwares que alteran el binario del backup y del original haciéndolo imposible de recuperar, aparte de que también pueden estar orientados al .efi
