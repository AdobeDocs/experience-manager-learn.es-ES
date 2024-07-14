---
title: Creación de una configuración de Cloud Service de etiquetas en AEM Sites
description: Obtenga información sobre cómo crear una configuración de Cloud Service AEM de etiquetas en.
solution: Experience Manager, Data Collection, Experience Platform
jira: KT-5982
thumbnail: 38566.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="Integración" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: a72ddced-37de-4b62-9e28-fa5b6c8ce5b7
duration: 99
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '495'
ht-degree: 0%

---

# Creación de una configuración de Cloud Service AEM de etiquetas en el {#create-launch-cloud-service}

Obtenga información sobre cómo crear una configuración de Cloud Service de etiquetas en Adobe Experience Manager. AEM Se puede aplicar la configuración del Cloud Service de etiquetas de a un sitio existente, y se puede observar cómo se cargan las bibliotecas de etiquetas en los entornos de Author y Publish.

## Crear servicio de nube de etiquetas

Cree la configuración del servicio de nube de etiquetas siguiendo estos pasos.

1. En el menú **Herramientas**, seleccione la sección **Cloud Service** y haga clic en **Configuraciones de Adobe**
1. Seleccione la carpeta de configuración de su sitio o seleccione **Sitio WKND** (si utiliza el proyecto de guía WKND) y haga clic en **Crear**
1. En la ficha _General_, asigne un nombre a la configuración mediante el campo **Título** y seleccione **Adobe Launch** del menú desplegable _Configuración de Adobe IMS asociada_. A continuación, seleccione el nombre de su empresa en el menú desplegable _Empresa_ y seleccione la propiedad creada anteriormente en el menú desplegable _Propiedad_.
1. Desde las pestañas _Ensayo_ y _Producción_ mantenga las configuraciones predeterminadas. Sin embargo, se recomienda revisar y cambiar las configuraciones para la configuración de producción real, específicamente la opción _Cargar biblioteca de forma asíncrona_ en función de sus requisitos de rendimiento y optimización. Tenga en cuenta también que el valor _URI de biblioteca_ es diferente para Ensayo y Producción.
1. Finalmente, haga clic en **Crear** para completar los servicios de nube de etiquetas.

   ![Configuración de Cloud Service de etiquetas](assets/launch-cloud-services-config.png)

## Aplicar el servicio de nube de etiquetas al sitio

AEM Para cargar la propiedad Tag y sus bibliotecas en el sitio de, se aplica la configuración del servicio en la nube de etiquetas al sitio. En el paso anterior, la configuración del servicio en la nube se crea en la carpeta de nombre del sitio (WKND Site), por lo que debe aplicarse automáticamente. Vamos a verificarla.

1. En el menú **Navegación**, seleccione el icono **Sitios**.

1. AEM Seleccione la página raíz del sitio de y haga clic en **Propiedades**. A continuación, vaya a la pestaña **Avanzado** y en la sección **Configuración**, compruebe que el valor de Configuración de nube apunte a la carpeta `conf` específica del sitio.

   ![Aplicar configuración de Cloud Service al sitio](assets/apply-cloud-services-config-to-site.png)

## Verificar la carga de la propiedad Tag en las páginas de Autor y Publish

AEM Ahora es el momento de comprobar que la propiedad Tag y sus bibliotecas se cargan en la página del sitio de la.

1. Abra su página de sitio favorita en el modo **Ver como publicado**, en la consola del explorador debería ver el mensaje de registro. Es el mismo mensaje del fragmento de código JavaScript de la regla de propiedad Tag que se activa cuando se activa el evento _Library Loaded (Page Top)_.

1. Para verificarlo en Publish, publique primero la configuración de **tags cloud service** y abra la página del sitio en la instancia de Publish.

   ![Propiedad de etiquetas en páginas de autor y Publish](assets/tag-property-on-author-publish-pages.png)

Enhorabuena. AEM Ha completado la integración de etiquetas de recopilación de datos y de datos que inserta código de JavaScript AEM AEM en el sitio de la sin actualizar el código de proyecto.

## Reto: actualizar y publicar reglas en la propiedad de etiquetas

AEM Utilice las lecciones aprendidas del [Crear una propiedad de etiqueta](./create-tag-property.md) anterior para completar el desafío simple, actualice la regla existente para agregar una instrucción de consola adicional y use _Flujo de publicación_ para implementarla en el sitio de la consola de la aplicación de la consola de la aplicación de la aplicación de la aplicación de la aplicación de la aplicación de la aplicación de la aplicación de publicación en el sitio de la.

## Siguientes pasos

[Depuración de una implementación de etiquetas](debug-tags-implementation.md)
