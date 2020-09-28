---
title: Obtener parámetro de solicitud
description: Acceso al parámetro de solicitud a un servicio de cumplimentación previa del modelo de datos de formulario
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5815
thumbnail: kt-5815.jpg
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '179'
ht-degree: 0%

---

# Obtener parámetro de solicitud

## Obtener parámetro empID

El siguiente paso es acceder al parámetro empID desde la dirección URL. El valor del parámetro de solicitud empID se pasa a la operación de **_obtener_** servicio del modelo de datos de formulario.
Para este curso hemos creado y proporcionado lo siguiente

* Plantilla de formulario adaptable denominada **_FDMDemo_**
* Componente de página llamado **_fdmdemo_**
* Se ha incluido nuestro jsp personalizado con el componente de página
* Se asoció la plantilla de formulario adaptable al componente de página

Al hacer esto, nuestro código en el jsp personalizado solo se ejecutará cuando se procese un formulario adaptable basado en esta plantilla personalizada

* [Importar el paquete](assets/template-page-component.zip) mediante el administrador de [paquetes](http://localhost:4502/crx/packmgr/index.jsp)
* [Abrir fdmrequest.jsp](http://localhost:4502/crx/de/index.jsp#/apps/fdmdemo/component/page/fdmdemo/fdmrequest.jsp)
* Quite los comentarios de las líneas comentadas.
* Guardar los cambios

```java
if(request.getParameter("empID")!=null)
    {
      //System.out.println("Adobe !!!There is a empID parameter in the request "+request.getParameter("empID"));
      //java.util.Map paraMap = new java.util.HashMap();
      //paraMap.put("empID",request.getParameter("empID"));
      //slingRequest.setAttribute("paramMap",paraMap);
    }
```

El valor de empID está asociado con la clave denominada empID en paraMap. Este mapa se pasa entonces a slingRequest

>[!NOTE]
>
>La clave empID debe coincidir con el valor de enlace de todas las entidades que obtienen el servicio
