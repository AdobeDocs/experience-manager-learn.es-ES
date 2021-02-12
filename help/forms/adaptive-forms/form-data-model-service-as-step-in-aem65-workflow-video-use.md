---
title: Uso del servicio Modelo de datos de formulario como paso en el flujo de trabajo de AEM 6.5
seo-title: Uso del servicio Modelo de datos de formulario como paso en el flujo de trabajo de AEM 6.5
description: AEM Forms 6.5 ha introducido la capacidad de crear variables en el flujo de trabajo de AEM. Con esta nueva capacidad, utilizar el "servicio Invoke Form Data Model" en AEM flujo de trabajo se ha vuelto muy fácil. El siguiente vídeo le guiará a través de los pasos necesarios para utilizar el servicio Invocar modelo de datos de formulario en AEM flujo de trabajo.
seo-description: AEM Forms 6.5 ha introducido la capacidad de crear variables en el flujo de trabajo de AEM. Con esta nueva capacidad, utilizar el "servicio Invoke Form Data Model" en AEM flujo de trabajo se ha vuelto muy fácil. El siguiente vídeo le guiará a través de los pasos necesarios para utilizar el servicio Invocar modelo de datos de formulario en AEM flujo de trabajo.
feature: workflow.
topics: workflow.
audience: developer.
doc-type: technical video.
activity: setup.
version: 6.5.
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '334'
ht-degree: 0%

---


# Uso del servicio de modelo de datos de formulario como paso en AEM flujo de trabajo de 6.5 {#using-form-data-model-service-as-step-in-workflow}

A partir de AEM Forms 6.4, ahora podemos utilizar el servicio Modelo de datos de formulario como parte de AEM flujo de trabajo. El siguiente vídeo recorre los pasos necesarios para configurar el paso del modelo de datos de formulario en AEM flujo de trabajo

>!![NOTE]La capacidad demostrada en este vídeo requiere AEM Forms 6.5.1


>[!VIDEO](https://video.tv.adobe.com/v/28145?quality=9&learn=on)

Para probar esta capacidad en el servidor, siga las instrucciones siguientes

* El programa de instalación se configura con el archivo SampleRest.war como se describe [aquí](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html).El archivo de guerra implementado en Tomcat tiene el código para devolver el puntaje de crédito del solicitante.El puntaje de crédito es un número aleatorio entre 200 y 800

* [ Importación de recursos en AEM mediante el administrador de paquetes](assets/aem65-loanapplication.zip)
* El paquete contiene lo siguiente:

   * Modelo de flujo de trabajo que utiliza el paso FDM.
   * Modelo de datos de formulario que se utiliza en el paso FDM.
   * Formulario adaptable para el déclencheur del flujo de trabajo durante el envío.
* Abra el [formularioDeAplicaciónHipoteca](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled). Rellene los detalles y envíe. En el envío del formulario, se activa el [flujo de trabajo de solicitud de préstamo](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html).

![ flujo de trabajo ](assets/invokefdm651.PNG).
El flujo de trabajo utiliza el componente O dividir para enrutar la aplicación a la administración si la puntuación de crédito es superior a 500. Si el puntaje de crédito es inferior a 500, la solicitud se distribuye a la entrega.
