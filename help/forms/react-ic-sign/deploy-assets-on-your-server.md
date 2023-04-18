---
title: Implementar los recursos de ejemplo en el servidor
description: Obtener el caso de uso que funciona en el servidor local
feature: Adaptive Forms,Acrobat Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
kt: 13099
last-substantial-update: 2023-04-13T00:00:00Z
source-git-commit: 155e6e42d4251b731d00e2b456004016152f81fe
workflow-type: tm+mt
source-wordcount: '148'
ht-degree: 0%

---

# Implementar los recursos

Se implementaron los siguientes recursos/configuraciones en un servidor de publicación de AEM Forms.

* [Paquete de envoltura de Adobe Sign](assets/AcrobatSign.core-1.0.0-SNAPSHOT.jar)

* [Ejemplo de plantilla de comunicación interactiva](assets/waiver-interactive-communication.zip)
* [Implementar el paquete DevelopingWithServiceUser](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip)
* Añada la siguiente entrada en el servicio de Apache Sling Service User Mapper Service utilizando OSGi configMgr
   **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service**
* [El código de la aplicación React de muestra se puede descargar desde aquí](assets/src.zip)



La aplicación de reacción de ejemplo debe implementarse en el entorno local

Deberá cambiar la dirección URL del extremo para que coincida con su entorno. Abra el archivo EmergencyContact.js y cambie la dirección URL en el método de recuperación

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

Para habilitar la realización de llamadas de POST al extremo de AEM desde la aplicación REACT, deberá especificar las entradas adecuadas en el campo Orígenes permitidos de la configuración de la directiva de uso compartido de recursos de origen cruzado de Adobe Granite

![configuración de cors](assets/cors-settings.png)



