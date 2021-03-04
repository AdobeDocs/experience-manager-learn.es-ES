---
title: Uso del servicio del modelo de datos de formulario como paso del flujo de trabajo
seo-title: Uso del servicio del modelo de datos de formulario como paso del flujo de trabajo
description: A partir de AEM Forms 6.4, ahora se puede utilizar el modelo de datos de formulario como parte de AEM Workflow. El siguiente vídeo recorre los pasos necesarios para configurar el paso del Modelo de datos de formulario en AEM Workflow.
seo-description: A partir de AEM Forms 6.4, ahora se puede utilizar el modelo de datos de formulario como parte de AEM Workflow. El siguiente vídeo recorre los pasos necesarios para configurar el paso del Modelo de datos de formulario en AEM Workflow.
uuid: ecd5d5aa-01eb-48fb-872f-66c656ae14df.
feature: Flujo de trabajo
topics: integrations
audience: developer
doc-type: technical video
activity: setup
version: 6.4,6.5
discoiquuid: c442f439-1e5d-4f96-85df-b818c28389ff
topic: Desarrollo
role: Desarrollador
level: Intermedio
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '330'
ht-degree: 1%

---


# Uso del servicio del modelo de datos de formulario como paso en el flujo de trabajo {#using-form-data-model-service-as-step-in-workflow}

A partir de AEM Forms 6.4, ahora se puede utilizar el modelo de datos de formulario como parte de AEM Workflow. El siguiente vídeo recorre los pasos necesarios para configurar el paso del Modelo de datos de formulario en el flujo de trabajo de AEM


>[!VIDEO](https://video.tv.adobe.com/v/21719/?quality=9&learn=on)

Para probar esta capacidad en el servidor, siga las siguientes instrucciones
* [Descargue e implemente el paquete setvalue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Este es el paquete OSGI personalizado que establece propiedades de metadatos.
>!![NOTE]En AEM Forms 6.5 y versiones posteriores, esta funcionalidad está disponible de forma predeterminada, tal como  [se describe aquí](form-data-model-service-as-step-in-aem65-workflow-video-use.md)

* Configure para que el archivo SampleRest.war se muestre como se describe [aquí](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html). El archivo war implementado en Tomcat tiene el código para devolver la puntuación de crédito del solicitante. La puntuación de crédito es un número aleatorio entre 200 y 800

* [Importe los recursos en AEM mediante el administrador de paquetes](assets/invoke-fdm-as-service-step.zip). El paquete contiene lo siguiente:

   * Modelo de flujo de trabajo que utiliza el paso FDM.
   * Modelo de datos de formulario que se utiliza en el paso FDM.
   * Formulario adaptable para activar el flujo de trabajo en el envío.
* Abra [MortgageApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled). Complete los detalles y envíe. En el envío del formulario, se activa el [loanapplication workflow](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html).

![ flujo de trabajo ](assets/fdm-as-service-step-workflow.PNG).
El flujo de trabajo utiliza el componente O dividido para dirigir la aplicación al administrador si la puntuación de crédito es superior a 500. Si la puntuación de crédito es inferior a 500, la aplicación se envía a la entrega
