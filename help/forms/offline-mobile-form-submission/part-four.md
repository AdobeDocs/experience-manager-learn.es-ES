---
title: 'Déclencheur AEM del flujo de trabajo de la en el envío de formularios HTM5: Poniendo a trabajar el caso de uso'
seo-title: Trigger AEM Workflow on HTML5 Form Submission
description: Continúe rellenando el formulario móvil en el modo sin conexión y envíe el formulario móvil al flujo de trabajo de déclencheur AEM de la
seo-description: Continue filling mobile form in offline mode and submit mobile form to trigger AEM workflow
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 79935ef0-bc73-4625-97dd-767d47a8b8bb
source-git-commit: 012850e3fa80021317f59384c57adf56d67f0280
workflow-type: tm+mt
source-wordcount: '452'
ht-degree: 1%

---

# Cómo hacer que este caso de uso funcione en su sistema

>[!NOTE]
>
>Para que los recursos de ejemplo funcionen en el sistema, se da por hecho que tiene las instancias AEM Author y Publish ejecutándose en el puerto 4502 y 4503 respectivamente. También se da por hecho que el autor de AEM es accesible a través de `admin`/`admin`. Si se han cambiado los números de puerto o la contraseña de administrador, estos recursos de ejemplo no funcionarán. Debe crear sus propios recursos utilizando el código de ejemplo proporcionado.

Para que este caso de uso funcione en el sistema local, siga estos pasos:

* Instale la instancia de autor de AEM en el puerto 4502 y la instancia de publicación de AEM en el puerto 4503
* [Siga las instrucciones especificadas en Desarrollo con usuario de servicio en AEM Forms](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html). Asegúrese de crear el usuario del servicio e implementar el paquete en la instancia de autor y publicación de AEM.
* [Abra la configuración de osgi ](http://localhost:4503/system/console/configMgr).
* Buscar por  **Filtro de referente de Apache Sling**. Asegúrese de que la casilla de verificación Permitir vacío está seleccionada.
* [Implementar el paquete personalizado de AEM FormsDocumentService](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar). Este paquete debe implementarse en la instancia de publicación de AEM. Este paquete tiene el código para generar un PDF interactivo desde un formulario móvil.
* [Descargue y descomprima los recursos relacionados con este artículo.](assets/offline-pdf-submission-assets.zip) Obtendrá lo siguiente
   * **offline-submission-profile.zip** AEM - Este paquete de contiene el perfil personalizado que le permite descargar el pdf interactivo en su sistema de archivos local. Implemente este paquete en la instancia de publicación de AEM.
   * **xdp-form-and-workflow.zip** AEM : Este paquete de contiene XDP, un flujo de trabajo de ejemplo y un lanzador configurado en el contenido del nodo/envíos de archivos PDF. Implemente este paquete en las instancias de autor de AEM y de publicación.
   * **HandlePDFSubmission.HandlePDFSubmission.core-1.0-SNAPSHOT.jar** AEM - Este es el paquete de que hace la mayor parte del trabajo. Este paquete contiene el servlet montado en `/bin/startworkflow`. Este servlet guarda los datos de formulario enviados en `/content/pdfsubmissions` AEM nodo en el repositorio de. Implemente este paquete en las instancias de autor de AEM y publicación.
* [Previsualización del formulario móvil](http://localhost:4503/content/dam/formsanddocuments/testsubmision.xdp/jcr:content)
* Rellene varios campos y, a continuación, haga clic en el botón de la barra de herramientas para descargar el PDF interactivo.
* Rellene el PDF descargado con Acrobat y pulse el botón Enviar.
* Debería recibir un mensaje de éxito
* Inicie sesión en la instancia de autor de AEM como administrador
* [AEM Compruebe la bandeja de entrada de](http://localhost:4502/aem/inbox)
* Debe contar con un elemento de trabajo para revisar el PDF enviado

>[!NOTE]
>
>En lugar de enviar el PDF al servlet que se ejecuta en la instancia de publicación, algunos clientes han implementado el servlet en el contenedor de servlets como Tomcat. Todo depende de la topología con la que se sienta cómodo el cliente. a efectos de este tutorial, vamos a utilizar el servlet implementado en la instancia de publicación para gestionar los envíos de PDF.
