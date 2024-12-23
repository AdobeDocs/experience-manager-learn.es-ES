---
title: Consola para desarrolladores
description: AEM as a Cloud Service proporciona un Developer Console AEM para cada entorno que expone varios detalles del servicio de depuración en ejecución que son útiles para la depuración.
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
jira: KT-5433
thumbnail: kt-5433.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 0499ff9f-d452-459f-b1a2-2853a228efd1
duration: 295
source-git-commit: 1d9aeb4e5bd41096a28e3375d124bd6b6b8784aa
workflow-type: tm+mt
source-wordcount: '1562'
ht-degree: 0%

---

# Depuración de AEM as a Cloud Service con Developer Console

AEM as a Cloud Service proporciona un Developer Console AEM para cada entorno que expone varios detalles del servicio de depuración en ejecución que son útiles para la depuración.

Cada entorno de AEM as a Cloud Service tiene su propia Developer Console.

## Navegar a Developer Console

Se accede a Developer Console por entorno de AEM as a Cloud Service a través de Cloud Manager.

![Navegar a Developer Console](./assets/developer-console/navigate.png)

1. Vaya a __[Cloud Manager](https://my.cloudmanager.adobe.com/)__
2. Abra el __Programa__ que contiene el entorno de AEM as a Cloud Service para abrir Developer Console.
3. Busque __Entorno__ y seleccione `...`.
4. Seleccione __Developer Console__ de la lista desplegable.


## Acceso a Developer Console

Para acceder y utilizar Developer Console, se deben otorgar los siguientes permisos al Adobe ID del desarrollador a través del Admin Console de [Adobe](https://adminconsole.adobe.com).

1. Asegúrese de que en el conmutador de organización de Adobe puede ver la organización de Adobe relacionada con los entornos que desea inspeccionar en Developer Console.
1. Para poder iniciar sesión en Developer Console, el desarrollador debe ser miembro de cualquiera de las siguientes funciones:
   + [Desarrollador __del producto Cloud Manager - Cloud Service__ Perfil del producto](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-cloud-manager.html#assign-developer): En este caso, el desarrollador verá la lista completa de entornos disponibles en la URL de Developer Console seleccionada; si se ha seleccionado un entorno de desarrollo o RDE en Cloud Manager, puede que aparezcan otros entornos de desarrollo o RDE en el mismo programa.
   + AEM AEM Perfil de producto de [__Administradores de__ en __Author__](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-aem.html#aem-product-profiles): En este caso, la lista de entornos descritos en la viñeta anterior se limitará a los perfiles de producto relacionados donde se asigna esta función.
1. AEM AEM AEM El desarrollador debe ser miembro del perfil de producto [__Usuarios de la red__ o __Administradores de la red__ en Author y/o Publish](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-aem.html#aem-product-profiles).
   + Si esta pertenencia no existe, los volcados [status](#status) agotarán el tiempo de espera con un error 401 no autorizado.

### Solución de problemas de acceso a Developer Console

#### Al iniciar sesión, no veo el entorno que estoy buscando en la lista

Asegúrese de lo siguiente:

+ Ha seleccionado la URL de Developer Console correcta haciendo clic en los tres puntos para el entorno seleccionado a través de Cloud Manager y seleccione Developer Console.
+ O bien tiene __Desarrollador - Cloud Service AEM AEM__ Perfil de producto](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-cloud-manager.html#assign-developer) del producto Cloud Manager para ver la lista completa de entornos, o bien forma parte del perfil de producto [__Administradores__ en __Administrador o__](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-aem.html#aem-product-profiles) para el entorno que no encuentra.[

#### 401 Error no autorizado al descargar el estado

![Developer Console - 401 no autorizado](./assets/developer-console/troubleshooting__401-unauthorized.png)

Si se descarga cualquier estado y se informa de un error 401 no autorizado, significa que el usuario aún no existe con los permisos necesarios en AEM as a Cloud Service o que el uso de los tokens de inicio de sesión no es válido o ha caducado.

Para resolver el problema 401 No autorizado:

1. AEM AEM Asegúrese de que el usuario sea miembro del perfil de producto de Adobe IMS correspondiente (administradores de la aplicación o usuarios de la aplicación de) para la instancia de producto de AEM as a Cloud Service asociada a Developer Console.
   + Recuerde que las instancias de producto Developer Console acceden a 2 instancias de producto Adobe IMS; las instancias de producto AEM as a Cloud Service Author y Publish, por lo que asegúrese de que se utilizan los perfiles de producto correctos en función del nivel de servicio que requiera acceso a través de Developer Console.
1. Inicie sesión en AEM as a Cloud Service (Author o Publish AEM) y asegúrese de que los usuarios y grupos se hayan sincronizado correctamente con los usuarios de la aplicación de forma segura y sin problemas en la interfaz de usuario de.
   + Developer Console AEM requiere que el registro de usuario se cree en el nivel de servicio de la correspondiente para que se autentique en ese nivel de servicio.
1. Borre las cookies de los exploradores, así como el estado de la aplicación (almacenamiento local) y vuelva a iniciar sesión en Developer Console, asegurándose de que el token de acceso que utiliza Developer Console sea correcto y no haya caducado.

## Pod

Los servicios de AEM as a Cloud Service Author y Publish están compuestos por varias instancias respectivamente para gestionar la variabilidad del tráfico y las actualizaciones móviles sin tiempo de inactividad. Estas instancias se denominan Pods. La selección de la secuencia en Developer Console define el ámbito de los datos que se expondrán a través de los demás controles.

![Developer Console - Pod](./assets/developer-console/pod.png)

+ AEM Un pod es una instancia discreta que forma parte de un servicio de (Author o Publish)
+ Los pods son transitorios, lo que significa que AEM as a Cloud Service los crea y los destruye según sea necesario
+ Solo los pods que forman parte del entorno de AEM as a Cloud Service asociado se enumeran en el conmutador de pods de ese entorno de Developer Console.
+ En la parte inferior del conmutador de pods, las opciones de conveniencia permiten seleccionar pods por tipo de servicio:
   + Todos los autores
   + Todos los editores
   + Todas las instancias

## Estado

AEM Status proporciona opciones para generar un estado de tiempo de ejecución de un determinado parámetro de tiempo de ejecución en texto o salida JSON. Developer Console AEM proporciona información similar a la consola web OSGi de inicio rápido local del SDK de la, con la marcada diferencia de que Developer Console es de solo lectura.

![Developer Console - Estado](./assets/developer-console/status.png)

### Paquetes

AEM Paquetes enumera todos los paquetes OSGi en la lista de paquetes que se encuentran en la. AEM Esta funcionalidad es similar a los paquetes OSGi ](http://localhost:4502/system/console/bundles) del inicio rápido local del SDK de [en `/system/console/bundles`.

Los paquetes ayudan en la depuración mediante lo siguiente:

+ AEM Enumeración de todos los paquetes OSGi implementados en el servicio de as a
+ Enumeración del estado de cada paquete OSGi, incluso si están activos o no
+ Proporcionar detalles sobre dependencias sin resolver que hacen que los paquetes OSGi se activen

### Componentes

AEM Componentes enumera todos los componentes de OSGi en la. AEM Esta funcionalidad es similar a los [Componentes OSGi](http://localhost:4502/system/console/components) del inicio rápido local del SDK de la en `/system/console/components`.

Los componentes ayudan en la depuración al:

+ Enumeración de todos los componentes OSGi implementados en AEM as a Cloud Service
+ Proporcionar el estado de cada componente OSGi; incluso si están activos o no satisfechos
+ Proporcionar detalles sobre referencias de servicio no satisfechas puede provocar que los componentes OSGi se activen
+ Enumerar las propiedades OSGi y sus valores enlazados al componente OSGi.
   + Esto mostrará los valores reales insertados a través de [variables de configuración de entorno OSGi](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values).

### Configuraciones

Configuraciones enumera todas las configuraciones del componente OSGi (propiedades y valores OSGi). AEM Esta funcionalidad es similar al Administrador de configuración OSGi ](http://localhost:4502/system/console/configMgr) del inicio rápido local del SDK de [en `/system/console/configMgr`.

Las configuraciones ayudan en la depuración al:

+ Lista de propiedades OSGi y sus valores por componente OSGi
   + Esto NO mostrará los valores reales insertados mediante [variables de configuración de entorno OSGi](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values). Consulte [Componentes](#components) más arriba para ver los valores insertados.
+ Localización e identificación de propiedades mal configuradas

### Índices Oak

Los índices Oak proporcionan un volcado de los nodos definidos debajo de `/oak:index`. AEM Tenga en cuenta que esto no muestra los índices combinados, lo que ocurre cuando se modifica un índice de.

Los índices Oak ayudan en la depuración al:

+ Enumeración de todas las definiciones de índice de Oak AEM que proporcionan información sobre cómo se ejecutan las consultas de búsqueda en los informes de búsqueda de la aplicación de la. AEM Tenga en cuenta que las modificaciones en los índices de no se reflejan aquí. AEM Esta vista solo es útil para los índices que solo proporciona el código personalizado, o que solo proporciona el código personalizado.

### Servicios OSGi

Componentes enumera todos los servicios OSGi. AEM Esta funcionalidad es similar a los servicios OSGi ](http://localhost:4502/system/console/services) del inicio rápido local del SDK de [en `/system/console/services`.

Los servicios OSGi ayudan en la depuración al:

+ AEM Enumerar todos los servicios de OSGi en la lista de servicios de OSGi en la lista de servicios de OSGi, junto con su paquete OSGi proporcionado y todos los paquetes OSGi que lo consumen.

### Trabajos de Sling

Trabajos de Sling enumera todas las colas de trabajos de Sling. AEM Esta funcionalidad es similar a los [Trabajos de inicio rápido locales del SDK de la](http://localhost:4502/system/console/slingevent) en `/system/console/slingevent`.

Los trabajos de Sling ayudan en la depuración al:

+ Lista de colas de trabajos de Sling y sus configuraciones
+ AEM Proporcionar información sobre el número de trabajos de Sling activos, en cola y procesados, lo que resulta útil para depurar problemas con el flujo de trabajo, el flujo de trabajo transitorio y otro trabajo realizado por los trabajos de Sling en la.

## Paquetes Java

Los paquetes Java permiten comprobar si hay un paquete Java y una versión disponibles para usar en AEM as a Cloud Service. AEM Esta funcionalidad es la misma que el Buscador de dependencias ](http://localhost:4502/system/console/depfinder) del inicio rápido local del SDK de [en `/system/console/depfinder`.

![Developer Console - Paquetes Java](./assets/developer-console/java-packages.png)

Los paquetes Java se utilizan para solucionar problemas cuando los paquetes no se inician debido a importaciones sin resolver o clases sin resolver en scripts (HTL, JSP, etc.). Si los paquetes Java informan de que no hay paquetes que exporten un paquete Java (o la versión no coincide con la importada por un paquete OSGi):

+ AEM AEM Asegúrese de que la versión de la dependencia de Maven de la API de del proyecto coincida con la versión de la versión de la versión de Maven del entorno (y, si es posible, actualice todo a la versión más reciente).
+ Si se utilizan dependencias Maven adicionales en el proyecto Maven
   + AEM Determine si se puede utilizar una API alternativa proporcionada por la dependencia de la API del SDK de la en su lugar.
   + Si se requiere la dependencia adicional, asegúrese de que se proporcione como un paquete OSGi (en lugar de como un Jar sin formato) y de que esté incrustado en el paquete de código del proyecto, (`ui.apps`), de forma similar a como está incrustado el paquete OSGi principal en el paquete `ui.apps`.

## Servlets

AEM Servlets se utiliza para proporcionar información sobre cómo resuelve una URL en un servlet o script Java (HTL, JSP) que, en última instancia, gestiona la solicitud. AEM Esta funcionalidad es la misma que [Resolver servlet Sling del inicio rápido local del SDK de](http://localhost:4502/system/console/servletresolver) en `/system/console/servletresolver`.

![Developer Console - Servlets](./assets/developer-console/servlets.png)

Servlets ayuda a depurar y determinar lo siguiente:

+ Cómo se descompone una dirección URL en sus partes accesibles (recurso, selector, extensión).
+ Qué servlet o script resuelve una URL, lo que ayuda a identificar direcciones URL o servlets/scripts mal formados.

## Consultas

AEM Las consultas ayudan a proporcionar información sobre qué y cómo se ejecutan las consultas de búsqueda en las consultas de búsqueda en el momento de la ejecución de la. AEM Esta funcionalidad es la misma que la consola Herramientas > Rendimiento de consultas del SDK local de inicio rápido [](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html).

AEM Consultas solo funciona cuando se selecciona un pod específico, ya que abre la consola web de Rendimiento de las consultas de ese pod, lo que requiere que el desarrollador tenga acceso para iniciar sesión en el servicio de la.

![Developer Console - Consultas - Explicar consulta](./assets/developer-console/queries__explain-query.png)

Las consultas ayudan a depurar:

+ Explicar cómo Oak interpreta, analiza y ejecuta las consultas. Esto es muy importante a la hora de rastrear por qué una consulta es lenta y comprender cómo se puede acelerar.
+ AEM Enumeración de las consultas más populares que se ejecutan en la, con la capacidad de Explicarlas.
+ AEM Enumeración de las consultas más lentas que se ejecutan en la, con la capacidad de Explicarlas.
