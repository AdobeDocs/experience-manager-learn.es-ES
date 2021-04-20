---
title: Configuración de DataSource con Salesforce en AEM Forms 6.3 y 6.4
seo-title: Configuración de DataSource con Salesforce en AEM Forms 6.3 y 6.4
description: Integración de AEM Forms con Salesforce mediante el modelo de datos de formulario
seo-description: Integración de AEM Forms con Salesforce mediante el modelo de datos de formulario
uuid: 0124526d-f1a3-4f57-b090-a418a595632e
feature: Adaptive Forms, Form Data Model
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 8e314fc3-62d0-4c42-b1ff-49ee34255e83
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '936'
ht-degree: 0%

---


# Configuración de DataSource con Salesforce en AEM Forms 6.3 y 6.4{#configuring-datasource-with-salesforce-in-aem-forms-and}

## Requisitos previos {#prerequisites}

En este artículo, explicaremos el proceso de creación de fuentes de datos con Salesforce

Requisitos previos para este tutorial:

* Desplácese hasta la parte inferior de esta página y descargue el archivo de intercambio y guárdelo en su disco duro.
* AEM Forms con SSL habilitado

   * [Documentación oficial para habilitar SSL en AEM 6.3](https://helpx.adobe.com/experience-manager/6-3/sites/administering/using/ssl-by-default.html)
   * [Documentación oficial para habilitar SSL en AEM 6.4](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/ssl-by-default.html)

* Necesitará tener la cuenta de Salesforce
* Deberá crear una aplicación conectada. La documentación oficial de Salesforce para crear la aplicación se muestra [aquí](https://help.salesforce.com/articleView?id=connected_app_create.htm&amp;type=0).
* Proporcionar los ámbitos de OAuth adecuados para la aplicación (he seleccionado todos los ámbitos de OAuth disponibles para realizar pruebas)
* Proporcione la URL de devolución de llamada. La URL de devolución de llamada en mi caso era

   * Si utiliza **AEM Forms 6.3**, la URL de retorno será https://gbedekar-w7-1:6443/etc/cloudservices/fdm/createlead.html. En esta URL creada, leída es el nombre de mi modelo de datos de formulario.

   * Si utiliza** AEM Forms 6.4**, la URL de retorno será [https://gbedekar-w7-:6443/libs/fd/fdm/gui/components/admin/fdmcloudservice/createcloudconfigwizard/cloudservices.html](https://gbedekar-w7-1:6443/libs/fd/fdm/gui/components/admin/fdmcloudservice/createcloudconfigwizard/cloudservices.html)

En este ejemplo, gbedekar -w7-1:6443 es el nombre de mi servidor y el puerto en el que se está ejecutando AEM.

Una vez que haya creado la aplicación conectada, anote la **clave de consumidor y clave secreta**. Los necesitará al crear el origen de datos en AEM Forms.

Ahora que ha creado la aplicación conectada, debe crear un archivo de intercambio para las operaciones que debe realizar en salesforce. Se incluye un archivo de intercambio de muestras como parte de los recursos descargables. Este archivo de intercambio le permite crear el objeto &quot;Posible cliente&quot; en el envío del formulario adaptable. Explore este archivo de intercambio.

El siguiente paso es crear una fuente de datos en AEM Forms. Siga los siguientes pasos según su versión de AEM Forms

## AEM Forms 6.3 {#aem-forms}

* Inicie sesión en AEM Forms mediante el protocolo https
* Vaya a los servicios de nube escribiendo https://&lt;server_name>:&lt;server_port> /etc/cloudservices.html, por ejemplo https://gbedekar-w7-1:6443/etc/cloudservices.html
* Desplácese hacia abajo hasta &quot;Modelo de datos de formulario&quot;.
* Haga clic en &quot;Mostrar configuraciones&quot;.
* Haga clic en &quot;+&quot; para añadir una nueva configuración
* Seleccione &quot;Rest Full Service&quot;. Proporcione un Título y un Nombre significativos a la configuración. Por ejemplo,

   * Nombre: CreateLeadInSalesForce
   * Título: CreateLeadInSalesForce

* Haga clic en &quot;Crear&quot;

**En la siguiente pantalla **

* Seleccione &quot;Archivo&quot; como la opción para el archivo de origen de intercambio. Vaya al archivo que descargó anteriormente
* Seleccione Tipo de autenticación como OAuth2.0
* Proporcione los valores ClientID y Client Secret
* La dirección URL de OAuth es: **https://login.salesforce.com/services/oauth2/authorize**
* Actualizar Url Del Token: **https://na5.salesforce.com/services/oauth2/token**
* **Dirección Url De La Toma De Acceso: https://na5.salesforce.com/services/oauth2/token**
* Ámbito de autorización: ** api   id completo de chatter_api   openid   refresh_token visualforce web**
* Gestor de autenticación: Portador de autorización
* Haga clic en &quot;Conectar con OAUTH&quot;. Si todo va bien, no debería ver ningún error

Una vez creado el Modelo de datos de formulario con Salesforce, puede crear la integración de datos de formulario con el origen de datos que acaba de crear. La documentación oficial para crear la integración de datos de formulario es [aquí](https://helpx.adobe.com/aem-forms/6-3/data-integration.html).

Asegúrese de configurar el Modelo de datos de formulario para incluir el servicio POST y crear un objeto de posible cliente en SFDC.

También deberá configurar el servicio de lectura y escritura para el objeto Lead . Consulte las capturas de pantalla de la parte inferior de esta página.

Después de crear el Modelo de datos de formulario, puede crear formularios adaptables basados en este modelo y utilizar los métodos de envío del Modelo de datos de formulario para crear posibles clientes en SFDC.

## AEM Forms 6.4 {#aem-forms-1}

* Crear fuente de datos

   * [Navegar a las fuentes de datos](http://localhost:4502/libs/fd/fdm/gui/components/admin/fdmcloudservice/fdm.html/conf/global)

   * Haga clic en el botón &quot;Crear&quot;
   * Proporcionar algunos valores significativos

      * Nombre: CreateLeadInSalesForce
      * Título: CreateLeadInSalesForce
      * Tipo de servicio: Servicio RESTful
   * Haga clic en Siguiente
   * Fuente de intercambio: Archivo
   * Busque y seleccione el archivo de cambio que descargó en el paso anterior
   * Tipo de autenticación: OAuth 2.0. Especifique los siguientes valores
   * Proporcione los valores ClientID y Client Secret
   * La dirección URL de OAuth es: **https://login.salesforce.com/services/oauth2/authorize**
   * Actualizar Url Del Token: **https://na5.salesforce.com/services/oauth2/token**
   * Usuario del token de acceso **l - https://na5.salesforce.com/services/oauth2/token**
   * Ámbito de autorización: ** api chatter_api id completo openid refresh_token visualforce web**
   * Gestor de autenticación: Portador de autorización
   * Haga clic en el botón &quot;Conectar a OAuth&quot;. Si observa algún error, revise los pasos anteriores para asegurarse de que toda la información se introdujo con precisión.


Una vez que haya creado la fuente de datos mediante SalesForce, puede crear la integración de datos de formulario con la fuente de datos que acaba de crear. El enlace de documentación para esto es [aquí](https://helpx.adobe.com/experience-manager/6-4/forms/using/create-form-data-models.html)

Asegúrese de configurar el Modelo de datos de formulario para incluir el servicio POST y crear un objeto de posible cliente en SFDC.

También deberá configurar el servicio de lectura y escritura para el objeto Lead . Consulte las capturas de pantalla de la parte inferior de esta página.

Después de crear el Modelo de datos de formulario, puede crear formularios adaptables basados en este modelo y utilizar los métodos de envío del Modelo de datos de formulario para crear posibles clientes en SFDC.

>[!NOTE]
>
>Asegúrese de que la dirección URL del archivo de intercambio corresponde a su región. Por ejemplo, la dirección url del archivo de muestra de swagger es &quot;na46.salesforce.com&quot;, ya que la cuenta se creó en Norteamérica. La forma más sencilla es iniciar sesión en su cuenta de Salesforce y comprobar la dirección url .

![sfdc1](assets/sfdc1.gif)

![sfdc2](assets/sfdc2.png)

[ArchivoSwaggerDeMuestra](assets/swagger-sales-force-lead.json)
