---
title: Envío del Documento de comunicación interactiva - Canal web AEM Forms
seo-title: Envío del Documento de comunicación interactiva - Canal web AEM Forms
description: Envío del documento del canal web a través del vínculo en el correo electrónico
seo-description: Envío del documento del canal web a través del vínculo en el correo electrónico
feature: interactive-communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '293'
ht-degree: 0%

---


# Envío de correo electrónico del Documento de Canal web

Una vez definido y probado el documento de comunicación interactivo de canal web, necesita un mecanismo de envío para entregar el documento de canal web al destinatario.

En este artículo, vemos el correo electrónico como un mecanismo de envío para el documento de canal web. El destinatario recibirá un vínculo al documento de canal web por correo electrónico.Al hacer clic en el vínculo, se pedirá al usuario que se autentique y el documento de canal web se rellenará con los datos específicos del usuario que ha iniciado sesión.

Veamos el siguiente fragmento de código. Este código forma parte de GET.jsp, que se activa cuando el usuario hace clic en el vínculo del correo electrónico para realizar la vista del documento de canal web. Obtenemos al usuario que ha iniciado sesión usando el comando jackrabbit UserManager. Una vez que recibimos el usuario que ha iniciado sesión, obtenemos el valor de la propiedad accountNumber asociada al perfil del usuario.

A continuación, asociamos el valor accountNumber con una clave denominada accountnumber en el mapa. El **número** de cuenta clave se define en el modo de datos de formulario como un atributo de solicitud. El valor de este atributo se pasa como un parámetro de entrada al método de servicio de lectura de Form Data Modal.

Línea 7: Estamos enviando la solicitud recibida a otro servlet, en función del tipo de recurso identificado por la dirección URL del Documento de comunicación interactiva. La respuesta devuelta por este segundo servlet se incluye en la respuesta del primer servlet.

```java
org.apache.jackrabbit.api.security.user.UserManager um = ((org.apache.jackrabbit.api.JackrabbitSession) session).getUserManager();
org.apache.jackrabbit.api.security.user.Authorizable loggedinUser = um.getAuthorizable(session.getUserID());
String accountNumber = loggedinUser.getProperty("profile/accountNumber")[0].getString();
map.put("accountnumber",accountNumber);
slingRequest.setAttribute("paramMap",map);
CustomParameterRequest wrapperRequest = new CustomParameterRequest(slingRequest,"GET");
wrapperRequest.getRequestDispatcher("/content/forms/af/401kstatement/irastatement/channels/web.html").include(wrapperRequest, response);
```

![includemethod](assets/includemethod.jpg)

Representación visual del código de línea 7

![requestparameter](assets/requestparameter.png)

Atributo de solicitud definido para el servicio de lectura del modal de datos de formulario


[Ejemplo de paquete](assets/webchanneldelivery.zip)de AEM.
