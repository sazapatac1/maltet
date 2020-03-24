# Entrega 2
**Análisis y Diseño:**
 **1. Definición de Equipo (integrantes, emails)**
 
	Laura Sanchez Cordoba - 
	
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

Laura Sanchez Cordoba:

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

Luego, en los detalles de auto-scaling, se coloca el grupo de destino del load balancer para que todas las instancias creadas vayan directamente a el.

**![](https://lh3.googleusercontent.com/jSpVSe4a3zKqsS9EhbZsZ0MoHOwHSdbzRfGv34mKCIxp9RSmL0IchW03Pz5b8qCTsuWsXH88kq4BmFwk0DqoV1rkAVFOBneDcKEpZt_2lKeUzq2T0bRP3GxMi7tIqdSG2JwIpzSi)**

Aquí se pueden ver las intancias creadas por auto-scaling en el balanceador.

**![](https://lh4.googleusercontent.com/eKwXgbDyFCYY1TiZVxI09Sr8zRZ2VamRLi63WLsLEKjfJvUUFPx46aN1PX7z7VgTyDyYzWXYSRK1YypQ7d-lPqpeS8zkECNTl44klG19qRfldyVrC7bc-VKV9M7m7NTMl7lAY4A-)**

Se coloca en DNS del dominio un record CNAME apuntando al DNS Name del balanceador debido que no provee una dirección ip.

**![](https://lh6.googleusercontent.com/3iteCB_-hZjfqzT8073csiJS4PYrnXMRyfPrkvXX8gFuKdC2AD4dPBI57VTu4QQpF-0XftqpUNQ26IhWxNBeFet3E9zCzxWlzwju57DeTM9FhwdVT9XwZTx-hIdvSOoJjEur4MD0)**

Por último se activa la opción de "Always HTTPS" para redirigir todas las peticiones http a https.

**![](https://lh6.googleusercontent.com/XM5p-pTW9MNfZfRREHijJhn4I7GgHOqjOcwec1JE8W642ZAffwpTiJ5lAypYGaQePOn2rdhjsZvlN6i8AP2xLaqTPN5WJYt2Kwvtaz-kYyC-WSfiJ2KFrWkHuSeSX4HqC3PXO90i)**
