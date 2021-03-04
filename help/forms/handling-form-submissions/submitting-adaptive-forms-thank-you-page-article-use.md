---
title: Enviar A La Página De Agradecimiento
seo-title: Enviar A La Página De Agradecimiento
description: Mostrar una página de agradecimiento al enviar el formulario adaptable
seo-description: Mostrar una página de agradecimiento al enviar el formulario adaptable
uuid: ec695b87-083a-47f6-92ac-c9a6dc2b85fb
feature: Formularios adaptables
topics: developing
audience: implementer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 58c6bf42-efe5-41a3-8023-d84f3675f689
topic: Desarrollo
role: Desarrollador
level: Principiante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 1%

---


# Envío a la página de agradecimiento {#submitting-to-thank-you-page}

La opción Enviar a extremo REST pasa los datos rellenados en el formulario a una página de confirmación configurada como parte de la solicitud HTTP GET. Puede añadir el nombre de los campos que desea solicitar. El formato de la solicitud es:

\{fieldName\} = \{parameterName\}. Por ejemplo, el nombre del remitente es el nombre de un campo de formulario adaptable y el remitente es el nombre del parámetro. En la página de agradecimiento, puede acceder al parámetro de remitente utilizando request.getParameter(&quot;submitter&quot;) para obtener el valor del campo de nombre del remitente.

submitterName=submitter

En la captura de pantalla siguiente, presentamos el formulario adaptable para agradecerle a la página ubicada en /content/thankyou. A esta página de agradecimiento, se pasan 3 atributos de solicitud que albergarán los valores de campo del formulario.

![thank](assets/thankyoupage.gif)

También puede enviar al extremo externo a través de POST. Para ello, solo tiene que seleccionar la casilla de verificación &quot;habilitar solicitud posterior&quot; y proporcionar la URL para el punto final externo. Al enviar el formulario, obtendrá la página de agradecimiento y el punto final POST se invocará simultáneamente.

![captura](assets/capture.gif)


Para probar esta capacidad en su servidor, siga las instrucciones que se mencionan a continuación:

* Importe el archivo de [activos asociado a este artículo en AEM mediante el administrador de paquetes](assets/submittingtorestendpoint.zip)
* Apunte el navegador al [Formulario de solicitud de tiempo de espera](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* Rellene el campo requerido y envíe el formulario
* Debería obtener la página de agradecimiento con la información rellenada en la página

