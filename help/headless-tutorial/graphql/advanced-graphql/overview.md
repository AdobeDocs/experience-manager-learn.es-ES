---
title: 'AEM Conceptos avanzados de la tecnología sin encabezado de: GraphQL'
description: Un tutorial completo que ilustra conceptos avanzados de las API de Adobe Experience Manager AEM () GraphQL.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: daae6145-5267-4958-9abe-f6b7f469f803
duration: 508
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '1052'
ht-degree: 0%

---

# AEM Conceptos avanzados de la tecnología sin encabezado

{{aem-headless-trials-promo}}

Este tutorial completo continúa con la [tutorial básico](../multi-step/overview.md) que abarcaba los aspectos básicos de Adobe Experience Manager AEM () Headless y GraphQL. AEM El tutorial avanzado ilustra aspectos exhaustivos del trabajo con modelos de fragmentos de contenido, fragmentos de contenido y las consultas persistentes de GraphQL de, incluido el uso de consultas persistentes de GraphQL en una aplicación cliente.

## Requisitos previos

Complete la [AEM configuración rápida para el as a Cloud Service de la](../quick-setup/cloud-service.md) AEM para configurar el entorno as a Cloud Service de la.

Se recomienda completar el paso anterior [tutorial básico](../multi-step/overview.md) y [serie de vídeos](../video-series/modeling-basics.md) tutoriales antes de continuar con este tutorial avanzado. AEM AEM Aunque puede completar el tutorial utilizando un entorno de local, este tutorial solo cubre el flujo de trabajo para los usuarios as a Cloud Service de la.

>[!CAUTION]
>
>AEM Si no tiene acceso a un entorno as a Cloud Service, puede completar el proceso de [AEM Configuración rápida sin encabezado mediante el SDK local](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/quick-setup/local-sdk.html). Sin embargo, es importante tener en cuenta que algunas páginas de la interfaz de usuario del producto, como la navegación por fragmentos de contenido, son diferentes.



## Objetivos

Este tutorial abarca los siguientes temas:

* Cree modelos de fragmentos de contenido con reglas de validación y tipos de datos más avanzados, como marcadores de posición de tabulación, referencias de fragmento anidadas, objetos JSON y tipos de datos de fecha y hora.
* Cree fragmentos de contenido mientras trabaja con contenido anidado y referencias de fragmento, y configure directivas de carpeta para el control de creación de fragmentos de contenido.
* AEM Explore las funcionalidades de la API de GraphQL de la mediante consultas de GraphQL con variables y directivas.
* Mantenga las consultas de GraphQL AEM con parámetros en la caché y aprenda a utilizar parámetros de control de caché con consultas persistentes.
* Integre solicitudes para consultas persistentes en la aplicación de WKND GraphQL AEM React de ejemplo mediante el SDK de JavaScript sin encabezado.

## AEM Conceptos avanzados de información general sobre el acceso sin encabezado

El siguiente vídeo proporciona información general de alto nivel sobre los conceptos que se tratan en este tutorial. El tutorial incluye la definición de modelos de fragmentos de contenido con tipos de datos más avanzados, el anidamiento de fragmentos de contenido y la persistencia de consultas GraphQL AEM en la creación de segmentos de.

>[!VIDEO](https://video.tv.adobe.com/v/340035?quality=12&learn=on)

>[!CAUTION]
>
>Este vídeo (a las 2:25) menciona cómo instalar el editor de consultas de GraphiQL a través del Administrador de paquetes para explorar consultas de GraphQL. AEM Sin embargo, en las versiones más recientes de la aplicación, se ha incorporado el nombre de Cloud Service a. **Explorador de GraphiQL** se proporciona, por lo que no es necesario instalar el paquete. Consulte [Uso del IDE de GraphiQL](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/graphiql-ide.html) para obtener más información.


## Configuración del proyecto

El proyecto del sitio WKND tiene todas las configuraciones necesarias, por lo que puede iniciar el tutorial justo después de completar el [configuración rápida](../quick-setup/cloud-service.md). AEM En esta sección solo se destacan algunos pasos importantes que puede seguir al crear su propio proyecto sin encabezado de la aplicación de la interfaz de usuario de la aplicación de.


### Revisar configuración existente

AEM El primer paso para iniciar cualquier nuevo proyecto en el área de trabajo es crear su configuración, como espacio de trabajo, y crear puntos finales de API de GraphQL. Para revisar o crear una configuración, vaya a **Herramientas** > **General** > **Explorador de configuración**.

![Navegar al Explorador de configuración](assets/overview/create-configuration.png)

Observe que la variable `WKND Shared` ya se ha creado la configuración del sitio para el tutorial. Para crear una configuración para su propio proyecto, seleccione **Crear** en la esquina superior derecha y complete el formulario en el modal Crear configuración que aparece.

![Revisar configuración compartida de WKND](assets/overview/review-wknd-shared-configuration.png)

### Revisar extremos de API de GraphQL

A continuación, debe configurar los extremos de la API para enviar consultas de GraphQL a. Para revisar los extremos existentes o crear uno, vaya a **Herramientas** > **General** > **GraphQL**.

![Configurar extremos](assets/overview/endpoints.png)

Observe que la variable `WKND Shared Endpoint` ya se ha creado. Para crear un extremo para el proyecto, seleccione **Crear** en la esquina superior derecha y siga el flujo de trabajo.

![Revisar extremo compartido de WKND](assets/overview/review-wknd-shared-endpoint.png)

>[!NOTE]
>
> Después de guardar el extremo, verá un modal sobre la visita a la consola de seguridad, que le permite ajustar la configuración de seguridad si desea configurar el acceso al extremo. Sin embargo, los permisos de seguridad están fuera del ámbito de este tutorial. Para obtener más información, consulte la [AEM documentación de la](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security.html?lang=es).

### Revisar la estructura de contenido de WKND y la carpeta raíz del idioma

AEM Una estructura de contenido bien definida es clave para el éxito de la implementación sin encabezado de la aplicación de la distribución de contenido sin encabezado. Resulta útil para la escalabilidad, la facilidad de uso y la administración de permisos del contenido.

Una carpeta raíz de idioma es una carpeta con un código de idioma ISO como su nombre, como EN o FR. AEM El sistema de gestión de la traducción de la utiliza estas carpetas para definir el idioma principal del contenido y los idiomas para la traducción de contenido.

Ir a **Navegación** > **Assets** > **Archivos**.

![Navegar a archivos](assets/overview/files.png)

Vaya a **WKND compartido** carpeta. Observe la carpeta con el título &quot;English&quot; y el nombre &quot;EN&quot;. Esta carpeta es la carpeta raíz de idioma del proyecto del sitio WKND.

![Carpeta en inglés](assets/overview/english.png)

Para su propio proyecto, cree una carpeta raíz de idioma dentro de la configuración. Consulte la sección sobre [creación de carpetas](/help/headless-tutorial/graphql/advanced-graphql/author-content-fragments.md#create-folders) para obtener más información.

### Asignar una configuración a la carpeta anidada

Finalmente, debe asignar la configuración del proyecto a la carpeta raíz del idioma. Esta asignación permite la creación de fragmentos de contenido basados en los modelos de fragmentos de contenido definidos en la configuración del proyecto.

Para asignar la carpeta raíz de idioma a la configuración, seleccione la carpeta y, a continuación, seleccione **Propiedades** en la barra de navegación superior.

![Seleccionar propiedades](assets/overview/properties.png)

A continuación, vaya a **Cloud Service** y seleccione el icono de carpeta en la pestaña **Configuración de nube** field.

![Configuración de la nube](assets/overview/cloud-conf.png)

En el modal que aparece, seleccione la configuración creada anteriormente para asignarle la carpeta raíz del idioma.

### Prácticas recomendadas

AEM Las siguientes son las prácticas recomendadas al crear su propio proyecto en:

* La jerarquía de carpetas debe modelarse teniendo en cuenta la localización y la traducción. En otras palabras, las carpetas de idioma deben anidarse dentro de las carpetas de configuración, lo que permite traducir fácilmente el contenido dentro de esas carpetas de configuración.
* La jerarquía de carpetas debe mantenerse plana y directa. Evite mover o cambiar el nombre de carpetas y fragmentos más adelante, especialmente después de publicar para uso activo, ya que cambia las rutas que pueden afectar a las referencias de fragmento y a las consultas de GraphQL.

## Paquetes de inicio y solución

AEM Dos **paquetes** están disponibles y se pueden instalar mediante [Administrador de paquetes](/help/headless-tutorial/graphql/advanced-graphql/author-content-fragments.md#sample-content)

* [Advanced-GraphQL-Tutorial-Starter-Package-1.1.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Starter-Package-1.1.zip) se utiliza más adelante en el tutorial y contiene imágenes y carpetas de ejemplo.
* [Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip) contiene la solución final para los capítulos 1 a 4, incluidos los nuevos modelos de fragmentos de contenido, los fragmentos de contenido y las consultas de GraphQL persistentes. Útil para aquellos que desean saltar directamente al [Integración de aplicaciones cliente](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md) capítulo.


El [React App - Tutorial avanzado - WKND Adventures](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/advanced-tutorial/README.md) El proyecto está disponible para revisar y explorar la aplicación de ejemplo. AEM Esta aplicación de ejemplo recupera el contenido de la aplicación invocando las consultas de GraphQL persistentes y lo procesa en una experiencia inmersiva.

## Introducción

Para empezar con este tutorial avanzado, siga estos pasos:

1. Configuración de un entorno de desarrollo mediante [AEM as a Cloud Service](../quick-setup/cloud-service.md).
1. Inicie el capítulo del tutorial en [Crear modelos de fragmentos de contenido](/help/headless-tutorial/graphql/advanced-graphql/create-content-fragment-models.md).
