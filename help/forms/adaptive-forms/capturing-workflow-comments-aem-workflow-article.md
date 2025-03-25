---
title: Captura de comentarios de flujo de trabajo en Forms Workflow adaptable
description: Captura de comentarios de flujo de trabajo en AEM Workflow
feature: Workflow
version: Experience Manager 6.4
topic: Development
role: Developer
level: Experienced
exl-id: 5c250bbb-bac6-427d-8aca-1fbb1229e02c
last-substantial-update: 2020-10-10T00:00:00Z
duration: 73
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '350'
ht-degree: 0%

---

# Captura de comentarios de flujo de trabajo en Forms Workflow adaptable{#capturing-workflow-comments-in-adaptive-forms-workflow}

>[Solo se aplica a AEM Forms 6.4. En AEM Forms 6.5, utilice la función de variables para lograr este caso de uso]

Una solicitud habitual es la de poder incluir los comentarios introducidos por el revisor de la tarea en un mensaje de correo electrónico. En AEM Forms 6.4 no hay ningún mecanismo predeterminado para capturar los comentarios introducidos por el usuario e incluirlos en el correo electrónico.

Para cumplir este requisito, se proporciona un paquete OSGi de muestra que se puede utilizar para capturar comentarios y almacenarlos como propiedad de metadatos de flujo de trabajo.

La siguiente captura de pantalla muestra cómo usar el paso de proceso en [Flujo de trabajo de AEM](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html) para capturar comentarios y almacenarlos como propiedad de metadatos. El &quot;Capture Workflow Comments&quot; es el nombre de la clase java que debe utilizarse en el paso del proceso. Debe pasar el nombre de la propiedad de metadatos que contendrá los comentarios. En la captura de pantalla siguiente, managerComments es la propiedad de metadatos que almacenará los comentarios.

![workflowcomments1](assets/workflowcomments1.gif)

Para probar esta capacidad en su sistema, siga los siguientes pasos:
* [Asegúrese de que el paso del proceso en el flujo de trabajo está configurado para utilizar Capturar comentarios del flujo de trabajo](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html)

* [Implementar el paquete Develingwithserviceuser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Implementar el paquete SetValue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Este paquete contiene el código de ejemplo para capturar los comentarios y almacenarlos como una propiedad de metadatos

* [Descargue y descomprima los recursos relacionados con este artículo en su sistema de archivos](assets/capturecomments.zip). Los recursos contienen un modelo de flujo de trabajo y un formulario adaptable de ejemplo.

* Importación de los 2 archivos zip en AEM mediante el administrador de paquetes

* [Vista previa del formulario navegando a esta dirección URL](http://localhost:4502/content/dam/formsanddocuments/capturecomments/jcr:content?wcmmode=disabled)

* Rellene los campos del formulario y envíe el formulario

* [Compruebe su bandeja de entrada de AEM](http://localhost:4502/aem/inbox)

* Abra la tarea desde la bandeja de entrada y envíe el formulario. Introduzca algunos comentarios cuando se le solicite.

Los comentarios se almacenan en la propiedad de metadatos `managerComments` del repositorio de AEM. Para comprobar los comentarios, inicie sesión en crx como administrador. Las instancias de flujo de trabajo se almacenan en la siguiente ruta:

`/var/workflow/instances/server0`

Seleccione la instancia de flujo de trabajo adecuada y compruebe la propiedad managerComments en el nodo de metadatos.
