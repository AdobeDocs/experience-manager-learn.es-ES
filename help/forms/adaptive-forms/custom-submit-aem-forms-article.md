---
title: Escritura de un envío personalizado en AEM Forms
seo-title: Escritura de un envío personalizado en AEM Forms
description: Forma rápida y sencilla de crear su propia acción de envío personalizada para el formulario adaptable
seo-description: Forma rápida y sencilla de crear su propia acción de envío personalizada para el formulario adaptable
feature: Formularios adaptables
topics: integrations
audience: developer
doc-type: article
activity: implement
version: 6.3,6.4,6.5
uuid: a26db0b9-7db4-4e80-813d-5c0438fabd1e
discoiquuid: 28611011-2ff9-477e-b654-e62e7374096a
topic: Desarrollo
role: Desarrollador
level: Con experiencia
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 3%

---


# Escritura de un envío personalizado en AEM Forms {#writing-a-custom-submit-in-aem-forms}

Forma rápida y sencilla de crear su propia acción de envío personalizada para el formulario adaptable

Este artículo le guiará por los pasos necesarios para crear una acción de envío personalizada para gestionar el envío de formularios adaptables.

* Iniciar sesión en crx
* Cree un nodo de tipo &quot;sling :folder&quot; en aplicaciones. Llamemos a este nodo CustomSubmitHelpx.
* Guarde el nodo recién creado.
* Agregue las dos propiedades siguientes al nodo recién creado
* NombrePropiedad       | Valor de propiedad
* guideComponentType | fd/af/components/guide submitype
* guideDataModel     | xfa,xsd,basic
* jcr:description   | CustomSubmitHelpx
* Guarde los cambios
* Cree un nuevo archivo llamado post.POST.jsp bajo el nodo CustomSubmitHelpx. Cuando se envía un formulario adaptable, se llama a este JSP. Puede escribir el código JSP según sus necesidades en este archivo. El siguiente código reenvía la solicitud al servlet.

```java
<%
%><%@include file="/libs/foundation/global.jsp"%>
<%@taglib prefix="cq" uri="http://www.day.com/taglibs/cq/1.0"%>
<%@ page import="org.apache.sling.api.request.RequestParameter,com.day.cq.wcm.api.WCMMode,com.adobe.forms.common.submitutils.CustomParameterRequest,com.adobe.aemds.guide.submitutils.*" %>

<%@ page import="org.apache.sling.api.request.RequestParameter,com.day.cq.wcm.api.WCMMode" %>
<%@page session="false" %>
<%

   com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/storeafsubmission",null,null);

%>
```

* Cree un archivo llamado adfields.jsp bajo el nodo CustomSubmitHelpx. Este archivo le permitirá acceder al documento firmado.
* Agregue el siguiente código a este archivo

```java
    <%@include file="/libs/fd/af/components/guidesglobal.jsp"%>

    <%@page import="org.slf4j.LoggerFactory" %>

    <%@page import="org.slf4j.Logger" %>

    <input type="hidden" id="useSignedPdf" name="_useSignedPdf" value=""/>;
```

* Guarde los cambios

Ahora empezará a ver &quot;CustomSubmitHelpx&quot; en las acciones de envío del formulario adaptable como se muestra en esta imagen.

![Formulario adaptable con envío personalizado](assets/capture-2.gif)

