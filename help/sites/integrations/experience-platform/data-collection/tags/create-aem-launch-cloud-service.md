---
title: Creación de una configuración de Cloud Service de Launch en AEM Sites
description: Obtenga información sobre cómo crear una configuración de Cloud Service AEM de Launch en. La configuración del Cloud Service de Launch se puede aplicar a un sitio existente, y las bibliotecas de etiquetas se pueden observar cargando tanto en entornos de autor como de publicación.
topics: integrations
audience: administrator
solution: Experience Manager, Data Collection, Experience Platform
doc-type: technical video
activity: setup
kt: 5982
thumbnail: 38566.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="Integración" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
exl-id: a72ddced-37de-4b62-9e28-fa5b6c8ce5b7
source-git-commit: b044c9982fc9309fb73509dd3117f5467903bd6a
workflow-type: tm+mt
source-wordcount: '556'
ht-degree: 0%

---

# Creación de una configuración de Cloud Service AEM de Launch en el {#create-launch-cloud-service}

>[!NOTE]
>
>El proceso de cambiar el nombre de Adobe Experience Platform Launch AEM a como un conjunto de tecnologías de recopilación de datos se está implementando en la interfaz de usuario, el contenido y la documentación del producto de la, por lo que el término Launch aún se está utilizando aquí.

Obtenga información sobre cómo crear una configuración de Cloud Service de Launch en Adobe Experience Manager. AEM La configuración del Cloud Service de Launch se puede aplicar a un sitio existente, y las bibliotecas de etiquetas se pueden observar cargando tanto en entornos de creación como de publicación.

## Crear un servicio en la nube de Launch

Cree la configuración del servicio en la nube de Launch siguiendo estos pasos.

1. Desde el **Herramientas** menú, seleccione **Cloud Services** y haga clic en **Configuraciones de Adobe de Launch**

1. Seleccione la carpeta de configuración del sitio o seleccione **Sitio WKND** (si utiliza el proyecto de guía WKND) y haga clic en **Crear**

1. Desde el _General_ , asigne un nombre a la configuración mediante la pestaña **Título** y seleccione. **Adobe Launch** desde el _Configuración de Adobe IMS asociada_ desplegable. A continuación, seleccione el nombre de su empresa en la _Compañía_ y seleccione la propiedad creada anteriormente en el menú desplegable _Propiedad_ desplegable.

1. Desde el _Ensayo_ y _Producción_ mantenga las configuraciones predeterminadas. Sin embargo, se recomienda revisar y cambiar las configuraciones para la configuración de producción real, específicamente la _Cargar biblioteca asincrónicamente_ de alternancia en función de sus requisitos de rendimiento y optimización. Tenga en cuenta también que la variable _URI de biblioteca_ El valor es diferente para Ensayo y Producción.

1. Finalmente, haga clic en **Crear** para completar Launch cloud services.

   ![Configuración de Cloud Services de Launch](assets/launch-cloud-services-config.png)

## Aplicar Launch Cloud Service al sitio

AEM Para cargar la propiedad Tag y sus bibliotecas en el sitio de, se aplica la configuración del servicio en la nube de Launch. En el paso anterior, la configuración del servicio en la nube se crea en la carpeta de nombre del sitio (WKND Site), por lo que debe aplicarse automáticamente. Vamos a verificarla.

1. Desde el **Navegación** menú, seleccione **Sites** icono.

1. AEM Seleccione la página raíz del sitio de y haga clic en **Propiedades**. A continuación, vaya a **Avanzadas** y debajo de **Configuración** , compruebe que el valor de Configuración de nube apunta a la configuración específica del sitio `conf` carpeta.

   ![Aplicar configuración de Cloud Services al sitio](assets/apply-cloud-services-config-to-site.png)

## Verificar la carga de la propiedad Tag en las páginas de Autor y Publicación

AEM Ahora es el momento de comprobar que la propiedad Tag y sus bibliotecas se cargan en la página del sitio de la.

1. Abra su página de sitio favorita en la **Ver como aparece publicado** modo, en la consola del explorador debería ver el mensaje de registro. Es el mismo mensaje del fragmento de código JavaScript de la regla de propiedad Tag que se activa cuando _Library Loaded (Page Top)_ se activa el evento.

1. Para verificarlo en Publicar, publique primero su **Launch Cloud Service** y abra la página del sitio en la instancia de publicación.

   ![Propiedad Tag en las páginas de creación y publicación](assets/tag-property-on-author-publish-pages.png)

Felicitaciones. AEM AEM AEM Ha completado la integración de etiquetas de recopilación de datos y de datos que insertan código JavaScript en el sitio de la sin actualizar el código del proyecto de la aplicación de datos y el proyecto de la.

## Reto: actualizar y publicar reglas en la propiedad de etiquetas

Aproveche las lecciones aprendidas de la [Crear una propiedad de etiqueta](./create-tag-property.md) para completar el desafío simple, actualice la regla existente para agregar una instrucción de consola adicional y utilice _Flujo de publicación_ AEM impleméntelo en el sitio de la.

## Pasos siguientes

[Depuración de una implementación de etiquetas](debug-tags-implementation.md)
