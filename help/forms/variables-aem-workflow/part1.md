---
title: Variables en AEM flujo de trabajo[Parte 1]
description: Uso de variables de tipo XML, JSON, ArrayList, Document en un flujo de trabajo AEM
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
exl-id: f9782684-3a74-4080-9680-589d3f901617
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '422'
ht-degree: 0%

---

# Variables XML en AEM flujo de trabajo

Las variables de tipo XML generalmente se utilizan cuando se tiene un formulario adaptable basado en XSD y se desea extraer valores del envío del formulario adaptable en el flujo de trabajo.

El siguiente vídeo le explica los pasos necesarios para crear variables de tipo String y XML y utilizarlas en el flujo de trabajo.

La variable XML se puede utilizar para rellenar previamente el formulario adaptable o almacenar los datos de envío del formulario adaptable en el flujo de trabajo.

Xpathing puede rellenar la variable de cadena en la variable XML. Esta variable de cadena se utiliza generalmente para rellenar los marcadores de posición de plantilla de correo electrónico en el componente Enviar correo electrónico

>[!NOTE]
>
>Si el formulario adaptable no está asociado con XSD, el XPath para obtener el valor de un elemento será como
>
>**/afData/afUnboundData/data/submitterName**

Los datos del formulario adaptable se almacenan bajo el elemento de datos como se muestra arriba. **_En el XPath submitName anterior es el nombre del campo de texto del formulario adaptable._**

>[!NOTE]
>
>**AEM Forms 6.5.0** - Cuando esté creando una variable de tipo XML para capturar los datos enviados en el modelo de flujo de trabajo, no asocie el XSD con la variable . Esto se debe a que al enviar el formulario adaptable basado en XSD, los datos enviados no son compatibles con el XSD. Los datos de queja XSD se incluyen en el elemento /afData/afBoundData/ .
>
>**AEM Forms 6.5.1** - Si asocia XSD con la variable XML, puede examinar los elementos de esquema para realizar la asignación de variables. No se podrá acceder a los datos de formulario no enlazados a elementos de esquema. Si el caso de uso es acceder a datos enlazados a elementos de esquema así como a datos no enlazados, no enlazar el esquema con la variable XML en el flujo de trabajo. Tendrá que utilizar la expresión XPath adecuada para obtener los datos que necesite

## Creación de variables XML

>[!VIDEO](https://video.tv.adobe.com/v/26440?quality=12&learn=on)

### Uso del esquema con la variable XML

**Asignación de una variable XML con un esquema. Utilice esta funcionalidad con AEM Forms 6.5.1 y versiones posteriores**

>[!VIDEO](https://video.tv.adobe.com/v/28098?quality=12&learn=on)

#### Uso de la variable en el correo electrónico de envío

>[!VIDEO](https://video.tv.adobe.com/v/26441?quality=12&learn=on)

Para que los recursos funcionen en su sistema, siga los siguientes pasos:

* [Descargar e importar los recursos en AEM mediante el administrador de paquetes](assets/xmlandstringvariable.zip)
* [Explorar el modelo de flujo de trabajo](http://localhost:4502/editor.html/conf/global/settings/workflow/models/vacationrequest.html) para comprender las variables que se utilizan en el flujo de trabajo
* [Configuración del servicio de correo electrónico](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [Abrir el formulario adaptable](http://localhost:4502/content/dam/formsanddocuments/applicationfortimeoff/jcr:content?wcmmode=disabled)
* Complete los detalles y envíe el formulario.
