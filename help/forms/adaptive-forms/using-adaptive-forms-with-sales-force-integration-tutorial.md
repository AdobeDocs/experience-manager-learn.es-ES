---
title: Configuración de DataSource con Salesforce en AEM Forms 6.3 y 6.4
seo-title: Configuración de DataSource con Salesforce en AEM Forms 6.3 y 6.4
description: Integración de AEM Forms con Salesforce mediante el modelo de datos de formulario
seo-description: Integración de AEM Forms con Salesforce mediante el modelo de datos de formulario
uuid: 0124526d-f1a3-4f57-b090-a418a595632e
feature: adaptive-forms, form-data-model
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 8e314fc3-62d0-4c42-b1ff-49ee34255e83
translation-type: tm+mt
source-git-commit: cce9f5d1dae05a36b942f6b07a46c65f82eac43c
workflow-type: tm+mt
source-wordcount: '928'
ht-degree: 0%

---


# Configuración de DataSource con Salesforce en AEM Forms 6.3 y 6.4{#configuring-datasource-with-salesforce-in-aem-forms-and}

## Requisitos previos {#prerequisites}

En este artículo, veremos el proceso de creación de fuentes de datos con Salesforce

Requisitos previos para este tutorial:

* Desplácese hasta la parte inferior de esta página y descargue el archivo swagger y guárdelo en su disco duro.
* AEM Forms con SSL habilitado

   * [Documentación oficial para habilitar SSL en AEM 6.3](https://helpx.adobe.com/experience-manager/6-3/sites/administering/using/ssl-by-default.html)
   * [Documentación oficial para habilitar SSL en AEM 6.4](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/ssl-by-default.html)

* Necesitará tener la cuenta de Salesforce
* Deberá crear una aplicación conectada. La documentación oficial de Salesforce para crear la aplicación se muestra [aquí](https://help.salesforce.com/articleView?id=connected_app_create.htm&amp;type=0).
* Proporcione los ámbitos de OAuth adecuados para la aplicación (he seleccionado todos los ámbitos de OAuth disponibles para la realización de pruebas)
* Proporcione la dirección URL de llamada de retorno. La dirección URL de llamada de retorno en mi caso era

   * Si utiliza **AEM Forms 6.3**, la URL de llamada de retorno será https://gbedekar-w7-1:6443/etc/cloudservices/fdm/createlead.html. En esta URL, createlead es el nombre del modelo de datos de formulario.

   * Si utiliza** AEM Forms 6.4**, la URL de llamada de retorno será [https://gbedekar-w7-:6443/libs/fd/fdm/gui/components/admin/fdmcloudservice/createcloudconfigwizard/cloudservices.html](https://gbedekar-w7-1:6443/libs/fd/fdm/gui/components/admin/fdmcloudservice/createcloudconfigwizard/cloudservices.html)

En este ejemplo gbedekar -w7-1:6443 es el nombre de mi servidor y el puerto en el que se está ejecutando AEM.

Una vez que haya creado la aplicación conectada, anote la **Clave del cliente y la clave** secreta. Necesitará estos datos al crear la fuente de datos en AEM Forms.

Ahora que ha creado la aplicación conectada, deberá crear un archivo de swagger para las operaciones que debe realizar en Salesforce. Se incluye un archivo de muestra como parte de los recursos descargables. Este archivo de swagger permite crear el objeto &quot;Posible cliente&quot; en el envío de formulario adaptable. Explore este archivo de swagger.

El siguiente paso es crear la fuente de datos en AEM Forms. Siga los siguientes pasos según la versión de AEM Forms

## AEM Forms 6.3 {#aem-forms}

* Inicie sesión en AEM Forms mediante el protocolo https
* Vaya a los servicios en la nube escribiendo https://&lt;nombre_servidor>:&lt;puerto_servidor> /etc/cloudservices.html, por ejemplo: https://gbedekar-w7-1:6443/etc/cloudservices.html
* Desplácese hacia abajo hasta &quot;Modelo de datos de formulario&quot;.
* Haga clic en &quot;Mostrar configuraciones&quot;.
* Haga clic en &quot;+&quot; para agregar nueva configuración
* Seleccione &quot;Rest Full Service&quot;. Proporcione un Título y un Nombre significativos a la configuración. Por ejemplo,

   * Nombre: CreateLeadInSalesForce
   * Título: CreateLeadInSalesForce

* Haga clic en &quot;Crear&quot;

**En la siguiente pantalla **

* Seleccione &quot;Archivo&quot; como opción para el archivo de origen de swagger. Busque el archivo que descargó anteriormente
* Seleccione el tipo de autenticación como OAuth2.0
* Proporcione los valores ClientID y Client Secret
* La dirección URL de OAuth es: **https://login.salesforce.com/services/oauth2/authorize**
* Actualizar Url De Token: **https://na5.salesforce.com/services/oauth2/token**
* **Dirección Url De La Toma De Acceso: https://na5.salesforce.com/services/oauth2/token**
* Ámbito de autorización: ** api chatter_api id completo id open refresh_token visualforce web**
* Controlador de autenticación: Operador de autorización
* Haga clic en &quot;Conectar a OAUTH&quot;.Si todo va bien, no debería ver ningún error

Una vez creado el modelo de datos de formulario con Salesforce, puede crear la integración de datos de formulario con el origen de datos que acaba de crear. La documentación oficial para crear la integración de datos de formulario está [aquí](https://helpx.adobe.com/aem-forms/6-3/data-integration.html).

Asegúrese de configurar el Modelo de datos de formulario para incluir el servicio POST y crear un objeto de posible cliente en SFDC.

También tendrá que configurar el servicio de lectura y escritura para el objeto Posible cliente. Consulte las capturas de pantalla de la parte inferior de esta página.

Después de crear el modelo de datos de formulario, puede crear un Forms adaptable basado en este modelo y utilizar los métodos de envío del modelo de datos de formulario para crear posible cliente en SFDC.

## AEM Forms 6.4 {#aem-forms-1}

* Crear fuente de datos

   * [Navegar a las fuentes de datos](http://localhost:4502/libs/fd/fdm/gui/components/admin/fdmcloudservice/fdm.html/conf/global)

   * Haga clic en el botón &quot;Crear&quot;
   * Proporcionar algunos valores significativos

      * Nombre: CreateLeadInSalesForce
      * Título: CreateLeadInSalesForce
      * Tipo de servicio: Servicio RESTful
   * Haga clic en Siguiente
   * Origen de Swagger: Archivo
   * Busque y seleccione el archivo de swagger que descargó en el paso anterior
   * Tipo de autenticación: OAuth 2.0. Especifique los siguientes valores
   * Proporcione los valores ClientID y Client Secret
   * La dirección URL de OAuth es: **https://login.salesforce.com/services/oauth2/authorize**
   * Actualizar Url De Token: **https://na5.salesforce.com/services/oauth2/token**
   * token de acceso **Url: https://na5.salesforce.com/services/oauth2/token**
   * Ámbito de autorización: ** api chatter_api id completo id open refresh_token visualforce web**
   * Controlador de autenticación: Operador de autorización
   * Haga clic en el botón &quot;Conectar a OAuth&quot;. Si observa algún error, revise los pasos anteriores para asegurarse de que toda la información se ha introducido con precisión.


Una vez que haya creado la fuente de datos mediante SalesForce, podrá crear la integración de datos de formulario utilizando la fuente de datos que acaba de crear. El vínculo de documentación para eso está [aquí](https://helpx.adobe.com/experience-manager/6-4/forms/using/create-form-data-models.html)

Asegúrese de configurar el Modelo de datos de formulario para incluir el servicio POST y crear un objeto de posible cliente en SFDC.

También tendrá que configurar el servicio de lectura y escritura para el objeto Posible cliente. Consulte las capturas de pantalla de la parte inferior de esta página.

Después de crear el modelo de datos de formulario, puede crear un Forms adaptable basado en este modelo y utilizar los métodos de envío del modelo de datos de formulario para crear posible cliente en SFDC.

>[!NOTE]
>
>Asegúrese de que la dirección URL del archivo swagger corresponde a su región. Por ejemplo, la dirección URL del archivo de swagger de ejemplo es &quot;na46.salesforce.com&quot;, ya que la cuenta se creó en Norteamérica. La manera más fácil es iniciar sesión en su cuenta de Salesforce y comprobar la dirección URL .

![sfdc1](assets/sfdc1.gif)

![sfdc2](assets/sfdc2.png)

[SampleSwaggerFile](assets/swagger-sales-force-lead.json)
