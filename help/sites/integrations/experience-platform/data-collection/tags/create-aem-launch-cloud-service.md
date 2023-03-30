---
title: Crear una configuración de Cloud Service de Launch en AEM
description: Obtenga información sobre cómo crear una configuración de Cloud Service de Launch en AEM. La configuración del Cloud Service de Launch se puede aplicar a un sitio existente y las bibliotecas de etiquetas se pueden observar cargando tanto en entornos de autor como de publicación.
topics: integrations
audience: administrator
solution: Experience Manager, Data Collection, Experience Platform
doc-type: technical video
activity: setup
version: Cloud Service
kt: 5982
thumbnail: 38566.jpg
topic: Integrations
role: Developer
level: Intermediate
exl-id: a72ddced-37de-4b62-9e28-fa5b6c8ce5b7
source-git-commit: 18a72187290d26007cdc09c45a050df8f152833b
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# Crear una configuración de Cloud Service de Launch en AEM {#create-launch-cloud-service}

>[!NOTE]
>
>El proceso de cambiar el nombre de Adobe Experience Platform Launch como un conjunto de tecnologías de recopilación de datos se está implementando en la interfaz de usuario, el contenido y la documentación del producto de AEM, por lo que el término Launch se sigue utilizando aquí.

Obtenga información sobre cómo crear una configuración de Cloud Service de Launch en Adobe Experience Manager. AEM configuración del Cloud Service de Launch se puede aplicar a un sitio existente y las bibliotecas de etiquetas se pueden observar cargando tanto en entornos de Autor como de Publicación.

## Creación del servicio en la nube de Launch

Cree la configuración del servicio en la nube de Launch siguiendo los pasos que se indican a continuación.

1. En el **Herramientas** seleccione **Cloud Services** y haga clic en **Configuraciones de Launch de Adobe**

1. Seleccione la carpeta de configuración del sitio o seleccione **Sitio WKND** (si utiliza el proyecto de guía WKND) y haga clic en **Crear**

1. En el _General_ , asigne un nombre a la configuración mediante la pestaña **Título** y seleccione **Adobe Launch** de la variable _Configuración de Adobe IMS asociada_ lista desplegable. A continuación, seleccione el nombre de su empresa en la _Empresa_ lista desplegable y seleccione la propiedad creada anteriormente en la _Propiedad_ lista desplegable.

1. En el _Ensayo_ y _Producción_ mantenga las configuraciones predeterminadas. Sin embargo, se recomienda revisar y cambiar las configuraciones para la configuración de producción real, específicamente el _Cargar biblioteca asincrónicamente_ alterne en función de sus requisitos de rendimiento y optimización. Tenga en cuenta también que la variable _URI de biblioteca_ es diferente para Ensayo y Producción.

1. Finalmente, haga clic en **Crear** para completar los servicios de nube de Launch.

   ![Configuración de Cloud Services de Launch](assets/launch-cloud-services-config.png)

## Aplicar el servicio de nube de Launch al sitio

Para cargar la propiedad Tag y sus bibliotecas en el sitio AEM, la configuración del servicio en la nube de Launch se aplica al sitio. En el paso anterior, la configuración del servicio de nube se crea en la carpeta de nombres de sitio (WKND Site), por lo que debe aplicarse automáticamente, vamos a verificarla.

1. En el **Navegación** seleccione **Sitios** icono.

1. Seleccione la página raíz del sitio AEM y haga clic en **Propiedades**. A continuación, vaya a la **Avanzadas** en **Configuración** , compruebe que el valor de Configuración de nube esté apuntando al específico del sitio `conf` carpeta.

   ![Aplicar configuración de Cloud Services al sitio](assets/apply-cloud-services-config-to-site.png)

## Verificación de la carga de la propiedad Tag en las páginas Author y Publish

Ahora es el momento de verificar que la propiedad Tag y sus bibliotecas se cargan en la página del sitio AEM.

1. Abra la página de sitio favorita en el **Ver tal y como aparece publicado** , en la consola del explorador debe ver el mensaje de registro. Es el mismo mensaje del fragmento de código JavaScript de la regla de propiedad Tag que se activa cuando _Biblioteca cargada (Principio de página)_ se activa.

1. Para verificar en Publicar, publique primero su **Launch cloud service** y abra la página del sitio en la instancia de publicación.

   ![Etiqueta de propiedad en páginas de creación y publicación](assets/tag-property-on-author-publish-pages.png)

Felicitaciones. Ha completado la integración de etiquetas de recopilación de AEM y datos que inserta código JavaScript en el sitio AEM sin actualizar el código del proyecto de AEM.

## Desafío: actualizar y publicar regla en la propiedad Etiqueta

Aprovechar las lecciones aprendidas de las anteriores [Crear una propiedad de etiqueta](./create-tag-property.md) para completar el desafío simple, actualice la regla existente para agregar una instrucción de consola adicional y use _Flujo de publicación_ impleméntelo en el sitio AEM.

## Pasos siguientes

[Depuración de una implementación de etiquetas](debug-tags-implementation.md)
