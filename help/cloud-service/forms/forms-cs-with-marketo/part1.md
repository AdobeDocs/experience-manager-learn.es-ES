---
title: Integración de AEM Forms as a Cloud Service y Marketo
description: Aprenda a integrar AEM Forms y Marketo mediante el modelo de datos de formulario de AEM Forms.
feature: Form Data Model,Integration
version: Experience Manager as a Cloud Service
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="Integración" type="positive"
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
last-substantial-update: 2024-07-24T00:00:00Z
jira: KT-15876
exl-id: c3145149-bfa4-4dcb-acde-c359e9348f99
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '342'
ht-degree: 1%

---

# Integración de AEM Forms y Marketo

Marketo, que forma parte de Adobe, proporciona software de automatización de marketing centrado en el marketing basado en cuentas, que incluye correo electrónico, móvil, social, anuncios digitales, administración web y análisis.

Con el modelo de datos de formulario de AEM Forms, ahora podemos integrar fácilmente formularios de AEM con Marketo.

[Más información sobre el modelo de datos de formulario](https://helpx.adobe.com/es/experience-manager/6-5/forms/using/data-integration.html)

Marketo expone una API de REST que permite la ejecución remota de muchas de las funcionalidades del sistema. Desde la creación de programas hasta la importación masiva de posibles clientes, hay muchas opciones que permiten un control preciso de una instancia de Marketo. Con el modelo de datos de formulario es bastante sencillo integrar AEM Forms con Marketo.

Este tutorial le guiará por los pasos necesarios para integrar AEM Forms con Marketo mediante el modelo de datos de formulario. Al completar el tutorial, tendrá un paquete OSGi que hará la autenticación personalizada contra Marketo. También habrá configurado la fuente de datos utilizando el archivo Swagger proporcionado.

Para empezar, es muy recomendable que esté familiarizado con los siguientes temas enumerados en la sección Requisitos previos.

## Requisitos previos

1. Acceso a la instancia de AEM Forms as a Cloud Service
1. Familiarizado con el modelo de datos de formulario
1. Conocimientos básicos de los archivos Swagger
1. Creación de Forms adaptable

**ID secreto de cliente y clave secreta de cliente**

El primer paso en la integración de Marketo con AEM Forms es obtener las credenciales de la API necesarias para realizar las llamadas de REST mediante API. Necesitará lo siguiente

1. client_id
1. client_secret
1. identity_endpoint

[Siga la documentación oficial de Marketo para obtener las propiedades mencionadas.](https://developers.marketo.com/rest-api/) También puede comunicarse con el administrador de su instancia de Marketo.

**Antes de comenzar**

* [Descargue y descomprima los recursos relacionados con este tutorial](assets/marketo.zip)

El archivo zip contiene lo siguiente:

1. marketo.json: es el archivo swagger que se utiliza para configurar la fuente de datos.
1. Asegúrese de cambiar la propiedad del host en marketo.json para que apunte a la instancia de marketo

## Siguientes pasos

[Crear Source de datos](./part2.md)
