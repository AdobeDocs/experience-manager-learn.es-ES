---
title: Envío del formulario adaptable al servidor externo
seo-title: Envío del formulario adaptable al servidor externo
description: Envío del formulario adaptable al extremo REST que se ejecuta en un servidor externo
seo-description: Envío del formulario adaptable al extremo REST que se ejecuta en un servidor externo
uuid: 1a46e206-6188-4096-816a-d59e9fb43263
feature: adaptive-forms
topics: developing
audience: implementer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 9e936885-4e10-4c05-b572-b8da56fcac73
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '362'
ht-degree: 0%

---


# Envío del formulario adaptable al servidor externo {#submitting-adaptive-form-to-external-server}

Utilice la acción Enviar a extremo REST para anunciar los datos enviados en una URL REST. La dirección URL puede ser de un servidor interno (el servidor en el que se procesa el formulario) o de un servidor externo.

Normalmente, los clientes desean enviar los datos del formulario a un servidor externo para un procesamiento posterior.

Para enviar datos a un servidor interno, proporcione una ruta del recurso. Los datos se publican en la ruta del recurso. Por ejemplo, &lt;/content/restEndPoint> . Para esas solicitudes de anuncios, se utiliza la información de autenticación de la solicitud de envío.

Para enviar datos a un servidor externo, proporcione una URL. The format of the URL is <http://host:port/path_to_rest_end_point>. Asegúrese de haber configurado la ruta para gestionar la solicitud de POST de forma anónima.

A los efectos de este artículo, he escrito un simple archivo de guerra que puede ser implementado en tu instancia de tomcat. Si damos por supuesto que tomcat se está ejecutando en el puerto 8080, la dirección URL del POST será

<http://localhost:8080/AemFormsEnablement/HandleFormSubmission>

al configurar el formulario adaptable para que se envíe a este extremo, el siguiente código puede extraer los datos del formulario y los datos adjuntos, si los hay, en el servlet

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

![envío de formulario](assets/formsubmission.gif)Para probar esto en su servidor, haga lo siguiente

1. Instale Tomcat si todavía no lo tiene. [Las instrucciones para instalar tomcat están disponibles aquí](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html)
1. Descargue el archivo [zip](assets/aemformsenablement.zip) asociado a este artículo. Descomprima el archivo para obtener el archivo de guerra.
1. Implemente el archivo de guerra en el servidor tomcat.
1. Cree un formulario adaptable sencillo con el componente de archivo adjunto y configure su acción de envío como se muestra en la captura de pantalla anterior. La dirección URL del POST es <http://localhost:8080/AemFormsEnablement/HandleFormSubmission>. Si su AEM y tomcat no se están ejecutando en localhost, cambie la dirección URL según corresponda.
1. Para habilitar el envío de datos de formularios de varias partes en tomcat, agregue el atributo siguiente al elemento de contexto de &lt;tomcatInstallDir>\conf\context.xml y reinicie el servidor de Tomcat.
1. **&lt;Context allowCustomMultipartParsing=&quot;true&quot;>**
1. Previsualización del formulario adaptable, agregue un archivo adjunto y envíe el archivo. Compruebe si hay mensajes en la ventana de la consola de tomcat.

