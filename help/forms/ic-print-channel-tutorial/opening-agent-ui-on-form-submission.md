---
title: Apertura De La Interfaz De Usuario Del Agente Al Enviar El POST
seo-title: Apertura De La Interfaz De Usuario Del Agente Al Enviar El POST
description: Esta es la parte 11 de un tutorial de varios pasos para crear su primer documento interactivo de comunicaciones para el canal de impresión. En esta parte, lanzaremos la interfaz de agente ui para crear correspondencia ad-hoc sobre el envío de formularios.
seo-description: Esta es la parte 11 de un tutorial de varios pasos para crear su primer documento interactivo de comunicaciones para el canal de impresión. En esta parte, lanzaremos la interfaz de agente ui para crear correspondencia ad-hoc sobre el envío de formularios.
uuid: 96f34986-a5c3-400b-b51b-775da5d2cbd7
feature: interactive-communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6168
thumbnail: 40122.jpg
translation-type: tm+mt
source-git-commit: 824efde8d90dd77d41dce093998b4215db2532ae
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 0%

---


# Apertura De La Interfaz De Usuario Del Agente Al Enviar El POST

En esta parte, lanzaremos la interfaz de agente ui para crear correspondencia ad-hoc sobre el envío de formularios.

Este artículo le guiará a través de los pasos necesarios para abrir la interfaz ui del agente al enviar un formulario. El caso de uso habitual es que el agente de servicio al cliente rellene un formulario con algunos parámetros de entrada y, en el caso de que el agente de envío de formulario se abra con datos previamente rellenados desde el servicio de cumplimentación previa del modelo de datos de formulario.Los parámetros de entrada al servicio de cumplimentación previa del modelo de datos de formulario se extraen del envío del formulario.

El siguiente vídeo muestra el caso de uso

>[!VIDEO](https://video.tv.adobe.com/v/40122/?quality=9&learn=on)

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

Línea 1: Obtener el número de cuenta del parámetro de solicitud

Línea 2-8: Cree una asignación de parámetros y defina las claves y los valores adecuados para reflejar documentId,Random.

Línea 9-10: Cree otro objeto Map para mantener el parámetro de entrada definido en el Modelo de datos de formulario.

Línea 11: Configure el atributo slingRequest &quot;paramMap&quot;

Línea 12-13: Reenviar la solicitud al servlet

Para probar esta capacidad en el servidor

* [Importe e instale los recursos relacionados con este artículo mediante el administrador de paquetes.](assets/launch-agent-ui.zip)
* [Iniciar sesión en configMgr](http://localhost:4502/system/console/configMgr)
* Buscar el filtro CSRF de _Adobe Granite_
* Añadir _/content/getprintchannel_ en las rutas excluidas
* Guarde los cambios.
* [Abra POST.jsp](http://localhost:4502/apps/AEMForms/openprintchannel/POST.jsp). Asegúrese de que la cadena que se pasa a FormFieldRequestParameter sea documentId válida.(Línea 19).
* [Abra la página](http://localhost:4502/content/OpenPrintChannel.html) web, introduzca el número de cuenta y envíe el formulario.
* La interfaz de usuario del agente debe abrirse con los datos previamente rellenados específicos del número de cuenta introducido en el formulario.

>[!NOTE]
>
>Asegúrese de que el parámetro de entrada de la operación Get del Modelo de datos de formulario está enlazado al atributo Request llamado &quot;accountnumber&quot; para que funcione. Si cambia el nombre del valor de enlace a cualquier otro nombre, asegúrese de que el cambio se refleja en la línea 25 del POST.jsp

