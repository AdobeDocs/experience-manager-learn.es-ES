---
title: AEM Variables en el flujo de trabajo de la[Part1]
description: AEM Uso de variables de tipo XML, JSON, ArrayList o Document en un flujo de trabajo de
feature: Adaptive Forms, Workflow
version: 6.5
topic: Development
role: Developer
level: Beginner
exl-id: f9782684-3a74-4080-9680-589d3f901617
duration: 570
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '404'
ht-degree: 0%

---

# AEM Variables XML en flujo de trabajo de

Las variables de tipo XML se utilizan normalmente cuando tiene un formulario adaptable basado en XSD y desea extraer valores del envío del formulario adaptable en el flujo de trabajo.

El siguiente vídeo le guía por los pasos necesarios para crear variables de tipo Cadena y XML y utilizarlas en el flujo de trabajo.

La variable XML se puede utilizar para rellenar previamente el formulario adaptable o almacenar los datos de envío del formulario adaptable en el flujo de trabajo.

Xpathing puede rellenar la variable de cadena en la variable XML. Esta variable de cadena se utiliza normalmente para rellenar los marcadores de posición de la plantilla de correo electrónico en el componente Enviar correo electrónico

>[!NOTE]
>
>Si el formulario adaptable no está asociado con XSD, la sintaxis XPath para obtener el valor de un elemento será la siguiente
>
>**/afData/afUnboundData/data/submitName**

Los datos del formulario adaptable se almacenan en el elemento de datos como se muestra arriba. **_En el XPath anterior submitterName es el nombre del campo de texto en el formulario adaptable._**

>[!NOTE]
>
>**AEM Forms 6.5.0** : Cuando esté creando una variable de tipo XML para capturar los datos enviados en el modelo de flujo de trabajo, no asocie el XSD con la variable. Esto se debe a que cuando envía un formulario adaptable basado en XSD, los datos enviados no son compatibles con el XSD. Los datos de quejas XSD se incluyen en el elemento /afData/afBoundData/.
>
>**AEM Forms 6.5.1** : Si asocia XSD con la variable XML, puede examinar los elementos de esquema para realizar la asignación de variables. No podrá acceder a los datos del formulario no enlazados a los elementos del esquema. Si su caso de uso es acceder a datos enlazados a elementos de esquema, así como a datos no enlazados, no enlace el esquema con la variable XML en el flujo de trabajo. Tendrá que utilizar la expresión XPath adecuada para llegar a los datos que necesite

## Crear variables XML

>[!VIDEO](https://video.tv.adobe.com/v/26440?quality=12&learn=on)

### Usar el esquema con la variable XML

**Asignar una variable XML al esquema. Utilice esta capacidad con AEM Forms 6.5.1 en adelante**

>[!VIDEO](https://video.tv.adobe.com/v/28098?quality=12&learn=on)

#### Uso de la variable en el correo electrónico de envío

>[!VIDEO](https://video.tv.adobe.com/v/26441?quality=12&learn=on)

Para que los recursos funcionen en el sistema, siga estos pasos:

* [AEM Descargar e importar los recursos en el administrador de paquetes mediante el uso de un administrador de paquetes](assets/xmlandstringvariable.zip)
* [Exploración del modelo de flujo de trabajo](http://localhost:4502/editor.html/conf/global/settings/workflow/models/vacationrequest.html) para comprender las variables que se utilizan en el flujo de trabajo
* [Configuración del servicio de correo electrónico](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [Abra el formulario adaptable](http://localhost:4502/content/dam/formsanddocuments/applicationfortimeoff/jcr:content?wcmmode=disabled)
* Complete los detalles y envíe el formulario.
