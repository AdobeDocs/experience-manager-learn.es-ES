---
title: Enviar formulario adaptable al servidor externo
description: Enviar formulario adaptable al extremo REST que se ejecuta en un servidor externo
feature: Adaptive Forms
doc-type: article
version: 6.4,6.5
discoiquuid: 9e936885-4e10-4c05-b572-b8da56fcac73
topic: Development
role: Developer
level: Beginner
exl-id: 5363c3f7-9006-4430-b647-f3283a366a64
last-substantial-update: 2020-07-07T00:00:00Z
duration: 92
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '340'
ht-degree: 12%

---

# Enviar formulario adaptable al servidor externo {#submitting-adaptive-form-to-external-server}

Utilice la acción Enviar al punto final REST para publicar los datos enviados en una URL REST. La URL puede ser de un servidor interno (el servidor en el que se procesa el formulario) o externo.

Normalmente, los clientes desearán enviar los datos del formulario a un servidor externo para un procesamiento posterior.

Para enviar datos a un servidor interno, proporcione una ruta del recurso. Los datos se publican en la ruta del recurso. Por ejemplo, &lt;/content restendpoint=&quot;&quot;> . Para estas peticiones POST se utiliza la información de autenticación de la solicitud de envío.

Para enviar datos a un servidor externo, proporcione una URL. El formato de la URL es el siguiente <http://host:port/path_to_rest_end_point>. Asegúrese de haber configurado la ruta para administrar la solicitud del POST de forma anónima.

Para el propósito de este artículo, he escrito un simple archivo de guerra que puede ser implementado en su instancia de Tomcat. Suponiendo que el tomcat se ejecute en el puerto 8080, la dirección URL del POST será

<http://localhost:8080/AemFormsEnablement/HandleFormSubmission>

cuando configure el formulario adaptable para que se envíe a este punto final, los datos del formulario y los archivos adjuntos, si los hay, se pueden extraer en el servlet mediante el siguiente código

```java
System.out.println("form was submitted");
Part attachment = request.getPart("attachments");
if(attachment!=null)
{
    System.out.println("The content type of the attachment added is "+attachment.getContentType());
}
Enumeration<String> params = request.getParameterNames();
while(params.hasMoreElements())
{
String paramName = params.nextElement();
System.out.println("The param Name is "+paramName);
String data = request.getParameter(paramName);System.out.println("The data  is "+data);
}
```

![formsubmission](assets/formsubmission.gif)
Para probar esto en el servidor, haga lo siguiente

1. Instale Tomcat si todavía no lo tiene. [Las instrucciones para instalar tomcat están disponibles aquí](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html)
1. Descargue la [archivo zip](assets/aemformsenablement.zip) asociadas a este artículo. Descomprima el archivo para obtener el archivo WAR.
1. Implemente el archivo WAR en el servidor Tomcat.
1. Cree un formulario adaptable simple con un componente de archivo adjunto y configure su acción de envío como se muestra en la captura de pantalla anterior. La URL del POST es <http://localhost:8080/AemFormsEnablement/HandleFormSubmission>. AEM Si su y tomcat no se están ejecutando en localhost, cambie la dirección URL según corresponda.
1. Para habilitar el envío de datos de formulario de varias partes a tomcat, agregue el siguiente atributo al elemento de contexto del &lt;tomcatinstalldir>\conf\context.xml y reinicie el servidor Tomcat.
1. **&lt;Context allowCasualMultipartParsing=&quot;true&quot;>**
1. Obtenga una vista previa del formulario adaptable, agregue un archivo adjunto y envíelo. Compruebe si hay mensajes en la ventana de la consola tomcat.
