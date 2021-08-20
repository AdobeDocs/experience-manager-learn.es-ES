---
title: Variables en AEM flujo de trabajo[Part2]
description: Uso de variables de tipo XML, JSON, ArrayList, Document en un flujo de trabajo AEM
version: 6.5
topic: Desarrollo
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '282'
ht-degree: 0%

---

# Variables de tipo JSON en AEM flujo de trabajo

A partir de AEM Forms 6.5, ahora podemos crear variables de tipo JSON en AEM flujo de trabajo. Normalmente creará variables de tipo JSON si envía Forms adaptable basado en un esquema JSON a un flujo de trabajo AEM o si desea almacenar los resultados de una operación de invocación del modelo de datos de formulario. El siguiente vídeo le explica los pasos necesarios para crear y utilizar una variable de tipo JSON en AEM flujo de trabajo

**Si utiliza AEM Forms 6.5.0**

Cuando esté creando una variable de tipo JSON para capturar los datos enviados en el modelo de flujo de trabajo, no asocie el esquema JSON con la variable . Esto se debe a que al enviar el esquema JSON basado en el formulario adaptable, los datos enviados no son compatibles con el esquema JSON. Los datos de queja del esquema JSON se incluyen en el elemento afData.afBoundData.data .

>[!VIDEO](https://video.tv.adobe.com/v/26444?quality=12&learn=on)


**Si utiliza AEM Forms 6.5.1 o superior**

Puede asignar el esquema con la variable de tipo JSON en el modelo de flujo de trabajo. A continuación, puede utilizar el navegador de esquema para asignar los elementos de esquema a las variables de cadena/número del modelo de flujo de trabajo

>[!VIDEO](https://video.tv.adobe.com/v/28097?quality=12&learn=on)

Para que los recursos funcionen en su sistema, siga los siguientes pasos:

* [Descargar e importar los recursos en AEM mediante el administrador de paquetes](assets/jsonandstringvariable.zip)
* [Explorar el modelo de flujo de trabajo ](http://localhost:4502/editor.html/conf/global/settings/workflow/models/jsonvariable.html) para comprender las variables que se utilizan en el flujo de trabajo
* [Configuración del servicio de correo electrónico](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [Abrir el formulario adaptable](http://localhost:4502/content/dam/formsanddocuments/afbasedonjson/jcr:content?wcmmode=disabled)
* Rellene los detalles y envíe el formulario
