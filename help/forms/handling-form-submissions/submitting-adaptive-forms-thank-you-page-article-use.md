---
title: Enviar a página de agradecimiento
seo-title: Enviar a página de agradecimiento
description: Mostrar una página de agradecimiento al enviar el formulario adaptable
seo-description: Mostrar una página de agradecimiento al enviar el formulario adaptable
uuid: ec695b87-083a-47f6-92ac-c9a6dc2b85fb
feature: adaptive-forms
topics: developing
audience: implementer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 58c6bf42-efe5-41a3-8023-d84f3675f689
translation-type: tm+mt
source-git-commit: 0bccdea82f6db391cbca3ab06009a7b4420a38bf
workflow-type: tm+mt
source-wordcount: '275'
ht-degree: 0%

---


# Enviar a página de agradecimiento {#submitting-to-thank-you-page}

Enviar a extremo REST pasa los datos rellenados en el formulario a una página de confirmación configurada como parte de la solicitud de GET HTTP. Puede agregar el nombre de los campos que se van a solicitar. El formato de la solicitud es:

\{fieldName\} = \{parameterName\}. Por ejemplo, el nombre del remitente es el nombre de un campo de formulario adaptable y el remitente es el nombre del parámetro. En la página de agradecimiento, puede acceder al parámetro de remitente mediante request.getParameter(&quot;submitter&quot;) para obtener el valor del campo de nombre del remitente.

nombreremitente=remitente

En la captura de pantalla de abajo, enviamos el formulario adaptable a la página de agradecimiento ubicada en /content/thankyou. A esta página de agradecimiento, se están pasando 3 atributos de solicitud que contendrán los valores de campo del formulario.

![thank](assets/thankyoupage.gif)

También puede enviar al extremo externo mediante POST. Para lograrlo, solo tiene que seleccionar la casilla de verificación &quot;habilitar solicitud posterior&quot; y proporcionar la dirección URL para el extremo externo. Al enviar el formulario, obtendrá una página de agradecimiento y se invocará el extremo de POST simultáneamente.

![captura](assets/capture.gif)


Para probar esta capacidad en su servidor, siga las instrucciones que se mencionan a continuación:

* Importar el archivo de [recursos asociado a este artículo a AEM mediante el administrador de paquetes](assets/submittingtorestendpoint.zip)
* Seleccione el explorador para el formulario de solicitud de [tiempo de espera](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* Rellene el campo requerido y envíe el formulario
* Debería obtener la página de agradecimiento con la información rellenada en la página

