---
title: Uso del servicio del modelo de datos de formulario como paso en el flujo de trabajo
seo-title: Uso del servicio del modelo de datos de formulario como paso en el flujo de trabajo
description: A partir de AEM Forms 6.4, ahora podemos utilizar el modelo de datos de formulario como parte de AEM flujo de trabajo. El siguiente vídeo muestra los pasos necesarios para configurar el paso del modelo de datos de formulario en AEM flujo de trabajo.
seo-description: A partir de AEM Forms 6.4, ahora podemos utilizar el modelo de datos de formulario como parte de AEM flujo de trabajo. El siguiente vídeo muestra los pasos necesarios para configurar el paso del modelo de datos de formulario en AEM flujo de trabajo.
uuid: ecd5d5aa-01eb-48fb-872f-66c656ae14df.
feature: workflow
topics: integrations
audience: developer
doc-type: technical video
activity: setup
version: 6.4,6.5
discoiquuid: c442f439-1e5d-4f96-85df-b818c28389ff
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '326'
ht-degree: 0%

---


# Uso del servicio del modelo de datos de formulario como paso en el flujo de trabajo {#using-form-data-model-service-as-step-in-workflow}

A partir de AEM Forms 6.4, ahora podemos utilizar el modelo de datos de formulario como parte de AEM flujo de trabajo. El siguiente vídeo recorre los pasos necesarios para configurar el paso del modelo de datos de formulario en AEM flujo de trabajo


>[!VIDEO](https://video.tv.adobe.com/v/21719/?quality=9&learn=on)

Para probar esta capacidad en el servidor, siga las instrucciones siguientes
* [Descargue e implemente el paquete](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)setvalue. Es el paquete OSGI personalizado que establece las propiedades de metadatos.
>!![NOTE]En AEM Forms 6.5 y versiones posteriores, esta capacidad está disponible de forma predeterminada, como [se describe aquí](form-data-model-service-as-step-in-aem65-workflow-video-use.md)

* El programa de instalación se configura con el archivo SampleRest.war como se describe [aquí](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html). El archivo de guerra implementado en Tomcat tiene el código para devolver la puntuación de crédito del solicitante. La puntuación de crédito es un número aleatorio entre 200 y 800

* [Importe los recursos en AEM mediante el administrador](assets/invoke-fdm-as-service-step.zip)de paquetes. El paquete contiene lo siguiente:

   * Modelo de flujo de trabajo que utiliza el paso FDM.
   * Modelo de datos de formulario que se utiliza en el paso FDM.
   * Formulario adaptable para activar el flujo de trabajo al enviar.
* Abra [MortgageApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled). Rellene los detalles y envíe. En el envío del formulario, se activa el flujo de trabajo [de la aplicación de](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html) préstamo.

![ flujo de trabajo ](assets/fdm-as-service-step-workflow.PNG).
El flujo de trabajo utiliza el componente O dividir para enrutar la aplicación a la administración si la puntuación de crédito es superior a 500. Si el puntaje de crédito es inferior a 500, la solicitud se distribuye a la entrega
