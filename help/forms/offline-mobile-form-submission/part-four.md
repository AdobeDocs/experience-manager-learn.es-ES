---
title: Activador del flujo de trabajo de AEM en el envío de formularios HTML5
seo-title: Activar el flujo de trabajo de AEM en el envío de formularios HTML5
description: Siga rellenando el formulario móvil en modo sin conexión y envíe el formulario móvil para activar el flujo de trabajo de AEM
seo-description: Siga rellenando el formulario móvil en modo sin conexión y envíe el formulario móvil para activar el flujo de trabajo de AEM
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Desarrollo
role: Desarrollador
level: Con experiencia
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '476'
ht-degree: 0%

---


# Cómo hacer que este caso de uso funcione en su sistema

>[!NOTE]
>
>Para que los recursos de ejemplo funcionen en el sistema, se da por hecho que tiene la instancia de AEM Author y Publish ejecutándose en los puertos 4502 y 4503 respectivamente. También se da por hecho que se puede acceder al Autor de AEM a través de `admin`/`admin`. Si se han cambiado los números de puerto o la contraseña de administrador, estos recursos de ejemplo no funcionarán. Debe crear sus propios recursos utilizando el código de muestra proporcionado.

Para que este caso de uso funcione en el sistema local, siga estos pasos:

* Instale la instancia de AEM Author en el puerto 4502 y la instancia de AEM Publish en el puerto 4503
* [Siga las instrucciones especificadas en desarrollo con el usuario de servicios en AEM Forms](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html). Asegúrese de crear el usuario del servicio e implementar el paquete en la instancia de AEM Author y Publish.
* [Abra la configuración de osgi  ](http://localhost:4503/system/console/configMgr).
* Busque **Apache Sling Referrer Filter**. Asegúrese de que la casilla de verificación Permitir vacío esté seleccionada.
* [Implemente el paquete personalizado AEMFormDocumentService](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar). Este paquete debe implementarse en la instancia de AEM Publish. Este paquete tiene el código para generar un PDF interactivo a partir de un formulario móvil.
* [Descargue y descomprima los recursos relacionados con este artículo.](assets/offline-pdf-submission-assets.zip) Obtendrá lo siguiente
   * **offline-submit-profile.zip** : este paquete de AEM contiene el perfil personalizado que le permite descargar el pdf interactivo en su sistema de archivos local. Implemente este paquete en la instancia de AEM Publish.
   * **xdp-form-and-workflow.zip** : este paquete de AEM contiene XDP, un flujo de trabajo de muestra, un lanzador configurado en el contenido del nodo/envíos pdf. Implemente este paquete en su instancia de AEM Author y Publish.
   * **HandlePDFSubmission.HandlePDFSubmission.core-1.0-SNAPSHOT.jar** : este es el paquete de AEM que realiza la mayor parte del trabajo. Este paquete contiene el servlet montado en `/bin/startworkflow`. Este servlet guarda los datos del formulario enviados en el nodo `/content/pdfsubmissions` del repositorio de AEM. Implemente este paquete en su instancia de AEM Author y Publish.
* [Vista previa del formulario móvil](http://localhost:4503/content/dam/formsanddocuments/testsubmision.xdp/jcr:content)
* Rellene varios campos y, a continuación, haga clic en el botón de la barra de herramientas para descargar el PDF interactivo.
* Complete el PDF descargado con Acrobat y pulse el botón Enviar.
* Debería recibir un mensaje de éxito
* Inicie sesión en la instancia de AEM Author como administrador
* [Compruebe la bandeja de entrada de AEM](http://localhost:4502/aem/inbox)
* Debe tener un elemento de trabajo para revisar el PDF enviado

>[!NOTE]
>
>En lugar de enviar el PDF al servlet que se ejecuta en la instancia de publicación, algunos clientes han implementado el servlet en el contenedor de servlet como Tomcat. Todo depende de la topología con la que se sienta cómodo el cliente. A los efectos de este tutorial, vamos a utilizar el servlet implementado en la instancia de publicación para gestionar los envíos pdf.

