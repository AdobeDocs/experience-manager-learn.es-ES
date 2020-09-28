---
title: AEM Forms con marketing (parte 1)
seo-title: AEM Forms con marketing (parte 1)
description: Tutorial para integrar AEM Forms con Marketing mediante el Modelo de datos de formulario de AEM Forms.
seo-description: Tutorial para integrar AEM Forms con Marketing mediante el Modelo de datos de formulario de AEM Forms.
feature: adaptive-forms, form-data-model
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '388'
ht-degree: 0%

---


# AEM Forms Con Marketing

Marketo, parte de Adobe, ofrece un software de automatización de marketing centrado en la mercadotecnia basada en cuenta, que incluye correo electrónico, anuncios móviles, sociales, digitales, administración de Web y análisis.

Con el Modelo de datos de formulario de AEM Forms, ahora podemos integrar AEM formulario con Marketing sin problemas.

[Más información sobre el modelo de datos de formulario](https://helpx.adobe.com/experience-manager/6-5/forms/using/data-integration.html)

Marketing expone una API REST que permite la ejecución remota de muchas de las capacidades del sistema. Desde la creación de programas hasta la importación masiva de posibles clientes, hay muchas opciones que permiten un control preciso de una instancia de Marketing. Con el Modelo de datos de formulario es muy sencillo integrar AEM Forms con Marketing.

Este tutorial le guiará por los pasos necesarios para integrar AEM Forms con Marketing mediante el modelo de datos de formulario. Al completar el tutorial, dispondrá de un paquete OSGi que realizará la autenticación personalizada con Marketing. También habrá configurado el origen de datos mediante el archivo de swagger proporcionado.

Para empezar, es muy recomendable que esté familiarizado con los siguientes temas enumerados en la sección Requisitos previos.

## Requisitos previos

1. [AEM servidor con AEM Forms Añadir en el paquete instalado](/help/forms/adaptive-forms/installing-aem-form-on-windows-tutorial-use.md)
1. Entorno de desarrollo AEM local
1. Familiarizado con el modelo de datos de formulario
1. Conocimientos básicos de Swagger Files
1. Creación de Forms adaptable

**ID secreta del cliente y clave secreta del cliente**

El primer paso en la integración de Marketing con AEM Forms es obtener las credenciales de API necesarias para realizar las llamadas REST mediante API. Necesitará lo siguiente

1. client_id
1. client_secret
1. identity_end
1. dirección URL de autenticación.

[Siga la documentación oficial de Marketing para obtener las propiedades mencionadas anteriormente.](https://developers.marketo.com/rest-api/) También puede ponerse en contacto con el administrador de la instancia de Marketing.

**Antes de empezar**

[Descargue y descomprima los recursos relacionados con este artículo.](assets/aemformsandmarketo.zip) El archivo zip contiene lo siguiente:

1. BlankTemplatePackage.zip: es la plantilla de formulario adaptable. Importe esto con el administrador de paquetes.
1. marketo.json: Es el archivo swagger que se utilizará para configurar el origen de datos.
1. MarketoAndForms.MarketoAndForms.core-1.0-SNAPSHOT.jar: es el paquete que realiza la autenticación personalizada. No dude en usar esto si no puede completar el tutorial o si el paquete no funciona correctamente.
