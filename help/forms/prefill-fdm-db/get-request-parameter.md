---
title: Obtener parámetro de solicitud
description: Acceda al parámetro de solicitud del servicio de relleno previo de un modelo de datos de formulario
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-5815
thumbnail: kt-5815.jpg
topic: Development
role: Developer
level: Beginner
exl-id: a640539d-c67f-4224-ad81-dd0b62e18c79
duration: 40
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '176'
ht-degree: 1%

---

# Obtener parámetro de solicitud

## Obtener parámetro empID

El siguiente paso es acceder al parámetro empID desde la dirección URL. A continuación, se pasa el valor del parámetro de solicitud empID a la operación de servicio **_get_** del modelo de datos de formulario.
Para el propósito de este curso hemos creado y proporcionado lo siguiente

* Plantilla de formulario adaptable llamada **_FDMDemo_**
* Componente de página llamado **_fdmdemo_**
* Se ha incluido nuestro jsp personalizado con el componente de página
* Se ha asociado la plantilla de formulario adaptable con el componente de página

Al hacer esto, el código en el jsp personalizado solo se ejecutará cuando se procese el formulario adaptable basado en esta plantilla personalizada

* [Importe el paquete](assets/template-page-component.zip) con [administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp)
* [Abrir fdmrequest.jsp](http://localhost:4502/crx/de/index.jsp#/apps/fdmdemo/component/page/fdmdemo/fdmrequest.jsp)
* Quitar comentarios de las líneas comentadas.
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

El valor de empID está asociado con una clave denominada empID en paraMap. Este mapa se pasa a slingRequest

>[!NOTE]
>
>La clave empID debe coincidir con el valor de enlace de las nuevas entidades que reciben el servicio

## Siguientes pasos

[Crear un formulario adaptable basado en el modelo de datos de formulario](./create-adaptive-form.md)
