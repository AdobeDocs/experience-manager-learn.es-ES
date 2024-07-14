---
title: 'Déclencheur AEM del flujo de trabajo de la en el envío de formularios HTM5: Poniendo a trabajar el caso de uso'
description: Continúe rellenando el formulario móvil en el modo sin conexión y envíe el formulario móvil al flujo de trabajo de déclencheur AEM de la
feature: Mobile Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 79935ef0-bc73-4625-97dd-767d47a8b8bb
duration: 90
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '435'
ht-degree: 0%

---

# Cómo hacer que este caso de uso funcione en su sistema

>[!NOTE]
>
>AEM Para que los recursos de ejemplo funcionen en el sistema, se da por hecho que tiene las instancias de autor y Publish de la aplicación de ejemplo que se ejecutan en el puerto 4502 y 4503, respectivamente. AEM También se supone que el autor de la es accesible a través de `admin`/`admin`. Si se han cambiado los números de puerto o la contraseña de administrador, estos recursos de ejemplo no funcionarán. Debe crear sus propios recursos utilizando el código de ejemplo proporcionado.

Para que este caso de uso funcione en el sistema local, siga estos pasos:

* AEM AEM Instale la instancia de Autor de la en el puerto 4502 y la instancia de Publish de la en el puerto 4503
* [Siga las instrucciones especificadas en Desarrollo con usuario de servicio en AEM Forms](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html). AEM Asegúrese de crear el usuario del servicio e implementar el paquete en la instancia de autor de la y en la instancia de Publish.
* [Abra la configuración de osgi](http://localhost:4503/system/console/configMgr).
* Busque **Filtro de referente de Apache Sling**. Asegúrese de que la casilla de verificación Permitir vacío está seleccionada.
* AEM [Implemente el paquete AEMFormDocumentService personalizado](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar). Este paquete debe implementarse en la instancia de Publish de su. Este paquete tiene el código para generar un PDF interactivo desde un formulario móvil.
* [Descargue y descomprima los recursos relacionados con este artículo.](assets/offline-pdf-submission-assets.zip) Obtendrá lo siguiente
   * AEM **offline-submission-profile.zip**: este paquete de contiene el perfil personalizado que le permite descargar el PDF interactivo en su sistema de archivos local. AEM Implemente este paquete en la instancia de Publish de la.
   * AEM **xdp-form-and-workflow.zip**: este paquete contiene XDP, un flujo de trabajo de ejemplo y un lanzador configurado en el contenido del nodo/envíos PDF. AEM Implemente este paquete tanto en la instancia de autor de la como en la de Publish.
   * AEM **HandlePDFSubmission.HandlePDFSubmission.core-1.0-SNAPSHOT.jar**: este es el paquete de la que realiza la mayor parte del trabajo. Este paquete contiene el servlet montado en `/bin/startworkflow`. AEM Este servlet guarda los datos de formulario enviados en el nodo `/content/pdfsubmissions` en el repositorio de. AEM Implemente este paquete tanto en la instancia de autor de como en la de Publish.
* [Vista previa del formulario móvil](http://localhost:4503/content/dam/formsanddocuments/testsubmision.xdp/jcr:content)
* Rellene varios campos y, a continuación, haga clic en el botón de la barra de herramientas para descargar el PDF interactivo.
* Rellene el PDF descargado con Acrobat y pulse el botón Enviar.
* Debería recibir un mensaje de éxito
* AEM Inicie sesión en la instancia de autor de como administrador
* AEM [Comprobar la bandeja de entrada de la](http://localhost:4502/aem/inbox)
* Debe contar con un elemento de trabajo para revisar el PDF enviado

>[!NOTE]
>
>En lugar de enviar el PDF al servlet que se ejecuta en la instancia de publicación, algunos clientes han implementado el servlet en el contenedor de servlets como Tomcat. Todo depende de la topología con la que se sienta cómodo el cliente. a efectos de este tutorial, vamos a utilizar el servlet implementado en la instancia de publicación para gestionar los envíos de PDF.
