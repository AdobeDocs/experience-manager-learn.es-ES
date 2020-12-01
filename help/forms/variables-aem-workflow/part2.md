---
title: Variables en AEM flujo de trabajo[Parte 2]
seo-title: Variables en AEM flujo de trabajo[Parte 2]
description: Uso de variables de tipo xml,json,arraylist,documento en el flujo de trabajo de aem
seo-description: Uso de variables de tipo xml,json,arraylist,documento en el flujo de trabajo de aem
feature: workflow
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
translation-type: tm+mt
source-git-commit: ca4a8f02ea9ec5db15dbe6f322731748da90be6b
workflow-type: tm+mt
source-wordcount: '296'
ht-degree: 0%

---

# Variables de tipo JSON en AEM flujo de trabajo

A partir de AEM Forms 6.5, ahora podemos crear variables de tipo JSON en AEM flujo de trabajo. Normalmente, creará variables de tipo JSON si está enviando Forms adaptable basado en el esquema JSON a un flujo de trabajo AEM o desea almacenar los resultados de una operación de invocación del modelo de datos de formulario. El siguiente vídeo le guía por los pasos necesarios para crear y utilizar una variable de tipo JSON en AEM flujo de trabajo

**Si utiliza AEM Forms 6.5.0**

Cuando esté creando una variable de tipo JSON para capturar los datos enviados en el modelo de flujo de trabajo, no asocie el esquema JSON con la variable. Esto se debe a que al enviar el formulario adaptable basado en esquemas JSON, los datos enviados no son compatibles con el esquema JSON. Los datos de la queja de esquema JSON se incluyen en el elemento afData.afBoundData.data.

>[!VIDEO](https://video.tv.adobe.com/v/26444?quality=12&learn=on)


**Si utiliza AEM Forms 6.5.1 y posterior**

Puede asignar el esquema con la variable de tipo JSON en el modelo de flujo de trabajo. A continuación, puede utilizar el navegador de esquema para asignar los elementos de esquema a las variables de cadena y número del modelo de flujo de trabajo

>[!VIDEO](https://video.tv.adobe.com/v/28097?quality=12&learn=on)

Para que los recursos funcionen en su sistema, siga los pasos siguientes:

* [Descargar e importar recursos en AEM mediante el administrador de paquetes](assets/jsonandstringvariable.zip)
* [Explore el ](http://localhost:4502/editor.html/conf/global/settings/workflow/models/jsonvariable.html) modelo de flujo de trabajo para comprender las variables que se utilizan en el flujo de trabajo
* [Configuración del servicio de correo electrónico](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [Abrir el formulario adaptable](http://localhost:4502/content/dam/formsanddocuments/afbasedonjson/jcr:content?wcmmode=disabled)
* Rellene los detalles y envíe el formulario
