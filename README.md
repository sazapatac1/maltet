# Entrega 2
**Análisis y Diseño:**

 **1. Definición de Equipo (integrantes, emails)**
 
	Laura Sanchez Cordoba - lsanchezc@eafit.edu.co
	
	Mateo Florez Restrepo - mflorezr@eafit.edu.co
	
	Santiago Arturo Zapata Chacón - sazapatac1@eafit.edu.co
 
 **2. Asignación de roles y responsabilidades de cada integrante del equipo en el desarrollo del proyecto2.**
 
Santiago Arturo Zapata Chacón:
 - Availability (Load balancer, Auto-scaling, failover)
 - Pedir dominio
 - Pedir certificado SSL
 - Despliegue monolitco DCA
 - Despliegue monolitico AWS
 - Creación base de datos RDS
 - Asignar dominio al Load Balancer
 
Mateo Florez Restrepo:
 - Test de velocidad
 - Test de concurrencia(jmeter)
 - CDN(Cloudflare)
 - Caché
 - Rendimiento(análisis y diseño)

Laura Sanchez Cordoba:
 - Protección de datos
 - Autenticación de dos factores y con terceros (Facebook y Google)
 - Manejo de vulnerabilidades descritas por la OWASP
 - Análisis dinámico de la aplicaciòn (Test de vulnerabilidad) 
 - Seguridad (protección de ataques y fortalecimiento de autenticación)


 **3. Github del proyecto:**
	 [https://github.com/sazapatac1/maltet](https://github.com/sazapatac1/maltet)
	 
**4. Especificación de requisitos no funcionales.**

**a. Disponibilidad**

Para el módulo de High availability se tomó en cuenta la arquitectura diseñada, por lo tanto se creó una AMI (imagen) donde crea automáticamente un servidor LAMP con wordpress que apunta a la base de datos alojada en RDS.

La creación del AMI apoya para la creación de un load balancer y autoScaling, se creó un balanceador con dos listeners (http y https). Además, en el load balancer se añadió una VPC con dos subnets públicas y se activó la opción de ‘Cloudwatch monitoring’ para monitorizar el comportamiento de las peticiones, uso de CPU, etc.

**![](https://lh5.googleusercontent.com/v7cJifC-Tm3Gn9dE3ERIf7RIu7jMPErn8Fv51Lz-w1bTiWeYXu7pS9sSU7CkDmKn8A1uUNmGMp_U-NuVH9rH5pfEpshWldH97kJZb5lqJvv_LeRLLfMlEeh0kL1WTaagfnLo2XU7)**

**Listeners**

**![](https://lh3.googleusercontent.com/oZAsKDUehAGRmapx6SbpZMkfttkDHuqZsWTjPgKI5PcPSC2LFvjkzmReS9SxT1OHYdGbMCaNfz5uiRC_TbmmMCyXJBW_qtdukzaHQp2eXuN-0_Kg6bKUlyR7pt_5vYy4QAwQ8FKq)**

**![](https://lh6.googleusercontent.com/uvMYcJIyWJXhiP1j2mggVCA_aKWXgMuIU87Vcyti3m5klxRTi0qUDOpzAyv6rZshkUVYVcqxICcOrboQWygAZMJFxElSE64ckNgFHhyBzlRwQhVnU7fVliDSAbpsy8fGeoRsEfMo)**

En el auto-Scaling se tiene un limite de 2 a 20 instancias, debido a las pruebas de concurrencia realizadas se decidió establecer este límite para cubrir 2000 peticiones al mismo tiempo, teniendo solo la politica de añadir o eliminar instancias para mantener una utilización de CPU del 50%.

**![](https://lh5.googleusercontent.com/OiHZys3FKCgWYm5XotqP3kukoe_GClAJ7Xlqp8ZqhpdwDkn5UANHfqu2kpHY2-o3s6oyKgIZ72uhzlLKJWBkGjigLM8e0sJ7PE_Pjuq5uI-D23dKjdA_I9S1406szDh6UVaLGIQq)**

Luego, en los detalles de auto-scaling, se coloca el grupo de destino del load balancer para que todas las instancias creadas vayan directamente a él.


**![](https://lh3.googleusercontent.com/jSpVSe4a3zKqsS9EhbZsZ0MoHOwHSdbzRfGv34mKCIxp9RSmL0IchW03Pz5b8qCTsuWsXH88kq4BmFwk0DqoV1rkAVFOBneDcKEpZt_2lKeUzq2T0bRP3GxMi7tIqdSG2JwIpzSi)**

Aquí se pueden ver las intancias creadas por auto-scaling en el balanceador.

**![](https://lh4.googleusercontent.com/eKwXgbDyFCYY1TiZVxI09Sr8zRZ2VamRLi63WLsLEKjfJvUUFPx46aN1PX7z7VgTyDyYzWXYSRK1YypQ7d-lPqpeS8zkECNTl44klG19qRfldyVrC7bc-VKV9M7m7NTMl7lAY4A-)**

Se coloca en DNS del dominio un record CNAME apuntando al DNS Name del balanceador debido que no provee una dirección ip.

**![](https://lh6.googleusercontent.com/3iteCB_-hZjfqzT8073csiJS4PYrnXMRyfPrkvXX8gFuKdC2AD4dPBI57VTu4QQpF-0XftqpUNQ26IhWxNBeFet3E9zCzxWlzwju57DeTM9FhwdVT9XwZTx-hIdvSOoJjEur4MD0)**

Por último se activa la opción de "Always HTTPS" para redirigir todas las peticiones http a https.

**![](https://lh6.googleusercontent.com/XM5p-pTW9MNfZfRREHijJhn4I7GgHOqjOcwec1JE8W642ZAffwpTiJ5lAypYGaQePOn2rdhjsZvlN6i8AP2xLaqTPN5WJYt2Kwvtaz-kYyC-WSfiJ2KFrWkHuSeSX4HqC3PXO90i)**

**b. Rendimiento**

En cuestiones de robustez de una aplicación una de las partes más importantes es el rendimiento de nuestro producto o aplicación. El rendimiento de una aplicación puede ser medida desde diferentes aspectos de esta. Para nuestra aplicación entonces tomaremos en cuenta varias características que optimizaremos para lograr unas mejores cifras de rendimiento. Estas características serán:

Velocidad: Los tiempos de respuesta a las peticiones de los usuarios deben tener un tiempo menor o igual a 1 segundo. Las peticiones y respuestas deben ser procesadas en el menor tiempo posible para hacer nuestro sitio más rápido ante los usuarios.

Cache: La caché es una memoria intermedia que permite guardar datos para que las solicitudes o peticiones futuras puedan acceder a ellos más rápidamente sin tener que recurrir a los procesos ya hechos recientemente 

Tamaño de Scripts: Comprimir o minificar al máximo los archivos y recursos de nuestra aplicación de manera que el tiempo de carga sea el menor posible.

Teniendo en cuenta las características anteriores existe además un principio o herramienta que puede ayudarnos con la optimización del rendimiento, y es el CDN.
El CDN (Content Delivery Network) es un conjunto de ubicaciones (servidores distribuidos geográficamente) en el mundo que distribuyen localmente el contenido de los servidores y guardan en cache los archivos que no necesitan actualización permanente.  Este principio permite entonces a un usuario a acceder al contenido y los servicios de la página desde cualquier lugar del mundo.
La CDN permite acelerar las cargas de las paginas, mejorar los tiempos de respuesta y la experiencia del usuario, proteger datos, mejorar el posicionamiento de los sitios web y reducir el consumo de ancho de banda en cada uno de los países.  


**![](https://assets.digitalocean.com/articles/CDN/CDN.png)**


Existen muchas herramientas que nos ayudan a implementar el CDN en nuestra aplicación, sin embargo, escogimos “CloudFlare” ya que esta herramienta además del rendimiento nos ayuda a optimizar la seguridad y los otros atributos de calidad de la aplicación, y nos ofrece buenos servicios de manera gratuita. 
Estas son las ventajas de CloudFlare para el rendimiento:


**![](http://imgfz.com/i/mviwRX7.png)**
**![](http://imgfz.com/i/rWzuqvL.png)**
**![](http://imgfz.com/i/8SOlWGM.png)**


CDN en cloudflare: 

La Red de entrega de contenido (CDN) de Cloudflare es un grupo de servidores distribuidos geográficamente que aseguran la entrega rápida de contenido de Internet, incluidas páginas HTML, archivos JavaScript, hojas de estilo e imágenes. El almacenamiento en caché de recursos estáticos en Cloudflare reduce la carga del servidor y el ancho de banda, sin cargos adicionales por picos de ancho de banda.
Para comprender mas acerca del servicio de CDN de cloudflare: https://support.cloudflare.com/hc/es-es/articles/200172516-Comprender-la-CDN-de-Cloudflare

**Servicios de CloudFlare**

En CloudFlare podemos ver los tiempos de respuesta con los servicios habilitados.

**![](http://imgfz.com/i/gPUGDjl.png)**
**![](http://imgfz.com/i/kHScjv3.png)**

Hace más veloces los tiempos de respuesta.

**![](http://imgfz.com/i/E72RYFl.png)**

Permite minificar y comprimir archivos de gran tamaño y suprimir los espacios en blanco.

**![](http://imgfz.com/i/AORgGyJ.png)**
**![](http://imgfz.com/i/hGCtoBL.png)**

Además, te permite manejar y personalizar la cache de tu sistema, de acuerdo con los requerimientos.

**TEST**

**Test de velocidad**

Usamos entonces una herramienta para calcular los tiempos de respuesta de la página. 
PageSpeed Insights es la herramienta oficial de Google para valorar el grado de optimización de nuestra página web. El reporte de optimización muestra en que está fallando nuestra página y además nos da sugerencias para solucionarlo. 

**Test antes de cloudflare:**

**![](http://imgfz.com/i/DdVmk5C.png)**

Según los parámetros de calificación de la herramienta de Google nuestra pagina tiene una muy buena optimización, por lo menos para usuarios desktop, sin embargo, nos ofrece algunas sugerencias que podríamos implementar en nuestra página. 

**![](http://imgfz.com/i/4Ou2bKS.png)**


**Test con Cloudflare**

Después de aplicar los cambios y optimizaciones volvimos a analizar el sitio con la herramienta de Google, PageSpeed Insights y vimos como mejoro en casi todos los resultados:

**![](http://imgfz.com/i/UwjKpns.png)**
**![](http://imgfz.com/i/oB5sOYS.png)**

Muchos de los resultados mejoraron debido a que CloudFlare cubria algunas de las sugerencias sobre cache, minificacion y velocidad que nos arrojaron los primeros resultados. Todos los tiempos estuvieron por debajo de 1 segundo, lo que significa que el tiempo de respuesta de nuestro sitio es optimo.
También utilizamos otras herramientas de medida de rendimiento de nuestro sitio como lo fueron Pingdom, que permite ingresar desde diferentes partes del mundo, da un reporte muy completo de tiempos de carga de cada recurso y tamaño de estos:
Lo probamos desde Japon, en Asia:

**![](http://imgfz.com/i/NMCmwbc.png)**
**![](http://imgfz.com/i/gLiDlC5.png)**
**![](http://imgfz.com/i/jr5VJ14.png)**

La cual nos arrojó excelentes resultados.


Y también hicimos pruebas en GTMetrix, otra herramienta muy útil que nos da dos valores, uno de velocidad de página y otro que emplea YSlow. Lo probamos desde Vancouver, Canadá:


**![](http://imgfz.com/i/dyo6eT4.png)**
**![](http://imgfz.com/i/7OvoVyH.png)**


En esta última prueba nos califican el grado de cumplimiento de las buenas prácticas y optimizaciones principales de rendimiento de nuestra página, en la cual los resultados fueron muy buenos.


De acuerdo con las tres pruebas en las diferentes herramientas de medida de rendimiento nuestra página arroja tiempos de carga y velocidad óptimos. Según las diferentes pruebas los tiempos de carga varían entre 0 y 1,4 segundos, y además el sistema cubre muchas de las buenas prácticas para alcanzar un óptimo rendimiento.

**Test de concurrencia**

Herramienta: Jmeter

Test1: Petición HTTP a la página de Inicio

Utilizando Jmeter hicimos algunas pruebas de rendimiento para ver como respondía nuestra aplicación a las diferentes peticiones de un usuario. Así que por medio de un servidor proxy capturamos, o grabamos algunas de las peticiones que hicimos en un navegador.
Para una mejor organización el test solo nos centramos en la petición a la página de inicio.

Después, lo que hicimos fue agregar listeners para capturar la información de los resultados generados anteriormente. 	
El primer listener o receptor que agregamos fue el “Árbol de resultados” que nos muestra las peticiones que fallan o no, los datos de cada petición y además las respuestas a cada una de ellas, despues pusimos "Informe Agregado", "Ver resultado en tabla" y "Grafica de resultados" para entender mejor los comportamientos de las peticiones tanto a nivel general como individual.

Hicimos las pruebas con 100, 300, 500, 1000 y 2000 usuarios. En las primeras pruebas con 100 y 300 usuarios tuvimos algunos problemas con el servidor, ya que se al principio solo teníamos 2 instancias corriendo y el servidor se estresaba con las 100 peticiones y fallaba. Luego aumentamos las instancias a medida que se llenaba el 60% de la CPU para evitar que el servidor entero fallara con las peticiones. Cuando intentamos con 100 y 300 peticiones estas todas resultaron exitosas sin embargo a medida que íbamos aumentando la concurrencia iba aumentando el índice de error. Según los resultados arrojados se podían ver errores muy comunes como el “Server Internal Error” que era cuando las instancias generadas por la autoescalabilidad causaban errores de respuesta, o “Request TimeOut” y “Gateway TimeOut” cuando se caía completamente el servidor. Tuvimos entonces que ir aumentando el número de instancias para cumplir nuestra meta de 2000 usuarios concurrentes y con el menor error y tiempo de respuesta posible. 
A continuación, los resultados de las primeras pruebas y las ultimas:


Concurrencia: 100 usuarios

**![](http://imgfz.com/i/JR9sKZM.png)**

Según el informe agregado vemos que las 100 peticiones concurrentes salieron bien sin ningún error y además con tiempos de respuesta en promedio de 753 ms los cuales son óptimos.

Concurrencia: 2000 usuarios

**![](http://imgfz.com/i/DnIO3HZ.png)**

Hicimos la prueba con 2000 hilos en 1 segundo, a la cual nos respondió con 15% de error por lo que aproximadamente 1700 peticiones y respuestas fueron procesadas con éxito con un tiempo promedio de 9 segundos (Se tiene en cuenta la velocidad del internet y el procesador del computador desde donde se hacen las pruebas). Las instancias EC2 prendidas para lograr que los usuarios estuvieran conectados al mismo tiempo fueron 15 aproximadamente. Por lo que para lograr nuestra meta de 2000 usuarios y la escalabilidad adecuada debe de haber entre 15 y 20 instancias prendidas. 

**![](http://imgfz.com/i/RTn548i.png)**

En este listener podemos ver la información por cada petición de usuario. Es decir, podemos ver la muestra, el hilo, la petición, el tiempo de la muestra, la latencia, etc. 










**c. Seguridad**
Nota: el dominio que figura en las pruebas es diferente, ya que era el dominio de prueba para el apartado de seguridad

En el módulo de seguridad se trabajaron varios niveles de protección, empezando con que una vez el usuario se registra, para loguearse debe pasar una autenticación 2Factor a través de Google Authenticator.
**![](http://imgfz.com/i/zXm7H21.png)**

**![](http://imgfz.com/i/r4oajN3.png)**

Para Protección de algunas amenazas/vulnerabilidades que puedan llegar a la página, como SQL Injections, el uso de exploits, fuerza bruta, etc. Se hace uso de dos plugins.

En primer lugar se hace uso de Anti-Malware, el cual identifica amenazas como las anteriormente mencionadas, habilita el Firewall, escanea para eliminar amenazas detectadas y además, permite bloquear el acceso al archivo XMLRPC.PHP que es el que configura el protocolo XMLRCP en Wordpress el cual actúa como interfaz con servicios externos. El bloqueo de este impide ataques como hallar la contraseña del sitio a través de fuerza bruta y DDOS.  

**![](http://imgfz.com/i/TnSobui.png)**

Otro plugin usado es Wordfence, que además de arreglar vulnerabilidades posee un modo de aprendizaje desde el momento de la instalación donde revisa el sitio (las IP que acceden, las características) para prevenir ataques. Además su herramienta de escaneo identifica las amenazas, de donde vienen y las modificaciones a los ficheros, permitiendo volver a la configuración inicial, permite hacer copias de seguridad y las ubica en la sección de cuarentena para posterior análisis. 

**![](http://imgfz.com/i/NXgAzni.png)**

Monitoreo de tráfico

**![](http://imgfz.com/i/EiLRWFq.png)**

Nota: Algunas vulnerabilidades no pudieron ser cubiertas dado que para hacerlo se necesitan versiones Premium de los plugins.

Autenticación con Redes Sociales

También se agregó la posibilidad de que el usuario se identifique a través de las opciones de Facebook y Google para facilidad de este. Esto se realizó a través del plugin
**![](http://imgfz.com/i/YX8Kr2V.png)**

Análisis de vulnerabilidades según el sitio WebSec
**![](http://imgfz.com/i/ptsYhSw.png)**

**5. Diseño para la Escalabilidad (disponibilidad, rendimiento y seguridad)**

	a. Qué patrones de arquitectura específicos (patrones de escalabilidad y buenas prácticas) se utilizarán en el SISTEMA para 	apoyar esta escalabilidad:
	
		i.Mejores prácticas
		ii.Selección de tácticas 
		iii.Decisiones de diseño
	
	Uno de los patrones de escalabilidad usados fue Workload/demand distribution ya que con el uso de balanceadores de carga las peticiones son equilibradas, permitiendo un mejor trabajo de las instancias. 
	Se hace un backup de la base de datos, en el caso de algún problema. 
	En cuanto a performance, el uso de cloudfare nos da ventajas como la minimización de archivos JavaScript y CCS y el manejo de caché.
	El uso Autoscaling Group y sus políticas permiten que a medida que entren los usuarios se vayan creando instancias cada que sea necesario.
	Para la seguridad se llevaron a cabo tácticas como la doble autenticación y la autenticación con terceros, además de la protección del sitio frente a vulnerabilidades como inyección de código, robo de datos y ataques DoS.  
	
	b. Definición de Herramientas a utilizar.
	
		- Docker: Docker  es una herramienta PaaS (plataform as a service) que usa la virtualización a nivel de Sistema Operativo para alojar el software en contenedores, con todo lo que necesita para funcionar. Cada contenedor es independiente de los demás	
		
		- Balanceador de carga: Herramienta de Software (en este caso) que permite distribuir las peticiones entre servidores a través de algoritmos para mejorar el funcionamiento de los servidores ante un tráfico muy alto.
		
		- Auto Scaling Groups: herramienta de AWS que a partir de unas políticas y definición de un rango de instancias EC2 a usar, bajo demanda creará las instancias que sean necesarias dadas las peticiones que se hagan.
	 	
		- Cloudfare: Herramienta que actúa como proxy entre el servidor y los clientes, reduce el número de peticiones al servidor gracias a que como proxy, guarda temporalmente contenido estático del sitio.
		
		- Amazon Relational Database Service (Amazon RDS) : servicio que suministra una base de datos relacional escalable y se encarga de su aprovisionamiento, copias de seguridad, etc. para que el usuario se encargue de cómo se complementa su aplicación con la base de datos.
		
		- EC2: es un servicio web que proporciona capacidad informática en la nube segura y de tamaño modificable.
		
		- JMeter: es un proyecto de Apache que se utiliza como herramienta de prueba de carga para medir el rendimiento de una aplicación web.


		
		
		
