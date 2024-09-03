---
title: 'Déclencheur AEM de flujo de trabajo de la en el envío de formularios de HTML5: obtención de casos de uso para funcionar'
description: Implementar los recursos de ejemplo en el sistema local
feature: Mobile Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
jira: kt-16133
exl-id: 79935ef0-bc73-4625-97dd-767d47a8b8bb
badgeVersions: label="AEM Forms 6.5" before-title="false"
duration: 90
source-git-commit: 9545fae5a5f5edd6f525729e648b2ca34ddbfd9f
workflow-type: tm+mt
source-wordcount: '457'
ht-degree: 0%

---

# Cómo hacer que este caso de uso funcione en su sistema

>[!NOTE]
>
>Para que los recursos de ejemplo funcionen en el sistema, se supone que tiene acceso a una instancia de autor y publicación de AEM Forms y AEM Forms.

Para que este caso de uso funcione en el sistema local, siga estos pasos:

## Implemente lo siguiente en la instancia de autor de AEM Forms

* [Instalación del paquete MobileFormToWorkflow](assets/MobileFormToWorkflow.core-1.0.0-SNAPSHOT.jar)

* [Importe el perfil personalizado](assets/customprofile.zip) que combina los datos del formulario de HTML5 con el XDP y devuelve un pdf interactivo.

* [Implementar el paquete de desarrollo con usuario de servicio](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip?lang=en)
Añada la siguiente entrada en el servicio del asignador de usuarios del servicio Apache Sling mediante configMgr

```
DevelopingWithServiceUser.core:getformsresourceresolver=fd-service
```

* AEM Puede almacenar los envíos de formularios en una carpeta diferente especificando el nombre de la carpeta en la configuración de credenciales del servidor de la mediante [configMgr](http://localhost:4502/system/console/configMg). Si cambia la carpeta, asegúrese de crear un iniciador en la carpeta para almacenar en déclencheur el flujo de trabajo **ReviewSubmittedPDF**

![autor de configuración](assets/author-config.png)
* [Importe el xdp de muestra y el paquete de flujo de trabajo mediante el administrador de paquetes](assets/xdp-form-and-workflow.zip).


## Implemente los siguientes recursos en la instancia de publicación

* [Instalación del paquete MobileFormToWorkflow](assets/MobileFormToWorkflow.core-1.0.0-SNAPSHOT.jar)

* AEM AEM Especifique el nombre de usuario y la contraseña para la instancia de autor y una ubicación **existente en su repositorio de** para almacenar los datos enviados en las credenciales del servidor de la con [configMgr](http://localhost:4503/system/console/configMgr). AEM Puede dejar la dirección URL del punto de conexión en el servidor de flujo de trabajo de tal cual. Este es el extremo que extrae y almacena los datos del envío en el nodo especificado.
  ![publish-config](assets/publish-config.png)

* [Implementar el paquete de desarrollo con usuario de servicio](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip?lang=en)
* [Abra la configuración de osgi](http://localhost:4503/system/console/configMgr).
* Busque **Filtro de referente de Apache Sling**. Asegúrese de que la casilla de verificación Permitir vacío está seleccionada.
* [Importe el perfil personalizado](assets/customprofile.zip) que combina los datos del formulario de HTML5 con el XDP y devuelve un pdf interactivo.


## Prueba de la solución

* Inicie sesión en la instancia de autor
* [Editar las propiedades avanzadas del archivo w9.xdp](http://localhost:4502/libs/fd/fm/gui/content/forms/formmetadataeditor.html/content/dam/formsanddocuments/w9.xdp). Asegúrese de que la dirección URL de envío y el perfil de procesamiento estén correctamente configurados, como se muestra a continuación.
  ![xdp-advanced-properties](assets/mobile-form-properties.png)

* Publish: w9.xdp
* Iniciar sesión en la instancia de publicación
* [Vista previa del formulario w9](http://localhost:4503/content/dam/formsanddocuments/w9.xdp/jcr:content)
* Rellene varios campos y, a continuación, haga clic en el botón de la barra de herramientas para descargar el PDF interactivo.
* Rellene el PDF descargado con Acrobat y pulse el botón Enviar.
* Debería recibir un mensaje de éxito
* AEM Inicie sesión en la instancia de autor de como administrador
* AEM [Comprobar la bandeja de entrada de la](http://localhost:4502/aem/inbox)
* Debe contar con un elemento de trabajo para revisar el PDF enviado

>[!NOTE]
>
>En lugar de enviar el PDF al servlet que se ejecuta en la instancia de publicación, algunos clientes han implementado el servlet en el contenedor de servlets como Tomcat. Todo depende de la topología con la que se sienta cómodo el cliente. a efectos de este tutorial, vamos a utilizar el servlet implementado en la instancia de publicación para gestionar los envíos de PDF.
