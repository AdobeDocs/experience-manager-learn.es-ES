---
title: Variables en AEM flujo de trabajo[Parte 1]
seo-title: Variables en AEM flujo de trabajo[Parte 1]
description: Uso de variables de tipo xml,json,arraylist,documento en el flujo de trabajo de aem
seo-description: Uso de variables de tipo xml,json,arraylist,documento en el flujo de trabajo de aem
feature: workflow
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '437'
ht-degree: 0%

---


# Variables XML en AEM flujo de trabajo

Las variables de tipo XML se suelen utilizar cuando se tiene un formulario adaptable basado en XSD y se desea extraer valores del envío del formulario adaptable en el flujo de trabajo.

El siguiente vídeo le guía por los pasos necesarios para crear variables de tipo String y XML y utilizarlas en el flujo de trabajo.

La variable XML puede utilizarse para rellenar previamente el formulario adaptable o almacenar los datos de envío del formulario adaptable en el flujo de trabajo.

Xpathing puede rellenar la variable de cadena en la variable XML. Esta variable de cadena se suele utilizar para rellenar los marcadores de posición de la plantilla de correo electrónico en el componente Enviar correo electrónico

>[!NOTE]
Si el formulario adaptable no está asociado con XSD, el XPath para obtener el valor de un elemento tendrá este aspecto**:**

Los datos del formulario adaptable se almacenan bajo el elemento de datos como se muestra arriba. **_En el XPath submitName anterior es el nombre del campo de texto en el formulario adaptable._**

>[!NOTE]
**AEM Forms 6.5.0** : cuando esté creando una variable de tipo XML para capturar los datos enviados en el modelo de flujo de trabajo, no asocie el XSD con la variable. Esto se debe a que al enviar el formulario adaptable basado en XSD, los datos enviados no son compatibles con el XSD. Los datos de la queja XSD se incluyen en /afData/afBoundData/ element.

**AEM Forms 6.5.1** - Si asocia XSD con la variable XML, puede explorar los elementos de esquema para realizar la asignación de variables. No podrá acceder a los datos de formulario que no estén enlazados a elementos de esquema. Si el caso de uso es para acceder a datos enlazados a elementos de esquema así como a datos no enlazados, no enlazar el esquema con la variable XML en el flujo de trabajo.Tendrá que utilizar la expresión XPath adecuada para llegar a los datos que necesita

## Creación de variables XML

>[!VIDEO](https://video.tv.adobe.com/v/26440?quality=12?autoplay=1)

### Uso de Esquema con variable XML

**Asignación de una variable XML con esquema. Utilice esta función con AEM Forms 6.5.1 y versiones posteriores**
>[!VIDEO](https://video.tv.adobe.com/v/28098?quality=9&learn=on)

#### Uso de la variable en el correo electrónico de envío

>[!VIDEO](https://video.tv.adobe.com/v/26441?quality=12&learn=on)

Para que los recursos funcionen en su sistema, siga los pasos siguientes:

* [Descargar e importar recursos en AEM mediante el administrador de paquetes](assets/xmlandstringvariable.zip)
* [Explore el modelo](http://localhost:4502/editor.html/conf/global/settings/workflow/models/vacationrequest.html) de flujo de trabajo para comprender las variables que se utilizan en el flujo de trabajo
* [Configuración del servicio de correo electrónico](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [Abrir el formulario adaptable](http://localhost:4502/content/dam/formsanddocuments/applicationfortimeoff/jcr:content?wcmmode=disabled)
* Rellene los detalles y envíe el formulario.

