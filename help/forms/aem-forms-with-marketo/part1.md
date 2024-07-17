---
title: Integración de AEM Forms y Marketo
description: Aprenda a integrar AEM Forms y Marketo mediante el modelo de datos de formulario de AEM Forms.
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Integrations, Development
role: Developer
level: Experienced
exl-id: 45047852-4fdb-4702-8a99-faaad7213b61
badgeIntegration: label="Integración" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
last-substantial-update: 2020-03-20T00:00:00Z
duration: 77
source-git-commit: 8bde459ae9a6e261cfc3aff308babe9de6e56059
workflow-type: tm+mt
source-wordcount: '354'
ht-degree: 1%

---

# Integración de AEM Forms y Marketo

Marketo, parte de Adobe, proporciona software de automatización de marketing centrado en el marketing basado en cuentas, que incluye correo electrónico, móvil, social, anuncios digitales, administración web y análisis.

Con el modelo de datos de formulario de AEM Forms AEM, ahora podemos integrar fácilmente formularios de datos de formulario con de Marketo.

[Más información sobre el modelo de datos de formulario](https://helpx.adobe.com/experience-manager/6-5/forms/using/data-integration.html)

Marketo expone una API de REST que permite la ejecución remota de muchas de las funcionalidades del sistema. Desde la creación de programas hasta la importación masiva de posibles clientes, hay muchas opciones que permiten un control preciso de una instancia de Marketo. Con el modelo de datos de formulario es bastante sencillo integrar AEM Forms con Marketo.

Este tutorial le guiará por los pasos necesarios para integrar AEM Forms con Marketo mediante el modelo de datos de formulario. Al completar el tutorial, tendrá un paquete OSGi que hará la autenticación personalizada contra Marketo. También habrá configurado la fuente de datos utilizando el archivo Swagger proporcionado.

Para empezar, es muy recomendable que esté familiarizado con los siguientes temas enumerados en la sección Requisitos previos.

## Requisitos previos

1. [AEM Servidor de con el paquete AEM Forms Add on instalado](/help/forms/adaptive-forms/installing-aem-form-on-windows-tutorial-use.md)
1. AEM Entorno de desarrollo local
1. Familiarizado con el modelo de datos de formulario
1. Conocimientos básicos de los archivos Swagger
1. Creación de Forms adaptable

**ID secreto de cliente y clave secreta de cliente**

El primer paso en la integración de Marketo con AEM Forms es obtener las credenciales de la API necesarias para realizar las llamadas de REST mediante API. Necesitará lo siguiente

1. client_id
1. client_secret
1. identity_endpoint
1. URL de autenticación

[Siga la documentación oficial de Marketo para obtener las propiedades mencionadas.](https://developers.marketo.com/rest-api/) También puede comunicarse con el administrador de su instancia de Marketo.

**Antes de comenzar**

* [Descargue y descomprima los recursos relacionados con este tutorial](assets/marketo-integration-assets.zip)

El archivo zip contiene lo siguiente:

1. BlankTemplatePackage.zip: esta es la plantilla del formulario adaptable. Importe esto mediante el administrador de paquetes.
1. marketo.json: es el archivo swagger que se utiliza para configurar la fuente de datos.
1. Asegúrese de cambiar la propiedad del host en marketo.json para que apunte a la instancia de marketo

## Siguientes pasos

[Crear Source de datos](./part2.md)
