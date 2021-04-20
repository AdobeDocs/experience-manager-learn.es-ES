---
title: Variables en AEM Workflow[Part2]
seo-title: Variables en AEM Workflow[Part2]
description: Uso de variables de tipo xml,json,arraylist,document en el flujo de trabajo de aem
seo-description: Uso de variables de tipo xml,json,arraylist,document en el flujo de trabajo de aem
feature: Workflow
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '300'
ht-degree: 1%

---

# Variables de tipo JSON en el flujo de trabajo de AEM

A partir de AEM Forms 6.5, ahora podemos crear variables de tipo JSON en AEM Workflow. Normalmente creará variables de tipo JSON si envía formularios adaptables basados en el esquema JSON a un flujo de trabajo de AEM o si desea almacenar los resultados de una operación de invocación del modelo de datos de formulario. El siguiente vídeo le explica los pasos necesarios para crear y utilizar una variable de tipo JSON en el flujo de trabajo de AEM

**Si utiliza AEM Forms 6.5.0**

Cuando esté creando una variable de tipo JSON para capturar los datos enviados en el modelo de flujo de trabajo, no asocie el esquema JSON con la variable . Esto se debe a que al enviar el esquema JSON basado en el formulario adaptable, los datos enviados no son compatibles con el esquema JSON. Los datos de queja del esquema JSON se incluyen en el elemento afData.afBoundData.data .

>[!VIDEO](https://video.tv.adobe.com/v/26444?quality=12&learn=on)


**Si utiliza AEM Forms 6.5.1 y versiones posteriores**

Puede asignar el esquema con la variable de tipo JSON en el modelo de flujo de trabajo. A continuación, puede utilizar el navegador de esquema para asignar los elementos de esquema a las variables de cadena/número del modelo de flujo de trabajo

>[!VIDEO](https://video.tv.adobe.com/v/28097?quality=12&learn=on)

Para que los recursos funcionen en su sistema, siga los siguientes pasos:

* [Descargar e importar los recursos en AEM mediante el administrador de paquetes](assets/jsonandstringvariable.zip)
* [Explorar el modelo de flujo de trabajo ](http://localhost:4502/editor.html/conf/global/settings/workflow/models/jsonvariable.html) para comprender las variables que se utilizan en el flujo de trabajo
* [Configuración del servicio de correo electrónico](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [Abrir el formulario adaptable](http://localhost:4502/content/dam/formsanddocuments/afbasedonjson/jcr:content?wcmmode=disabled)
* Rellene los detalles y envíe el formulario
