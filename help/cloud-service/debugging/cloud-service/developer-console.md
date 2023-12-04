---
title: Consola para desarrolladores
description: AEM as a Cloud Service AEM proporciona una consola de desarrollador para cada entorno que expone varios detalles del servicio de ejecución de la que son útiles en la depuración.
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
jira: KT-5433
thumbnail: kt-5433.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 0499ff9f-d452-459f-b1a2-2853a228efd1
duration: 388
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '1408'
ht-degree: 0%

---

# AEM Depuración as a Cloud Service de la con Developer Console

AEM as a Cloud Service AEM proporciona una consola de desarrollador para cada entorno que expone varios detalles del servicio de ejecución de la que son útiles en la depuración.

AEM Cada entorno as a Cloud Service tiene su propia consola de desarrollador.

## Vaya a Developer Console

AEM Se accede a Developer Console desde un entorno as a Cloud Service a través de Cloud Manager, lo que resulta más sencillo para todos los usuarios.

![Vaya a Developer Console](./assets/developer-console/navigate.png)

1. Vaya a __[Cloud Manager](https://my.cloudmanager.adobe.com/)__
2. Abra el __Programa__ AEM que contiene el entorno as a Cloud Service de la para abrir Developer Console.
3. Busque el __Entorno__ y seleccione la opción `...`.
4. Seleccionar __Developer Console__ en la lista desplegable.


## Acceso a Developer Console

Para acceder y utilizar Developer Console, se deben otorgar los siguientes permisos a Adobe ID del desarrollador a través de [Admin Console del Adobe](https://adminconsole.adobe.com).

1. Asegúrese de que la organización de Adobe AEM que ha afectado a Cloud Manager y a los productos as a Cloud Service de la esté activa en el conmutador de organización de Adobe.
1. El desarrollador debe ser miembro de [Elementos del producto de Cloud Manager __Desarrollador - Cloud Service__ Perfil del producto](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-cloud-manager.html#assign-developer).
   + Si esta pertenencia no existe, el desarrollador no podrá iniciar sesión en Developer Console.
1. El desarrollador debe ser miembro de [__AEM Usuarios de__ o __AEM Administradores de__ AEM Perfil del producto en el autor o publicación de la](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-aem.html#aem-product-profiles).
   + Si esta pertenencia no existe, la variable [status](#status) Los volcados agotarán el tiempo de espera con un error 401 no autorizado.

### Solución de problemas del acceso a Developer Console

#### 401 Error no autorizado al descargar el estado

![Consola de desarrollador: 401 no autorizado](./assets/developer-console/troubleshooting__401-unauthorized.png)

AEM Si se descarga cualquier estado y se informa de un error 401 No autorizado, significa que el usuario aún no existe con los permisos necesarios en as a Cloud Service o que el uso de tokens de inicio de sesión no es válido o han caducado.

Para resolver el problema 401 No autorizado:

1. AEM AEM AEM Asegúrese de que el usuario sea miembro del perfil de producto de Adobe IMS correspondiente (administradores de la aplicación o usuarios de la aplicación) para la instancia de producto as a Cloud Service asociada de la consola de desarrollador ().
   + AEM Recuerde que Developer Console accede a 2 instancias de producto de Adobe IMS; las instancias de producto de autor y publicación as a Cloud Service, por lo que asegúrese de que se utilizan los perfiles de producto correctos en función de qué nivel de servicio requiere acceso a través de Developer Console.
1. AEM Inicie sesión en el as a Cloud Service AEM de (Autor o Publicación) y asegúrese de que el usuario y los grupos se hayan sincronizado correctamente con los grupos de trabajo de la interfaz de usuario de.
   + AEM Developer Console requiere que se cree el registro de usuario en el nivel de servicio de correspondiente para que se autentique en ese nivel de servicio.
1. Borre las cookies de los exploradores, así como el estado de la aplicación (almacenamiento local) y vuelva a iniciar sesión en Developer Console, asegurándose de que el token de acceso que está utilizando Developer Console sea correcto y no haya caducado.

## Pod

AEM Los servicios de autor y publicación as a Cloud Service están compuestos por varias instancias, respectivamente, para administrar la variabilidad del tráfico y las actualizaciones móviles sin tiempo de inactividad. Estas instancias se denominan Pods. La selección de la secuencia en Developer Console define el ámbito de los datos que se expondrán a través de los demás controles.

![Consola de desarrollador: pod](./assets/developer-console/pod.png)

+ AEM Un pod es una instancia discreta que forma parte de un servicio de (Author o Publish)
+ AEM Los pods son transitorios, lo que significa que los as a Cloud Service los crea y los destruye según sea necesario
+ AEM Solo los pods que formen parte del entorno as a Cloud Service asociado se enumeran como el conmutador de pods de la consola de desarrollador de ese entorno.
+ En la parte inferior del conmutador de pods, las opciones de conveniencia permiten seleccionar pods por tipo de servicio:
   + Todos los autores
   + Todos los editores
   + Todas las instancias

## Estado

AEM Status proporciona opciones para generar un estado de tiempo de ejecución de un determinado parámetro de tiempo de ejecución en texto o salida JSON. AEM Developer Console proporciona información similar a la consola web OSGi de inicio rápido local del SDK de la, con la marcada diferencia de que Developer Console es de solo lectura.

![Consola de desarrollador: estado](./assets/developer-console/status.png)

### Paquetes

AEM Paquetes enumera todos los paquetes OSGi en la lista de paquetes que se encuentran en la. Esta funcionalidad es similar a [AEM Paquetes OSGi de inicio rápido locales del SDK de la](http://localhost:4502/system/console/bundles) en `/system/console/bundles`.

Los paquetes ayudan en la depuración mediante lo siguiente:

+ AEM Enumeración de todos los paquetes OSGi implementados en el servicio de as a
+ Enumeración del estado de cada paquete OSGi, incluso si están activos o no
+ Proporcionar detalles sobre dependencias sin resolver que hacen que los paquetes OSGi se activen

### Componentes

AEM Componentes enumera todos los componentes de OSGi en la. Esta funcionalidad es similar a [AEM Componentes OSGi del inicio rápido local del SDK de la](http://localhost:4502/system/console/components) en `/system/console/components`.

Los componentes ayudan en la depuración al:

+ AEM Lista de todos los componentes de OSGi implementados para el as a Cloud Service de la
+ Proporcionar el estado de cada componente OSGi; incluso si están activos o no satisfechos
+ Proporcionar detalles sobre referencias de servicio no satisfechas puede provocar que los componentes OSGi se activen
+ Enumerar las propiedades OSGi y sus valores enlazados al componente OSGi.
   + Esto mostrará los valores reales insertados mediante [Variables de configuración de entorno OSGi](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values).

### Configuraciones

Configuraciones enumera todas las configuraciones del componente OSGi (propiedades y valores OSGi). Esta funcionalidad es similar a [AEM Administrador de configuración OSGi de inicio rápido local del SDK de](http://localhost:4502/system/console/configMgr) en `/system/console/configMgr`.

Las configuraciones ayudan en la depuración al:

+ Lista de propiedades OSGi y sus valores por componente OSGi
   + Esto NO mostrará los valores reales insertados mediante [Variables de configuración de entorno OSGi](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values). Consulte [Componentes](#components) arriba, para los valores inyectados.
+ Localización e identificación de propiedades mal configuradas

### Índices Oak

Los índices Oak proporcionan un volcado de los nodos definidos debajo de `/oak:index`. AEM Tenga en cuenta que esto no muestra los índices combinados, lo que ocurre cuando se modifica un índice de.

Los índices Oak ayudan en la depuración al:

+ AEM Enumeración de todas las definiciones de índice de Oak que proporcionan perspectivas sobre cómo se ejecutan las consultas de búsqueda en los informes de búsqueda de la lista de. AEM Tenga en cuenta que las modificaciones en los índices de no se reflejan aquí. AEM Esta vista solo es útil para los índices que solo proporciona el código personalizado, o que solo proporciona el código personalizado.

### Servicios OSGi

Componentes enumera todos los servicios OSGi. Esta funcionalidad es similar a [AEM Servicios OSGi de inicio rápido local del SDK de la](http://localhost:4502/system/console/services) en `/system/console/services`.

Los servicios OSGi ayudan en la depuración al:

+ AEM Enumerar todos los servicios de OSGi en la lista de servicios de OSGi en la lista de servicios de OSGi, junto con su paquete OSGi proporcionado y todos los paquetes OSGi que lo consumen.

### Trabajos de Sling

Trabajos de Sling enumera todas las colas de trabajos de Sling. Esta funcionalidad es similar a [AEM Trabajos de inicio rápido locales del SDK de la](http://localhost:4502/system/console/slingevent) en `/system/console/slingevent`.

Los trabajos de Sling ayudan en la depuración al:

+ Lista de colas de trabajos de Sling y sus configuraciones
+ AEM Proporcionar información sobre el número de trabajos de Sling activos, en cola y procesados, lo que resulta útil para depurar problemas con el flujo de trabajo, el flujo de trabajo transitorio y otro trabajo realizado por los trabajos de Sling en la.

## Paquetes Java

AEM Los paquetes Java permiten comprobar si hay un paquete Java y una versión disponibles para su uso en el as a Cloud Service de la. Esta funcionalidad es la misma que [AEM Buscador de dependencias de inicio rápido local del SDK de la](http://localhost:4502/system/console/depfinder) en `/system/console/depfinder`.

![Consola de desarrollador: paquetes Java](./assets/developer-console/java-packages.png)

Los paquetes Java se utilizan para solucionar problemas cuando los paquetes no se inician debido a importaciones sin resolver o clases sin resolver en scripts (HTL, JSP, etc.). Si los paquetes Java informan de que no hay paquetes que exporten un paquete Java (o la versión no coincide con la importada por un paquete OSGi):

+ AEM AEM Asegúrese de que la versión de la dependencia de Maven de la API de del proyecto coincida con la versión de la versión de la versión de Maven del entorno (y, si es posible, actualice todo a la versión más reciente).
+ Si se utilizan dependencias Maven adicionales en el proyecto Maven
   + AEM Determine si se puede utilizar una API alternativa proporcionada por la dependencia de la API del SDK de la en su lugar.
   + Si se requiere la dependencia adicional, asegúrese de que se proporcione como un paquete OSGi (en lugar de como un Jar sin formato) y de que esté incrustado en el paquete de código del proyecto, (`ui.apps`), de forma similar a como el paquete OSGi principal está incrustado en el `ui.apps` paquete.

## Servlets

AEM Servlets se utiliza para proporcionar información sobre cómo resuelve una URL en un servlet o script Java (HTL, JSP) que, en última instancia, gestiona la solicitud. Esta funcionalidad es la misma que [AEM Sling Servlet Resolver del inicio rápido local del SDK de la](http://localhost:4502/system/console/servletresolver) en `/system/console/servletresolver`.

![Consola de desarrollador: servlets](./assets/developer-console/servlets.png)

Servlets ayuda a depurar y determinar lo siguiente:

+ Cómo se descompone una dirección URL en sus partes accesibles (recurso, selector, extensión).
+ Qué servlet o script resuelve una URL, lo que ayuda a identificar direcciones URL o servlets/scripts mal formados.

## Consultas

AEM Las consultas ayudan a proporcionar información sobre qué y cómo se ejecutan las consultas de búsqueda en las consultas de búsqueda en el momento de la ejecución de la. Esta funcionalidad es la misma que  [AEM Herramientas > Rendimiento de las consultas del inicio rápido local del SDK](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) consola.

AEM Consultas solo funciona cuando se selecciona un pod específico, ya que abre la consola web de Rendimiento de las consultas de ese pod, lo que requiere que el desarrollador tenga acceso para iniciar sesión en el servicio de la.

![Consola de desarrollador: consultas, explicar consulta](./assets/developer-console/queries__explain-query.png)

Las consultas ayudan a depurar:

+ Explicar cómo Oak interpreta, analiza y ejecuta las consultas. Esto es muy importante a la hora de rastrear por qué una consulta es lenta y comprender cómo se puede acelerar.
+ AEM Enumeración de las consultas más populares que se ejecutan en la, con la capacidad de Explicarlas.
+ AEM Enumeración de las consultas más lentas que se ejecutan en la, con la capacidad de Explicarlas.
