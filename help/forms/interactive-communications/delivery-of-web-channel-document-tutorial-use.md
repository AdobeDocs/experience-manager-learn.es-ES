---
title: 'Envío de documento de comunicación interactiva: AEM Forms de canal web'
description: Envío del documento del canal web mediante un vínculo en el correo electrónico
feature: Interactive Communication
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 50858100-3d0c-42bd-87b8-f626212669e2
last-substantial-update: 2019-07-07T00:00:00Z
duration: 60
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '279'
ht-degree: 0%

---

# Entrega por correo electrónico del documento del canal web

Una vez definido y probado el documento de comunicación interactiva del canal web, necesita un mecanismo de entrega para enviar el documento del canal web al destinatario.

En este artículo, echamos un vistazo al correo electrónico como mecanismo de envío para el documento del canal web. El destinatario obtiene un vínculo al documento del canal web por correo electrónico. Al hacer clic en el vínculo, se solicita al usuario que se autentique y el documento del canal web se rellena con los datos específicos del usuario que ha iniciado sesión.

Echemos un vistazo al siguiente fragmento de código. Este código forma parte de GET.jsp, que se activa cuando el usuario hace clic en el vínculo del correo electrónico para ver el documento del canal web. Obtenemos el usuario que inició sesión usando el jackrabbit UserManager. Una vez que obtenemos el usuario que ha iniciado sesión, obtenemos el valor de la propiedad accountNumber asociada al perfil del usuario.

Luego asociamos el valor accountNumber con una clave denominada accountnumber en el mapa. La clave **accountnumber** se define en el modal de datos del formulario como un atributo de solicitud. El valor de este atributo se pasa como parámetro de entrada al método de servicio de lectura modal de datos de formulario.

Línea 7: estamos enviando la solicitud recibida a otro servlet, según el tipo de recurso identificado por la dirección URL del documento de comunicación interactiva. La respuesta devuelta por este segundo servlet se incluye en la respuesta del primer servlet.

```java
org.apache.jackrabbit.api.security.user.UserManager um = ((org.apache.jackrabbit.api.JackrabbitSession) session).getUserManager();
org.apache.jackrabbit.api.security.user.Authorizable loggedinUser = um.getAuthorizable(session.getUserID());
String accountNumber = loggedinUser.getProperty("profile/accountNumber")[0].getString();
map.put("accountnumber",accountNumber);
slingRequest.setAttribute("paramMap",map);
CustomParameterRequest wrapperRequest = new CustomParameterRequest(slingRequest,"GET");
wrapperRequest.getRequestDispatcher("/content/forms/af/401kstatement/irastatement/channels/web.html").include(wrapperRequest, response);
```

![Método Include](assets/includemethod.jpg)

Representación visual del código de Línea 7

![Solicitar configuración de parámetros](assets/requestparameter.png)

Atributo de solicitud definido para el servicio de lectura del modal de datos de formulario

[AEM Paquete de muestra](assets/webchanneldelivery.zip).
