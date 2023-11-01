 de seguridad
Dia de la semana (Lunes a Domingo) en el que se realiza la copia de seguridad

El resto es el comando para comprimir el archivo que se divide en:
sudo : realizar la acción con permisos de administrador grupo sudoers
tar: llama a la herramienta para comprimir 
-c crea un archivo y sobrescribe el anterior
-p mantiene los permisos originales 
-z descomprime 
-f para nombrar al archivo
ruta donde se creará el archivo
ruta de los archivos de los que se desea crear la copia de seguridad.

# Actividad 7

En esta actividad, vamos a configurar y automatizar la copia de seguridad en un entorno Linux de una estructura de directorios, considerando que la copia se realizará en otro equipo Linux/Windows (host remoto).

Para llevar a cabo la copia de seguridad de directorios en un equipo remoto, debemos seguir los siguientes pasos:

1. Asegúrate de que en el equipo de origen (local) tengas instalados el programa `rsync` y un servicio `ssh`. Verifica que existe una conexión entre las dos máquinas y asegúrate de haber pasado la clave pública de la máquina que realiza las copias al equipo remoto.

2. A continuación, crea un script que llame al programa `rsync`, especificando la ubicación de los directorios que deseas copiar y el directorio donde deseas almacenar la copia de seguridad.

DirectorioLocal=/ruta/local
DirectorioRemoto=user@IpServidor:/ruta/remota
rsync -avz --delete $DirectorioLocal $DirectorioRemoto

3. Para programar la copia de seguridad de forma automática, utiliza el comando `crontab -e`. En este archivo, programaremos cuándo deseamos realizar la copia de seguridad y añadiremos un enlace al script que hemos creado previamente.

Por ejemplo, para ejecutar el script todos los días a las 20:00, puedes añadir la siguiente línea en el archivo `crontab`:

0 20 * * * /home/usuario/backup_script.sh

Asegúrate de ajustar la hora y la ubicación del script según tus necesidades. Con estos pasos, habrás configurado y automatizado la copia de seguridad de directorios en un entorno Linux hacia un equipo remoto.

Recuerda mantener la seguridad de tus claves y configuraciones de SSH para proteger tus datos durante la transferencia.

# Actividad 9

En esta actividad, vamos a analizar, implementar y configurar la herramienta rsync en un entorno Linux.

Rsync es una herramienta de sincronización que permite copiar archivos en Linux desde otro equipo. La mayoría de los equipos Linux ya tienen instalado rsync por defecto, pero para asegurarnos, podemos instalarlo usando el siguiente comando:

sudo apt install rsync

Esta herramienta se puede utilizar con la siguiente sintaxis:

rsync opciones origen destino


Rsync ofrece varias opciones, algunas de las más comunes son:

- `-e` que especifica el protocolo con el que se accederá a la máquina para realizar las copias.
- `--delete` que elimina los archivos que no están presentes en el origen.

Puedes configurar y utilizar rsync para copiar y sincronizar archivos en un entorno Linux según tus necesidades. Esta herramienta es muy útil para mantener actualizada la información entre diferentes sistemas.


# Actividad 10

En esta actividad, vamos a configurar y automatizar la copia de seguridad en un entorno Linux de una estructura de directorios utilizando el comando `dd` y el servicio `crond`. Consideraremos que la copia se realiza en el mismo equipo y configuraremos distintos programas de copia con sus momentos correspondientes.

Para realizar una copia de seguridad utilizando `dd`, primero debemos crear un script en el que definiremos el directorio de origen y el lugar donde queremos almacenar la copia de seguridad, además de ejecutar el comando `dd`. Aquí está un ejemplo de cómo hacerlo:

DirectorioOrigen=/ruta/local
DirectorioRespaldo=/ruta/Respaldo
dd if="$DirectorioOrigen" of="$DirectorioRespaldo" bs=4M

En este script, `bs=4M` refleja el tamaño de bloques en los que se enviará la información.

Una vez que hayas creado el script, ejecutaremos el programa `cron` para programar la copia de seguridad de forma automática. Utiliza el comando `crontab -e` y en la última línea del archivo, añade los parámetros para determinar cada cuánto tiempo se realizará la copia de seguridad y la ruta del script. Por ejemplo, para ejecutar el script cada día a la medianoche, puedes agregar la siguiente línea en el archivo `crontab`:

0 0 * * * /ruta/al/script_de_copia.sh

Asegúrate de ajustar la hora y la ubicación del script según tus necesidades. Con estos pasos, habrás configurado y automatizado la copia de seguridad de directorios en un entorno Linux utilizando el comando `dd` y el servicio `crond`.

# Actividad 11

En esta actividad, abordaremos la implantación de AWS Backup, el servicio de copias de seguridad de Amazon Web Services.

1. Lo primero que debemos hacer en nuestra cuenta de Amazon es suscribirnos al servicio de AWS Backup.

2. Las copias de seguridad se pueden generar bajo demanda o programadas.

   - Para crear copias bajo demanda, debemos seleccionar los Recursos protegidos y, a continuación, Crear una copia de seguridad bajo demanda. En esta ubicación, elegiremos los IDs de los recursos a los cuales se les realizará la copia de seguridad. También podemos decidir el período de caducidad de la copia de seguridad.

   - Una vez hecho esto, podremos crear un almacén para guardar nuestras copias de seguridad. Debemos elegir un Rol IAM para generar las copias. Finalmente, seleccionamos "Create on-demand backup", lo que nos llevará a la ventana de trabajos donde, dependiendo del recurso del que se va a realizar la copia de seguridad, necesitaremos hacer uso de diferentes herramientas.

3. Para crear copias de seguridad programadas, accedemos a "Administrar planes de Backup". Aquí, elegimos una plantilla según la frecuencia con la que queramos realizar la copia y nombramos nuestro plan de backup. En el menú de resumen, seleccionamos "editar" y elegimos las reglas de respaldo que consideremos oportunas. Luego, elegimos el almacén al que aplicaremos estas reglas y guardamos las reglas de backup.

4. Para añadir recursos a este plan de seguridad, debemos dirigirnos al "asignador de recursos" y asignar un rol, que generalmente es el predeterminado. A continuación, incluimos los recursos que deseamos respaldar.

AWS Backup es una herramienta poderosa para gestionar y automatizar las copias de seguridad en el entorno de Amazon Web Services, proporcionando flexibilidad tanto para copias bajo demanda como para copias programadas.

# Actividad 12

En esta actividad, configuraremos y automatizaremos la copia de seguridad en un entorno Linux de una estructura de directorios utilizando el comando `duplicity` y el servicio `crond`. Consideraremos que la copia se realiza en el mismo equipo.

Para realizar copias de seguridad con `duplicity`, primero debemos definir el directorio del cual se realizará la copia de seguridad y el lugar donde se almacenará. Usaremos el siguiente comando:

duplicity --encrypt-key "ClavePublicaGpg" /directoria/a/copiar file:///directorio/donde/se/almacena

Este comando se introduce en un script que, al ejecutarlo, creará una copia de seguridad de un directorio en el directorio que le hayamos asignado.

Para automatizar esta función, utilizaremos `crontab`. Añadiremos los parámetros que determinan cada cuánto tiempo queremos realizar la copia de seguridad junto con el script que se iniciará. Por ejemplo, para ejecutar el script cada día a la medianoche, puedes agregar la siguiente línea en el archivo `crontab`:

0 0 * * * /ruta/al/script_de_copia.sh

Asegúrate de ajustar la hora y la ubicación del script según tus necesidades. Con estos pasos, habrás configurado y automatizado la copia de seguridad de directorios en un entorno Linux utilizando el comando `duplicity` y el servicio `crond`.

# Actividad 18

En esta actividad, explicaremos y ejercitaremos las opciones más importantes de la herramienta de clonación Acronis True Image. Consideraremos que la clonación se realiza en el mismo equipo o en otro equipo. También realizaremos la restauración para comprobar el proceso realizado.

Paso 1: Descargar el programa Acronis True Image desde su página oficial:

https://www.acronis.com/es-es/homecomputing/thanks/home-office/

Paso 2: Una vez iniciado el programa, nos mostrará un tutorial con todas las opciones que ofrece. Entre las más destacadas se encuentran:

- Clonar discos
- Generar medios de rescate
- Acceso a Parallels
- Limpieza del sistema
- Acronis DriveCleanser
- Acronis Cloud Backup Download
- Otras opciones

Paso 3: Procedemos a crear unos directorios en el disco duro.

Paso 4: Creamos una copia de seguridad en un directorio del propio equipo, en la sección "Copias de seguridad".

Paso 5: Formateamos el disco duro.

Paso 6: Procedemos a restaurar los archivos utilizando la sección "Restaurar" del programa.

Acronis True Image es una herramienta versátil para la clonación, generación de copias de seguridad y restauración de datos, lo que permite mantener la integridad y disponibilidad de la información en equipos tanto locales como remotos.

# Actividad 20

Para realizar la instalación de Ubuntu Server en un RAID 1, siga estos pasos:

Paso 1: Configure su máquina virtual y agregue 2 discos duros que le permitirán crear el RAID.

Paso 2: Inicie la instalación del sistema operativo Ubuntu Server de manera normal. Durante el proceso de instalación, llegará a la configuración del almacenamiento.

Paso 3: En la configuración del almacenamiento, elija la opción personalizada (custom).

Paso 4: En esta sección, debe agregar el arranque a uno de los discos duros.

Paso 5: Seleccione la opción "Create software RAID", donde podrá elegir los dos discos que conformarán el RAID.

Paso 6: Complete la instalación siguiendo los pasos adicionales según sus necesidades.

Paso 7: Una vez finalizada la instalación, puede comprobar el correcto funcionamiento del RAID utilizando el comando `lsblk`.

Con estos pasos, habrá realizado la instalación de Ubuntu Server en un RAID 1. El RAID 1 proporciona redundancia de datos al mantener una copia idéntica de los datos en ambos discos, lo que mejora la fiabilidad y la disponibilidad de sus datos.

# Práctica 21: Instalación y puesta en marcha de un sistema de almacenamiento compartido NAS con Openfiler

Para instalar y configurar un sistema de almacenamiento compartido NAS con Openfiler, siga estos pasos:

Paso 1: Descargue Openfiler desde su página web principal: https://www.openfiler.com/community/download.

Paso 2: Una vez descargado, proceda a la instalación de Openfiler en VirtualBox. Configure la red como "puente" y agregue un nuevo disco duro a la máquina virtual.

Paso 3: Complete la instalación y configure el servidor Openfiler mediante el cliente web. Acceda con el usuario: "openfiler" y la contraseña: "password".

Paso 4: Habilite una dirección para compartir su disco en la sección "System".

Paso 5: Cree un volumen en la sección "Volumes" con el tamaño que desee compartir.

Paso 6: Para acceder a este volumen desde un cliente, configure el iniciador iSCSI. En el apartado "Detection", seleccione "Detect Portal" e ingrese la IP de la máquina con Openfiler instalado.

Paso 7: Una vez se haya detectado el dispositivo, vaya al apartado "General", seleccione el destino detectado y haga clic en "Conectar".

Paso 8: Una vez completado el proceso, diríjase al administrador de discos, donde aparecerá una nueva unidad.

Con estos pasos, habrá instalado y puesto en marcha un sistema de almacenamiento compartido NAS utilizando Openfiler. Este sistema permitirá compartir recursos de almacenamiento de manera eficiente entre múltiples clientes.


# Práctica 22: Instalación y puesta en marcha de un sistema de almacenamiento compartido SAN con Openfiler

Para instalar y configurar un sistema de almacenamiento compartido SAN con Openfiler, siga estos pasos:

Paso 1: Descargue Openfiler desde su página web principal: [https://www.openfiler.com/community/download](https://www.openfiler.com/community/download).

Paso 2: Una vez descargado, proceda a la instalación de Openfiler en VirtualBox. Configure la red como "puente" y agregue un nuevo disco duro a la máquina virtual.

Paso 3: Complete la instalación y configure el servidor Openfiler mediante el cliente web. Acceda con el usuario: "openfiler" y la contraseña: "password".

Paso 4: Para compartir un recurso desde Openfiler, cree un nuevo volumen.

Paso 5: Una vez que el volumen esté creado, acceda a la sección "Shares" donde creará un directorio que desea compartir.

Paso 6: Configure los permisos del directorio compartido. En este caso, se pueden dejar como "públicos" para facilitar el acceso.

Paso 7: Una vez que el recurso esté listo, acceda a su máquina cliente y monte el directorio compartido en la ubicación que elija.

Con estos pasos, habrá instalado y puesto en marcha un sistema de almacenamiento compartido SAN utilizando Openfiler. Esto le permitirá compartir recursos de almacenamiento de manera eficiente entre múltiples clientes.

# Práctica final: Instalación y puesta en marcha de un cluster de dos nodos virtualizados utilizando Proxmox

Para realizar esta práctica, siga estos pasos:

Paso 1: Instale un sistema Debian previamente a la instalación de Proxmox para que Proxmox funcione correctamente.

Paso 2: Descargue Proxmox desde la web oficial: [https://www.proxmox.com/en/downloads/proxmox-virtual-environment/iso/proxmox-ve-8-0-iso-installer](https://www.proxmox.com/en/downloads/proxmox-virtual-environment/iso/proxmox-ve-8-0-iso-installer).

Paso 3: Una vez instalado el sistema operativo, configure la información de contacto, como el correo electrónico y la contraseña.

Paso 4: Configure la dirección IP de su máquina para que esté en la red. Repita este proceso en todas las máquinas que formarán el cluster.

Paso 5: Acceda a su host y conéctese mediante el cliente web al primer nodo. En el apartado "Cluster", seleccione "Crear".

Paso 6: Cuando se crea el cluster, aparecerá una nueva sección con un código para unirse al cluster. Copie ese código.

Paso 7: Acceda vía web a la segunda máquina. En la sección del cluster, seleccione "Unirse" (Join) y aparecerá una ventana en la que deberá introducir el código del cluster y la contraseña del usuario que lo creó.

Paso 8: Refresque las páginas y verá cómo ambas máquinas son ahora nodos que pertenecen al mismo cluster.

Con estos pasos, habrá instalado y puesto en marcha un cluster de dos nodos virtualizados utilizando Proxmox. Esto le permitirá administrar y desplegar máquinas virtuales de manera eficiente en un entorno de cluster.


