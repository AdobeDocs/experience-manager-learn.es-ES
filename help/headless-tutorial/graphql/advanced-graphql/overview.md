---
title: 'Conceptos avanzados de AEM sin encabezado: GraphQL'
description: Un tutorial completo que ilustra los conceptos avanzados de las API de Adobe Experience Manager (AEM) GraphQL.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: daae6145-5267-4958-9abe-f6b7f469f803
source-git-commit: ee6f65fba8db5ae30cc14aacdefbeba39803527b
workflow-type: tm+mt
source-wordcount: '1076'
ht-degree: 1%

---

# Conceptos avanzados de AEM sin cabeza

Este tutorial completo continúa con el [tutorial básico](../multi-step/overview.md) que cubrían los fundamentos de Adobe Experience Manager (AEM) Headless y GraphQL. El tutorial avanzado ilustra aspectos detallados del trabajo con modelos de fragmento de contenido, fragmentos de contenido y consultas persistentes de AEM GraphQL, incluido el uso de consultas persistentes de GraphQL en una aplicación cliente.

## Requisitos previos

Complete el [configuración rápida para AEM as a Cloud Service](../quick-setup/cloud-service.md) para configurar el entorno as a Cloud Service AEM.

Se recomienda completar la anterior [tutorial básico](../multi-step/overview.md) y [serie de vídeos](../video-series/modeling-basics.md) tutoriales antes de continuar con este tutorial avanzado. Aunque puede completar el tutorial utilizando un entorno de AEM local, este tutorial solo cubre el flujo de trabajo de AEM as a Cloud Service.

>[!CAUTION]
>
>Si no tiene acceso a AEM entorno as a Cloud Service, puede completar la [Configuración rápida AEM sin encabezado mediante el SDK local](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/quick-setup/local-sdk.html). Sin embargo, es importante tener en cuenta que algunas páginas de la interfaz de usuario del producto, como la navegación por fragmentos de contenido, son diferentes.



## Objetivos

Este tutorial trata los siguientes temas:

* Cree modelos de fragmento de contenido mediante reglas de validación y tipos de datos más avanzados, como marcadores de posición de pestañas, referencias de fragmento anidadas, objetos JSON y tipos de datos de fecha y hora.
* Cree fragmentos de contenido mientras trabaja con contenido anidado y referencias de fragmento, y configure políticas de carpeta para la gobernanza de la creación de fragmentos de contenido.
* Explore AEM capacidades de la API de GraphQL mediante consultas de GraphQL con variables y directivas.
* Mantenga las consultas de GraphQL con parámetros en AEM y aprenda a utilizar parámetros de control de caché con consultas persistentes.
* Integre solicitudes de consultas persistentes en la aplicación WKND GraphQL React de muestra mediante el SDK de JavaScript sin encabezado de AEM.

## Conceptos avanzados de AEM información general sin encabezado

El siguiente vídeo proporciona información general de alto nivel sobre los conceptos que se tratan en este tutorial. El tutorial incluye la definición de modelos de fragmento de contenido con tipos de datos más avanzados, la anidación de fragmentos de contenido y la persistencia de consultas de GraphQL en AEM.

>[!VIDEO](https://video.tv.adobe.com/v/340035/?quality=12&learn=on)

>[!CAUTION]
>
>Este vídeo (a las 2:25) menciona la instalación del editor de consultas de GraphiQL mediante el Administrador de paquetes para explorar las consultas de GraphQL. Sin embargo, en las versiones más recientes de AEM como Cloud Service se ha incorporado un **Explorador de GraphiQL** se proporciona, por lo tanto la instalación del paquete no es necesaria. Consulte [Uso del IDE de GraphiQL](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/graphiql-ide.html) para obtener más información.


## Configuración del proyecto

El proyecto WKND Site tiene todas las configuraciones necesarias, por lo que puede iniciar el tutorial justo después de completar la [configuración rápida](../quick-setup/cloud-service.md). En esta sección solo se destacan algunos pasos importantes que puede utilizar al crear su propio proyecto sin encabezado de AEM.


### Revisar la configuración existente

El primer paso para iniciar cualquier proyecto nuevo en AEM es crear su configuración, como espacio de trabajo y crear extremos de API de GraphQL. Para revisar o crear una configuración, vaya a **Herramientas** > **General** > **Explorador de configuración**.

![Navegar al explorador de configuración](assets/overview/create-configuration.png)

Observe que la variable `WKND Shared` la configuración del sitio ya se ha creado para el tutorial. Para crear una configuración para su propio proyecto, seleccione **Crear** en la esquina superior derecha y complete el formulario en el modal Crear configuración que aparece.

![Revisión de la configuración compartida WKND](assets/overview/review-wknd-shared-configuration.png)

### Revisión de los extremos de la API de GraphQL

A continuación, debe configurar los extremos de la API para enviar consultas de GraphQL a . Para revisar los puntos finales existentes o crear uno, vaya a **Herramientas** > **General** > **GraphQL**.

![Configuración de puntos finales](assets/overview/endpoints.png)

Observe que la variable `WKND Shared Endpoint` ya se ha creado. Para crear un punto final para el proyecto, seleccione **Crear** en la esquina superior derecha y siga el flujo de trabajo.

![Revisar extremo compartido WKND](assets/overview/review-wknd-shared-endpoint.png)

>[!NOTE]
>
> Después de guardar el punto final, verá un modal sobre la visita a la Consola de seguridad, que le permite ajustar la configuración de seguridad si desea configurar el acceso al punto final. Sin embargo, los propios permisos de seguridad están fuera del ámbito de este tutorial. Para obtener más información, consulte [AEM documentación](https://experienceleague.adobe.com/docs/experience-manager-64/administering/security/security.html?lang=es).

### Revise la estructura del contenido WKND y la carpeta raíz del idioma

Una estructura de contenido bien definida es clave para el éxito de AEM implementación sin objetivos. Resulta útil para la escalabilidad, el uso y la administración de permisos del contenido.

Una carpeta raíz de idioma es una carpeta con un código de idioma ISO como su nombre, como EN o FR. El sistema de administración de AEM traducción utiliza estas carpetas para definir el idioma principal del contenido y los idiomas para la traducción del contenido.

Vaya a **Navegación** > **Recursos** > **Archivos**.

![Navegar a archivos](assets/overview/files.png)

Vaya a **WKND compartido** carpeta. Observe la carpeta con el título &quot;Inglés&quot; y el nombre &quot;EN&quot;. Esta carpeta es la carpeta raíz del idioma para el proyecto WKND Site.

![Carpeta en inglés](assets/overview/english.png)

Para su propio proyecto, cree una carpeta raíz de idioma dentro de la configuración. Consulte la sección sobre [creación de carpetas](/help/headless-tutorial/graphql/advanced-graphql/author-content-fragments.md#create-folders) para obtener más información.

### Asignar una configuración a la carpeta anidada

Finalmente, debe asignar la configuración del proyecto a la carpeta raíz del idioma. Esta asignación permite la creación de fragmentos de contenido basados en modelos de fragmento de contenido definidos en la configuración del proyecto.

Para asignar la carpeta raíz del idioma a la configuración, seleccione la carpeta y, a continuación, seleccione **Propiedades** en la barra de navegación superior.

![Seleccione Propiedades](assets/overview/properties.png)

A continuación, vaya a la **Cloud Services** y seleccione el icono de carpeta en la pestaña **Configuración de nube** campo .

![Configuración de nube](assets/overview/cloud-conf.png)

En el modal que aparece, seleccione la configuración creada anteriormente para asignarle la carpeta raíz del idioma.

### Prácticas recomendadas

A continuación se indican las prácticas recomendadas al crear su propio proyecto en AEM:

* La jerarquía de carpetas debe modelarse teniendo en cuenta la localización y la traducción. En otras palabras, las carpetas de idioma deben estar anidadas dentro de las carpetas de configuración, lo que permite una fácil traducción del contenido dentro de esas carpetas de configuración.
* La jerarquía de carpetas debe mantenerse plana y sencilla. Evite mover o cambiar el nombre de carpetas y fragmentos más adelante, especialmente después de publicar para uso activo, ya que cambia las rutas que pueden afectar a las referencias de fragmento y a las consultas de GraphQL.

## Paquetes de inicio y solución

Dos AEM **paquetes** están disponibles y pueden instalarse mediante [Administrador de paquetes](/help/headless-tutorial/graphql/advanced-graphql/author-content-fragments.md#sample-content)

* [Advanced-GraphQL-Tutorial-Starter-Package-1.1.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Starter-Package-1.1.zip) se utiliza más adelante en el tutorial y contiene imágenes y carpetas de ejemplo.
* [Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip) contiene la solución terminada para los capítulos 1 a 4, incluidos los nuevos modelos de fragmento de contenido, fragmentos de contenido y consultas de GraphQL persistentes. Útil para aquellos que quieran saltarse directamente en la [Integración de aplicaciones de cliente](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md) capítulo.


La variable [Aplicación React: Tutorial avanzado - Aventuras de WKND](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/advanced-tutorial/README.md) El proyecto está disponible para revisar y explorar la aplicación de ejemplo. Esta aplicación de ejemplo recupera el contenido de AEM invocando las consultas de GraphQL persistentes y lo procesa en una experiencia inmersiva.

## Introducción

Para comenzar con este tutorial avanzado, siga estos pasos:

1. Configuración de un entorno de desarrollo mediante [AEM as a Cloud Service](../quick-setup/cloud-service.md).
1. Inicie el capítulo del tutorial en [Crear modelos de fragmento de contenido](/help/headless-tutorial/graphql/advanced-graphql/create-content-fragment-models.md).
