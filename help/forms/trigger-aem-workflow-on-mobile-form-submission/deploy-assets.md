---
title: 'Déclencheur del flujo de trabajo de AEM en el envío de formularios HTML5: Poniendo a trabajar el caso de uso'
description: Implementar los recursos de ejemplo en el sistema local
feature: Mobile Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-16215
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: 9417235f-2e8d-45c7-86eb-104478a69a19
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '392'
ht-degree: 0%

---

# Cómo hacer que este caso de uso funcione en su sistema

>[!NOTE]
>
>Para que los recursos de ejemplo funcionen en el sistema, se supone que tiene acceso a una instancia de autor y publicación de AEM Forms y AEM Forms.

Para que este caso de uso funcione en el sistema local, siga estos pasos:

## Implemente lo siguiente en la instancia de autor de AEM Forms

* [Instalación del paquete MobileFormToWorkflow](assets/MobileFormToWorkflow.core-1.0.0-SNAPSHOT.jar)

* [Implementar el paquete de desarrollo con usuario de servicio](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip?lang=es)
Añada la siguiente entrada en el servicio del asignador de usuarios del servicio Apache Sling mediante configMgr

```
DevelopingWithServiceUser.core:getformsresourceresolver=fd-service
```

* Puede almacenar los envíos de formularios en una carpeta diferente especificando el nombre de la carpeta en la configuración de las credenciales de AEM Server mediante [configMgr](http://localhost:4502/system/console/configMg). Si cambia la carpeta, asegúrese de crear un iniciador en la carpeta para almacenar en déclencheur el flujo de trabajo **ReviewSubmittedPDF**

![autor de configuración](assets/author-config.png)
* [Importe el xdp de muestra y el paquete de flujo de trabajo mediante el administrador de paquetes](assets/xdp-form-and-workflow.zip).


## Implemente los siguientes recursos en la instancia de publicación

* [Instalación del paquete MobileFormToWorkflow](assets/MobileFormToWorkflow.core-1.0.0-SNAPSHOT.jar)

* Especifique el nombre de usuario/contraseña para la instancia de autor y una **ubicación existente en su repositorio de AEM** para almacenar los datos enviados en las credenciales de AEM Server mediante [configMgr](http://localhost:4503/system/console/configMgr). Puede dejar la dirección URL del extremo en el servidor de flujo de trabajo de AEM tal cual. Este es el extremo que extrae y almacena los datos del envío en el nodo especificado.
  ![publish-config](assets/publish-config.png)

* [Implementar el paquete de desarrollo con usuario de servicio](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip?lang=es)
* [Abra la configuración de osgi](http://localhost:4503/system/console/configMgr).
* Busque **Filtro de referente de Apache Sling**. Asegúrese de que la casilla de verificación Permitir vacío está seleccionada.


## Prueba de la solución

* Inicie sesión en la instancia de autor
* [Editar las propiedades avanzadas del archivo w9.xdp](http://localhost:4502/libs/fd/fm/gui/content/forms/formmetadataeditor.html/content/dam/formsanddocuments/w9.xdp). Asegúrese de que la dirección URL de envío y el perfil de procesamiento estén correctamente configurados, como se muestra a continuación.
  ![xdp-advanced-properties](assets/mobile-form-properties.png)

* Publicar w9.xdp
* Iniciar sesión en la instancia de publicación
* [Vista previa del formulario w9](http://localhost:4503/content/dam/formsanddocuments/w9.xdp/jcr:content)
* Rellene algunos campos de formulario y envíe el formulario
* Inicie sesión en la instancia de autor de AEM como administrador
* [Comprobar la bandeja de entrada AEM](http://localhost:4502/aem/inbox)
* Debe contar con un elemento de trabajo para revisar el PDF enviado

>[!NOTE]
>
>En lugar de enviar PDF al servlet que se ejecuta en la instancia de publicación, algunos clientes han implementado el servlet en el contenedor de servlet, como Tomcat. Todo depende de la topología con la que se sienta cómodo el cliente. Para el propósito de este tutorial, vamos a utilizar el servlet implementado en la instancia de publicación para administrar los envíos de formularios.
