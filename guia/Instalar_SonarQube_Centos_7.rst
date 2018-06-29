Instalar SonarQube
==================

SonarQube es una herramienta software libre para el análisis de la calidad de código, esta escrito en Java y soporta múltiples Base de Datos. Provee la capacidad de inspección de código continuo, muestra la salud de la aplicación. Contiene un analizador de código que esta equipado para detectar errores.

Instalaremos la ultima versión de SonarQube en CentOS 7.

Perform a system update
+++++++++++++++++++++++

Antes de instalar cualquier paquete en CentOS server, es recomendado actualizar el sistema.::

	sudo yum -y install epel-release
	sudo yum -y update
	sudo shutdown -r now

Instalando Java
++++++++++++++++++

Descargamos desde Oracle los paquetes RPM de los JDK que vamos a requerir y lo instalamos.::

	$ wget --no-cookies --no-check-certificate --header "Cookie:oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/8u131-b11/d54c1d3a095b4ff2b6607d096fa80163/jdk-8u131-linux-x64.rpm"

	$ sudo yum -y localinstall jdk-8u131-linux-x64.rpm

	$ java -version

Instalamos y configuramos PostgreSQL
++++++++++++++++++++++++++++++++++++

Instalamos el repositorio de PostgreSQL.::

	$ sudo rpm -Uvh https://download.postgresql.org/pub/repos/yum/9.6/redhat/rhel-7-x86_64/pgdg-centos96-9.6-3.noarch.rpm

Instalamos PostgreSQL server desde el repositorio.::

	$ sudo yum -y install postgresql96-server postgresql96-contrib

Inicializar la base de datos.::

	$ sudo /usr/pgsql-9.6/bin/postgresql96-setup initdb

Editamos /var/lib/pgsql/9.6/data/pg_hba.conf para habilitar el MD5-based authentication.::

	$ sudo vi /var/lib/pgsql/9.6/data/pg_hba.conf

	Buscar las siguientes palabras y cambiarlas por, peer por trust and ident por md5

	# TYPE  DATABASE        USER            ADDRESS                 METHOD

	# "local" is for Unix domain socket connections only
	local   all             all                                     peer
	# IPv4 local connections:
	host    all             all             127.0.0.1/32            ident
	# IPv6 local connections:
	host    all             all             ::1/128                 ident
	
	Una vez actualizado, la configuracion debe quedar como esto.

	# TYPE  DATABASE        USER            ADDRESS                 METHOD

	# "local" is for Unix domain socket connections only
	local   all             all                                     trust
	# IPv4 local connections:
	host    all             all             127.0.0.1/32            md5
	# IPv6 local connections:
	host    all             all             ::1/128                 md5

Iniciar PostgreSQL server y habilitarlo para que quede de forma automatica su inicio luego de un restart.::

	$ sudo systemctl start postgresql-9.6
	$ sudo systemctl enable postgresql-9.6

Cambiamos el password de postgres.::

	$ sudo passwd postgres

Nos cambiamos al usuario de postgres.::

	$ su - postgres

Creamos un nuevo usuario.::

	$ createuser sonar

Nos cambiamos al SHELL de PostgreSQL.::

	$ psql

Cambiamos la clave del nuevo usuario creado para la base de datos de SonarQube.::

	postgres=# ALTER USER sonar WITH ENCRYPTED password 'r00tme';

Crear la nueva base de datos para SonarQube.::

	postgres=# CREATE DATABASE sonar OWNER sonar;

Nos salimos del SHELL de psql y del usuario nuevo.::

	postgres=# \q

Descargar e Instalar SonarQube
++++++++++++++++++++++++++++++

Descargamos el ultimo SonarQube LTS. Podemos ver siempre la ultima versión desde https://www.sonarqube.org/downloads/

	$ wget https://sonarsource.bintray.com/Distribution/sonarqube/sonarqube-6.7.4.zip


Instalamos unzip.::

	$ sudo yum -y install unzip


Descomprimimos el archivo.::

	$ sudo unzip sonarqube-6.4.zip -d /opt


Renombramos el directorio.::

	$ sudo mv /opt/sonarqube-6.7.4/ /opt/sonarqube


Abrir el archivo de configuracion de SonarQube.::

	$ sudo vi /opt/sonarqube/conf/sonar.properties

	Buscar las siguientes lineas.

	#sonar.jdbc.username=
	#sonar.jdbc.password=

	Descomentar y suministrar los datos

	sonar.jdbc.username=sonar
	sonar.jdbc.password=r00tme

Buscar esta otra, Descomentar la linea y salvar.::

	$ #sonar.jdbc.url=jdbc:postgresql://localhost/sonar


Configurar el servicio Systemd 
++++++++++++++++++++++++++++++

SonarQube puede iniciar directamente usando el script que viene en el instalador. Usted puede configurar el SystemD para SonarQube.::

	$ sudo vi /etc/systemd/system/sonar.service

	[Unit]
	Description=SonarQube service
	After=syslog.target network.target

	[Service]
	Type=forking

	ExecStart=/opt/sonarqube/bin/linux-x86-64/sonar.sh start
	ExecStop=/opt/sonarqube/bin/linux-x86-64/sonar.sh stop

	User=root
	Group=root
	Restart=always

	[Install]
	WantedBy=multi-user.target



Iniciamos la aplicación de SonarQube y lo habilitamos.::

	$ sudo systemctl start sonar
	$ sudo systemctl enable sonar
	$ sudo systemctl status sonar


Configuramos un reverse proxy
++++++++++++++++++++++++++++++

Por defecto SonarQube escucha por localhost puerto 9000. En este tutorial utilizaremos Apache como reverse proxy para que pueden acceder a la aplicación por el HTTP.::

	$ sudo yum -y install httpd

Creamos el Virtual Host.::

	$ sudo vi /etc/httpd/conf.d/sonar.yourdomain.com.conf


Iniciar Apache y habilitarlo de forma automatica.::

	$ sudo systemctl start httpd
	$ sudo systemctl enable httpd


Configurar el Firewall.
+++++++++++++++++++++++

Permitimos las peticiones por el puerto HTTP en el firewall.::

	$ sudo firewall-cmd --add-service=http --permanent
	$ sudo firewall-cmd --reload

Iniciamos el servicio de SonarQube.::

	$ sudo systemctl start sonar


Verificamos que ya podemos ingresar por la URL::

	http://sonar.yourdomain.com





