---
title: AEM Forms con Marketo (parte 1)
description: Tutorial para integrar AEM Forms con Marketo mediante el Modelo de datos de formulario de AEM Forms.
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 45047852-4fdb-4702-8a99-faaad7213b61
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '370'
ht-degree: 0%

---

# AEM Forms Con Marketo

Marketo, parte de Adobe, proporciona un software de automatización de marketing centrado en el marketing basado en cuentas, que incluye correo electrónico, móvil, social, publicidad digital, administración de sitios web y análisis.

Con el Modelo de datos de formulario de AEM Forms, ahora podemos integrar AEM formulario con Marketo sin problemas.

[Obtenga más información sobre el modelo de datos de formulario](https://helpx.adobe.com/experience-manager/6-5/forms/using/data-integration.html)

Marketo expone una API de REST que permite la ejecución remota de muchas de las capacidades del sistema. Desde la creación de programas hasta la importación masiva de posibles clientes, hay muchas opciones que permiten un control detallado de una instancia de Marketo. Con el Modelo de datos de formulario es bastante sencillo integrar AEM Forms con Marketo.

Este tutorial le guiará por los pasos necesarios para integrar AEM Forms con Marketo mediante el modelo de datos de formulario. Al completar el tutorial, tendrá un paquete OSGi que realizará la autenticación personalizada con Marketo. También habrá configurado la fuente de datos mediante el archivo de intercambio proporcionado.

Para comenzar, es muy recomendable que esté familiarizado con los siguientes temas enumerados en la sección Requisitos previos .

## Requisitos previos

1. [AEM servidor con AEM Forms Add en paquete instalado](/help/forms/adaptive-forms/installing-aem-form-on-windows-tutorial-use.md)
1. Entorno de desarrollo de AEM local
1. Familiarizado con el modelo de datos de formulario
1. Conocimientos básicos de Swagger Files
1. Creación de Forms adaptable

**ID secreto de cliente y clave secreta de cliente**

El primer paso en la integración de Marketo con AEM Forms es obtener las credenciales de API necesarias para realizar las llamadas de REST mediante API. Necesitará lo siguiente

1. client_id
1. client_secret
1. identity_endpoint
1. URL de autenticación.

[Siga la documentación oficial de Marketo para obtener las propiedades mencionadas anteriormente.](https://developers.marketo.com/rest-api/) También puede ponerse en contacto con el administrador de la instancia de Marketo.

**Antes de empezar**

[Descargue y descomprima los recursos relacionados con este artículo.](assets/aemformsandmarketo.zip) El archivo zip contiene lo siguiente:

1. BlankTemplatePackage.zip : es la plantilla de formulario adaptable. Importe esto mediante el administrador de paquetes.
1. marketo.json : es el archivo swagger que se utilizará para configurar la fuente de datos.
1. MarketoAndForms.MarketoAndForms.core-1.0-SNAPSHOT.jar : Este es el paquete que realiza la autenticación personalizada. Siéntase libre de usar esto si no puede completar el tutorial o si su paquete no funciona como se espera.
