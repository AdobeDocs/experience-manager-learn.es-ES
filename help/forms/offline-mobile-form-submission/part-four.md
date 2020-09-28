---
title: Activar AEM flujo de trabajo en el envío de formulario HTML5
seo-title: Activar flujo de trabajo AEM en envío de formulario HTML5
description: Seguir rellenando formularios móviles en modo sin conexión y enviar formularios móviles para activar AEM flujo de trabajo
seo-description: Seguir rellenando formularios móviles en modo sin conexión y enviar formularios móviles para activar AEM flujo de trabajo
feature: mobile-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '471'
ht-degree: 0%

---


# Cómo hacer que este caso de uso funcione en el sistema

>[!NOTE]
>
>Para que los recursos de ejemplo funcionen en el sistema, se da por hecho que la instancia de AEM Author y Publish se ejecuta en los puertos 4502 y 4503 respectivamente. También se da por hecho que AEM Author es accesible `admin`/`admin`. Si se han cambiado los números de puerto o la contraseña de administrador, estos recursos de ejemplo no funcionarán. Deberá crear sus propios recursos utilizando el código de muestra proporcionado.

Para que este caso de uso funcione en el sistema local, siga estos pasos:

* Instale la instancia de AEM Author en el puerto 4502 y la instancia de AEM Publish en el puerto 4503
* [Siga las instrucciones especificadas para el desarrollo con el usuario de servicios en AEM Forms](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html). Asegúrese de crear el usuario del servicio e implementar el paquete en la instancia de AEM Author y Publish.
* [Abra la configuración osgi ](http://localhost:4503/system/console/configMgr).
* Busque **Apache Sling Remitente del reenvío Filter**. Asegúrese de que la casilla de verificación Permitir vacío está seleccionada.
* [Implemente el paquete](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)personalizado AEMFormDocumentService.Este paquete debe implementarse en la instancia de AEM Publish. Este paquete tiene el código para generar un PDF interactivo a partir de un formulario móvil.
* [Descargue y descomprima los recursos relacionados con este artículo.](assets/offline-pdf-submission-assets.zip) Obtendrá lo siguiente
   * **offline-submit-perfil.zip** : Este paquete AEM contiene el perfil personalizado que le permite descargar el PDF interactivo en su sistema de archivos local. Implemente este paquete en su instancia de AEM Publish.
   * **xdp-form-and-workflow.zip** : este paquete de AEM contiene XDP, flujo de trabajo de muestra, iniciador configurado en contenido de nodo/pdfenvíos. Implemente este paquete en la instancia de AEM Author y Publish.
   * **HandlePDFSubmission.HandlePDFSubmission.core-1.0-SNAPSHOT.jar** : es el paquete de AEM que realiza la mayor parte del trabajo. Este paquete contiene el servlet montado en `/bin/startworkflow`. Este servlet guarda los datos del formulario enviados en `/content/pdfsubmissions` nodo en AEM repositorio. Implemente este paquete en la instancia de AEM Author y Publish.
* [Previsualización del formulario móvil](http://localhost:4503/content/dam/formsanddocuments/testsubmision.xdp/jcr:content)
* Rellene varios campos y, a continuación, haga clic en el botón de la barra de herramientas para descargar el PDF interactivo.
* Llene el PDF descargado con Acrobat y pulse el botón de envío.
* Debe recibir un mensaje de éxito
* Iniciar sesión en la instancia de AEM Author como administrador
* [Marque la Bandeja de entrada AEM](http://localhost:4502/aem/inbox)
* Debe tener un elemento de trabajo para revisar el PDF enviado

>[!NOTE]
>
>En lugar de enviar el PDF al servlet que se ejecuta en la instancia de publicación, algunos clientes han implementado el servlet en el contenedor servlet, como Tomcat. Todo depende de la topología con la que el cliente se sienta cómodo.Para este tutorial vamos a utilizar el servlet implementado en la instancia de publicación para gestionar los envíos de pdf.

