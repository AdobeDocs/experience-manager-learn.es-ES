---
title: Obtener parámetro de solicitud
description: Acceda al parámetro de solicitud al servicio de cumplimentación previa de un modelo de datos de formulario
feature: Adaptive Forms
version: 6.4,6.5
kt: 5815
thumbnail: kt-5815.jpg
topic: Development
role: Developer
level: Beginner
exl-id: a640539d-c67f-4224-ad81-dd0b62e18c79
source-git-commit: bd41cd9d64253413e793479b5ba900c8e01c0eab
workflow-type: tm+mt
source-wordcount: '190'
ht-degree: 3%

---

# Obtener parámetro de solicitud

## Obtener parámetro empID

El siguiente paso es acceder al parámetro empID desde la dirección URL. A continuación, el valor del parámetro de solicitud empID se pasa al **_get_** operación de servicio del modelo de datos de formulario.
A los efectos de este curso hemos creado y proporcionado lo siguiente

* Plantilla de formulario adaptable denominada **_FDMDemo_**
* Componente de página llamado **_fdmdemo_**
* Incluido nuestro jsp personalizado con el componente de página
* Se ha asociado la plantilla de formulario adaptable al componente de página

Al hacer esto, nuestro código en el jsp personalizado solo se ejecuta cuando se procesa el formulario adaptable basado en esta plantilla personalizada

* [Importar el paquete](assets/template-page-component.zip) using [gestor de paquetes](http://localhost:4502/crx/packmgr/index.jsp)
* [Abra fdmrequest.jsp](http://localhost:4502/crx/de/index.jsp#/apps/fdmdemo/component/page/fdmdemo/fdmrequest.jsp)
* Descomente las líneas comentadas.
* Guarde los cambios

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
>La clave empID debe coincidir con el valor de enlace de las entidades nuevas que obtienen el servicio

## Pasos siguientes

[Creación de un formulario adaptable basado en el modelo de datos de formulario](./create-adaptive-form.md)
