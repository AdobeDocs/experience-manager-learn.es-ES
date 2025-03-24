---
title: 'Conceptos avanzados de AEM sin encabezado: GraphQL'
description: Un tutorial completo que ilustra conceptos avanzados de las API de GraphQL de Adobe Experience Manager (AEM).
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: daae6145-5267-4958-9abe-f6b7f469f803
duration: 441
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1052'
ht-degree: 0%

---

# Conceptos avanzados de AEM Headless

{{aem-headless-trials-promo}}

Este tutorial completo continúa el [tutorial básico](../multi-step/overview.md) que cubre los aspectos básicos de Adobe Experience Manager (AEM) Headless y GraphQL. El tutorial avanzado ilustra aspectos detallados del trabajo con modelos de fragmentos de contenido, fragmentos de contenido y las consultas persistentes de AEM GraphQL, incluido el uso de consultas persistentes de GraphQL en una aplicación cliente.

## Requisitos previos

Complete la [configuración rápida de AEM as a Cloud Service](../quick-setup/cloud-service.md) para configurar su entorno de AEM as a Cloud Service.

Se recomienda completar los tutoriales anteriores de [tutorial básico](../multi-step/overview.md) y [series de vídeos](../video-series/modeling-basics.md) antes de continuar con este tutorial avanzado. Aunque puede completar el tutorial utilizando un entorno local de AEM, este tutorial solo cubre el flujo de trabajo de AEM as a Cloud Service.

>[!CAUTION]
>
>Si no tiene acceso al entorno de AEM as a Cloud Service, puede completar la configuración rápida de [AEM sin encabezado mediante SDK local](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/quick-setup/local-sdk.html). Sin embargo, es importante tener en cuenta que algunas páginas de la interfaz de usuario del producto, como la navegación por fragmentos de contenido, son diferentes.



## Objetivos

Este tutorial abarca los siguientes temas:

* Cree modelos de fragmentos de contenido con reglas de validación y tipos de datos más avanzados, como marcadores de posición de tabulación, referencias de fragmento anidadas, objetos JSON y tipos de datos de fecha y hora.
* Cree fragmentos de contenido mientras trabaja con contenido anidado y referencias de fragmento, y configure directivas de carpeta para el control de creación de fragmentos de contenido.
* Explore las funcionalidades de la API de AEM GraphQL mediante consultas de GraphQL con variables y directivas.
* Mantenga las consultas de GraphQL con parámetros en AEM y aprenda a utilizar parámetros de control de caché con consultas persistentes.
* Integre solicitudes para consultas persistentes en la aplicación WKND GraphQL React de ejemplo mediante AEM Headless JavaScript SDK.

## Información general sobre conceptos avanzados de AEM Headless

El siguiente vídeo proporciona información general de alto nivel sobre los conceptos que se tratan en este tutorial. El tutorial incluye la definición de modelos de fragmentos de contenido con tipos de datos más avanzados, el anidamiento de fragmentos de contenido y la persistencia de consultas de GraphQL en AEM.

>[!VIDEO](https://video.tv.adobe.com/v/340035?quality=12&learn=on)

>[!CAUTION]
>
>Este vídeo (a las 2:25) menciona cómo instalar el editor de consultas de GraphiQL a través del Administrador de paquetes para explorar consultas de GraphQL. Sin embargo, en las versiones más recientes de AEM as a Cloud Service se proporciona un explorador integrado **GraphiQL Explorer**, por lo que no se requiere la instalación del paquete. Consulte [Uso del IDE de GraphiQL](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/graphiql-ide.html) para obtener más información.


## Configuración del proyecto

El proyecto del sitio WKND tiene todas las configuraciones necesarias para que puedas iniciar el tutorial justo después de completar la [configuración rápida](../quick-setup/cloud-service.md). En esta sección solo se destacan algunos pasos importantes que puede seguir al crear su propio proyecto de AEM sin encabezado.


### Revisar configuración existente

El primer paso para iniciar cualquier nuevo proyecto en AEM es crear su configuración, como espacio de trabajo y crear puntos finales de API de GraphQL. Para revisar o crear una configuración, vaya a **Herramientas** > **General** > **Explorador de configuración**.

![Navegar al Explorador de configuración](assets/overview/create-configuration.png)

Observe que la configuración del sitio `WKND Shared` ya se ha creado para el tutorial. Para crear una configuración para su propio proyecto, seleccione **Crear** en la esquina superior derecha y complete el formulario en el modal Crear configuración que aparece.

![Revisar configuración compartida WKND](assets/overview/review-wknd-shared-configuration.png)

### Revisar extremos de API de GraphQL

A continuación, debe configurar los extremos de la API para enviar consultas de GraphQL a. Para revisar los extremos existentes o crear uno, vaya a **Herramientas** > **General** > **GraphQL**.

![Configurar extremos](assets/overview/endpoints.png)

Observe que `WKND Shared Endpoint` ya se ha creado. Para crear un punto final para su proyecto, seleccione **Crear** en la esquina superior derecha y siga el flujo de trabajo.

![Revisar extremo compartido de WKND](assets/overview/review-wknd-shared-endpoint.png)

>[!NOTE]
>
> Después de guardar el extremo, verá un modal sobre la visita a la consola de seguridad, que le permite ajustar la configuración de seguridad si desea configurar el acceso al extremo. Sin embargo, los permisos de seguridad están fuera del ámbito de este tutorial. Para obtener más información, consulte la [documentación de AEM](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security.html?lang=es).

### Revisar la estructura de contenido de WKND y la carpeta raíz del idioma

Una estructura de contenido bien definida es clave para el éxito de la implementación sin encabezado de AEM. Resulta útil para la escalabilidad, la facilidad de uso y la administración de permisos del contenido.

Una carpeta raíz de idioma es una carpeta con un código de idioma ISO como su nombre, como EN o FR. El sistema de administración de traducciones de AEM utiliza estas carpetas para definir el idioma principal del contenido y los idiomas para la traducción de contenido.

Vaya a **Navegación** > **Assets** > **Archivos**.

![Navegar a archivos](assets/overview/files.png)

Vaya a la carpeta **WKND Shared**. Observe la carpeta con el título &quot;English&quot; y el nombre &quot;EN&quot;. Esta carpeta es la carpeta raíz de idioma del proyecto del sitio WKND.

![Carpeta en inglés](assets/overview/english.png)

Para su propio proyecto, cree una carpeta raíz de idioma dentro de la configuración. Consulte la sección sobre [creación de carpetas](/help/headless-tutorial/graphql/advanced-graphql/author-content-fragments.md#create-folders) para obtener más información.

### Asignar una configuración a la carpeta anidada

Finalmente, debe asignar la configuración del proyecto a la carpeta raíz del idioma. Esta asignación permite la creación de fragmentos de contenido basados en los modelos de fragmentos de contenido definidos en la configuración del proyecto.

Para asignar la carpeta raíz de idioma a la configuración, selecciónela y, a continuación, seleccione **Propiedades** en la barra de navegación superior.

![Seleccionar propiedades](assets/overview/properties.png)

A continuación, vaya a la pestaña **Cloud Services** y seleccione el icono de la carpeta en el campo **Configuración de nube**.

![Configuración de la nube](assets/overview/cloud-conf.png)

En el modal que aparece, seleccione la configuración creada anteriormente para asignarle la carpeta raíz del idioma.

### Prácticas recomendadas

Las siguientes son las prácticas recomendadas al crear su propio proyecto en AEM:

* La jerarquía de carpetas debe modelarse teniendo en cuenta la localización y la traducción. En otras palabras, las carpetas de idioma deben anidarse dentro de las carpetas de configuración, lo que permite traducir fácilmente el contenido dentro de esas carpetas de configuración.
* La jerarquía de carpetas debe mantenerse plana y directa. Evite mover o cambiar el nombre de carpetas y fragmentos más adelante, especialmente después de publicar para uso activo, ya que cambia las rutas que pueden afectar a las referencias de fragmento y a las consultas de GraphQL.

## Paquetes de inicio y solución

Hay dos **paquetes** de AEM disponibles y se pueden instalar mediante [Administrador de paquetes](/help/headless-tutorial/graphql/advanced-graphql/author-content-fragments.md#sample-content)

* [Advanced-GraphQL-Tutorial-Starter-Package-1.1.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Starter-Package-1.1.zip) se usa más adelante en el tutorial y contiene carpetas e imágenes de ejemplo.
* [Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip) contiene la solución finalizada para los capítulos 1-4, incluidos los nuevos modelos de fragmentos de contenido, los fragmentos de contenido y las consultas de GraphQL persistentes. Útil para aquellos que desean saltar directamente al capítulo [Integración de aplicaciones cliente](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md).


El proyecto [React App - Tutorial avanzado - WKND Adventures](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/advanced-tutorial/README.md) está disponible para revisar y explorar la aplicación de ejemplo. Esta aplicación de ejemplo recupera el contenido de AEM invocando las consultas de GraphQL persistentes y lo procesa en una experiencia envolvente.

## Introducción

Para empezar con este tutorial avanzado, siga estos pasos:

1. Configure un entorno de desarrollo usando [AEM as a Cloud Service](../quick-setup/cloud-service.md).
1. Inicie el capítulo del tutorial en [Crear modelos de fragmentos de contenido](/help/headless-tutorial/graphql/advanced-graphql/create-content-fragment-models.md).
