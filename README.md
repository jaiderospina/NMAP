# NMAP
NMAP

<p align="center">
    NMAP<br> 
    CIBERSEGURIDAD <br />
    <br />
</p>


# Identificación de Infraestructuras Tecnológicas con NMAP

Nmap (“mapeador de redes”) es una herramienta de código abierto para exploración de red y auditoría de seguridad. Se diseñó para analizar rápidamente grandes redes, aunque funciona muy bien contra equipos individuales. Nmap utiliza paquetes IP "crudos" en formas originales para determinar qué equipos se encuentran disponibles en una red, qué servicios (nombre y versión de la aplicación) ofrecen, qué sistemas operativos (y sus versiones) ejecutan, qué tipo de filtros de paquetes o cortafuegos se están utilizando así como docenas de otras características. 
Aunque generalmente se utiliza Nmap en auditorías de seguridad, muchos administradores de redes y sistemas lo encuentran útil para realizar tareas rutinarias, como puede ser el inventariado de la red, la planificación de actualización de servicios y la monitorización del tiempo que los equipos o servicios se mantiene activos.
Además de la tabla de puertos interesantes, Nmap puede dar información adicional sobre los objetivos, incluyendo el nombre de DNS según la resolución inversa de la IP, un listado de sistemas operativos posibles, los tipos de dispositivo, y direcciones MAC.


### Prerequisitos
Antes de comenzar con esta guía, necesitará lo siguiente:
* Familiaridad con el shell scripting
* Conceptos solidos sobre protocolos de transporte
* El sistema operativo Kali Linux virtualizado

## 1. Negociacion en conexiones TCP
Primero, se realiza una solicitud de tipo SYN desde el cliente a un puerto, se le responde RST si el puerto está cerrado o SYN-ACK si está abierto, luego se envia el ACK desde el cliente al servidor para completar el proceso. Nmap utiliza este tipo de mensajes para determinar si un puerto está escuchando o no en el destino.

## 2. Rango de puertos
Los puertos registrados están en el rango desde el 1024 al 49151. Los números de puerto del 49152 al 65535 son puertos dinámicos (privados), no usados por aplicaciones definidas. Esto quiere decir que con cada conexión de origen que nosotros realicemos, se utilizan estos puertos que son dinámicos. 

![Common Ports](https://i.redd.it/5vzo7a8odbu31.jpg)

## 3. Resumen de opciones
El resumen de opciones se imprime cuando Nmap se ejecuta sin argumentos, y la última versión siempre está disponible en https://svn.nmap.org/nmap/docs/nmap.usage.txt. Ayuda a las personas a recordar las opciones más comunes, pero no sustituye a la documentación detallada del resto de este manual. Algunas opciones oscuras ni siquiera se incluyen aquí.
```
$ nmap
```

```
Nmap 7.91 ( https://nmap.org )
Usage: nmap [Scan Type(s)] [Options] {target specification}
TARGET SPECIFICATION:
  Can pass hostnames, IP addresses, networks, etc.
  Ex: scanme.nmap.org, microsoft.com/24, 192.168.0.1; 10.0.0-255.1-254
  -iL <inputfilename>: Input from list of hosts/networks
  -iR <num hosts>: Choose random targets
  --exclude <host1[,host2][,host3],...>: Exclude hosts/networks
  --excludefile <exclude_file>: Exclude list from file
HOST DISCOVERY:
  -sL: List Scan - simply list targets to scan
  -sn: Ping Scan - disable port scan
  -Pn: Treat all hosts as online -- skip host discovery
  -PS/PA/PU/PY[portlist]: TCP SYN/ACK, UDP or SCTP discovery to given ports
  -PE/PP/PM: ICMP echo, timestamp, and netmask request discovery probes
  -PO[protocol list]: IP Protocol Ping
  -n/-R: Never do DNS resolution/Always resolve [default: sometimes]
  --dns-servers <serv1[,serv2],...>: Specify custom DNS servers
  --system-dns: Use OS's DNS resolver
  --traceroute: Trace hop path to each host
SCAN TECHNIQUES:
  -sS/sT/sA/sW/sM: TCP SYN/Connect()/ACK/Window/Maimon scans
  -sU: UDP Scan
  -sN/sF/sX: TCP Null, FIN, and Xmas scans
  --scanflags <flags>: Customize TCP scan flags
  -sI <zombie host[:probeport]>: Idle scan
  -sY/sZ: SCTP INIT/COOKIE-ECHO scans
  -sO: IP protocol scan
  -b <FTP relay host>: FTP bounce scan
PORT SPECIFICATION AND SCAN ORDER:
  -p <port ranges>: Only scan specified ports
    Ex: -p22; -p1-65535; -p U:53,111,137,T:21-25,80,139,8080,S:9
  --exclude-ports <port ranges>: Exclude the specified ports from scanning
  -F: Fast mode - Scan fewer ports than the default scan
  -r: Scan ports consecutively - don't randomize
  --top-ports <number>: Scan <number> most common ports
  --port-ratio <ratio>: Scan ports more common than <ratio>
SERVICE/VERSION DETECTION:
  -sV: Probe open ports to determine service/version info
  --version-intensity <level>: Set from 0 (light) to 9 (try all probes)
  --version-light: Limit to most likely probes (intensity 2)
  --version-all: Try every single probe (intensity 9)
  --version-trace: Show detailed version scan activity (for debugging)
SCRIPT SCAN:
  -sC: equivalent to --script=default
  --script=<Lua scripts>: <Lua scripts> is a comma separated list of
           directories, script-files or script-categories
  --script-args=<n1=v1,[n2=v2,...]>: provide arguments to scripts
  --script-args-file=filename: provide NSE script args in a file
  --script-trace: Show all data sent and received
  --script-updatedb: Update the script database.
  --script-help=<Lua scripts>: Show help about scripts.
           <Lua scripts> is a comma-separated list of script-files or
           script-categories.
OS DETECTION:
  -O: Enable OS detection
  --osscan-limit: Limit OS detection to promising targets
  --osscan-guess: Guess OS more aggressively
TIMING AND PERFORMANCE:
  Options which take <time> are in seconds, or append 'ms' (milliseconds),
  's' (seconds), 'm' (minutes), or 'h' (hours) to the value (e.g. 30m).
  -T<0-5>: Set timing template (higher is faster)
  --min-hostgroup/max-hostgroup <size>: Parallel host scan group sizes
  --min-parallelism/max-parallelism <numprobes>: Probe parallelization
  --min-rtt-timeout/max-rtt-timeout/initial-rtt-timeout <time>: Specifies
      probe round trip time.
  --max-retries <tries>: Caps number of port scan probe retransmissions.
  --host-timeout <time>: Give up on target after this long
  --scan-delay/--max-scan-delay <time>: Adjust delay between probes
  --min-rate <number>: Send packets no slower than <number> per second
  --max-rate <number>: Send packets no faster than <number> per second
FIREWALL/IDS EVASION AND SPOOFING:
  -f; --mtu <val>: fragment packets (optionally w/given MTU)
  -D <decoy1,decoy2[,ME],...>: Cloak a scan with decoys
  -S <IP_Address>: Spoof source address
  -e <iface>: Use specified interface
  -g/--source-port <portnum>: Use given port number
  --proxies <url1,[url2],...>: Relay connections through HTTP/SOCKS4 proxies
  --data <hex string>: Append a custom payload to sent packets
  --data-string <string>: Append a custom ASCII string to sent packets
  --data-length <num>: Append random data to sent packets
  --ip-options <options>: Send packets with specified ip options
  --ttl <val>: Set IP time-to-live field
  --spoof-mac <mac address/prefix/vendor name>: Spoof your MAC address
  --badsum: Send packets with a bogus TCP/UDP/SCTP checksum
OUTPUT:
  -oN/-oX/-oS/-oG <file>: Output scan in normal, XML, s|<rIpt kIddi3,
     and Grepable format, respectively, to the given filename.
  -oA <basename>: Output in the three major formats at once
  -v: Increase verbosity level (use -vv or more for greater effect)
  -d: Increase debugging level (use -dd or more for greater effect)
  --reason: Display the reason a port is in a particular state
  --open: Only show open (or possibly open) ports
  --packet-trace: Show all packets sent and received
  --iflist: Print host interfaces and routes (for debugging)
  --append-output: Append to rather than clobber specified output files
  --resume <filename>: Resume an aborted scan
  --stylesheet <path/URL>: XSL stylesheet to transform XML output to HTML
  --webxml: Reference stylesheet from Nmap.Org for more portable XML
  --no-stylesheet: Prevent associating of XSL stylesheet w/XML output
MISC:
  -6: Enable IPv6 scanning
  -A: Enable OS detection, version detection, script scanning, and traceroute
  --datadir <dirname>: Specify custom Nmap data file location
  --send-eth/--send-ip: Send using raw ethernet frames or IP packets
  --privileged: Assume that the user is fully privileged
  --unprivileged: Assume the user lacks raw socket privileges
  -V: Print version number
  -h: Print this help summary page.
EXAMPLES:
  nmap -v -A scanme.nmap.org
  nmap -v -sn 192.168.0.0/16 10.0.0.0/8
  nmap -v -iR 10000 -Pn -p 80
SEE THE MAN PAGE (https://nmap.org/book/man.html) FOR MORE OPTIONS AND EXAMPLES
```


 
## 4. Sintaxis general y primer escaneo

Sintaxis general:
```
$ nmap [  ...] [  ] { }
```

A continuación, procedemos a realizar el primer escaneo:
```
$ nmap 45.33.49.119
```

Como no estamos estableciendo ningún tipo de mapeo, utilizara por defecto TCP SYN. Nmap envía un SYN y asume que el puerto está abierto si recibe un SYN ACK. Como no hay ningún parámetro adicional y solo tiene una única IP, el resultado sera similar al siguiente:
```
Starting Nmap 7.91 ( https://nmap.org ) at 2021-10-17 18:55 EDT
Nmap scan report for ack.nmap.org (45.33.49.119)
Host is up (0.15s latency).
Not shown: 993 filtered ports
PORT      STATE  SERVICE
22/tcp    open   ssh
25/tcp    open   smtp
70/tcp    closed gopher
80/tcp    open   http
113/tcp   closed ident
443/tcp   open   https
31337/tcp closed Elite

Nmap done: 1 IP address (1 host up) scanned in 11.37 seconds
```

Esto ya es un primer punto de análisis, por ejemplo aunque el software que este detrás de servicio FTP, SSH, etc., estén totalmente actualizados y no haya vulnerabilidades conocidas, es posible lanzar un ataque de fuerza bruta con diccionario sobre el SSH o FTP intentando acceder ya que existen una gran cantidad de servidores de este tipo con credenciales por defecto o inseguras.

Actualmente los bots están constantemente escaneando amplios rangos de IPs buscando puertos abiertos reconocibles como las base de datos (MongoDB, MySQL, PostgreSQL, etc.), y cuando detectan un puerto así abierto, automáticamente intentan un login con credenciales por defecto.  Así se han hackeado una enorme cantidad de bases de datos sin intervención humana previa. Esto es viable incluso aunque lo tengamos abierto en otro puerto, dado que es posible identificar en muchos casos que lo que hay en el puerto 5555 es un mySQL a través del fingerprint del servicio.


## 5. Primer parámetro
Si necesitamos mas información sobre cómo ha obtenido nmap esta información, podemos utilizar el parámetro -v (verbose) o -vv, donde podemos ver que nmap ha estado lanzando comandos SYN y en algunos casos recibiendo RESET (puerto cerrado), en otros SYN-ACK (puerto abierto) y en otros ningún tipo de respuesta (“filtered”), lo cual nos puede hacer entender que un firewall está parando nuestra petición:

```
PORT    STATE  SERVICE REASON
22/tcp  open   ssh     syn-ack
25/tcp  open   smtp    syn-ack
70/tcp  closed gopher  conn-refused
80/tcp  open   http    syn-ack
113/tcp closed ident   conn-refused
443/tcp open   https   syn-ack
```

## 6. Escanear rangos de IPs
Nmap permite establecer rangos de IPs para escanear con diversos modos. Por ejemplo:
```
nmap 192.168.1.0/24 (subred completa)
nmap 192.168.1.1-20 (20 IPs)
nmap 192.168.1.*
nmap 192.168.1.1 192.168.1.2 192.168.1.3
```

Si tenemos un archivo con las distintas IPs separadas por tabuladores o saltos de línea (una IP o rango por línea), podemos utilizarlo con el parámetro -iL (input list) y realizar así el escaneo de todas las IPs que se encuentre en la lista. También permite excluir IPs en concreto los parametros con –exclude y –excludefile.

## 7. Definir puertos a escanear

Es posible definir manualmente los puertos que queremos escanear. Si buscamos servidores web en puertos 80, 443 y 8080 en una subred podríamos hacerlo con el parámetro _-p_:
```
nmap -p 80,443,8080 192.168.1.0/24
```

También podemos definir que nmap escanee los N (número entero) puertos más comunes; por ejemplo, para escanear los 10 puertos más comunes en un rango de IPs:
```
nmap --top-ports 10 192.168.1.0/24
```

Recibiendo una respuesta como esta:

```
PORT     STATE     SERVICE
21/tcp    closed   ftp
22/tcp    open     ssh
23/tcp    closed   telnet
25/tcp    closed   smtp
53/tcp    open     domain
80/tcp    open     http
110/tcp   closed   pop3
111/tcp   open     rpcbind
135/tcp   closed   msrpc
139/tcp   open     netbios-ssn
```
Si deseamos escanear todos los puertos TCP, UDP y SCTP (más lento), puedes utilizar el identificador -p- :
```
nmap -p- 192.168.1.0/24
```

## 8. Identificacion de sistemas operativos y servicios
Nmap nos permite detectar puertos que están habilitados en una IP o un rango. Además, nmap nos permite intentar identificar qué tecnología (producto, versión, etc.) hay detrás de un puerto abierto, incluso el sistema operativo con los parámetros -O y -sV. Esta detección se basa en la comparacion de la firma (fingerprint) de las respuestas que da el servicio a determinadas llamadas.
```
$ nmap -O -sV 192.168.1.5
```

```
PORT         STATE         SERVICE       VERSION
22/tcp       open          ssh           OpenSSH 6.7p1 Raspbian 5+deb8u3 (protocol 2.0)
53/tcp       open          domain        ISC BIND 9.9.5 (Raspbian Linux 8.0 (Jessie based))
80/tcp       open          http          Apache httpd 2.4.10 ((Raspbian))
111/tcp      open          rpcbind       2-4 (RPC #100000)
139/tcp      open          netbios-ssn   Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp      open          netbios-ssn   Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
4567/tcp     open          http          Jetty 9.4.8.v20171121
5900/tcp     open          vnc           RealVNC Enterprise 5.3 or later (protocol 5.0)
MAC Address: B8:27:EB:CD:FE:89 (Raspberry Pi Foundation)
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.9
Network Distance: 1 hop
Service Info: Host: RASPBERRYPI; OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

En este caso, no solamente sabemos que esta máquina tiene determinados puertos abiertos sino que también  se nos dice que es una Raspberry corriendo Raspbian. Con lo que podríamos hacer una prueba de fuerza bruta con un usuario “pi” por defecto. Ademas de las versiones de los distintos puertos que están escuchando, de forma que esta información puede ser utilizada para explotar vulnerabilidades sobre versiones no parcheadas, etc.

## 9. Utilizando más técnicas de escaneo
Por defecto nmap utiliza SYN que es técnica rápida y poco intrusiva/detectable, pero demas soporta en total 12 técnicas distintas que podemos definir como parámetros.

Si queremos hacer un escaneo basado en solicitudes UDP, podemos hacer una llamada del tipo:

```
$ nmap  -sU 192.168.1.5
```

## 10. Escaneo de vulnerabilidades
Nmap también nos permite realizar análisis de vulnerabilidades y utiliza una serie de scripts (en el caso de Kali, en /usr/share/nmap/scripts/ ) y que se pueden invocar con –script o su equivalente -sC.
Los scripts pueden pertenecer a una o varias categorías, de forma que podemos pedir a nmap que evalúe, por ejemplo, todos los scripts de una categoría contra un host. Hay algunas categorías especialmente interesantes como “vuln” (scripts dedicados a detectar vulnerabilidades en el destino), “exploit”, etc.

Si queremos escanear los scripts de categoría vulnerabilidad contra un host:
```
$ nmap --script vuln scanme.nmap.org
```

```
PORT       STATE      SERVICE
22/tcp     open       ssh
80/tcp     open       http
| http-csrf: 
| Spidering limited to: maxdepth=3; maxpagecount=20; withinhost=scanme.nmap.org
|   Found the following possible CSRF vulnerabilities: 
|     
|     Path: http://scanme.nmap.org:80/
|     Form id: cse-search-box-sidebar
|_    Form action: https://nmap.org/search.html
|http-dombased-xss: Couldn't find any DOM based XSS. | http-enum:  |   /images/: Potentially interesting directory w/ listing on 'apache/2.4.7 (ubuntu)' |  /shared/: Potentially interesting directory w/ listing on 'apache/2.4.7 (ubuntu)'
| http-slowloris-check: 
|   VULNERABLE:
|   Slowloris DOS attack
|     State: LIKELY VULNERABLE
|     IDs:  CVE:CVE-2007-6750
|       Slowloris tries to keep many connections to the target web server open and hold them open as long as possible.  It accomplishes this by opening connections to the target web server and sending a partial request. By doing so, it starves the http server's resources causing Denial Of Service.
|     Disclosure date: 2009-09-17
|     References:
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2007-6750
|_      http://ha.ckers.org/slowloris/
|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.
9929/tcp  open  nping-echo
31337/tcp open  Elite
```

El script ha detectado una potencial vulnerabilidad basada en el ataque de denegación de servicio de Slowloris. Si analizamos los scripts disponibles, vemos que precisamente hay uno que explota esta vulnerabilidad, llamado http-slowloris. Si queremos más información sobre el script podemos lanzar el siguiente comando:
```
$ nmap --script-help http-slowloris

$ nmap --script http-slowloris --max-paralelism 400 192.168.0.13
```

```
PORT     STATE SERVICE REASON  VERSION
80/tcp   open  http    syn-ack Apache httpd 2.2.20 ((Ubuntu))
| http-slowloris:
|   Vulnerable:
|   the DoS attack took +2m22s
|   with 501 concurrent connections
|_  and 441 sent queries
```

Esto nos permite entender cómo funciona el script y cómo lanzarlo (es posible hacerlo también con el propio nmap con –script y –script-args), etc. Podemos también obtener una descripción de todos los scripts que buscan vulnerabilidades:

```
$ nmap --script-help vuln
```

También es posible lanzar todos los scripts de un tipo determinado. Si queremos escanear vulnerabilidades sobre protocolo SMB en un host determinado:
```
$ nmap --script smb-* 192.168.1.5
```

Ademas podemos evaluar una vulnerabilidad sobre toda una red, seleccionando un script en concreto contra un rango con el parámetro –script:
```
$ nmap --script-help vuln
$ nmap --script
```

Asi es como Nmap incluye opciones interesantes para evaluar vulnerabilidades e incluso lanzar exploits. Entre los tipos de scripts, podemos encontrar:

* auth
* broadcast
* brute
* default
* discovery
* dos
* exploit
* external
* fuzzer
* intrusive
* malware
* safe
* version
* vuln

## 11. Algunos ejemplos
Aquí hay algunos ejemplos de uso de Nmap, desde lo simple y rutinario hasta un poco más complejo y esotérico. Algunas direcciones IP y nombres de dominio reales se utilizan para hacer las cosas más concretas. En su lugar, debe sustituir direcciones / nombres de su propia red . Si bien no creo que el escaneo de puertos en otras redes sea o deba ser ilegal, algunos administradores de red no aprecian el escaneo no solicitado de sus redes y pueden quejarse. Obtener el permiso primero es el mejor enfoque.
```
Esta opción analiza todos los puertos TCP reservados en la máquina scanme.nmap.org. La -v opción habilita el modo detallado.

$ nmap -v scanme.nmap.org

```

```
Lanza un escaneo SYN sigiloso contra cada máquina que está en funcionamiento de las 256 IP en la red de tamaño / 24 donde reside Scanme. También intenta determinar qué sistema operativo se está ejecutando en cada host que está en funcionamiento. Esto requiere privilegios de root debido al escaneo SYN y la detección del sistema operativo.

$ nmap -sS -O scanme.nmap.org/24

```


```
Inicia la enumeración de host y un escaneo TCP en la primera mitad de cada una de las 255 subredes de ocho bits posibles en el espacio de direcciones 198.116.0.0/16. Esto prueba si los sistemas ejecutan SSH, DNS, POP3 o IMAP en sus puertos estándar, o cualquier cosa en el puerto 4564. Para cualquiera de estos puertos abiertos, la detección de versiones se usa para determinar qué aplicación se está ejecutando.

$ nmap -sV -p 22,53,110,143,4564 198.116.0-255.1-127

```

```
Pide a Nmap que elija 100.000 hosts al azar y los escanee en busca de servidores web (puerto 80). La enumeración de host está deshabilitada -Pn desde que primero se envían un par de sondeos para determinar si un host está activo es un desperdicio cuando solo se está probando un puerto en cada host de destino de todos modos.

$ nmap -v -iR 100000 -Pn -p 80

```


```
Esto escanea 4096 direcciones IP para cualquier servidor web (sin hacer ping) y guarda la salida en formatos grepables y XML.

$ nmap -Pn -p80 -oX logs / pb-port80scan.xml -oG logs / pb-port80scan.gnmap 216.163.128.20/20

```

## 12. Otras opciones interesantes
Nmap tiene muchísimas opciones y es imposible tratar de cubrir siquiera un pequeño porcentaje ya que hasta el libro oficial de nmap tiene casi 500 páginas. Pero a pesar de eso, te sugerimos lo siguiente:
* Es posible generar el resultado del escaneo en un archivo output procesable, por ejemplo, en formato XML, con el parametro -oX o -oN.
* Cuando escaneamos amplios rangos de IPs, podemos desactivar el intento de resolución inversa de DNS con el parámetro -n.
* Durante la ejecución de un comando, podemos aumentar el nivel de información mostrada en consola (verbosity) con la tecla V.
* Si tenemos un firewall que nos afecta, podemos probar con -Pn.

Finalmente, hemos intentado desarrollar una breve introducción a nmap como herramienta de identificación de infraestructuras tecnologicas mientras que se abarca las bases para la automatizacion del escaneo de vulnerabilidades.

## Referencias
_Sitio Oficial NMAP: https://nmap.org/man/es/index.html_ <br>
_Puertos de red: https://es.wikipedia.org/wiki/Anexo:Puertos_de_red_<br>
_Http-Slowloris script: https://nmap.org/nsedoc/scripts/http-slowloris.html_




Nmap Cheat Sheet
================

Target Specification
---------------------------------------------------

| Switch | Example | Description |
|--------|---------|-------------|
| |nmap 192.168.1.1| Scan a single IP |
|  |  nmap 192.168.1.1 192.168.2.1 |  Scan specific IPs |
|  |  nmap 192.168.1.1-254 |  Scan a range
|  |  nmap scanme.nmap.org |  Scan a domain |
|  |  nmap 192.168.1.0/24 |  Scan using CIDR notation
|  -iL |  nmap -iL targets.txt |  Scan targets from a file |
|  -iR |  nmap -iR 100 |  Scan 100 random hosts
|  --exclude |  nmap --exclude 192.168.1.1 |  Exclude listed hosts


Scan Techniques
---------------

| Switch | Example | Description  |
|----|-----|----|
|  -sS |  nmap 192.168.1.1 -sS |  TCP SYN port scan (Default)
|  -sT |  nmap 192.168.1.1 -sT | TCP connect port scan<br />(Default without root privilege)|
|  -sU |  nmap 192.168.1.1 -sU |  UDP port scan
|  -sA |  nmap 192.168.1.1 -sA |  TCP ACK port scan |
|  -sW |  nmap 192.168.1.1 -sW |  TCP Window port scan
|  -sM |  nmap 192.168.1.1 -sM |  TCP Maimon port scan


Host Discovery
--------------

| Switch | Example | Description  |
|----|-----|----|
|  -sL |  nmap 192.168.1.1-3 -sL |  No Scan. List targets only
|  -sn |  nmap 192.168.1.1/24 -sn | Disable port scanning. Host discovery only.<br /> |
|  -Pn |  nmap 192.168.1.1-5 -Pn | Disable host discovery. Port scan only. <br />|  -PS |  nmap 192.168.1.1-5 -PS22-25,80 | TCP SYN discovery on port x. Port 80 by default|
|  -PA |  nmap 192.168.1.1-5 -PA22-25,80 | TCP ACK discovery on port x.
Port 80 by default|
|  -PU |  nmap 192.168.1.1-5 -PU53 | UDP discovery on port x. Port 40125 by default|
|  -PR |  nmap 192.168.1.1-1/24 -PR |  ARP discovery on local network
|  -n |  nmap 192.168.1.1 -n |  Never do DNS resolution


Port Specification
------------------

| Switch | Example | Description  |
|----|-----|----|
|  -p |  nmap 192.168.1.1 -p 21 |  Port scan for port x
|  -p |  nmap 192.168.1.1 -p 21-100 |  Port range |
|  -p |  nmap 192.168.1.1 -p U:53,T:21-25,80 |  Port scan multiple TCP and UDP ports
|  -p- |  nmap 192.168.1.1 -p- |  Port scan all ports |
|  -p |  nmap 192.168.1.1 -p http,https |  Port scan from service name
|  -F |  nmap 192.168.1.1 -F |  Fast port scan (100 ports) |
|  --top-ports |  nmap 192.168.1.1 --top-ports 2000 |  Port scan the top x ports
|  -p-65535 |  nmap 192.168.1.1 -p-65535 | Leaving off initial port in range<br />makes the scan start at port 1|
|  -p0- |  nmap 192.168.1.1 -p0- | Leaving off end port in range makes the scan go through to port 65535|



Service and Version Detection
-----------------------------


| Switch | Example | Description  |
|----|-----|----|
|  -sV |  nmap 192.168.1.1 -sV |  Attempts to determine the version of the service running on port
|  -sV --version-intensity |  nmap 192.168.1.1 -sV --version-intensity 8 |  Intensity level 0 to 9. Higher number increases possibility of correctness |
|  -sV --version-light |  nmap 192.168.1.1 -sV --version-light |  Enable light mode. Lower possibility of correctness. Faster
|  -sV --version-all |  nmap 192.168.1.1 -sV --version-all |  Enable intensity level 9. Higher possibility of correctness. Slower |
|  -A |  nmap 192.168.1.1 -A |  Enables OS detection, version detection, script scanning, and traceroute


OS Detection
------------


| Switch | Example | Description  |
|----|-----|----|
|  -O |  nmap 192.168.1.1 -O | Remote OS detection using TCP/IP<br />stack fingerprinting|
|  -O --osscan-limit |  nmap 192.168.1.1 -O --osscan-limit | If at least one open and one closed<br />TCP port are not found it will not try<br />OS detection against host|
|  -O --osscan-guess |  nmap 192.168.1.1 -O --osscan-guess |  Makes Nmap guess more aggressively
|  -O --max-os-tries |  nmap 192.168.1.1 -O --max-os-tries 1 | Set the maximum number x of OS<br />detection tries against a target|
|  -A |  nmap 192.168.1.1 -A |  Enables OS detection, version detection, script scanning, and traceroute


Timing and Performance
----------------------


| Switch | Example | Description  |
|----|-----|----|
|  -T0 |  nmap 192.168.1.1 -T0 | Paranoid (0) Intrusion Detection<br />System evasion|
|  -T1 |  nmap 192.168.1.1 -T1 | Sneaky (1) Intrusion Detection System <br />evasion|
|  -T2 |  nmap 192.168.1.1 -T2 | Polite (2) slows down the scan to use <br />less bandwidth and use less target <br />machine resources|
|  -T3 |  nmap 192.168.1.1 -T3 |  Normal (3) which is default speed |
|  -T4 |  nmap 192.168.1.1 -T4 | Aggressive (4) speeds scans; assumes <br />you are on a reasonably fast and <br />reliable network|
|  -T5 |  nmap 192.168.1.1 -T5 | Insane (5) speeds scan; assumes you <br />are on an extraordinarily fast network|



| Switch | Example input | Description  |
|----|-----|----|
|  --host-timeout &lt;time&gt; |  1s; 4m; 2h |  Give up on target after this long
|  --min-rtt-timeout/max-rtt-timeout/initial-rtt-timeout &lt;time&gt; |  1s; 4m; 2h |  Specifies probe round trip time |
|  --min-hostgroup/max-hostgroup &lt;size&lt;size&gt; |  50; 1024 | Parallel host scan group <br />sizes|
|  --min-parallelism/max-parallelism &lt;numprobes&gt; |  10; 1 |  Probe parallelization |
|  --scan-delay/--max-scan-delay &lt;time&gt; |  20ms; 2s; 4m; 5h |  Adjust delay between probes
|  --max-retries &lt;tries&gt; |  3 | Specify the maximum number <br />of port scan probe retransmissions|
|  --min-rate &lt;number&gt; |  100 |  Send packets no slower than &lt;numberr&gt; per second
|  --max-rate &lt;number&gt; |  100 |  Send packets no faster than &lt;number&gt; per second


NSE Scripts
-----------


| Switch | Example | Description  |
|----|-----|----|
|  -sC |  nmap 192.168.1.1 -sC |  Scan with default NSE scripts. Considered useful for discovery and safe |
|  --script default |  nmap 192.168.1.1 --script default |  Scan with default NSE scripts. Considered useful for discovery and safe |
|  --script |  nmap 192.168.1.1 --script=banner |  Scan with a single script. Example banner |
|  --script |  nmap 192.168.1.1 --script=http* |  Scan with a wildcard. Example http |
|  --script |  nmap 192.168.1.1 --script=http,banner |  Scan with two scripts. Example http and banner |
|  --script |  nmap 192.168.1.1 --script &quot;not intrusive&quot; |  Scan default, but remove intrusive scripts |
|  --script-args |  nmap --script snmp-sysdescr --script-args snmpcommunity=admin 192.168.1.1 |  NSE script with arguments |


Useful NSE Script Examples


| Command | Description  |
|----|-----|
|  nmap -Pn --script=http-sitemap-generator scanme.nmap.org |  http site map generator
|  nmap -n -Pn -p 80 --open -sV -vvv --script banner,http-title -iR 1000 |  Fast search for random web servers |
|  nmap -Pn --script=dns-brute domain.com |  Brute forces DNS hostnames guessing subdomains
|  nmap -n -Pn -vv -O -sV --script smb-enum*,smb-ls,smb-mbenum,smb-os-discovery,smb-s*,smb-vuln*,smbv2* -vv 192.168.1.1 |  Safe SMB scripts to run |
|  nmap --script whois* domain.com |  Whois query
|  nmap -p80 --script http-unsafe-output-escaping scanme.nmap.org |  Detect cross site scripting vulnerabilities |
|  nmap -p80 --script http-sql-injection scanme.nmap.org |  Check for SQL injections


Firewall / IDS Evasion and Spoofing
-----------------------------------


| Switch | Example | Description  |
|----|-----|----|
|  -f |  nmap 192.168.1.1 -f |  Requested scan (including ping scans) use tiny fragmented IP packets. Harder for packet filters |
|  --mtu |  nmap 192.168.1.1 --mtu 32 |  Set your own offset size |
|  -D | nmap -D 192.168.1.101,192.168.1.102, <br />192.168.1.103,192.168.1.23 192.168.1.1|
|  Send scans from spoofed IPs
|  -D |  nmap -D decoy-ip1,decoy-ip2,your-own-ip,decoy-ip3,decoy-ip4 remote-host-ip |  Above example explained |
|  -S |  nmap -S www.microsoft.com www.facebook.com |  Scan Facebook from Microsoft (-e eth0 -Pn may be required)
|  -g |  nmap -g 53 192.168.1.1 |  Use given source port number |
|  --proxies |  nmap --proxies http://192.168.1.1:8080, http://192.168.1.2:8080 192.168.1.1 |  Relay connections through HTTP/SOCKS4 proxies
|  --data-length |  nmap --data-length 200 192.168.1.1 |  Appends random data to sent packets


Example IDS Evasion command

    nmap -f -t 0 -n -Pn –data-length 200 -D 192.168.1.101,192.168.1.102,192.168.1.103,192.168.1.23 192.168.1.1


Output
------

| Switch | Example | Description  |
|----|-----|----|
|  -oN |  nmap 192.168.1.1 -oN normal.file |  Normal output to the file normal.file
|  -oX |  nmap 192.168.1.1 -oX xml.file |  XML output to the file xml.file |
|  -oG |  nmap 192.168.1.1 -oG grep.file |  Grepable output to the file grep.file
|  -oA |  nmap 192.168.1.1 -oA results |  Output in the three major formats at once |
|  -oG - |  nmap 192.168.1.1 -oG - |  Grepable output to screen. -oN -, -oX - also usable
|  --append-output |  nmap 192.168.1.1 -oN file.file --append-output |  Append a scan to a previous scan file |
|  -v |  nmap 192.168.1.1 -v |  Increase the verbosity level (use -vv or more for greater effect)
|  -d |  nmap 192.168.1.1 -d |  Increase debugging level (use -dd or more for greater effect) |
|  --reason |  nmap 192.168.1.1 --reason |  Display the reason a port is in a particular state, same output as -vv
|  --open |  nmap 192.168.1.1 --open |  Only show open (or possibly open) ports |
|  --packet-trace |  nmap 192.168.1.1 -T4 --packet-trace |  Show all packets sent and received
|  --iflist |  nmap --iflist |  Shows the host interfaces and routes |
|  --resume |  nmap --resume results.file |  Resume a scan


Helpful Nmap Output examples



| Command | Description  |
|----|-----|
|  nmap -p80 -sV -oG - --open 192.168.1.1/24 | grep open |  Scan for web servers and grep to show which IPs are running web servers
|  nmap -iR 10 -n -oX out.xml | grep &quot;Nmap&quot; | cut -d &quot; &quot; -f5 &gt; live-hosts.txt |  Generate a list of the IPs of live hosts |
|  nmap -iR 10 -n -oX out2.xml | grep &quot;Nmap&quot; | cut -d &quot; &quot; -f5 &gt;&gt; live-hosts.txt |  Append IP to the list of live hosts
|  ndiff scanl.xml scan2.xml |  Compare output from nmap using the ndif |
|  xsltproc nmap.xml -o nmap.html |  Convert nmap xml files to html files
|  grep &quot; open &quot; results.nmap | sed -r 's/ +/ /g' | sort | uniq -c | sort -rn | less |  Reverse sorted list of how often ports turn up


Miscellaneous Options
---------------------

| Switch | Example | Description  |
|----|-----|----|
|  -6 |  nmap -6 2607:f0d0:1002:51::4 |  Enable IPv6 scanning |
|  -h |  nmap -h |  nmap help screen |


Other Useful Nmap Commands
--------------------------



| Command | Description  |
|----|-----|
|  nmap -iR 10 -PS22-25,80,113,1050,35000 -v -sn |  Discovery only on ports x, no port scan
|  nmap 192.168.1.1-1/24 -PR -sn -vv |  Arp discovery only on local network, no port scan |
|  nmap -iR 10 -sn -traceroute |  Traceroute to random targets, no port scan
|  nmap 192.168.1.1-50 -sL --dns-server 192.168.1.1 |  Query the Internal DNS for hosts, list targets only

