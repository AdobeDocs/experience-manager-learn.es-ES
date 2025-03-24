---
title: Consola para desarrolladores
description: AEM as a Cloud Service proporciona un Developer Console para cada entorno que expone varios detalles del servicio de AEM en ejecución que son útiles en la depuración.
feature: Developer Tools
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-5433
thumbnail: kt-5433.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 0499ff9f-d452-459f-b1a2-2853a228efd1
duration: 295
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1562'
ht-degree: 0%

---

# Depuración de AEM as a Cloud Service con Developer Console

AEM as a Cloud Service proporciona un Developer Console para cada entorno que expone varios detalles del servicio de AEM en ejecución que son útiles en la depuración.

Cada entorno de AEM as a Cloud Service tiene su propia Developer Console.

## Navegar a Developer Console

Se accede a Developer Console por entorno de AEM as a Cloud Service a través de Cloud Manager.

![Navegar a Developer Console](./assets/developer-console/navigate.png)

1. Vaya a __[Cloud Manager](https://my.cloudmanager.adobe.com/)__
2. Abra el __Programa__ que contiene el entorno de AEM as a Cloud Service para abrir Developer Console.
3. Busque __Entorno__ y seleccione `...`.
4. Seleccione __Developer Console__ de la lista desplegable.


## Acceso a Developer Console

Para acceder y utilizar Developer Console, se deben otorgar los siguientes permisos al Adobe ID del desarrollador a través de [Admin Console de Adobe](https://adminconsole.adobe.com).

1. Asegúrese de que en el conmutador de organización de Adobe pueda ver la organización de Adobe relacionada con los entornos que desea inspeccionar en Developer Console.
1. Para poder iniciar sesión en Developer Console, el desarrollador debe ser miembro de cualquiera de las siguientes funciones:
   + [Desarrollador __del producto Cloud Manager - Perfil del producto Cloud Service__](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-cloud-manager.html#assign-developer): En este caso, el desarrollador verá la lista completa de entornos disponibles en la URL de Developer Console seleccionada; si se ha seleccionado un entorno de desarrollo o RDE en Cloud Manager, puede que aparezcan otros entornos de desarrollo o RDE en el mismo programa.
   + Perfil de producto de [__Administradores de AEM__ en __Autor de AEM__](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-aem.html#aem-product-profiles): En este caso, la lista de entornos descritos en la viñeta anterior se limitará a los perfiles de producto relacionados donde se asigna este rol.
1. El desarrollador debe ser miembro del perfil de producto [__Usuarios de AEM__ o __Administradores de AEM__ en AEM Author y/o Publish](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-aem.html#aem-product-profiles).
   + Si esta pertenencia no existe, los volcados [status](#status) agotarán el tiempo de espera con un error 401 no autorizado.

### Solución de problemas de acceso a Developer Console

#### Al iniciar sesión, no veo el entorno que estoy buscando en la lista

Asegúrese de lo siguiente:

+ Ha seleccionado la URL de Developer Console correcta haciendo clic en los tres puntos para el entorno seleccionado a través de Cloud Manager y seleccione Developer Console.
+ O bien tiene __Desarrollador del producto Cloud Manager - Perfil del producto Cloud Service__](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-cloud-manager.html#assign-developer) para ver la lista completa de entornos, o bien forma parte del perfil del producto [__Administradores de AEM__ en __Autor de AEM__](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-aem.html#aem-product-profiles) para el entorno que no encuentra.[

#### 401 Error no autorizado al descargar el estado

![Developer Console - 401 no autorizado](./assets/developer-console/troubleshooting__401-unauthorized.png)

Si se descarga cualquier estado y se informa de un error 401 no autorizado, significa que el usuario aún no existe con los permisos necesarios en AEM as a Cloud Service o que el uso de los tokens de inicio de sesión no es válido o ha caducado.

Para resolver el problema 401 No autorizado:

1. Asegúrese de que el usuario sea miembro del perfil de producto de Adobe IMS correspondiente (administradores de AEM o usuarios de AEM) para la instancia de producto de AEM as a Cloud Service asociada a Developer Console.
   + Recuerde que las instancias de producto Developer Console acceden a 2 instancias de producto Adobe IMS; las instancias de producto AEM as a Cloud Service Author y Publish, por lo que debe asegurarse de que se utilizan los perfiles de producto correctos según el nivel de servicio que requiera acceso a través de Developer Console.
1. Inicie sesión en AEM as a Cloud Service (Autor o Publicación) y asegúrese de que los usuarios y grupos se hayan sincronizado correctamente con AEM.
   + Developer Console requiere que se cree su registro de usuario en el nivel de servicio de AEM correspondiente para que se autentique en ese nivel de servicio.
1. Borre las cookies de los exploradores, así como el estado de la aplicación (almacenamiento local) y vuelva a iniciar sesión en Developer Console, asegurándose de que el token de acceso que utiliza Developer Console sea correcto y no haya caducado.

## Pod

Los servicios de AEM as a Cloud Service Author y Publish se componen de varias instancias respectivamente para administrar la variabilidad del tráfico y las actualizaciones móviles sin tiempo de inactividad. Estas instancias se denominan Pods. La selección de la secuencia en Developer Console define el ámbito de los datos que se expondrán a través de los demás controles.

![Developer Console - Pod](./assets/developer-console/pod.png)

+ Un pod es una instancia discreta que forma parte de un servicio de AEM (creación o publicación)
+ Los pods son transitorios, lo que significa que AEM as a Cloud Service los crea y los destruye según sea necesario
+ Solo los pods que forman parte del entorno de AEM as a Cloud Service asociado se enumeran en el conmutador de pods de ese entorno de Developer Console.
+ En la parte inferior del conmutador de pods, las opciones de conveniencia permiten seleccionar pods por tipo de servicio:
   + Todos los autores
   + Todos los editores
   + Todas las instancias

## Estado

Status proporciona opciones para generar un estado de tiempo de ejecución de AEM específico en texto o en salida JSON. Developer Console proporciona información similar a la consola web OSGi del inicio rápido local de AEM SDK, con la marcada diferencia de que Developer Console es de solo lectura.

![Developer Console - Estado](./assets/developer-console/status.png)

### Paquetes

Paquetes enumera todos los paquetes OSGi de AEM. Esta funcionalidad es similar a los paquetes OSGi de inicio rápido local de [AEM SDK](http://localhost:4502/system/console/bundles) en `/system/console/bundles`.

Los paquetes ayudan en la depuración mediante lo siguiente:

+ Enumeración de todos los paquetes OSGi implementados en AEM as a Service
+ Enumeración del estado de cada paquete OSGi, incluso si están activos o no
+ Proporcionar detalles sobre dependencias sin resolver que hacen que los paquetes OSGi se activen

### Componentes

Componentes enumera todos los componentes OSGi de AEM. Esta funcionalidad es similar a los [componentes OSGi de inicio rápido local de AEM SDK](http://localhost:4502/system/console/components) en `/system/console/components`.

Los componentes ayudan en la depuración al:

+ Enumeración de todos los componentes OSGi implementados en AEM as a Cloud Service
+ Proporcionar el estado de cada componente OSGi; incluso si están activos o no satisfechos
+ Proporcionar detalles sobre referencias de servicio no satisfechas puede provocar que los componentes OSGi se activen
+ Enumerar las propiedades OSGi y sus valores enlazados al componente OSGi.
   + Esto mostrará los valores reales insertados a través de [variables de configuración de entorno OSGi](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values).

### Configuraciones

Configuraciones enumera todas las configuraciones del componente OSGi (propiedades y valores OSGi). Esta funcionalidad es similar al Administrador de configuración OSGi de [AEM SDK local quickstart](http://localhost:4502/system/console/configMgr) en `/system/console/configMgr`.

Las configuraciones ayudan en la depuración al:

+ Lista de propiedades OSGi y sus valores por componente OSGi
   + Esto NO mostrará los valores reales insertados mediante [variables de configuración de entorno OSGi](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values). Consulte [Componentes](#components) más arriba para ver los valores insertados.
+ Localización e identificación de propiedades mal configuradas

### Índices Oak

Los índices Oak proporcionan un volcado de los nodos definidos debajo de `/oak:index`. Tenga en cuenta que esto no muestra índices combinados, lo que ocurre cuando se modifica un índice AEM.

Los índices Oak ayudan en la depuración al:

+ Enumeración de todas las definiciones de índice de Oak que proporcionan información sobre cómo se ejecutan las consultas de búsqueda en AEM. Tenga en cuenta que las modificaciones en los índices AEM no se reflejan aquí. Esta vista solo es útil para índices que solo proporciona AEM o que solo proporciona el código personalizado.

### Servicios OSGi

Componentes enumera todos los servicios OSGi. Esta funcionalidad es similar a los servicios OSGi de [AEM SDK](http://localhost:4502/system/console/services) en `/system/console/services`.

Los servicios OSGi ayudan en la depuración al:

+ Enumerar todos los servicios OSGi en AEM, junto con su paquete OSGi proporcionado y todos los paquetes OSGi que lo consumen

### Trabajos de Sling

Trabajos de Sling enumera todas las colas de trabajos de Sling. Esta funcionalidad es similar a [Trabajos de inicio rápido local de AEM SDK](http://localhost:4502/system/console/slingevent) en `/system/console/slingevent`.

Los trabajos de Sling ayudan en la depuración al:

+ Lista de colas de trabajos de Sling y sus configuraciones
+ Proporcionar información sobre el número de trabajos de Sling activos, en cola y procesados, lo que resulta útil para depurar problemas con el flujo de trabajo, el flujo de trabajo transitorio y otro trabajo realizado por los trabajos de Sling en AEM.

## Paquetes Java

Los paquetes Java permiten comprobar si hay un paquete Java y una versión disponibles para usar en AEM as a Cloud Service. Esta funcionalidad es la misma que el Buscador de dependencias de inicio rápido local de [AEM SDK](http://localhost:4502/system/console/depfinder) en `/system/console/depfinder`.

![Developer Console - Paquetes Java](./assets/developer-console/java-packages.png)

Los paquetes Java se utilizan para solucionar problemas cuando los paquetes no se inician debido a importaciones sin resolver o clases sin resolver en scripts (HTL, JSP, etc.). Si los paquetes Java informan de que no hay paquetes que exporten un paquete Java (o la versión no coincide con la importada por un paquete OSGi):

+ Asegúrese de que la versión de la dependencia Maven de la API de AEM de su proyecto coincida con la versión de la versión de AEM del entorno (y, si es posible, actualice todo a la última).
+ Si se utilizan dependencias Maven adicionales en el proyecto Maven
   + Determine si se puede utilizar una API alternativa proporcionada por la dependencia de la API de AEM SDK en su lugar.
   + Si se requiere la dependencia adicional, asegúrese de que se proporcione como un paquete OSGi (en lugar de como un Jar sin formato) y de que esté incrustado en el paquete de código del proyecto, (`ui.apps`), de forma similar a como está incrustado el paquete OSGi principal en el paquete `ui.apps`.

## Servlets

Servlets se utiliza para proporcionar perspectiva sobre cómo AEM resuelve una URL en un servlet o script Java (HTL, JSP) que finalmente gestiona la solicitud. Esta funcionalidad es la misma que [Sling Servlet Resolver local de inicio rápido de AEM SDK](http://localhost:4502/system/console/servletresolver) en `/system/console/servletresolver`.

![Developer Console - Servlets](./assets/developer-console/servlets.png)

Servlets ayuda a depurar y determinar lo siguiente:

+ Cómo se descompone una dirección URL en sus partes accesibles (recurso, selector, extensión).
+ Qué servlet o script resuelve una URL, lo que ayuda a identificar direcciones URL o servlets/scripts mal formados.

## Consultas

Las consultas ayudan a proporcionar información sobre qué y cómo se ejecutan las consultas de búsqueda en AEM. Esta funcionalidad es la misma que la consola de [Herramientas > Rendimiento de consultas de inicio rápido local de AEM SDK](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html).

Las consultas solo funcionan cuando se selecciona un pod específico, ya que abre la consola web de rendimiento de las consultas de ese pod, lo que requiere que el desarrollador tenga acceso para iniciar sesión en el servicio de AEM.

![Developer Console - Consultas - Explicar consulta](./assets/developer-console/queries__explain-query.png)

Las consultas ayudan a depurar:

+ Explicar cómo Oak interpreta, analiza y ejecuta las consultas. Esto es muy importante a la hora de rastrear por qué una consulta es lenta y comprender cómo se puede acelerar.
+ Enumeración de las consultas más populares que se ejecutan en AEM, con la capacidad de Explicarlas.
+ Enumeración de las consultas más lentas que se ejecutan en AEM, con la capacidad de Explicarlas.
