# SEGURIDAD Y ALTA DISPONIBILIDAD

## Metasploiteable 3 Ubuntu

Para comenzar a ver las vulnerabilidades del sistema, se procede a realizar un escaneo de puertos mediante la herramienta nmap en la máquina objetivo utilizando el siguiente comando:

nmap 172.28.128.3/24


El escaneo revela que la máquina tiene los siguientes puertos abiertos:

- 21/tcp ftp
- 22/tcp ssh
- 80/tcp http
- 445/tcp microsoft-ds
- 631/tcp ipp
- 3306/tcp mysql
- 8080/tcp http-proxy

También se puede usar la herramienta Legión para obtener información sobre la versión de los protocolos que la máquina atacada está utilizando.

Al explorar la web de la máquina, se observa que tiene diferentes carpetas que conducen a diferentes sitios web con formularios. Esto sugiere la posibilidad de llevar a cabo una inyección SQL para recopilar información de estos formularios. Además, se encuentra una carpeta llamada "drupal", y tras realizar una búsqueda en Metasploit, se encuentran algunos exploits interesantes, aunque no arrojaron resultados satisfactorios.

Dado que los puertos SSH están abiertos, es posible llevar a cabo un ataque de fuerza bruta. Esto se puede hacer mediante herramientas como Hydra o las herramientas de Metasploit (msf6). Se realizó una prueba utilizando la base de datos de Metasploit, pero no se obtuvieron resultados exitosos. Por lo tanto, se descargaron dos diccionarios de Daniel Miessler, un experto en seguridad, llamados "top-username-shortlist" y "CommonAdmBase64". El segundo contiene contraseñas y usuarios que se separaron en dos archivos distintos.

Para llevar a cabo el ataque, se utilizó la herramienta de Metasploit `auxiliary/scanner/ssh/ssh_login` con los archivos de usuarios y contraseñas. La lista "CommonAdmBase64" no dio buenos resultados, por lo que se concatenaron las listas "top-username-shortlist" tanto para usuarios como para contraseñas. Esto resultó en una conexión SSH exitosa con las credenciales "vagrant:vagrant", lo que permitió el acceso al sistema objetivo y la exploración del directorio raíz.

Una vez que se logró el acceso exitoso, se repitió la misma operación, pero esta vez en el puerto FTP utilizando la herramienta `scanner/ftp/ftp_login`. Al igual que antes, se obtuvo éxito con las credenciales "vagrant:vagrant", lo que permitió el acceso al directorio raíz y la capacidad de transferir archivos.

## Metasploiteable 3 Windows

Después de los resultados exitosos en la máquina anterior, se realizó un escaneo de puertos en la máquina Windows con la esperanza de encontrar los puertos 21 y 22 para realizar el mismo tipo de ataque. El escaneo de puertos en la máquina Windows reveló los siguientes resultados:

- 21/tcp ftp
- 22/tcp ssh
- 80/tcp http
- 135/tcp msrpc
- 139/tcp netbios-ssn
- 445/tcp microsoft-ds
- 3306/tcp mysql
- 3389/tcp ms-wbt-server
- 4848/tcp appserv-http
- 7676/tcp imqbrokerd
- 8009/tcp ajp13
- 8080/tcp http-proxy
- 8181/tcp intermapper
- 8383/tcp m2mservices
- 9200/tcp wap-wsp
- 49152/tcp unknown
- 49153/tcp unknown
- 49154/tcp unknown
- 49175/tcp unknown
- 49176/tcp unknown

Dado que se encontraron los puertos 21 y 22, se realizó un ataque de fuerza bruta nuevamente utilizando la herramienta `auxiliary/scanner/ssh/ssh_login` con los diccionarios que habían tenido éxito en la máquina anterior. Este escaneo reveló un nuevo conjunto de credenciales de usuario y contraseña, específicamente "administrador:vagrant". Esto permitió nuevamente el acceso al sistema a través de SSH y la exploración del directorio raíz.

El mismo proceso se repitió para el puerto FTP utilizando la herramienta `scanner/ftp/ftp_login`, y nuevamente se obtuvo éxito con las credenciales "administrador:vagrant", lo que permitió el acceso al directorio raíz y la capacidad de transferir archivos.

En resumen, en ambos sistemas Metasploiteable 3 (Ubuntu y Windows), se identificaron vulnerabilidades en los puertos 21 y 22 que permitieron realizar ataques de fuerza bruta para obtener acceso a las máquinas. Estas vulnerabilidades representan riesgos significativos para la seguridad y la disponibilidad de los sistemas, lo que destaca la importancia de implementar medidas de seguridad adecuadas y parches para mitigar estas vulnerabilidades. Es importante recordar que estas actividades deben realizarse en un entorno controlado y con el permiso adecuado.



