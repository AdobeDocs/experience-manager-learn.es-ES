---
title: Implementar los recursos de ejemplo en el servidor
description: Ponga el caso de uso en funcionamiento en el servidor local
feature: Adaptive Forms,Acrobat Sign
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-13099
last-substantial-update: 2023-04-13T00:00:00Z
exl-id: f12f83fa-673a-454c-aa52-6ea769a182b7
duration: 36
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '157'
ht-degree: 0%

---

# Implementación de los recursos

Los siguientes activos/configuraciones se implementaron en un servidor de publicación de AEM Forms.

* [Paquete de envoltorio de Adobe Sign](assets/AcrobatSign.core-1.0.0-SNAPSHOT.jar)

* [Plantilla de comunicación interactiva de ejemplo](assets/waiver-interactive-communication.zip)
* [Implementar el paquete DevelopersWithServiceUser](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip?lang=es)
* Agregue la siguiente entrada en el servicio del asignador de usuarios del servicio Apache Sling mediante OSGi configMgr
  **Desarrollo con ServiceUser.core:getformsresourceresolver=fd-service**

## Implementación de la aplicación react de ejemplo

* [Descargue la aplicación react de ejemplo](assets/mult-step-form1.zip)
* Descomprima el contenido de la aplicación react en una carpeta nueva
* Vaya a la carpeta y ejecute los siguientes comandos

```java
npm install
npm start
```

Abra el archivo EmergencyContact.js y cambie la dirección URL en el método fetch para que coincida con su entorno.


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

Para habilitar la realización de llamadas de POST al extremo de AEM desde la aplicación REACT, deberá especificar las entradas adecuadas en el campo Orígenes permitidos en la configuración de la política de uso compartido de recursos de origen cruzado de Adobe Granite.

![configuración de cors](assets/cors-settings.png)

Consulte [Comprensión de CORS con AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/understand-cross-origin-resource-sharing.html?lang=es) para obtener más información sobre las opciones de configuración de CORS.
