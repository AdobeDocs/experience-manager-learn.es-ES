---
title: Enviando a página de agradecimiento
seo-title: Submitting To Thank You Page
description: Mostrar una página de agradecimiento al enviar el formulario adaptable
seo-description: Display a thank you page on submitting Adaptive Form
uuid: ec695b87-083a-47f6-92ac-c9a6dc2b85fb
feature: Adaptive Forms
topics: developing
audience: implementer
doc-type: article
activity: setup
version: 6.4,6.5
discoiquuid: 58c6bf42-efe5-41a3-8023-d84f3675f689
topic: Development
role: Developer
level: Beginner
exl-id: 85e1b450-39c0-4bb8-be5d-d7f50b102f3d
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '260'
ht-degree: 7%

---

# Enviando a página de agradecimiento {#submitting-to-thank-you-page}

La opción Enviar al extremo REST pasa los datos rellenados en el formulario a una página de confirmación configurada como parte de la solicitud de GET HTTP. Puede agregar el nombre de los campos que desea solicitar. El formato de la solicitud es el siguiente:

\{fieldName\} = \{parameterName\}. Por ejemplo, submitterName es el nombre de un campo de formulario adaptable y submitter es el nombre del parámetro. En la página de agradecimiento, puede acceder al parámetro del remitente mediante request.getParameter(&quot;submitter&quot;) para obtener el valor del campo del nombre del remitente.

`submitterName=submitter`

En la captura de pantalla siguiente, enviamos el formulario adaptable a la página de agradecimiento ubicada en /content/thank you. A esta página de agradecimiento, estamos pasando 3 atributos de solicitud que contienen los valores del campo de formulario.

![Página de agradecimiento](assets/thankyoupage.gif)

También puede enviar al extremo externo a través de POST. Para ello, solo tiene que seleccionar la casilla &quot;habilitar solicitud posterior&quot; y proporcionar la URL del punto de conexión externo. Al enviar el formulario, aparece una página de agradecimiento y el punto de conexión del POST se invoca simultáneamente.

![Capturar configuración](assets/capture.gif)

Para probar esta capacidad en su servidor, siga las instrucciones que se mencionan a continuación:

* Importe el [AEM el archivo de recursos asociado con este artículo en el que se utiliza el administrador de paquetes para la creación de informes de recursos de la aplicación](assets/submittingtorestendpoint.zip)
* Dirija el explorador a [Formulario de solicitud de tiempo libre](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* Rellene el campo requerido y envíe el formulario
* Debería obtener la página de agradecimiento con la información rellenada en la página
