---
title: Usar el servicio de modelo de datos de formulario como paso en el flujo de trabajo de AEM 6.5
description: AEM Forms 6.5 ha introducido la capacidad de crear variables en el flujo de trabajo de AEM. Con esta nueva capacidad, el uso del servicio de modelo de datos de formulario de invocación en el flujo de trabajo de AEM se ha vuelto muy fácil. El siguiente vídeo le guiará por los pasos necesarios para utilizar Invocar el servicio de modelo de datos de formulario en el flujo de trabajo de AEM.
feature: Workflow
type: Tutorial
version: Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 1f13d82e-c1d0-4c8c-8468-b4a4c5897c71
last-substantial-update: 2021-02-09T00:00:00Z
duration: 239
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '257'
ht-degree: 0%

---

# Usar el servicio de modelo de datos de formulario como paso en el flujo de trabajo de AEM 6.5 {#using-form-data-model-service-as-step-in-workflow}

A partir de AEM Forms 6.4, ahora podemos utilizar el servicio de modelo de datos de formulario como parte del flujo de trabajo de AEM. El siguiente vídeo explica los pasos necesarios para configurar el paso del modelo de datos de formulario en el flujo de trabajo de AEM

>[!NOTE]
>
>La funcionalidad que se muestra en este vídeo requiere AEM Forms 6.5.1


>[!VIDEO](https://video.tv.adobe.com/v/28145?quality=12&learn=on)

Para probar esta capacidad en el servidor, siga las instrucciones siguientes

* Configure tomcat con el archivo SampleRest.war como se describe [aquí](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html). El archivo war implementado en Tomcat tiene el código para devolver la puntuación crediticia del solicitante. La puntuación crediticia es un número aleatorio entre 200 y 800

* [Importación de recursos en AEM mediante el administrador de paquetes](assets/aem65-loanapplication.zip)
* El paquete contiene lo siguiente:

   * Modelo de flujo de trabajo que utiliza el paso FDM.
   * Modelo de datos de formulario que se utiliza en el paso FDM.
   * Formulario adaptable para almacenar en déclencheur el flujo de trabajo al enviar.
* Abra [MortgageApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled). Complete los detalles y envíe. Al enviar el formulario, se activa [el flujo de trabajo de la solicitud de préstamo](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html).

![flujo de trabajo](assets/invokefdm651.PNG).

El flujo de trabajo utiliza el componente OR Split para dirigir la solicitud al administrador si la puntuación crediticia es superior a 500. Si la puntuación crediticia es menor que 500, la solicitud se redirige a la captación.
