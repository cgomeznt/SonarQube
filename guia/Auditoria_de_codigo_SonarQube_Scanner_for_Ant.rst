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

Copiamos el "SonarQube Scanner for Ant 2.5" 
+++++++++++++++++++++++++++++++++++++++++++

Copiamos el "SonarQube Scanner for Ant 2.5" en un directorio donde luego lo podamos llamar desde el "build.xml"


Configurar "SonarQube Scanner for Ant 2.5" 
++++++++++++++++++++++++++++++++++++++++++

Ingresamos al carpeta de nuestro proyecto, editamos el "build.xml" y agregamos las siguientes lineas como nos lo indica la pagina oficial https://docs.sonarqube.org/display/SCAN/Analyzing+with+SonarQube+Scanner+for+Ant

::

	$ vi build.xml

	<project name="My Project" default="all" basedir="." xmlns:sonar="antlib:org.sonar.ant">
	  <target name="all" depends="build,pack" />

	<!-- Define the SonarQube global properties (the most usual way is to pass these properties via the command line) -->
	<property name="sonar.host.url" value="http://localhost:9000" />
	 
	  
	<!-- Define the SonarQube project properties -->
	<property name="sonar.projectKey" value="org.sonarqube:sonarqube-scanner-ant" />
	<property name="sonar.projectName" value="Example Basic of SonarQube Scanner for Ant Usage" />
	<property name="sonar.projectVersion" value="1.0" />
	<property name="sonar.sources" value="sources" />
	<property name="sonar.java.binaries" value="bin" />
	<property name="sonar.java.libraries" value="lib" />

	<!-- Define SonarQube Scanner for Ant Target -->
	<target name="sonar">
	    <taskdef uri="antlib:org.sonar.ant" resource="org/sonar/ant/antlib.xml">
		<!-- Update the following line, or put the "sonarqube-ant-task-*.jar" file in your "$HOME/.ant/lib" folder -->
		<classpath path="/home/cgomez/Documentos/app/sonarqube/sonarqube-ant-task-2.5.jar" />
	    </taskdef>
	 
	    <!-- Execute SonarQube Scanner for Ant Analysis -->
	    <sonar:sonar />
	</target>

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

Ahora podemos ejecutar el "ant" para que realice el analisis y lo mande al frontend sonarqube.::

	$ ant sonar
	Buildfile: /home/cgomez/Documentos/app/sonarqube/ejemplo/build.xml

	sonar:
	[sonar:sonar] Apache Ant(TM) version 1.9.9 compiled on March 1 2017
	[sonar:sonar] SonarQube Ant Task version: 2.5
	[sonar:sonar] Loaded from: file:/home/cgomez/Documentos/app/sonarqube/sonarqube-ant-task-2.5.jar
	[sonar:sonar] User cache: /home/cgomez/.sonar/cache
	[sonar:sonar] Load global settings
	[sonar:sonar] Load global settings (done) | time=59ms
	[sonar:sonar] User cache: /home/cgomez/.sonar/cache
	[sonar:sonar] Load plugins index
	[sonar:sonar] Load plugins index (done) | time=5ms
	[sonar:sonar] Default locale: "es_VE", source code encoding: "UTF-8" (analysis is platform dependent)
	[sonar:sonar] Process project properties
	[sonar:sonar] Load project repositories
	[sonar:sonar] Load project repositories (done) | time=398ms
	[sonar:sonar] Load quality profiles
	[sonar:sonar] Load quality profiles (done) | time=26ms
	[sonar:sonar] Load active rules
	[sonar:sonar] Load active rules (done) | time=932ms
	[sonar:sonar] Load metrics repository
	[sonar:sonar] Load metrics repository (done) | time=101ms
	[sonar:sonar] SCM provider autodetection failed. No SCM provider claims to support this project. Please use sonar.scm.provider to define SCM of your project.
	[sonar:sonar] Publish mode
	[sonar:sonar] Project key: org.sonarqube:sonarqube-scanner-ant
	[sonar:sonar] -------------  Scan Example Basic of SonarQube Scanner for Ant Usage
	[sonar:sonar] Load server rules
	[sonar:sonar] Load server rules (done) | time=256ms
	[sonar:sonar] Initializer GenericCoverageSensor
	[sonar:sonar] Initializer GenericCoverageSensor (done) | time=1ms
	[sonar:sonar] Base dir: /home/cgomez/Documentos/app/sonarqube/ejemplo
	[sonar:sonar] Working dir: /home/cgomez/Documentos/app/sonarqube/ejemplo/.sonar
	[sonar:sonar] Source paths: sources
	[sonar:sonar] Source encoding: UTF-8, default locale: es_VE
	[sonar:sonar] Index files
	[sonar:sonar] 2 files indexed
	[sonar:sonar] Quality profile for java: Sonar way
	[sonar:sonar] Sensor JavaSquidSensor [java]
	[sonar:sonar] Configured Java source version (sonar.java.source): none
	[sonar:sonar] JavaClasspath initialization
	[sonar:sonar] JavaClasspath initialization (done) | time=13ms
	[sonar:sonar] JavaTestClasspath initialization
	[sonar:sonar] JavaTestClasspath initialization (done) | time=0ms
	[sonar:sonar] Java Main Files AST scan
	[sonar:sonar] 2 source files to be analyzed
	[sonar:sonar] Java Main Files AST scan (done) | time=1005ms
	[sonar:sonar] 2/2 source files have been analyzed
	[sonar:sonar] Java Test Files AST scan
	[sonar:sonar] 0 source files to be analyzed
	[sonar:sonar] Java Test Files AST scan (done) | time=2ms
	[sonar:sonar] Sensor JavaSquidSensor [java] (done) | time=1907ms
	[sonar:sonar] Sensor SurefireSensor [java]
	[sonar:sonar] parsing /home/cgomez/Documentos/app/sonarqube/ejemplo/target/surefire-reports
	[sonar:sonar] Sensor SurefireSensor [java] (done) | time=1ms
	[sonar:sonar] Sensor JaCoCoSensor [java]
	[sonar:sonar] Sensor JaCoCoSensor [java] (done) | time=0ms
	[sonar:sonar] Sensor SonarJavaXmlFileSensor [java]
	[sonar:sonar] Sensor SonarJavaXmlFileSensor [java] (done) | time=0ms
	[sonar:sonar] Sensor Analyzer for "php.ini" files [php]
	[sonar:sonar] 0/0 source files have been analyzed
	[sonar:sonar] Sensor Analyzer for "php.ini" files [php] (done) | time=3ms
	[sonar:sonar] Sensor Zero Coverage Sensor
	[sonar:sonar] Sensor Zero Coverage Sensor (done) | time=61ms
	[sonar:sonar] Sensor CPD Block Indexer
	[sonar:sonar] Sensor CPD Block Indexer (done) | time=26ms
	[sonar:sonar] No SCM system was detected. You can use the 'sonar.scm.provider' property to explicitly specify it.
	[sonar:sonar] 2 files had no CPD blocks
	[sonar:sonar] Calculating CPD for 0 files
	[sonar:sonar] CPD calculation finished
	[sonar:sonar] Analysis report generated in 166ms, dir size=22 KB
	[sonar:sonar] Analysis reports compressed in 42ms, zip size=8 KB
	[sonar:sonar] Analysis report uploaded in 17ms
	[sonar:sonar] ANALYSIS SUCCESSFUL, you can browse http://localhost:9000/dashboard/index/org.sonarqube:sonarqube-scanner-ant
	[sonar:sonar] Note that you will be able to access the updated dashboard once the server has processed the submitted analysis report
	[sonar:sonar] More about the report processing at http://localhost:9000/api/ce/task?id=AWSU23xuSURXzBTx_hqy
	[sonar:sonar] Task total time: 6.078 s

	BUILD SUCCESSFUL
	Total time: 7 seconds


Cuando culmine el análisis, nos vamos al dashboard de "SonarQube 6.4.0" en la URL http://localhost:9000 y ahi debemos ver nuestro proyecto, esto puede demorar un poco.


Si queremos que con solo ejecutar el "ant" ejecute todo, editamos.::

	  <target name="all" depends="build,pack" />

	y la cambiamos por esta

	  <target name="all" depends="build,pack,sonar" />

Y ahora solo ejecutamos el "ant".::

	$ ant





