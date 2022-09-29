---
title: Captura de los comentarios del flujo de trabajo en el Forms Workflow adaptable
description: Captura de los comentarios del flujo de trabajo en AEM flujo de trabajo
feature: Workflow
version: 6.4
topic: Development
role: Developer
level: Experienced
exl-id: 5c250bbb-bac6-427d-8aca-1fbb1229e02c
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '375'
ht-degree: 0%

---

# Captura de los comentarios del flujo de trabajo en el Forms Workflow adaptable{#capturing-workflow-comments-in-adaptive-forms-workflow}

>[Se aplica solo a AEM Forms 6.4. En AEM Forms 6.5, utilice la función de variables para lograr este caso de uso]

Una solicitud común es la capacidad de incluir los comentarios introducidos por el revisor de tareas en un correo electrónico. En AEM Forms 6.4 no hay ningún mecanismo listo para usar para capturar los comentarios introducidos por el usuario e incluir estos comentarios en el correo electrónico.

Para cumplir este requisito, se proporciona un paquete OSGi de muestra que puede utilizarse para capturar comentarios y almacenar estos comentarios como propiedad de metadatos del flujo de trabajo.

La siguiente captura de pantalla muestra cómo usar el paso de proceso en [Flujo de trabajo AEM](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html) para capturar comentarios y almacenarlos como propiedad de metadatos. El &quot;Capturar comentarios del flujo de trabajo&quot; es el nombre de la clase java que debe utilizarse en el paso del proceso. Debe pasar el nombre de propiedad de metadatos que contendrá los comentarios. En la captura de pantalla siguiente, managerComments es la propiedad de metadatos que almacenará los comentarios.

![workflow comments1](assets/workflowcomments1.gif)

Para probar esta capacidad en su sistema, siga los siguientes pasos:
* [Asegúrese de que el paso del proceso en el flujo de trabajo esté configurado para utilizar los comentarios del flujo de trabajo de captura](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html)

* [Implementar el paquete de usuario Desarrollo con servicio](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Implementar el paquete SetValue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Este paquete contiene el código de muestra para capturar los comentarios y almacenarlos como una propiedad de metadatos

* [Descargue y descomprima los recursos relacionados con este artículo en su sistema de archivos](assets/capturecomments.zip) Los recursos contienen un modelo de flujo de trabajo y un ejemplo de formulario adaptable.

* Importar los 2 archivos zip en AEM mediante el administrador de paquetes

* [Obtener una vista previa del formulario navegando hasta esta dirección URL](http://localhost:4502/content/dam/formsanddocuments/capturecomments/jcr:content?wcmmode=disabled)

* Rellene los campos del formulario y envíe el formulario

* [Marque la bandeja de entrada AEM](http://localhost:4502/aem/inbox)

* Abra la tarea desde la bandeja de entrada y envíe el formulario. Introduzca algunos comentarios cuando se le solicite.

Los comentarios se almacenan en la propiedad de metadatos denominada `managerComments` en AEM repositorio. Para comprobar los comentarios, inicie sesión en crx como administrador. Las instancias de flujo de trabajo se almacenan en la siguiente ruta:

`/var/workflow/instances/server0`

Seleccione la instancia de flujo de trabajo adecuada y compruebe la propiedad managerComments en el nodo de metadatos.
