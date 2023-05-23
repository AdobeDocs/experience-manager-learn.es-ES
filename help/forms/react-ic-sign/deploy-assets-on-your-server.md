---
title: Implementar los recursos de ejemplo en el servidor
description: Ponga el caso de uso en funcionamiento en el servidor local
feature: Adaptive Forms,Acrobat Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
kt: 13099
last-substantial-update: 2023-04-13T00:00:00Z
exl-id: 44f4261b-d6fe-42ad-a3aa-2a36ca897b5e
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '148'
ht-degree: 0%

---

# Implementación de los recursos

Los siguientes activos/configuraciones se implementaron en un servidor de publicación de AEM Forms.

* [Paquete de envoltorio de Adobe Sign](assets/AcrobatSign.core-1.0.0-SNAPSHOT.jar)

* [Plantilla de comunicación interactiva de ejemplo](assets/waiver-interactive-communication.zip)
* [Implementar el paquete DevelopersWithServiceUser](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip)
* Agregue la siguiente entrada en el servicio del asignador de usuarios del servicio Apache Sling mediante OSGi configMgr
   **DesarrollarWithServiceUser.core:getformsresourceresolver=fd-service**
* [Puede descargar un ejemplo de código de aplicación de React desde aquí](assets/src.zip)



La aplicación react de ejemplo debe implementarse en el entorno local.

Debe cambiar la dirección URL del punto de conexión para que coincida con su entorno. Abra el archivo EmergencyContact.js y cambie la dirección URL en el método fetch

```javascript
 const getWebForm=async()=>
     {
        setSpinner(true)
        console.log("inside widgetURL function emergency contact");
        // NOTE: replace the `aemforms.azure.com:4503` with your AEM FORM server
        let res = await fetch("http://aemforms.azure.com:4503/bin/getwidgeturl",
          {
            method: "POST",
            body: JSON.stringify({"icTemplate":"/content/forms/af/waiver/waiver/channels/print","waiver":formData})
                     
         })
 
```

Para habilitar la realización de llamadas del POST AEM al punto de conexión de la aplicación REACT desde el punto de conexión de la aplicación, deberá especificar las entradas adecuadas en el campo Orígenes permitidos en la configuración de la política de uso compartido de recursos de origen cruzado de Granite de Adobe

![establecimiento de cors](assets/cors-settings.png)
