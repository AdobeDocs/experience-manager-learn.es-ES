---
title: Crear nuevo componente de página
description: Crear un nuevo componente de página
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
kt: 13717
source-git-commit: 52c8d96a03b4d6e4f2a0a3c92f4307203e236417
workflow-type: tm+mt
source-wordcount: '273'
ht-degree: 10%

---


# Componente Página 

Un componente de página es un componente normal responsable de procesar una página. Vamos a crear un nuevo componente de página y asociaremos este componente de página con una nueva plantilla de formulario adaptable. Esto garantiza que el código solo se ejecutará cuando un formulario adaptable se base en esta plantilla en particular.

## Crear componente de página

Inicie sesión en la instancia local de AEM Forms lista para la nube. Cree la siguiente estructura en la carpeta de aplicaciones
![página-componente](./assets/page-component1.png)

1. Haga clic con el botón derecho en la carpeta de página y cree un nodo llamado storeAndFetch de tipo cq:Component
1. Guarde los cambios
1. Añada las siguientes propiedades a `storeandfetch` nodo y guardar

| **Nombre de propiedad** | **Tipo de propiedad** | **Valor de propiedad** |
|-------------------------|-------------------|----------------------------------------|
| componentGroup | Cadena | oculto |
| jcr:description | Cadena | Tipo de página de plantilla de formulario adaptable |
| jcr:title | Cadena | Página de plantilla de formulario adaptable |
| sling:resourceSuperType | Cadena | `fd/af/components/page2/aftemplatedpage` |

Copie el `/libs/fd/af/components/page2/aftemplatedpage/aftemplatedpage.jsp` y péguelo en la etiqueta `storeandfetch` nodo. Cambie el nombre del `aftemplatedpage.jsp` hasta `storeandfetch.jsp`.

Abrir `storeandfetch.jsp` y añada la línea siguiente:

```jsp
<cq:include script="azureportal.jsp"/>
```

en el

```jsp
<cq:include script="fallbackLibrary.jsp"/>
```

El código final debería ser el siguiente

```jsp
<cq:include script="fallbackLibrary.jsp"/>
<cq:include script="azureportal.jsp"/>
```

Cree un archivo llamado azureportal.jsp en el nodo storeandfetch, copie el siguiente código en azureportal.jsp y guarde los cambios

```jsp
<%@page session="false" %>
<%@include file="/libs/fd/af/components/guidesglobal.jsp" %>
<%@ page import="org.apache.commons.logging.Log" %>
<%@ page import="org.apache.commons.logging.LogFactory" %>
<%
    if(request.getParameter("guid")!=null) {
            logger.debug( "Got Guid in the request" );
            String BlobId = request.getParameter("guid");
            java.util.Map paraMap = new java.util.HashMap();
            paraMap.put("BlobId",BlobId);
            slingRequest.setAttribute("paramMap",paraMap);
    } else {
            logger.debug( "There is no Guid in the request " );
    }            
%>
```

En este código se obtiene el valor del parámetro de solicitud **guid** y almacenarlo en una variable llamada BlobId. A continuación, este BlobId se pasa a la solicitud de sling mediante el atributo paramMap. Para que este código funcione, se da por hecho que tiene un formulario basado en un modelo de datos de formulario respaldado por un almacenamiento de Azure y el servicio de lectura del modelo de datos de formulario está enlazado a un atributo de solicitud denominado BlobId, como se muestra en la captura de pantalla siguiente.

![fdm-request-attribute](./assets/fdm-request-attribute.png)

### Pasos siguientes

[Asocie el componente de página con la plantilla](./associate-page-component.md)

