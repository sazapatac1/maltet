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

**c. Seguridad**
Nota: el dominio que figura en las pruebas es diferente, ya que era el dominio de prueba para el apartado de seguridad

En el módulo de seguridad se trabajaron varios niveles de protección, empezando con que una vez el usuario se registra, para loguearse debe pasar una autenticación 2Factor a través de Google Authenticator.
