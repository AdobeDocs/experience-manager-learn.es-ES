---
title: Uso del servicio del modelo de datos de formulario como paso del flujo de trabajo
description: A partir de AEM Forms 6.4, ahora se puede utilizar el modelo de datos de formulario como parte de AEM flujo de trabajo. El siguiente vídeo recorre los pasos necesarios para configurar el paso Modelo de datos de formulario en AEM flujo de trabajo.
feature: Workflow
type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 0c77a853-fa71-46ac-8626-99bc69d6222d
last-substantial-update: 2020-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '279'
ht-degree: 0%

---

# Uso del servicio del modelo de datos de formulario como paso del flujo de trabajo {#using-form-data-model-service-as-step-in-workflow}

A partir de AEM Forms 6.4, ahora se puede utilizar el modelo de datos de formulario como parte de AEM flujo de trabajo. El siguiente vídeo recorre los pasos necesarios para configurar el paso Modelo de datos de formulario en AEM flujo de trabajo


>[!VIDEO](https://video.tv.adobe.com/v/21719/?quality=9&learn=on)

Para probar esta capacidad en el servidor, siga las siguientes instrucciones
* [Descargar e implementar el paquete setvalue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Este es el paquete OSGI personalizado que establece propiedades de metadatos.
>!![NOTE]En AEM Forms 6.5 y versiones posteriores, esta funcionalidad está disponible de forma predeterminada como [describir aquí](form-data-model-service-as-step-in-aem65-workflow-video-use.md)

* Configuración de tomcat con el archivo SampleRest.war como se describe [here](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html).El archivo war implementado en Tomcat tiene el código para devolver la puntuación de crédito del solicitante. La puntuación de crédito es un número aleatorio entre 200 y 800

* [Importar los recursos en AEM mediante el administrador de paquetes](assets/invoke-fdm-as-service-step.zip).El paquete contiene lo siguiente:

   * Modelo de flujo de trabajo que utiliza el paso FDM.
   * Modelo de datos de formulario que se utiliza en el paso FDM.
   * Formulario adaptable para almacenar en déclencheur el flujo de trabajo tras el envío.
* Abra el [FormularioDeAplicaciónHipotecario](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled). Complete los detalles y envíe. En el envío del formulario, la variable [flujo de trabajo de la aplicación de préstamo](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html) se activa.

![ flujo de trabajo ](assets/fdm-as-service-step-workflow.PNG).
El flujo de trabajo utiliza el componente O dividido para dirigir la aplicación al administrador si la puntuación de crédito es superior a 500. Si la puntuación de crédito es inferior a 500, la aplicación se envía a la entrega
