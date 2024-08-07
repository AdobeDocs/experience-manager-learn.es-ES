---
title: Abrir La Interfaz De Usuario Del Agente Al Enviar El POST
description: Esta es la parte 11 del tutorial de varios pasos para crear el primer documento de comunicaciones interactivas para el canal Imprimir. En esta parte, iniciaremos la interfaz de la interfaz de usuario del agente para crear correspondencia ad-hoc sobre el envío de formularios.
feature: Interactive Communication
doc-type: Tutorial
version: 6.4,6.5
jira: KT-6168
thumbnail: 40122.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: 509b4d0d-9f3c-46cb-8ef7-07e831775086
duration: 170
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '322'
ht-degree: 0%

---

# Abrir La Interfaz De Usuario Del Agente Al Enviar El POST

En esta parte, iniciaremos la interfaz de la interfaz de usuario del agente para crear correspondencia ad-hoc sobre el envío de formularios.

Este artículo le guiará por los pasos necesarios para abrir la interfaz de usuario del agente al enviar un formulario. El caso de uso típico es que el agente de servicio al cliente rellene un formulario con algunos parámetros de entrada y en la interfaz de usuario del agente de envío de formularios se abra con datos previamente rellenados desde el servicio de prerrellenado del modelo de datos de formulario. Los parámetros de entrada al servicio de prerrellenado del modelo de datos de formulario se extraen del envío del formulario.

El siguiente vídeo muestra un caso de uso

>[!VIDEO](https://video.tv.adobe.com/v/40122?quality=12&learn=on)

```java
String accountNumber = request.getParameter("accountnumber"))
ParameterMap parameterMap = new ParameterMap();
RequestParameter icLetterId[] = new RequestParameter[1];
icLetterId[0] = new FormFieldRequestParameter("/content/dam/formsanddocuments/retirementstatementprint");
parameterMap.put("documentId", icLetterId);
RequestParameter Random[] = new RequestParameter[1];
Random[0] = new FormFieldRequestParameter("209457");
parameterMap.put("Random", Random);
Map map = new HashMap();
map.put("accountnumber",accountNumber);
slingRequest.setAttribute("paramMap",map);
CustomParameterRequest wrapperRequest = new CustomParameterRequest(slingRequest,parameterMap,"GET");
wrapperRequest.getRequestDispatcher("/aem/forms/createcorrespondence.html").include(wrapperRequest, response);
```

Línea 1 : Obtenga el número de cuenta del parámetro de solicitud

Línea 2-8: cree un mapa de parámetros y defina las claves y los valores adecuados para reflejar documentId,Random.

Línea 9-10: cree otro objeto Map para albergar el parámetro de entrada definido en el modelo de datos de formulario.

Línea 11: Establezca el atributo slingRequest &quot;paramMap&quot;

Línea 12-13: reenviar la solicitud al servlet

Para probar esta capacidad en el servidor

* [Importe e instale los recursos relacionados con este artículo mediante el administrador de paquetes.](assets/launch-agent-ui.zip)
* [Iniciar sesión en configMgr](http://localhost:4502/system/console/configMgr)
* Buscar _Adobe Granite CSRF Filter_
* Agregar _/content/getprintchannel_ en las rutas excluidas
* Guarde los cambios.
* [Abrir POST.jsp](http://localhost:4502/apps/AEMForms/openprintchannel/POST.jsp). Asegúrese de que la cadena pasada a FormFieldRequestParameter sea un documentId válido.(Línea 19).
* [Abra la página web](http://localhost:4502/content/OpenPrintChannel.html), escriba el número de cuenta y envíe el formulario.
* La interfaz de usuario del agente debe abrirse con los datos rellenados previamente específicos del número de cuenta introducido en el formulario.

>[!NOTE]
>
>Asegúrese de que el parámetro de entrada de la operación Get del modelo de datos de formulario esté enlazado al atributo de solicitud denominado &quot;accountnumber&quot; para que esto funcione. Si cambia el nombre del valor de enlace a cualquier otro nombre, asegúrese de que el cambio se refleje en la línea 25 de POST.jsp
