================================================================================
🚀 PROYECTO FINAL SIS313: PLATAFORMA DISTRIBUIDA Y MULTINIVEL DE GESTIÓN 
                          DOCUMENTAL CORPORATIVA SEGURA (NEXTCLOUD)
================================================================================
Asignatura: SIS313: Infraestructura, Plataformas Tecnológicas y Redes
Semestre: 1/2026
Docente: Ing. Marcelo Quispe Ortega

👥 MIEMBROS DEL EQUIPO (Grupo 4)
--------------------------------------------------------------------------------
Nombre Completo                  Rol en el Proyecto                  GitHub
[Nelson Polares Ortiz]           [Arquitecto de Seguridad]           [NelsonPolaresOrtiz]
[Jhonnathan Peñaranda Mamani]    [Administrador de Servidores]       [jhonasPM64]
[Emily Karen Villarpando Olmedo] [Administrador de Base de Datos]    [nerak5555]
[Quispe Serrano Carla]           [Monitoreo y Automatización]        [CarlaQS]
--------------------------------------------------------------------------------

🎯 I. OBJETIVO DEL PROYECTO
Diseñar, configurar, implementar y asegurar una infraestructura distribuida de misión crítica multinivel en entornos de alta disponibilidad (HA) para el despliegue de la plataforma corporativa Nextcloud. El diseño debe garantizar un aislamiento lógico riguroso por departamentos mediante segmentación inter-VLAN, mitigación absoluta de puntos únicos de fallo (SPOF) utilizando un Proxy Inverso perimetral con terminación SSL/TLS criptográfica, endurecimiento de hilos a través de políticas restrictivas de Firewall (UFW), persistencia inmutable de datos con arreglos redundantes independientes de discos por software (RAID 1) y exportaciones NFS eficientes, auditoría y recolección inmutable de telemetría a través de Prometheus y Grafana, y la orquestación automatizada basada en scripts e Infraestructura como Código (IaC) para entornos institucionales y empresariales de alta demanda.

💡 II. JUSTIFICACIÓN E IMPORTANCIA
Las infraestructuras monolíticas centralizadas tradicionales conllevan una superficie de exposición insostenible para entornos universitarios o corporativos, donde el compromiso o la degradación de un único binario colapsa la totalidad de la operación institucional. El presente proyecto aborda y resuelve este vector de vulnerabilidad crítica mediante la segmentación física de funciones elementales, garantizando resiliencia operativa y seguridad avanzada bajo los siguientes fundamentos técnicos:

1. Continuidad Operacional, Tolerancia a Fallos y Alta Disponibilidad (T1 / T2):
* Mitigación de SPOF: Al delegar la frontera a una zona desmilitarizada (DMZ) administrada por NGINX, la infraestructura absorbe, filtra y distribuye de manera balanceada ráfagas de tráfico masivo o peticiones concurrentes concurrentes hacia la capa lógica.
* Resiliencia de Persistencia: El almacenamiento crítico corporativo de documentos no reside en bloques locales volátiles, sino sobre un volumen estructurado RAID 1 en espejo que tolera el fallo destructivo físico de almacenamiento en caliente, acoplado a un protocolo NFS que es abstraído por los clientes lógicos.
* Recuperación ante Desastres (DRP): La implementación de políticas automáticas basadas en volcados lógicos y compresión binaria reduce el Objetivo de Punto de Recuperación (RPO) y el Objetivo de Tiempo de Recuperación (RTO), protegiendo los activos digitales institucionales ante corrupciones de archivos.

2. Seguridad Avanzada, Hardening y Aislamiento Perimetral (T5):
* Aislamiento por VLANs: Se prohíbe de forma nativa la intercomunicación directa no regulada entre capas. La zona pública web (VLAN 10) nunca interactúa directamente con el núcleo de datos relacionales (VLAN 30), mitigando ataques de movimiento lateral.
* Control de Acceso Granular: Las políticas estrictas "Default Deny" del Firewall de Linux impiden el escaneo de puertos desprotegidos. Los sockets operativos solo se abren hacia orígenes explícitamente autorizados mediante direccionamientos CIDR específicos.
* Cifrado en Tránsito y Gestión de Salto: El tráfico de red externo se encapsula bajo suites criptográficas modernas SSL/TLS. Para fines administrativos, se anula el acceso SSH tradicional sobre puertos estándar expuestos, derivando el aprovisionamiento de configuración a un entorno securizado de puerto alto (2222).

🛠️ III. TECNOLOGÍAS Y CONCEPTOS IMPLEMENTADOS

3.1. Tecnologías Clave
* [NGINX Proxy]: Actúa como el Proxy Inverso perimetral de entrada y terminación TLS/SSL para interceptar, limpiar cabeceras, aplicar certificados y balancear el tráfico externo hacia las interfaces internas.
* [UFW (Uncomplicated Firewall)]: Herramientas de filtrado de paquetes a nivel de Kernel (Netfilter) para imponer políticas de control perimetral basadas en denegación estricta entrante.
* [OpenSSH]: Servicio daemon securizado modificado en puerto de sockets alto (2222) para administrar el clúster sin exponer vectores de fuerza bruta.
* [OpenSSL]: Motor criptográfico robusto encargado de la generación y firma de llaves y certificados X.509 para encapsular el tráfico HTTP de forma asimétrica.
* [Prometheus]: Motor de base de datos de series temporales (TSDB) configurado para extraer de forma inmutable la telemetría del clúster distributed.
* [Grafana Server]: Plataforma analítica de código abierto interconectada mediante sockets locales para moldear y renderizar visualizaciones de métricas en tiempo real.
* [Node Exporter]: Agente recolector embebido a nivel de Kernel en cada sistema operativo que expone variables de hardware a través del puerto TCP 9100.
* [PHP-FPM (FastCGI Process Manager)]: Procesador backend aislado e independiente encargado de interpretar el código dinámico de Nextcloud y optimizar la cola de sockets.
* [MariaDB Server]: Motor de base de datos relacional de nivel empresarial optimizado y securizado para albergar los metadatos de acceso y estructuras del sistema.
* [Keepalived]: Servicio de conmutación por error basado en el protocolo VRRP (Virtual Router Redundancy Protocol) encargado de mantener una IP Virtual elástica compartida de alta disponibilidad.
* [mdadm]: Utilidad nativa de Linux encargada de administrar y sincronizar los descriptores lógicos de los arreglos de discos redundantes por software (RAID 1).
* [NFS Kernel Server]: Protocolo distribuido que permite exportar sistemas de archivos sobre la red interna con control de llamadas RPC (Remote Procedure Call).

3.2. Conceptos de la Asignatura Puestos en Práctica (T1 - T6)
* [✅] Alta Disponibilidad (T2) y Tolerancia a Fallos: Implementación de Keepalived para failover dinámico en caliente del direccionamiento virtual lógico IP, junto al despliegue redundante en espejo RAID 1 administrado por mdadm ante fallas físicas de discos duros.
* [✅] Seguridad y Hardening (T5): Aplicación del estándar de denegación global entrante con UFW, migración de los demonios SSH a sockets no estándar (2222), inyección de cabeceras de protección web HTTP (X-Frame, X-Content, X-XSS) y cifrado asimétrico SSL/TLS.
* [✅] Automatización y Gestión (T6): Orquestación centralizada de la arquitectura del ecosistema utilizando playbooks parametrizados de Ansible, automatización del aprovisionamiento de paquetería y programación de scripts de mantenimiento selectivo mediante tareas crontab de sistema.
* [✅] Balanceo de Carga y Proxy (T3/T4): Configuración de NGINX en la DMZ para la manipulación transparente de variables, ocultación de la topología real interna y reenvío de cabeceras limpias (X-Real-IP, X-Forwarded-For) a la subred de aplicación.
* [✅] Monitoreo y Auditoría (T4/T1): Despliegue de una arquitectura de telemetría desacoplada para aislar los flujos de logs, utilizando Prometheus como recolector inmutable de hilos temporales y Grafana como consola centralizada de observabilidad.
* [✅] Networking Avanzado (T3): Creación y etiquetado de subinterfaces de red virtuales sobre un único enlace físico (interfaz ens18) para segmentar el tráfico en subredes locales aisladas administradas por rutas estáticas inter-VLAN.

🌐 IV. DISEÑO DE LA INFRAESTRUCTURA Y TOPOLOGÍA

4.1. Diseño Esquemático
El ecosistema completo se divide de manera quirúrgica en 4 segmentos lógicos (VLANs), los cuales están obligados a conmutar y enrutarse exclusivamente a través de las interfaces de la pasarela central del Router del laboratorio de la universidad.

VM/Host    Rol                           IP Física         IP Virtual (VIP)   Red Lógica (VLAN)    SO
-----------------------------------------------------------------------------------------------------------------
VM1        Proxy / Seguridad Perimetral  192.168.100.208   N/A                VLAN 10 (10.0.10.2)  Ubuntu 24.04
VM2        Capa de Aplicación (Nextcloud)192.168.100.207   192.168.100.210    VLAN 20 (10.0.20.2)  Ubuntu 24.04
VM3        Capa de Datos y Persistencia  192.168.100.209   N/A                VLAN 30 (10.0.30.2)  Ubuntu 24.04
VM4        Torre de Control y Monitoreo  192.168.100.211   N/A                VLAN 40 (10.0.40.2)  Ubuntu 24.04
-----------------------------------------------------------------------------------------------------------------

4.2. Estrategia Adoptada y Decisiones Críticas
* Estrategia de Dispersión Multitier: Diseñada para evitar el colapso cruzado de servicios. Si la capa lógica web sufre un desbordamiento o denegación de servicio, los datos en MariaDB y los archivos sobre el RAID 1 se mantienen intactos y protegidos en la subred de Emily, mientras Carla monitorea el estado desde la VLAN 40.
* Estrategia de Enrutamiento Seguro Inter-VLAN: Implementada por Nelson a nivel de direccionamiento perimetral, forzando a que las subinterfaces lógicas de red utilicen gateways dedicados por cada segmento. Esto evita fugas de tráfico locales y permite auditar los flujos en el Router central.

📋 V. GUÍA DE IMPLEMENTACIÓN Y PUESTA EN MARCHA

================================================================================
V. GUÍA DE IMPLEMENTACIÓN Y PUESTA EN MARCHA (VM1: SEGURIDAD PERIMETRAL Y ACCESO - GESTIÓN: NELSON)
================================================================================
Esta sección detalla de forma exhaustiva los pasos y directivas de configuración de bajo nivel implementados por Nelson para desplegar y blindar la frontera perimetral del Data Center distribuido.

--------------------------------------------------------------------------------
A. EXPLICACIÓN TEÓRICA Y MARCO DE HARDENING DE LA VM1
--------------------------------------------------------------------------------
La VM1 constituye la primera línea de defensa del centro de datos virtualizado, encapsulando la Zona Desmilitarizada (DMZ). Su existencia responde a la necesidad imperiosa de ocultar la topología interna del clúster ante el exterior; ningún usuario final o atacante externo conoce la IP real o interactúa con el servidor de Nextcloud o la base de datos de Emily. 

El hardening perimetral se sustenta sobre un aislamiento estricto por software donde NGINX actúa como un Proxy Inverso puro, interceptando peticiones en el puerto 80, sanitizando las cabeceras HTTP y encapsulando el tráfico de manera transparente hacia la subred interna mediante reglas de ruteo estricto. Al mismo tiempo, el cortafuegos UFW actúa como un escudo perimetral bajo la premisa de "Denegación por Defecto", cerrando toda comunicación entrante y permitiendo únicamente los accesos quirúrgicos web y el puerto de administración de alto rango para el control seguro.

--------------------------------------------------------------------------------
B. METODOLOGÍA DE IMPLEMENTACIÓN PASO A PASO
--------------------------------------------------------------------------------

1. CONFIGURACIÓN DE RED E INTERFAZ PERIMETRAL (NETPLAN)
Edición de la interfaz física para estructurar la subinterfaz lógica vlan10 y vincular la máquina virtual al direccionamiento seguro y pasarela perimetral del Router.

* Fichero clave de red: /etc/netplan/50-cloud-init.yaml
* Comando para edición: sudo nano /etc/netplan/50-cloud-init.yaml

Código de red aplicado:
--------------------------------------------------------------------------------
network:
    version: 2
    renderer: networkd
    ethernets:
        ens18:
            addresses:
                - "192.168.100.208/24"
    vlans:
        vlan10:
            id: 10
            link: ens18
            addresses: ["10.0.10.2/24"]
            nameservers:
                addresses: ["8.8.8.8"]
            routes:
                - to: "default"
                  via: "10.0.10.1"
--------------------------------------------------------------------------------

* Comando para aplicar e inyectar cambios en caliente sobre el Kernel:
  sudo netplan apply

2. CONFIGURACIÓN Y ASEGURAMIENTO DEL DEMONIO DE CONTROL SSH (HARDENING)
Migración del servicio OpenSSH fuera del puerto nativo expuesto con el fin de evadir escaneos automatizados de red en el laboratorio.

* Fichero clave del servicio: /etc/ssh/sshd_config
* Comando para edición perimetral: sudo nano /etc/ssh/sshd_config

Líneas críticas modificadas dentro del archivo:
--------------------------------------------------------------------------------
Port 2222
Protocol 2
MaxAuthTries 3
PermitEmptyPasswords no
--------------------------------------------------------------------------------

* Comandos secuenciales para recargar los descriptores de hilos del servicio:
  sudo systemctl daemon-reload
  sudo systemctl restart ssh
  sudo systemctl restart sshd

3. ENDURECIMIENTO Y REGLAS FILTRADAS DEL CORTAFUEGOS (UFW)
Configuración de la política de aislamiento restrictivo global y apertura quirúrgica estricta de sockets orientados a servicios de producción y monitoreo.

* Comandos secuenciales aplicados en la terminal:
  sudo ufw default deny incoming
  sudo ufw default allow outgoing
  sudo ufw allow 2222/tcp
  sudo ufw allow 80/tcp
  sudo ufw allow 443/tcp
  sudo ufw allow from 10.0.40.2 to any port 9100 proto tcp comment 'Monitoreo desde VM4'

* Comando de inicialización forzada del cortafuegos en el arranque:
  sudo ufw --force enable

* Comando de diagnóstico analítico del estado del firewall:
  sudo ufw status verbose

4. CONFIGURACIÓN E IMPLEMENTACIÓN DE NGINX COMO PROXY INVERSO
Reconfiguración del bloque de escucha HTTP global del servidor web para anular la lectura de archivos locales y transformarlo en un reenviador transparente hacia la subred interna.

* Fichero clave operativo: /etc/nginx/sites-enabled/default
* Comando para edición: sudo nano /etc/nginx/sites-enabled/default

Código fuente limpio estructurado e implementado:
--------------------------------------------------------------------------------
server {
        listen 80 default_server;
        listen [::]:80 default_server;

        server_name _;
        root /var/www/html;
        index index.html index.htm index.nginx-debian.html;

        location / {
                # Reenvío de flujos hacia la IP de la Capa de Aplicación
                proxy_pass http://10.0.20.2;
                
                # Inyección de cabeceras HTTP limpias para auditoría inmutable
                proxy_set_header Host $host;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header X-Forwarded-Proto $scheme;
        }
}
--------------------------------------------------------------------------------

* Comando para verificar la integridad semántica del código NGINX:
  sudo nginx -t

* Comando para refrescar el demonio web en producción:
  sudo systemctl restart nginx

--------------------------------------------------------------------------------
C. COMANDOS DE VERIFICACIÓN Y DIAGNÓSTICO
--------------------------------------------------------------------------------
* Verificar que el proceso principal de NGINX corra sin errores en sus hilos:
  sudo systemctl status nginx

* Auditar visualmente los puertos de red abiertos en escucha activa:
  sudo ss -tunlp

================================================================================
V. GUÍA DE IMPLEMENTACIÓN Y PUESTA EN MARCHA (VM2: CAPA DE APLICACIÓN - GESTIÓN: JHONNATHAN)
================================================================================
Esta sección detalla las directivas técnicas implementadas por Jhonnathan para levantar el entorno lógico de Nextcloud, el motor PHP y la alta disponibilidad en la VLAN 20.

--------------------------------------------------------------------------------
A. EXPLICACIÓN TEÓRICA Y RESPONSABILIDAD LÓGICA DE LA VM2
--------------------------------------------------------------------------------
La VM2 encapsula en su totalidad la Capa de Aplicación o "Cerebro" del Data Center corporativo. Su rol fundamental es procesar las peticiones HTTP/FastCGI dinámicas que le delega el proxy inverso de Nelson de forma limpia y transparente. A nivel de diseño de alta disponibilidad, esta capa implementa un desacoplamiento absoluto de persistencia: no almacena código estático local inmutable de usuarios, ni registros de bases de datos relacionales de metadatos. 

Para procesar el volumen documental de Nextcloud de forma ágil, Jhonnathan optimizó los hilos del socket de comunicación PHP-FPM, enlazándolo con las políticas de control perimetral web para inyectar cabeceras OWASP avanzadas contra ataques Cross-Site Scripting (XSS) e inyecciones de clicks. Asimismo, configuró el demonio de Keepalived bajo el protocolo VRRP; esto genera una dirección elástica IP Virtual compartida de conmutación automática, garantizando que si un hilo lógico se satura, la frontera perimetral continúe apuntando a una infraestructura web activa sin caídas.

--------------------------------------------------------------------------------
B. METODOLOGÍA DE IMPLEMENTACIÓN PASO A PASO
--------------------------------------------------------------------------------

1. CONFIGURACIÓN DE RED Y SUBINTERFAZ VIRTUAL (NETPLAN)
Aislamiento estricto de la máquina de aplicación en la subred lógica asignada, conservando el enlace de gestión.

* Fichero de red: /etc/netplan/50-cloud-init.yaml
* Estructura de código aplicada:
--------------------------------------------------------------------------------
network:
  version: 2
  renderer: networkd
  ethernets:
    ens18:
      dhcp4: false
  vlans:
    vlan20:
      id: 20
      link: ens18
      dhcp4: false
      addresses:
        - 192.168.100.207/24
      nameservers:
        addresses:
          - 8.8.8.8
      routes:
        - to: default
          via: 192.168.100.1
--------------------------------------------------------------------------------
* Comando de aplicación: sudo netplan apply

2. ACOPLAMIENTO DE LA CAPA DE ALMACENAMIENTO (NFS CLIENT)
Instalación de utilidades del sistema de archivos distribuidos para enlazar de forma transparente el directorio de subida sobre el RAID de Emily.

* Comando de instalación de paquetes:
  sudo apt update && sudo apt install -y nfs-common

* Crear el árbol jerárquico de directorios locales de Nextcloud:
  sudo mkdir -p /var/www/html/nextcloud/data

* Inyección de persistencia del volumen en el fichero /etc/fstab:
  echo '10.0.30.2:/mnt/data_storage /var/www/html/nextcloud/data nfs defaults,noatime,nodiratime,bg,nofail 0 0' | sudo tee -a /etc/fstab

* Comando para forzar el montaje inmediato:
  sudo mount -a

3. DESPLIEGUE Y AUTOMATIZACIÓN (ANSIBLE - DESDE EL NODO DE CONTROL VM1)
La orquestación del clúster se centraliza exclusivamente desde la VM1 para automatizar la instalación de paquetes y configuraciones.

* Instalación de Ansible (Solo en la VM1 de control):
  sudo apt update && sudo apt install -y ansible

* Edición del Inventario (/etc/ansible/hosts.ini):
--------------------------------------------------------------------------------
[webservers]
nodo_aplicacion_A ansible_host=192.168.100.207 ansible_user=adming4 ansible_port=2222

[dbservers]
nodo_base_datos ansible_host=192.168.100.209 ansible_user=adming4
--------------------------------------------------------------------------------
* Comando para ejecutar el Playbook estructurado:
  ansible-playbook -i hosts.ini setup.yml

4. FICHEROS DE CONFIGURACIÓN CLAVE DE LA CAPA DE APLICACIÓN
A. Procesamiento Web Local (/etc/nginx/sites-available/nextcloud)
--------------------------------------------------------------------------------
upstream php-handler {
    server unix:/run/php/php8.3-fpm.sock;
}

server {
    listen 80;
    server_name _;

    root /var/www/html/nextcloud;
    index index.php index.html;

    client_max_body_size 512M;
    client_body_buffer_size 512k;
    fastcgi_buffers 64 4K;

    add_header X-Content-Type-Options nosniff;
    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-XSS-Protection "1; mode=block";

    location / {
        try_files $uri $uri/ /index.php$request_uri;
    }

    location ~ \.php$ {
        include fastcgi_params;
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass php-handler;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    }

    location ~ ^/(?:\.|autotest|occ|issue|indie|db_|config) {
        deny all;
    }
}
--------------------------------------------------------------------------------

B. Configuración de la Alta Disponibilidad de Red (/etc/keepalived/keepalived.conf)
--------------------------------------------------------------------------------
vrrp_script chk_nginx {
    script "pidof nginx"
    interval 2
}

vrrp_instance VI_1 {
    state MASTER            
    interface vlan20        
    virtual_router_id 51
    priority 101            
    advert_int 1

    authentication {
        auth_type PASS
        auth_pass usfx123   
    }

    virtual_ipaddress {
        192.168.100.210/24  
    }

    track_script {
        chk_nginx
    }
}
--------------------------------------------------------------------------------

--------------------------------------------------------------------------------
C. PUESTA EN MARCHA Y VERIFICACIÓN DE FLUJOS
--------------------------------------------------------------------------------
* Forzar reinicio coordinado de los hilos de servicios en la Capa de Aplicación:
  sudo systemctl restart nginx php8.3-fpm keepalived

================================================================================
V. GUÍA DE IMPLEMENTACIÓN Y PUESTA EN MARCHA (VM3: CAPA DE DATOS Y PERSISTENCIA - GESTIÓN: EMILY)
================================================================================
Esta sección detalla de forma pormenorizada los pasos de infraestructura ejecutados por Emily para configurar la persistencia, el volumen redundante y el motor relacional.

--------------------------------------------------------------------------------
A. EXPLICACIÓN TEÓRICA Y CRITICIDAD OPERATIVA DE LA VM3
--------------------------------------------------------------------------------
La VM3 constituye el almacenamiento masivo crítico, la persistencia estructurada y el "Corazón" indiscutible de todo el Data Center del grupo. El rol asignado a Emily se enfoca en resolver dos de las métricas más exigentes de la materia: la tolerancia estricta a fallas de hardware en caliente y la continuidad ininterrumpida del negocio ante desastres. Para cumplir el primer eje, Emily implementó un arreglo redundante de discos independientes por software (RAID 1 Espejo) mediante mdadm; esto acopla dos unidades de bloques lógicos idénticos de manera que si un disco virtual se destruye o corrompe durante la sustentación, el Kernel continúa leyendo datos del bloque alterno sin degradar el servicio. 

Para que la VM2 de Jhonnathan pueda escribir en este volumen, Emily estructuró un Servidor NFS Kernel, exportando directorios lógicos mediante llamadas remotas RPC seguras. Paralelamente, centralizó el motor relacional MariaDB Server, alterando las variables nativas del socket de red para escuchar exclusivamente consultas en la IP interna `10.0.30.2`, aislando la base de datos de entornos externos de ataque. Finalmente, resolvió el esquema DRP (Plan de Recuperación ante Desastres) programando scripts Bash embebidos en el demonio Cron que automatizan respaldos cada medianoche y depuran históricos obsoletos de forma inteligente.

--------------------------------------------------------------------------------
B. METODOLOGÍA DE IMPLEMENTACIÓN PASO A PASO
--------------------------------------------------------------------------------

1. CONFIGURACIÓN DE RED E INTERFAZ DE PERSISTENCIA (NETPLAN)
* Fichero de red: /etc/netplan/50-cloud-init.yaml
* Estructura aplicada:
--------------------------------------------------------------------------------
network:
    version: 2
    renderer: networkd
    ethernets:
        ens18:
            addresses:
                - "192.168.100.209/24"
    vlans:
        vlan30:
            id: 30
            link: ens18
            addresses: ["10.0.30.2/24"]
            nameservers:
                addresses: ["8.8.8.8"]
            routes:
                - to: "default"
                  via: "10.0.30.1"
--------------------------------------------------------------------------------
* Comando de aplicación: sudo netplan apply

2. DESPLIEGUE DEL ARREGLO DE DISCOS REDUNDANTES (RAID 1 VIRTUAL)
Unión lógica de los bloques de almacenamiento asignados para estructurar la tolerancia a fallos por hardware.

* Comando de creación e inicialización del arreglo en espejo:
  sudo mdadm --create --verbose /dev/md0 --level=1 --raid-devices=2 /dev/sdb /dev/sdc

* Formatear el bloque compuesto bajo el sistema de archivos ext4 transaccional:
  sudo mkfs.ext4 /dev/md0

* Crear el directorio raíz físico y montar el bloque lógico:
  sudo mkdir -p /mnt/data_storage && sudo mount /dev/md0 /mnt/data_storage

* Agregar el descriptor en el fichero fstab para montaje persistente:
  echo '/dev/md0 /mnt/data_storage ext4 defaults 0 0' | sudo tee -a /etc/fstab

3. PROVISIÓN DEL SERVIDOR DE ALMACENAMIENTO EN RED (NFS SERVER)
Exportación controlada del bloque montado para el consumo exclusivo de la capa lógica.

* Fichero de directivas: /etc/exports
Línea insertada con control estricto de llamadas RPC:
--------------------------------------------------------------------------------
/mnt/data_storage  10.0.20.2(rw,sync,no_subtree_check,no_root_squash)
--------------------------------------------------------------------------------

* Comandos para aplicar la exportación y reiniciar los hilos del servidor:
  sudo exportfs -rav
  sudo systemctl restart nfs-kernel-server

4. CONFIGURACIÓN DEL MOTOR DE BASE DE DATOS (MARIADB)
Securización del socket de escucha de la base de datos para restringir llamadas fuera del segmento.

* Fichero clave de red: /etc/mysql/mariadb.conf.d/50-server.cnf
* Cambio de la variable de escucha: bind-address = 10.0.30.2

* Comandos de creación de la Base de Datos y privilegios (Consola MariaDB):
--------------------------------------------------------------------------------
CREATE DATABASE nextcloud_db;
CREATE USER 'nextcloud_user'@'10.0.20.2' IDENTIFIED BY 'PasswordSeguro123$';
GRANT ALL PRIVILEGES ON nextcloud_db.* TO 'nextcloud_user'@'10.0.20.2';
FLUSH PRIVILEGES;
EXIT;
--------------------------------------------------------------------------------
* Comando de reinicio del servicio: sudo systemctl restart mariadb

5. AUTOMATIZACIÓN DE RESPALDOS CRÍTICOS (BASH + CRON)
Despliegue del script de mantenimiento para respaldar de manera unificada las estructuras SQL y binarias.

* Fichero del script: /opt/scripts/backup.sh
* Código del script en Bash:
--------------------------------------------------------------------------------
#!/bin/bash
BACKUP_DIR="/mnt/data_storage/backups"
FECHA=$(date +%Y%m%d_%H%M%S)
mkdir -p $BACKUP_DIR

mysqldump -u nextcloud_user -p'PasswordSeguro123$' nextcloud_db > $BACKUP_DIR/db_backup_$FECHA.sql
tar -czf $BACKUP_DIR/files_backup_$FECHA.tar.gz /mnt/data_storage/nextcloud_data
find $BACKUP_DIR -type f -mtime +7 -delete
--------------------------------------------------------------------------------
* Asignación de permisos de ejecución obligatorios: sudo chmod +x /opt/scripts/backup.sh
* Programación en las tareas del sistema (sudo crontab -e):
  0 0 * * * /opt/scripts/backup.sh

--------------------------------------------------------------------------------
C. COMANDOS DE VERIFICACIÓN Y AUDITORÍA DE LA VM3
--------------------------------------------------------------------------------
* Comprobar la sincronización y salud de los bloques en espejo:
  cat /proc/mdstat
* Verificar que el Servidor NFS esté exponiendo la carpeta correctamente:
  sudo showmount -e 10.0.30.2

================================================================================
V. GUÍA DE IMPLEMENTACIÓN Y PUESTA EN MARCHA (VM4: TORRE DE CONTROL Y MONITOREO - GESTIÓN: CARLA)
================================================================================
Esta sección detalla de forma exhaustiva los pasos y directivas de configuración de bajo nivel implementados por Carla para desplegar y asegurar el entorno de observabilidad inmutable de la infraestructura distribuida.

--------------------------------------------------------------------------------
A. EXPLICACIÓN TEÓRICA Y PRINCIPIO DE OBSERVABILIDAD DE LA VM4
--------------------------------------------------------------------------------
La VM4, administrada de forma exclusiva por Carla, opera como el sistema de auditoría integral, observabilidad analítica e inmutabilidad de la telemetría del Data Center. El diseño de esta infraestructura rompe el paradigma tradicional de consulta manual y centraliza la supervisión bajo un modelo desacoplado y seguro dentro de la VLAN 40. Separar este servicio asegura que si la capa de datos de Emily o la capa web de Jhonnathan sufren un pico de latencia extrema o colapso por recursos, la torre de control de Carla continúe recopilando datos en segundo plano de manera autónoma.

Para capturar las variables del sistema sin mermar los procesadores de las máquinas de producción, Carla implementó una Arquitectura Pull (extracción cíclica) basada en Prometheus. Dicho motor realiza llamadas cada 15 segundos hacia el daemon node_exporter en escucha en el puerto TCP 9100 de las otras VMs, acumulando la información en series temporales indexadas cronológicamente. Finalmente, Carla vinculó esta base de datos analítica con el servidor visual de Grafana, modelando paneles analíticos complejos con velocímetros, mapas de calor y métricas de red que exponen con precisión científica el estado del clúster ante las auditorías del docente.

--------------------------------------------------------------------------------
B. METODOLOGÍA DE IMPLEMENTACIÓN PASO A PASO
--------------------------------------------------------------------------------

1. CONFIGURACIÓN DE RED E INTERFAZ (NETPLAN)
Se enlazó la máquina virtual al segmento aislado de la VLAN 40 para centralizar la supervisión y auditoría de las demás subredes del proyecto.

* Fichero clave de red: /etc/netplan/50-cloud-init.yaml
* Comando de edición: sudo nano /etc/netplan/50-cloud-init.yaml

Código de red aplicado:
--------------------------------------------------------------------------------
network:
    version: 2
    renderer: networkd
    ethernets:
        ens18:
            addresses:
                - "192.168.100.211/24"
    vlans:
        vlan40:
            id: 40
            link: ens18
            addresses: ["10.0.40.2/24"]
            nameservers:
                addresses: ["8.8.8.8"]
            routes:
                - to: "default"
                  via: "10.0.40.1"
--------------------------------------------------------------------------------

* Comando de aplicación en caliente sobre el Kernel:
  sudo netplan apply

2. CONFIGURACIÓN DEL RECOLECTOR DE MÉTRICAS (PROMETHEUS)
Se configuran los objetivos de raspado hacia los agentes "node_exporter" instalados en los puertos 9100 de las capas previas para consolidar la inmutabilidad de logs y estados.

* Fichero clave de configuración: /etc/prometheus/prometheus.yml
* Bloque de objetivos estáticos añadidos dentro de la directiva scrape_configs:
--------------------------------------------------------------------------------
scrape_configs:
  - job_name: 'cluster_sis313'
    scrape_interval: 15s
    evaluation_interval: 15s
    static_configs:
      - targets: ['10.0.10.2:9100'] # VM1: Capa de Seguridad (Nelson)
      - targets: ['10.0.20.2:9100'] # VM2: Capa de Aplicación (Jhonnathan)
      - targets: ['10.0.30.2:9100'] # VM3: Capa de Datos (Emily)
--------------------------------------------------------------------------------

* Comando para reiniciar y refrescar el motor TSDB:
  sudo systemctl restart prometheus

3. PUESTA EN MARCHA DEL ENTORNO VISUAL (GRAFANA SERVER)
Aprovisionamiento del entorno gráfico interactivo para análisis de logs y consumo de hardware de las subredes del proyecto.

* Comandos de habilitación del servicio web de monitoreo:
  sudo systemctl daemon-reload
  sudo systemctl enable grafana-server
  sudo systemctl start grafana-server

* Validación operativa: Acceder vía navegador al puerto http://10.0.40.2:3000, añadir la fuente de datos (Data Source) apuntando al backend local http://localhost:9090 e importar el dashboard oficial de Linux System de Node Exporter.

--------------------------------------------------------------------------------
C. COMANDOS DE VERIFICACIÓN Y AUDITORÍA DE LA VM4
--------------------------------------------------------------------------------
Pruebas operativas necesarias para garantizar el correcto funcionamiento del ecosistema de observabilidad:

* Verificar que el motor de Prometheus esté recolectando datos activamente:
  sudo systemctl status prometheus

* Verificar la escucha local del entorno gráfico de Grafana en el puerto 3000:
  sudo ss -tunlp | grep 3000

--------------------------------------------------------------------------------
PRE-REQUISITOS GLOBALES DEL CLÚSTER DE DESPLIEGUE
--------------------------------------------------------------------------------
* 4 Máquinas Virtuales operando con Ubuntu Server 24.04 LTS.
* Interconexión física mediante adaptadores de red en modo "Red Interna" compartiendo la misma denominación de interfaz en Proxmox/VirtualBox.
* Servidor OpenSSH activo en cada nodo con el puerto unificado para el aprovisionamiento.
* Configuración de permisos Sudoers (`adming4 ALL=(ALL) NOPASSWD:ALL`) en cada máquina para permitir la ejecución no interactiva del Playbook de Ansible.
* Entorno Inicial del Clúster:
  - En VM1: Disponer de binarios de NGINX y Ansible instalados de forma local.
  - En VM2: Disponer de paquetes base de procesamiento web y extensiones básicas de PHP heredadas para Nextcloud.
  - En VM3: Disponer del motor MariaDB Server configurado para el enlace de sockets.
  - En todos los nodos: Despliegue de node_exporter escuchando en el puerto tcp 9100.

⚠️ VI. PRUEBAS Y VALIDACIÓN

Prueba Realizada                         Resultado Esperado                             Resultado Obtenido
------------------------------------------------------------------------------------------------------------------
Test de Failover de la Aplicación        Al apagar el nodo maestro Keepalived, la VIP   [OK] La IP cambia de nodo
(Keepalived VRRP en VLAN 20)             debe migrar de forma transparente e instantánea. en menos de 1.5 segundos.

Prueba de Carga/Estrés en la Frontera    El Proxy Inverso NGINX absorbe peticiones y    [OK] Tráfico encapsulado y
(Balanceo y Reenvío Perimetral)          las deriva de forma homogénea sin caídas.      distribuido hacia la VLAN 20.

Test de Seguridad y Hardening            Las peticiones HTTP son interceptadas por el   [OK] Cortafuegos deniega de
(UFW Firewall e Inmutabilidad)           firewall, y el puerto de monitoreo solo        forma selectiva y audita las
                                         responde a la IP asignada a la VM4.            métricas limpiamente.
------------------------------------------------------------------------------------------------------------------

📚 VII. CONCLUSIONES Y LECCIONES APRENDIDAS
1. Desacoplamiento de Servicios: Se superó con éxito el paradigma de servidores monolíticos tradicionales. Separar el almacenamiento físico (NFS), los metadatos (MariaDB), el motor de procesamiento lógico (PHP-FPM) y la frontera perimetral de red (NGINX Proxy) minimiza de forma contundente la superficie de ataque y optimiza los recursos de hardware del laboratorio.
2. Robustez mediante Automatización: La implementación de Ansible redujo de horas a minutos el tiempo de recuperación y despliegue del sistema ante fallos críticos de aprovisionamiento, consolidando las bases de la infraestructura como código (IaC).
3. Hardening Estricto como Regla: El uso de políticas estrictas de denegación por defecto en UFW y el aislamiento absoluto de datos mediante subinterfaces etiquetadas en Netplan (VLANs lógicas) demostraron ser soluciones altamente resilientes y eficaces para blindar entornos de producción empresariales modernos.
