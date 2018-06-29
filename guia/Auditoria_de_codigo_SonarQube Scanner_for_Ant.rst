Auditoria de código con SonarQube Scanner for Ant
===========================================

En este ejercicio vamos a verifcar con "SonarQube Scanner for Ant" un código de prueba creado por nosotros y lo vamos enviar al dashboard de SonarQube 6.4.0

El "SonarQube Scanner for Ant" hace el analisis desde ANT.


Ir al link https://www.sonarqube.org/downloads/ 
Buscar el link de la "Documentation and Download" http://redirect.sonarsource.com/doc/analyzing-source-code.html
Buscar el link "SonarQube Scanner" http://redirect.sonarsource.com/doc/analyzing-source-code.html
En el ulimo link "SonarQube Scanner for Ant" se encuentran los link de descarga y el manual oficial.

Debemos tener instalado el "SonarQube 6.4.0" y verificar que este iniciado y podamos ver el dashboard por el http://localhost:9000. 

Creamos nuestro codigo de ejemplo en Java
++++++++++++++++++++++++++++++++++++++++++

Tenemos dos fuentes en la carpeta sources uno llamado "HolaMundo.java".::

	/*
	* Hola Mund..... Ejemplo
	*/

	public class HolaMundo {

		public static void main (String args[]){

			System.out.println("Hola Mundo !!!" + args[0]);
		}
	}

y otro "BuenasMundo.java".::

	/*
	* Hola Mund..... Ejemplo
	*/

	public class BuenasMundo {

		public static void main (String args[]){

			System.out.println("Buenas Mundo !!!");
		}
	}


Asi seria el **build.xml**.::

	<project name="mi proyecto" default="all">
	  <target name="all" depends="build,pack" />

	  <target name="build">
	    <mkdir dir="bin" />
	    <!-- esto indica que compile todo lo que haya bajo sources y ponga las clases resultantes en bin -->
	    <javac srcdir="sources" destdir="bin" includes="**/*.java">
	      <!-- este es un classpath de ejemplo: un directorio lib al mismo nivel que sources, incluimos todos los jars que contenga -->
	      <classpath>
		<fileset dir="lib" includes="*.jar" />
	      </classpath>
	    </javac>
	  </target>

	  <target name="pack">
	    <jar file="SaludandoMundo.jar">
	      <!-- incluimos todas las clases bajo bin -->
	      <fileset dir="bin" includes="**/*.class" />
	      <!-- incluimos tambien los properties que estan directamente bajo sources (sin recursion) -->
	      <fileset dir="sources" includes="*.properties" />
	      <manifest>
		<attribute name="Main-Class" value="HolaMundo" />
	      </manifest>
	    </jar>
	  </target>

	</project>

Ya con el XML guardado con el nombre de  **build.xml** en el mismo nivel que el directorio sources, simplemente en ese directorio teclea "ant" y listo, te va a compilar todas las clases, las pondrá en un directorio bin y al final empaca todo lo compilado en un JAR (junto con los properties que tengas en sources) y hasta puedes definir atributos para el Manifest de tu jar.

Ejecutamos el ant.::

	# ant
	Buildfile: /home/cgomez/tmp/java/build.xml

	build:
	    [javac] /home/cgomez/tmp/java/build.xml:7: warning: 'includeantruntime' was not set, defaulting to build.sysclasspath=last; set to false for repeatable builds

	pack:

	all:

	BUILD SUCCESSFUL
	Total time: 0 seconds

Y probamos el .jar.::

	# java -jar SaludandoMundo.jar Carlos
	Hola Mundo !!!Carlos


Instalar "SonarQube Scanner for Ant 2.5" 
++++++++++++++++++++++++++++++++++++++++

Descargamos el "SonarQube Scanner for Ant 2.5"::

	$ wget https://sonarsource.bintray.com/Distribution/sonarqube-ant-task/sonarqube-ant-task-2.5.jar


Configurar "SonarQube Scanner for Ant 2.5" 
++++++++++++++++++++++++++++++++++++++++++

Ingresamos a la carpeta de trabajos de "SonarQube Scanner"

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
	
Se procede a ejecutar el "SonarQube Scanner for Ant 2.5".::

	$ ./sonar-scanner


Cuando culmine el análisis, nos vamos al dashboard de "SonarQube 6.4.0" en la URL http://localhost:9000 y ahi debemos ver nuestro proyecto, esto puede demorar un poco.





