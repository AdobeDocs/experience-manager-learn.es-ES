---
title: 'Envío de un documento de comunicación interactiva: canal web AEM Forms'
seo-title: 'Envío de un documento de comunicación interactiva: canal web AEM Forms'
description: Envío del documento del canal web mediante un vínculo en el correo electrónico
seo-description: Envío del documento del canal web mediante un vínculo en el correo electrónico
feature: Comunicación interactiva
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Desarrollo
role: Desarrollador
level: Principiante
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '298'
ht-degree: 1%

---


# Envío por correo electrónico de un documento de canal web

Una vez que haya definido y probado el documento de comunicación interactiva del canal web, necesita un mecanismo de envío para enviar el documento del canal web al destinatario.

En este artículo, echamos un vistazo al correo electrónico como un mecanismo de envío para el documento de canal web. El destinatario obtiene un vínculo al documento del canal web por correo electrónico. Al hacer clic en el vínculo, se le pedirá al usuario que se autentique y el documento del canal web se rellenará con los datos específicos del usuario que ha iniciado sesión.

Veamos el siguiente fragmento de código. Este código forma parte de GET.jsp que se activa cuando el usuario hace clic en el vínculo del correo electrónico para ver el documento del canal web. Obtenemos el usuario que ha iniciado sesión utilizando el UserManager de jackrabbit. Una vez que recibimos el usuario que ha iniciado sesión, obtenemos el valor de la propiedad accountNumber asociada al perfil del usuario.

A continuación, asociamos el valor accountNumber con una clave denominada número de cuenta en el mapa. La clave **accountnumber** se define en el modal de datos del formulario como un atributo de solicitud. El valor de este atributo se pasa como parámetro de entrada al método de servicio de lectura del modo de datos del formulario.

Línea 7: Estamos enviando la solicitud recibida a otro servlet, en función del tipo de recurso identificado por la dirección URL del documento de comunicación interactiva. La respuesta devuelta por este segundo servlet se incluye en la respuesta del primer servlet.

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

Atributo de solicitud definido para el servicio de lectura del modal de datos del formulario


[Paquete AEM de muestra](assets/webchanneldelivery.zip).
