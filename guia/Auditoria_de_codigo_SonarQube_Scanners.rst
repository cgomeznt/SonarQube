Auditoria de código con SonarQube Scanners
===========================================

En este ejercicio vamos a verifcar con "SonarQube Scanners" un código de prueba descargado de "https://github.com/jaruzafa/fitnesse_CI_DEMO" y lo vamos enviar al dashboard de SonarQube 6.4.0

El "SonarQube Scanners" es recomendado para ejecutar por defecto el análisis de un proyecto. este análisis es desde la linea de comandos cuando no tenemos otro analizador apropiado.


Ir al link https://www.sonarqube.org/downloads/ 
Buscar el link de la "Documentation and Download" http://redirect.sonarsource.com/doc/analyzing-source-code.html
Buscar el link "SonarQube Scanners" https://docs.sonarqube.org/display/SCAN/Analyzing+with+SonarQube+Scanner
En el ulimo link "SonarQube Scanners" se encuentran los link de descarga y el manual oficial.

Debemos tener instalado el "SonarQube 6.4.0" y verificar que este iniciado y podamos ver el dashboard por el http://localhost:9000. 

Descargar el código de prueba de fitnesse
++++++++++++++++++++++++++++++++++++++++++

Este es un código que vamos a utilizar como prueba.::

	$ git clone https://github.com/jaruzafa/fitnesse_CI_DEMO.git


Instalar "SonarQube Scanner 3.2" 
++++++++++++++++++++++++++++++++

Descargamos el "SonarQube Scanner 3.2"::

	$ wget https://sonarsource.bintray.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-3.2.0.1227-linux.zip


Descomprimimos el "SonarQube Scanner 3.2"::

	$ unzip sonar-scanner-cli-3.2.0.1227-linux.zip
	

Configurar "SonarQube Scanner 3.2" 
+++++++++++++++++++++++++++++++++++

Ingresamos a la carpeta de trabajos de "SonarQube Scanner"::

	$ cd sonar-scanner-3.2.0.1227-linux/

Modificamos el archivo de configuración para que utilice el "SonarQube 6.4.0".::

	$ vi conf/sonar-scanner.properties

	Buscamos esta linea

	#----- Default SonarQube server
	#sonar.host.url=http://localhost:9000

	Y la descomentamos para dejarla así

	#----- Default SonarQube server
	sonar.host.url=http://localhost:9000

Ingresamos en el directorio "bin" para crear el archivo de propiedades. Debemos crear un archivo llamado "sonar-project.properties" y con un contenido que podemos obtener de este link https://docs.sonarqube.org/display/SCAN/Analyzing+with+SonarQube+Scanner.::

	$ cd bin/

	$ vi sonar-project.properties
	# must be unique in a given SonarQube instance
	sonar.projectKey=my:project
	# this is the name and version displayed in the SonarQube UI. Was mandatory prior to SonarQube 6.1.
	sonar.projectName=My project
	sonar.projectVersion=1.0
	 
	# Path is relative to the sonar-project.properties file. Replace "\" by "/" on Windows.
	# This property is optional if sonar.modules is set. 
	sonar.sources=./codigo
	 
	# Encoding of the source code. Default is default system encoding
	sonar.sourceEncoding=UTF-8


Creamos un acceso directo llamado "main" justo en donde creamos el archivo "sonar-project.properties" del código que queremos verificar.::

	$ ln -s ../../fitnesse_CI_DEMO/ ./codigo                         
	
Se procede a ejecutar el "SonarQube Scanner 3.2".::

	$ ./sonar-scanner


Cuando culmine el análisis, nos vamos al dashboard de "SonarQube 6.4.0" en la URL http://localhost:9000 y ahi debemos ver nuestro proyecto, esto puede demorar un poco.





