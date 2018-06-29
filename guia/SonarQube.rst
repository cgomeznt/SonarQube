¿Qué es SonarQube?
==================

SonarQube es una plataforma de código abierto para el análisis de la calidad de código. 

Es una herramienta para gestionar la calidad del código. La herramienta controla la calidad en los siguiente ejes:
Source:
	Architecture & Design
	Duplications
	Unit test
	Complesity
	Potential bugs
	Coding rules
	Comments

Las plataformas soportadas actualmente son más de 20 lenguajes (Java, Javascript, Cobol, PL, C#...). Se puede extender a través de plugins, tanto para agragar nuevos lenguajes.

Tiene plugins para Eclipse, para Hudson y jenkins, Maven, Reportes,....Métricas adicionales...

SonarQube tiene numerosas y extensas reglas de Java. Existen más de 700 reglas de Java, y esta cantidad aumenta constantemente gracias a la comunidad


* SonarQube NO es una herramienta de construcción (build tools). Hay muchas herramientas disponibles ahí fuera que ya hacen las cosas tremendamente bien, como pueden ser: Maven, Ant, Gradle, etc. SonarQube  espera que antes de analizar un proyecto, el proyecto haya sido compilado y construido con tu herramienta favorita. SonarQube no es intrusivo en el proceso de desarrollo, no tienes que cambiar tu forma de construir tus proyectos.

* SonarQube NO es (solamente) una herramienta de análisis estático de código. SonarQube no sustituye a Findbugs o a CPPCheck o cualquier otra herramienta similar. Todo lo contrario, SonarQube, además de ofrecer su propio motor de análisis estático de código, permite integrarlo con todas esas herramientas externas. La consecuencia es que puedes obtener de forma homogénea, y agrupada todas las evidencias detectadas por esa gran variedad de herramientas de análisis estático y dinámico. Todo integrado en un único cuadro de mandos.

* SonarQube NO es una herramienta de cobertura de código, claramente no lo es. Igual que antes, se integra con las herramientas más populares de cobertura de código como JaCoCo, Cobertura, PHPUnit, NCover, … pero no calcula la cobertura de código de forma propia. Simplemente lee los informes de pruebas pre-generados y los muestra en un cuadro de mando más usable y conveniente. Una vez más, SonarQube, no es intrusivo en el proceso de desarrollo, si ya estás haciendo pruebas unitarias, basta con que le digas donde están los resultados.

* SonarQube NO es una herramienta de formateo de código. Nunca modificará una sola línea de código de tu proyecto. Lo que si que puede hacer es revisar el estilo para ver si cumple con ciertas convenciones de código, puedes utilizar tus reglas de Checkstyle, CPPCheck, ScalaStyle, … o implementar las tuyas propias.

* SonarQube NO es solo otra herramienta de revisión manual de código. Por supuesto que ofrece un mecanismo que facilita la revisión de código, pero no solo eso. Esta muy ligado al mecanismo de detección de evidencias de incumplimiento de buenas prácticas, por lo que cada revisión de código puede asociarse fácilmente a la parte concreta del código problemático y al desarrollador que lo ha escrito.

* SonarQube NO solo gestiona la calidad del código Java. Esa época se acabo hace mucho tiempo. Por defecto, en su versión “community” puedes analizar código en los lenguajes más populares de hoy en día: Java, C#.NET, JavaScript, PHP, … y si necesitas más, existen otros plugins opensource y comerciales para cubrir hasta más de 20 lenguajes de desarrollo. No dejes que te engañen, SonarQube ya es independiente de la tecnología de desarrollo y no está ligado a Java.

* SonarQube NO necesita Maven para analizar tu proyecto. Hace ya mucho tiempo que se desacopló de Maven, aunque por supuesto, puedes seguir usándolo. La realidad es que SonarQube dispone de un analizador autónomo (sonar-runner) que se encarga de llevar a cabo todo el análisis independientemente de tu tecnología y de tus herramientas de construcción. SonarQube nunca te va a pedir que cambies absolutamente nada en tu proceso de desarrollo, simplemente debes añadir un paso más para la inspección de código y empezar a medir.

* SonarQube NO solo usa reglas para validar la calidad. Además de las reglas, calcula muchas métricas referentes al código fuente (complejidad, código duplicado, deuda técnica, ...) y permite extender la funcionalidad para crear tus propias métricas y tus propios modelos de calidad.

* SonarQube NO solo permite ver la calidad desde la aplicación web. Por supuesto que cualquiera que quiera conocer todos los detalles de la calidad de su producto puede acceder vía web a la herramienta, ¿pero qué pasa con los desarrolladores? Los desarrolladores siempre están trabajando con su IDE favorito y no les gusta cambiar. SonarQube provee de extensiones para los principales IDEs (Eclipse, IntelliJ, VisualStudio) de forma que los desarrolladores no tengan que salir de su entorno favorito para saber si están haciendo las cosas con los niveles de calidad adecuados. Es más, los desarrolladores pueden incluso ejecutar análisis locales para así revisar las evidencias antes de subir el código al control de versiones.

¿qué podemos decir que es SonarQube?
++++++++++++++++++++++++++++++++++++

SonarQube es una plataforma de gestión de la calidad del código que permite a los equipos de desarrollo gestionar, hacer seguimiento y mejorar la calidad de su código fuente. 

Es una herramienta que mantiene datos históricos de una gran variedad de métricas y proporciona tendencias de los indicadores de referencia para no cometer los pecados capitales del desarrollo de software.
