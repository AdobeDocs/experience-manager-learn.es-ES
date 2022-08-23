---
title: Uso del servicio del modelo de datos de formulario como paso en el flujo de trabajo de AEM 6.5
description: AEM Forms 6.5 ha introducido la capacidad de crear variables en el flujo de trabajo AEM. Con esta nueva capacidad utilizando el servicio "Invocar modelo de datos de formulario" en AEM flujo de trabajo se ha vuelto muy fácil. El siguiente vídeo le guiará por los pasos necesarios para utilizar el servicio Invocar modelo de datos de formulario en AEM flujo de trabajo.
feature: Workflow
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 1f13d82e-c1d0-4c8c-8468-b4a4c5897c71
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '272'
ht-degree: 0%

---

# Uso del servicio del modelo de datos de formulario como paso en el flujo de trabajo de AEM 6.5 {#using-form-data-model-service-as-step-in-workflow}

A partir de AEM Forms 6.4, ahora se puede utilizar el servicio del modelo de datos de formulario como parte de AEM flujo de trabajo. El siguiente vídeo recorre los pasos necesarios para configurar el paso Modelo de datos de formulario en AEM flujo de trabajo

>!![NOTE]La capacidad demostrada en este vídeo requiere AEM Forms 6.5.1


>[!VIDEO](https://video.tv.adobe.com/v/28145?quality=9&learn=on)

Para probar esta capacidad en el servidor, siga las siguientes instrucciones

* Configuración de tomcat con el archivo SampleRest.war como se describe [here](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html).El archivo war implementado en Tomcat tiene el código para devolver la puntuación de crédito del solicitante.La puntuación de crédito es un número aleatorio entre 200 y 800

* [ Importar los recursos en AEM mediante el administrador de paquetes](assets/aem65-loanapplication.zip)
* El paquete contiene lo siguiente:

   * Modelo de flujo de trabajo que utiliza el paso FDM.
   * Modelo de datos de formulario que se utiliza en el paso FDM.
   * Formulario adaptable para almacenar en déclencheur el flujo de trabajo tras el envío.
* Abra el [FormularioDeAplicaciónHipotecario](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled). Complete los detalles y envíe. En el envío del formulario, la variable [flujo de trabajo de la aplicación de préstamo](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html) se activa.

![ flujo de trabajo ](assets/invokefdm651.PNG).
El flujo de trabajo utiliza el componente O dividido para dirigir la aplicación al administrador si la puntuación de crédito es superior a 500. Si la puntuación de crédito es inferior a 500, la aplicación se envía a la entrega.
