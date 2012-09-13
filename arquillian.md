# arquillian

.fx: home

![Arquillian](arquillian_logo_600px.png)

<span class="cite">http://arquillian.org/community</span>

---

# ¿Qué es arquillian?

.fx: statement

Es una plataforma de tests extensible para la JVM creada por la gente de JBoss. 
<br/>Es un proyecto open source.

![jboss](default/css/jboss_redhat_branding.png)

<span class="cite">http://arquillian.org/invasion</span>

---
# Disclaimer

.fx: bigbullets

- No trabajo ni estoy relacionado en ninguna manera con JBoss
- El estilo de esta presentación, así como las imágenes, están fusilados de la web arquillian.org

<span class="cite">http://arquillian.org/invasion/spread</span>


---

# ¿Qué promete?

.fx: statement

Permitir a los desarrolladores crear fácilmente tests automáticos de integración, funcionales y de aceptación para los middleware Java.

<span class="cite">http://arquillian.org/invasion</span>

---

# ¿Cómo?

.fx: statement

arquillian se encarga de la toda la fontanería, como la administración del contenedor, el despliegue y la inicialización de los frameworks para que te puedas concentrar en la tarea de escribir tus tests.

<span class="cite">http://arquillian.org/invasion</span>

---

# ¿Cómo?

.fx: bullets

arquillian cubre todos los aspectos de la ejecución de los tests:

- Administra el ciclo de vida del contenedor
- Empaqueta el test y las dependencias del test en un archivo (o archivos) de shrikwrap
- Despliega el archivo en el contenedor o contenedores
- Mejora el test proporcionando, entre otros, inyección de dependencias
- Ejecuta el test en el contenedor o contra el contenedor
- Captura los resultados y los devuelve al ejecutador de tests para que informe


<span class="cite">http://arquillian.org/invasion</span>

---

# Principios 

.fx: bigbullets

- Los tests deben poderse ejecutar en cualquier contenedor soportado
- Los tests deben poderse ejecutar desde el entorno de desarrollo y la herramienta de build.
- La plataforma debe integrarse o extender sistemas de testing existentes, como JUnit o TestNG

<span class="cite">http://arquillian.org/invasion</span>

---

# ¿Qué aspecto tiene?

.fx: code

    !java
    @RunWith(Arquillian.class) // (1)
    public class GreeterTest {

	    @Deployment // (2)
	    public static JavaArchive createDeployment() {
		    return ShrinkWrap.create(JavaArchive.class)
			    .addClass(Greeter.class)
			    .addAsManifestResource(EmptyAsset.INSTANCE, "beans.xml");
	    }

	    @Inject // (3)
	    Greeter greeter;

	    @Test // (4)
	    public void should_create_greeting() {
		assertEquals("Hello, Earthling!", greeter.greet("Earthling"));
	    }
    }

<span class="cite">http://arquillian.org</span>

---

# @RunWith

.fx: bigbullets

    !java
    @RunWith(Arquillian.class) // (1)
    public class GreeterTest {...


- Especifica que el runner que vamos a usar es arquillian

---

# @Inject

.fx: bigbullets

    !java
        @Inject 
        Greeter greeter;
    
- Tenemos disponible inyección de recursos en el test

![success](default/css/arquillian_ui_success_256px.png)

---
# Test

    !java
    @Test // (4)
    public void should_create_greeting() {
	    assertEquals("Hello, Earthling!", greeter.greet("Earthling"));
    }

- Éste es el código del test. 
- Dependiendo de la configuración del test se ejecutará en el contenedor o fuera de él
- En este caso se ejecuta EN el contenedor, sobre <code>greeter</code> que es una instancia inyectada, pero...

---
# @Deployment
    !java
     @Deployment 
     public static JavaArchive createDeployment() {
       return ShrinkWrap.create(JavaArchive.class)
   	   .addClass(Greeter.class)
   	   .addAsManifestResource(EmptyAsset.INSTANCE, "beans.xml");
     }

- Especificamos el desplegable que queremos probar usando ShrinkWrap
- Permite centrarnos en el código específico para el test.
- Nos permite también introducir descriptores de despliegue para activar y configurar los diversos servicios del contenedor, como por ejemplo:
    - JPA, CDI
    - En los contenedores soportados, creación de DataSources específicos para el test, colas JMS, etc

---
# Shrinkwrap (Cons)

.fx: right-image

- Tenemos que especificar el despliegue que queremos probar
- Esto implica especificar:
    - Clases
    - Recursos
    - Descriptores
    - Librerías
- Puede ser tedioso y llevar tiempo

![anger](default/css/arquillian_ui_error_256px.png)

---

# Shrinkwrap (Pros)

.fx: right-image

- Nos permite conocer con detalle las dependencias del código que estamos probando
- Dispone de múltiples herramientas, como resolvers de Maven, o la capacidad de trabajar con ficheros existentes (como un build de Maven)
- Su arquitectuta incremental nos permite reutilizar lógica de creación de los despliegues
- Esta arquitectura permite que arquillian despliegue automáticamente aparataje necesario para la ejecución de los tests en presencia de extensiones, además del propio test

![anger](default/css/arquillian_ui_success_256px.png)

---
# Resultado

.fx: statement

![Resultado](arquillian_tutorial_junit_green_bar.png)

¡El resultado se muestra en nuestro ejecutador de tests!

---
#Diferentes usos

- Como hemos dicho arquillian extiende sistemas de test existentes.
- La integración con el IDE nos permite usar arquillian durante el desarrollo de nuestra aplicación para comprobar que en nuestro avance no vamos rompiendo la aplicación. 
- La integración con sistemas de build nos permite usar arquillian para, por ejemplo, sistemas de integración continua. 
- Al ser extensible podemos usarlo para tests unitarios, funcionales, de integración, BDD... 

---
#Plataforma de estudio

.fx: statement

arquillian nos facilita aprender a utilizar los diferentes middlewares. </br>
Nos proporciona maneras de probar las funcionalidades de los middlewares y asegurarnos de que se ajustan a nuestras necesidades.

---

# ¿Cómo se configura?

.fx: bigbullets

* arquillian consta de modulos independientes que proporcionan funcionalidad concreta.
* La presencia del jar en el classpath "activa" el módulo
* Es recomendable usar Maven para la gestión de las dependencias.
---

# ¿Cómo se configura?

    !xml
	<dependencyManagement>
	    <dependencies>
		    <dependency>
			    <groupId>org.jboss.arquillian</groupId>
			    <artifactId>arquillian-bom</artifactId>
			    <version>1.0.2.Final</version>
			    <scope>import</scope>
			    <type>pom</type>
		    </dependency>
	    </dependencies>
	</dependencyManagement>
	<dependency>
	    <groupId>junit</groupId>
	    <artifactId>junit</artifactId>
	    <version>4.8.1</version>
	    <scope>test</scope>
	</dependency>
	<dependency>
	    <groupId>org.jboss.arquillian.junit</groupId>
	    <artifactId>arquillian-junit-container</artifactId>
	    <scope>test</scope>
	</dependency>

---
# ¿Cómo se configura?
    
    !xml
    <dependency>
	    <groupId>org.jboss.as</groupId>
	    <artifactId>jboss-as-arquillian-container-remote</artifactId>
	    <version>7.1.1.Final</version>
	    <scope>test</scope>
    </dependency>

- Tenemos que configurar el contenedor sobre el que vamos a ejecutar el test
- En este caso es un contenedor remoto que tiene que estar ejecutandose
- Un fichero arquillian.xml nos permite afinar la configuración de los contenedores y las extensiones

---

# Contenedores soportados

.fx: right-image

- Soporta diferentes contenedores mediante el uso de extensiones (SPI)
- Los contenedores pueden ser:
    - Incrustados (embedded)
    - Administrados (managed)
    - Remotos (remote)
- ¡Ojo! Cada tipo de contenedor tiene sus ventajas y sus inconvenientes. Conviene saber siempre las propiedades del contenedor que estamos usando. 

![warn](default/css/arquillian_ui_warning_256px.png)

---

# Contenedores soportados

- Actualmente solo se puede tener un módulo de contenedor activado al mismo tiempo
- Mediante perfiles de Maven podemos cambiar el contenedor que queremos usar
- Útil también para las integraciones con los IDE
- JBoss, GlassFish, Weld, Tomcat, Jetty, Weblogic, WebSphere, Spring ...
- Diferentes contenedores soportan diferentes perfiles

---

# Contras

.fx: right-image

- Falta de control de classpath
- Falta de modularidad en Java
- Los cambios de contenedores se tienen que hacer con perfiles de classpath
- Las cosas se vuelven poco intuitivas, sobre todo usando contenedores embebidos

![sob](default/css/arquillian_ui_failure_256px.png)

---
<h1>...</h1>

.fx: jumbo

Ejemplos

---
<h1>...</h1>

.fx: jumbo

Q & A

