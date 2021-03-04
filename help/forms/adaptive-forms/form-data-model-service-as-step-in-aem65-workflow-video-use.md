---
title: Uso del servicio del modelo de datos de formulario como paso en el flujo de trabajo de AEM 6.5
seo-title: Uso del servicio del modelo de datos de formulario como paso en el flujo de trabajo de AEM 6.5
description: AEM Forms 6.5 introdujo la capacidad de crear variables en el flujo de trabajo de AEM. Con esta nueva capacidad utilizando el "Invocar el servicio del modelo de datos de formulario" en AEM Workflow se ha vuelto muy fácil. El siguiente vídeo le guiará por los pasos necesarios para utilizar el servicio Invocar modelo de datos de formulario en AEM Workflow.
seo-description: AEM Forms 6.5 introdujo la capacidad de crear variables en el flujo de trabajo de AEM. Con esta nueva capacidad utilizando el "Invocar el servicio del modelo de datos de formulario" en AEM Workflow se ha vuelto muy fácil. El siguiente vídeo le guiará por los pasos necesarios para utilizar el servicio Invocar modelo de datos de formulario en AEM Workflow.
feature: Flujo de trabajo
topics: workflow.
audience: developer.
doc-type: technical video.
activity: setup.
version: 6.5.
topic: Desarrollo
role: Desarrollador
level: Intermedio
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '338'
ht-degree: 1%

---


# Uso del servicio del modelo de datos de formulario como paso en el flujo de trabajo de AEM 6.5 {#using-form-data-model-service-as-step-in-workflow}

A partir de AEM Forms 6.4, ahora se puede utilizar el servicio del modelo de datos de formulario como parte de AEM Workflow. El siguiente vídeo recorre los pasos necesarios para configurar el paso del Modelo de datos de formulario en el flujo de trabajo de AEM

>!![NOTE]La capacidad demostrada en este vídeo requiere AEM Forms 6.5.1


>[!VIDEO](https://video.tv.adobe.com/v/28145?quality=9&learn=on)

Para probar esta capacidad en el servidor, siga las siguientes instrucciones

* Configuración de tomcat con el archivo SampleRest.war como se describe [aquí](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html). El archivo war implementado en Tomcat tiene el código para devolver la puntuación de crédito del solicitante.La puntuación de crédito es un número aleatorio entre 200 y 800

* [ Importación de recursos en AEM mediante el administrador de paquetes](assets/aem65-loanapplication.zip)
* El paquete contiene lo siguiente:

   * Modelo de flujo de trabajo que utiliza el paso FDM.
   * Modelo de datos de formulario que se utiliza en el paso FDM.
   * Formulario adaptable para activar el flujo de trabajo en el envío.
* Abra [MortgageApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled). Complete los detalles y envíe. En el envío del formulario, se activa el [loanapplication workflow](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html).

![ flujo de trabajo ](assets/invokefdm651.PNG).
El flujo de trabajo utiliza el componente O dividido para dirigir la aplicación al administrador si la puntuación de crédito es superior a 500. Si la puntuación de crédito es inferior a 500, la aplicación se envía a la entrega.
