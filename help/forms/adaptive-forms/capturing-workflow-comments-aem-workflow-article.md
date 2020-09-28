---
title: Captura de comentarios de flujo de trabajo en el Forms Workflow adaptable
seo-title: Captura de comentarios de flujo de trabajo en el Forms Workflow adaptable
description: Captura de comentarios de flujo de trabajo en AEM flujo de trabajo
seo-description: Captura de comentarios de flujo de trabajo en AEM flujo de trabajo
uuid: df41fc6f-9abf-47b4-a014-b3b9fb58b6f7
feature: workflow
topics: development
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4
discoiquuid: d5e40650-3b1f-4875-91b2-e22d932b5e7c
translation-type: tm+mt
source-git-commit: f07680e73316efb859a675f4b2212d8c3e03f6a0
workflow-type: tm+mt
source-wordcount: '393'
ht-degree: 0%

---


# Captura de comentarios de flujo de trabajo en el Forms Workflow adaptable{#capturing-workflow-comments-in-adaptive-forms-workflow}

>[Solo se aplica a AEM Forms 6.4. En AEM Forms 6.5, utilice la función de variables para lograr este caso de uso]

Una solicitud común es la capacidad de incluir los comentarios introducidos por el revisor de tarea en un mensaje de correo electrónico. En AEM Forms 6.4 no hay ningún mecanismo listo para capturar los comentarios introducidos por el usuario e incluirlos en el correo electrónico.

Para cumplir este requisito, se proporciona un paquete OSGi de muestra que puede utilizarse para capturar comentarios y almacenarlos como propiedad de metadatos del flujo de trabajo.

La siguiente captura de pantalla muestra cómo utilizar el paso del proceso en [AEM flujo de trabajo](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html) para capturar comentarios y almacenarlos como propiedad de metadatos. &quot;Capturar comentarios del flujo de trabajo&quot; es el nombre de la clase java que debe utilizarse en el paso del proceso. Debe pasar el nombre de la propiedad metadata que contendrá los comentarios. En la siguiente captura de pantalla, managerComments es la propiedad de metadatos que almacenará los comentarios.

![workflowcomments1](assets/workflowcomments1.gif)

Para probar esta capacidad en su sistema, siga los pasos siguientes:
* [Asegúrese de que el paso del proceso en el flujo de trabajo está configurado para utilizar los comentarios del flujo de trabajo de captura](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html)

* [Implementar el paquete DevelopmentWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Implemente el paquete](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)SetValue. Este paquete contiene el código de muestra para capturar los comentarios y almacenarlos como propiedad de metadatos

* [Descargue y descomprima los recursos relacionados con este artículo en su sistema](assets/capturecomments.zip) de archivos. Los recursos contienen un modelo de flujo de trabajo y un formulario adaptable de ejemplo.

* Importar los 2 archivos zip en AEM mediante el administrador de paquetes

* [Previsualización del formulario navegando a esta dirección URL](http://localhost:4502/content/dam/formsanddocuments/capturecomments/jcr:content?wcmmode=disabled)

* Rellene los campos del formulario y envíe el formulario

* [Marque la bandeja de entrada AEM](http://localhost:4502/aem/inbox)

* Abra la tarea desde la bandeja de entrada y envíe el formulario. Escriba algunos comentarios cuando se le solicite.

Los comentarios se almacenarán en la propiedad de metadatos denominada managerComments en crx. Para buscar los comentarios, inicie sesión en crx como administrador. Las instancias de flujo de trabajo se almacenan en la siguiente ruta

/var/workflow/instance/server0

Seleccione la instancia de flujo de trabajo adecuada y compruebe la propiedad managerComments en el nodo de metadatos.

