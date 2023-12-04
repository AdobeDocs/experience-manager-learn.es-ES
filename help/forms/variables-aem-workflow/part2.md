---
title: AEM Variables en flujo de trabajo de la[Parte2]
description: AEM Uso de variables de tipo XML, JSON, ArrayList o Document en un flujo de trabajo de
version: 6.5
topic: Development
feature: Adaptive Forms, Workflow
role: Developer
level: Beginner
exl-id: e7d3e0be-5194-47c2-a668-ce78e727986e
duration: 376
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '262'
ht-degree: 0%

---

# AEM Variables de tipo JSON en el flujo de trabajo de

A partir de AEM Forms AEM 6.5, ahora podemos crear variables de tipo JSON en el flujo de trabajo de la. Normalmente, creará variables de tipo JSON si envía un Forms AEM adaptable basado en el esquema JSON a un flujo de trabajo de o si desea almacenar los resultados de una operación de invocación del modelo de datos de formulario. AEM El siguiente vídeo le guía por los pasos necesarios para crear y utilizar una variable de tipo JSON en el flujo de trabajo de la

**Si se usa AEM Forms 6.5.0**

Cuando esté creando una variable de tipo JSON para capturar los datos enviados en el modelo de flujo de trabajo, no asocie el esquema JSON a la variable. Esto se debe a que cuando envía un formulario adaptable basado en un esquema JSON, los datos enviados no son compatibles con el esquema JSON. Los datos de queja del esquema JSON se incluyen en el elemento afData.afBoundData.data.

>[!VIDEO](https://video.tv.adobe.com/v/26444?quality=12&learn=on)


**Si utiliza AEM Forms 6.5.1 y superior**

Puede asignar el esquema con la variable de tipo JSON en el modelo de flujo de trabajo. A continuación, puede utilizar el explorador de esquemas para asignar los elementos de esquema con las variables de cadena/número del modelo de flujo de trabajo

>[!VIDEO](https://video.tv.adobe.com/v/28097?quality=12&learn=on)

Para que los recursos funcionen en el sistema, siga estos pasos:

* [AEM Descargar e importar los recursos en el administrador de paquetes mediante el uso de un administrador de paquetes](assets/jsonandstringvariable.zip)
* [Exploración del modelo de flujo de trabajo](http://localhost:4502/editor.html/conf/global/settings/workflow/models/jsonvariable.html) para comprender las variables que se utilizan en el flujo de trabajo
* [Configuración del servicio de correo electrónico](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [Abra el formulario adaptable](http://localhost:4502/content/dam/formsanddocuments/afbasedonjson/jcr:content?wcmmode=disabled)
* Rellene los detalles y envíe el formulario
