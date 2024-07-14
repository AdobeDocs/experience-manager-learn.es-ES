---
title: Usar el servicio de modelo de datos de formulario como paso en el flujo de trabajo
description: A partir de AEM Forms AEM 6.4, ahora podemos utilizar el modelo de datos de formulario como parte del flujo de trabajo de la. AEM El siguiente vídeo explica los pasos necesarios para configurar el paso Modelo de datos de formulario en Flujo de trabajo de la aplicación de datos de formulario de la aplicación de.
feature: Workflow
type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 0c77a853-fa71-46ac-8626-99bc69d6222d
last-substantial-update: 2020-06-09T00:00:00Z
duration: 205
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '261'
ht-degree: 0%

---

# Usar el servicio de modelo de datos de formulario como paso en el flujo de trabajo {#using-form-data-model-service-as-step-in-workflow}

A partir de AEM Forms AEM 6.4, ahora podemos utilizar el modelo de datos de formulario como parte del flujo de trabajo de la. AEM El siguiente vídeo muestra los pasos necesarios para configurar el paso Modelo de datos de formulario en el flujo de trabajo de la aplicación de datos de formulario de la aplicación de la aplicación de flujo de trabajo de


>[!VIDEO](https://video.tv.adobe.com/v/21719?quality=12&learn=on)

Para probar esta capacidad en el servidor, siga las instrucciones siguientes
* [Descargue e implemente el paquete setvalue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Este es el paquete OSGI personalizado que establece las propiedades de los metadatos.
>En AEM Forms 6.5 y versiones posteriores, esta funcionalidad está disponible de forma predeterminada, tal como [se describe aquí](form-data-model-service-as-step-in-aem65-workflow-video-use.md)

* Configure tomcat con el archivo SampleRest.war como se describe [aquí](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html). El archivo war implementado en Tomcat tiene el código para devolver la puntuación crediticia del solicitante. La puntuación de crédito es un número aleatorio entre 200 y 800

* AEM [Importe los recursos en la carpeta mediante el administrador de paquetes](assets/invoke-fdm-as-service-step.zip). El paquete contiene lo siguiente:

   * Modelo de flujo de trabajo que utiliza el paso FDM.
   * Modelo de datos de formulario que se utiliza en el paso FDM.
   * Formulario adaptable para almacenar en déclencheur el flujo de trabajo al enviar.
* Abra [MortgageApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled). Complete los detalles y envíe. Al enviar el formulario, se activa [el flujo de trabajo de la solicitud de préstamo](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html).

![ flujo de trabajo ](assets/fdm-as-service-step-workflow.PNG).
El flujo de trabajo utiliza el componente OR Split para dirigir la solicitud al administrador si la puntuación crediticia es superior a 500. Si la puntuación crediticia es menor que 500, la solicitud se redirige a la recuperación
