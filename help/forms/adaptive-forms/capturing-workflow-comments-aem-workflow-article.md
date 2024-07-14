---
title: Captura de comentarios de flujo de trabajo en Forms Workflow adaptables
description: AEM Captura de comentarios de flujo de trabajo en flujo de trabajo de
feature: Workflow
version: 6.4
topic: Development
role: Developer
level: Experienced
exl-id: 5c250bbb-bac6-427d-8aca-1fbb1229e02c
last-substantial-update: 2020-10-10T00:00:00Z
duration: 73
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '350'
ht-degree: 0%

---

# Captura de comentarios de flujo de trabajo en Forms Workflow adaptables{#capturing-workflow-comments-in-adaptive-forms-workflow}

>[Solo se aplica a AEM Forms 6.4. En AEM Forms 6.5, utilice la función de variables para lograr este caso de uso]

Una solicitud habitual es la de poder incluir los comentarios introducidos por el revisor de la tarea en un mensaje de correo electrónico. En AEM Forms 6.4 no hay ningún mecanismo predeterminado para capturar los comentarios introducidos por el usuario e incluirlos en el correo electrónico.

Para cumplir este requisito, se proporciona un paquete OSGi de muestra que se puede utilizar para capturar comentarios y almacenarlos como propiedad de metadatos de flujo de trabajo.

AEM La siguiente captura de pantalla muestra cómo usar el paso de proceso en [Flujo de trabajo de la](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html) para capturar comentarios y almacenarlos como propiedad de metadatos. El &quot;Capture Workflow Comments&quot; es el nombre de la clase java que debe utilizarse en el paso del proceso. Debe pasar el nombre de la propiedad de metadatos que contendrá los comentarios. En la captura de pantalla siguiente, managerComments es la propiedad de metadatos que almacenará los comentarios.

![workflowcomments1](assets/workflowcomments1.gif)

Para probar esta capacidad en su sistema, siga los siguientes pasos:
* [Asegúrese de que el paso del proceso en el flujo de trabajo está configurado para utilizar Capturar comentarios del flujo de trabajo](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html)

* [Implementar el paquete Develingwithserviceuser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Implementar el paquete SetValue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Este paquete contiene el código de ejemplo para capturar los comentarios y almacenarlos como una propiedad de metadatos

* [Descargue y descomprima los recursos relacionados con este artículo en su sistema de archivos](assets/capturecomments.zip). Los recursos contienen un modelo de flujo de trabajo y un formulario adaptable de ejemplo.

* AEM Importe los 2 archivos zip a mediante el administrador de paquetes de la interfaz de usuario de

* [Vista previa del formulario navegando a esta dirección URL](http://localhost:4502/content/dam/formsanddocuments/capturecomments/jcr:content?wcmmode=disabled)

* Rellene los campos del formulario y envíe el formulario

* AEM [Comprueba tu bandeja de entrada de la](http://localhost:4502/aem/inbox)

* Abra la tarea desde la bandeja de entrada y envíe el formulario. Introduzca algunos comentarios cuando se le solicite.

AEM Los comentarios se almacenan en la propiedad de metadatos denominada `managerComments` en el repositorio que está en el que se encuentra el usuario en el que se está. Para comprobar los comentarios, inicie sesión en crx como administrador. Las instancias de flujo de trabajo se almacenan en la siguiente ruta:

`/var/workflow/instances/server0`

Seleccione la instancia de flujo de trabajo adecuada y compruebe la propiedad managerComments en el nodo de metadatos.
