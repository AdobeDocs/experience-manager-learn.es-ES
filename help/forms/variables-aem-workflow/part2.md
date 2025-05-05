---
title: Variables en el flujo de trabajo de AEM [Parte 2]
description: Uso de variables de tipo XML, JSON, ArrayList o Document en un flujo de trabajo de AEM
version: Experience Manager 6.5
topic: Development
feature: Adaptive Forms, Workflow
role: Developer
level: Beginner
exl-id: e7d3e0be-5194-47c2-a668-ce78e727986e
duration: 354
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '262'
ht-degree: 0%

---

# Variables de tipo JSON en el flujo de trabajo de AEM

A partir de AEM Forms 6.5, ahora podemos crear variables de tipo JSON en AEM Workflow. Normalmente, creará variables de tipo JSON si envía un Forms adaptable basado en el esquema JSON a un flujo de trabajo de AEM o si desea almacenar los resultados de una operación de invocación del modelo de datos de formulario. El siguiente vídeo le guía por los pasos necesarios para crear y utilizar una variable de tipo JSON en el flujo de trabajo de AEM

**Si usa AEM Forms 6.5.0**

Cuando esté creando una variable de tipo JSON para capturar los datos enviados en el modelo de flujo de trabajo, no asocie el esquema JSON a la variable. Esto se debe a que cuando envía un formulario adaptable basado en un esquema JSON, los datos enviados no son compatibles con el esquema JSON. Los datos de queja del esquema JSON se incluyen en el elemento afData.afBoundData.data.

>[!VIDEO](https://video.tv.adobe.com/v/26444?quality=12&learn=on)


**Si usa AEM Forms 6.5.1 y versiones posteriores**

Puede asignar el esquema con la variable de tipo JSON en el modelo de flujo de trabajo. A continuación, puede utilizar el explorador de esquemas para asignar los elementos de esquema con las variables de cadena/número del modelo de flujo de trabajo

>[!VIDEO](https://video.tv.adobe.com/v/35331?quality=12&learn=on&captions=spa)

Para que los recursos funcionen en el sistema, siga estos pasos:

* [Descargar e importar los recursos en AEM mediante el administrador de paquetes](assets/jsonandstringvariable.zip)
* [Explore el modelo de flujo de trabajo](http://localhost:4502/editor.html/conf/global/settings/workflow/models/jsonvariable.html) para comprender las variables que se utilizan en el flujo de trabajo
* [Configurar el servicio de correo electrónico](https://helpx.adobe.com/es/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [Abrir el formulario adaptable](http://localhost:4502/content/dam/formsanddocuments/afbasedonjson/jcr:content?wcmmode=disabled)
* Rellene los detalles y envíe el formulario
