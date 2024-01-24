---
title: Configuración de DataSource con Salesforce en AEM Forms 6.3 y 6.4
description: Integración de AEM Forms con Salesforce mediante el modelo de datos de formulario
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 7a4fd109-514a-41a8-a3fe-53c1de32cb6d
last-substantial-update: 2020-02-14T00:00:00Z
duration: 202
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '816'
ht-degree: 0%

---

# Configuración de DataSource con Salesforce en AEM Forms 6.3 y 6.4{#configuring-datasource-with-salesforce-in-aem-forms-and}

## Requisitos previos {#prerequisites}

En este artículo, explicamos el proceso de creación de una fuente de datos con Salesforce

Requisitos previos para este tutorial:

* Desplácese hasta la parte inferior de esta página, descargue el archivo swagger y guárdelo en el disco duro.
* AEM Forms con SSL habilitado

   * [AEM Documentación oficial para la activación de SSL en la versión 6.3 de](https://helpx.adobe.com/experience-manager/6-3/sites/administering/using/ssl-by-default.html)
   * [AEM Documentación oficial para la activación de SSL en la versión 6.4 de](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/ssl-by-default.html)

* Necesita tener una cuenta de Salesforce
* Debe crear una aplicación conectada. Se muestra la documentación oficial de Salesforce para crear la aplicación [aquí](https://help.salesforce.com/articleView?id=connected_app_create.htm&amp;type=0).
* Proporcionar los ámbitos de OAuth adecuados para la aplicación (he seleccionado todos los ámbitos de OAuth disponibles para realizar pruebas)
* Proporcione la URL de devolución de llamada. La URL de devolución de llamada en mi caso era

   * Si está utilizando **AEM Forms 6.3**, la URL de devolución de llamada es https://gbedekar-w7-1:6443/etc/cloudservices/fdm/createlead.html. En esta URL, createlead es el nombre de mi modelo de datos de formulario.

   * Si utiliza** AEM Forms 6.4**, la URL de devolución de llamada es https://gbedekar-w7-:6443/libs/fd/fdm/gui/components/admin/fdmcloudservice/createcloudconfigwizard/cloudservices.html

AEM En este ejemplo, gbedekar -w7-1:6443 es el nombre de mi servidor y el puerto en el que se está ejecutando la.

Una vez creada la aplicación conectada, tenga en cuenta lo siguiente **Clave de consumidor y clave secreta**. Los necesita al crear la fuente de datos en AEM Forms.

Ahora que ha creado la aplicación conectada, debe crear un archivo swagger para las operaciones que debe realizar en Salesforce. Se incluye un archivo swagger de muestra como parte de los recursos descargables. Este archivo Swagger le permite crear un objeto &quot;Posible cliente&quot; en el envío del formulario adaptable. Explore este archivo Swagger.

El siguiente paso es crear una fuente de datos en AEM Forms. Siga estos pasos según su versión de AEM Forms

## AEM Forms 6.3 {#aem-forms}

* Inicie sesión en AEM Forms mediante el protocolo https
* Vaya a Cloud Services escribiendo en https://&lt;servername>:&lt;serverport> /etc/cloudservices.html; por ejemplo, https://gbedekar-w7-1:6443/etc/cloudservices.html
* Desplácese hacia abajo hasta Modelo de datos de formulario.
* Haga clic en &quot;Mostrar configuraciones&quot;.
* Haga clic en &quot;+&quot; para añadir una nueva configuración
* Seleccione &quot;Rest Full Service&quot;. Proporcione un título y un nombre significativos a la configuración. Por ejemplo,

   * Nombre: CreateLeadInSalesForce
   * Título: CreateLeadInSalesForce

* Haga clic en &quot;Crear&quot;

**En el siguiente ** de pantalla

* Seleccione &quot;Archivo&quot; como opción para el archivo de origen Swagger. Busque el archivo que descargó anteriormente
* Seleccione el tipo de autenticación como OAuth2.0
* Proporcione los valores ClientID y Client Secret
* La URL de OAuth es - **https://login.salesforce.com/services/oauth2/authorize**
* URL de token de actualización - **https://na5.salesforce.com/services/oauth2/token**
* **URL de token de acceso: https://na5.salesforce.com/services/oauth2/token**
* Ámbito de autorización: ** api chapter_api full id openid refresh_token visualforce web**
* Controlador de autenticación: portador de autorización
* Haga clic en &quot;Conectarse a OAUTH&quot;. Si todo va bien, no debería ver ningún error

Una vez que haya creado el modelo de datos de formulario con Salesforce, puede crear la integración de datos de formulario con la fuente de datos que acaba de crear. La documentación oficial para crear la integración de datos de formulario es [aquí](https://helpx.adobe.com/aem-forms/6-3/data-integration.html).

Asegúrese de configurar el modelo de datos de formulario para incluir el servicio de POST para crear un objeto de posible cliente en SFDC.

También deberá configurar el servicio de lectura y escritura para el objeto de posible cliente. Consulte las capturas de pantalla en la parte inferior de esta página.

Después de crear el modelo de datos de formulario, puede crear un Forms adaptable basado en este modelo y utilizar los métodos de envío del modelo de datos de formulario para crear posibles clientes en SFDC.

## AEM Forms 6.4 {#aem-forms-1}

* Crear fuente de datos

   * [Navegar a fuentes de datos](http://localhost:4502/libs/fd/fdm/gui/components/admin/fdmcloudservice/fdm.html/conf/global)

   * Haga clic en el botón Crear.
   * Proporcionar algunos valores significativos

      * Nombre: CreateLeadInSalesForce
      * Título: CreateLeadInSalesForce
      * Tipo de servicio: servicio RESTful

   * Haga clic en Siguiente
   * Fuente Swagger: Archivo
   * Busque y seleccione el archivo swagger que descargó en el paso anterior
   * Tipo de autenticación: OAuth 2.0. Especifique los siguientes valores
   * Proporcione los valores ClientID y Client Secret
   * La URL de OAuth es - **https://login.salesforce.com/services/oauth2/authorize**
   * URL de token de actualización - **https://na5.salesforce.com/services/oauth2/token**
   * URL de token de acceso **l - https://na5.salesforce.com/services/oauth2/token**
   * Ámbito de autorización: ** api chapter_api full id openid refresh_token visualforce web**
   * Controlador de autenticación: portador de autorización
   * Haga clic en el botón &quot;Conectarse a OAuth&quot;. En caso de que vea algún error, revise los pasos anteriores para asegurarse de que toda la información se ingresó con precisión.

Una vez creada la fuente de datos con SalesForce, puede crear la integración de datos de formulario con la fuente de datos que acaba de crear. El vínculo de documentación para esto es [aquí](https://helpx.adobe.com/experience-manager/6-4/forms/using/create-form-data-models.html)

Asegúrese de configurar el modelo de datos de formulario para incluir el servicio de POST para crear un objeto de posible cliente en SFDC.

También deberá configurar el servicio de lectura y escritura para el objeto de posible cliente. Consulte las capturas de pantalla en la parte inferior de esta página.

Después de crear el modelo de datos de formulario, puede crear un Forms adaptable basado en este modelo y utilizar los métodos de envío del modelo de datos de formulario para crear posibles clientes en SFDC.

>[!NOTE]
>
>Asegúrese de que la dirección URL del archivo swagger corresponda a su región. Por ejemplo, la dirección URL del archivo swagger de ejemplo es &quot;na46.salesforce.com&quot;, ya que la cuenta se creó en Norteamérica. La forma más sencilla es iniciar sesión en su cuenta de Salesforce y comprobar la URL

![sfdc1](assets/sfdc1.gif)

![sfdc2](assets/sfdc2.png)

[SampleSwaggerFile](assets/swagger-sales-force-lead.json)
