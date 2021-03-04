---
title: Variables en AEM Workflow[Part1]
seo-title: Variables en AEM Workflow[Part1]
description: Uso de variables de tipo xml,json,arraylist,document en el flujo de trabajo de aem
seo-description: Uso de variables de tipo xml,json,arraylist,document en el flujo de trabajo de aem
feature: Flujo de trabajo
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
topic: Desarrollo
role: Desarrollador
level: Principiante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '441'
ht-degree: 0%

---


# Variables XML en el flujo de trabajo de AEM

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
>**AEM Forms 6.5.0** : cuando esté creando una variable de tipo XML para capturar los datos enviados en el modelo de flujo de trabajo, no asocie el XSD con la variable . Esto se debe a que al enviar el formulario adaptable basado en XSD, los datos enviados no son compatibles con el XSD. Los datos de queja XSD se incluyen en el elemento /afData/afBoundData/ .
>
>**AEM Forms 6.5.1** : si asocia XSD con la variable XML, puede examinar los elementos de esquema para realizar la asignación de variables. No se podrá acceder a los datos de formulario no enlazados a elementos de esquema. Si el caso de uso es acceder a datos enlazados a elementos de esquema así como a datos no enlazados, no enlazar el esquema con la variable XML en el flujo de trabajo. Tendrá que utilizar la expresión XPath adecuada para obtener los datos que necesite

## Creación de variables XML

>[!VIDEO](https://video.tv.adobe.com/v/26440?quality=12?autoplay=1)

### Uso del esquema con la variable XML

**Asignación de una variable XML con un esquema. Utilice esta capacidad con AEM Forms 6.5.1 en adelante**

>[!VIDEO](https://video.tv.adobe.com/v/28098?quality=9&learn=on)

#### Uso de la variable en el correo electrónico de envío

>[!VIDEO](https://video.tv.adobe.com/v/26441?quality=12&learn=on)

Para que los recursos funcionen en su sistema, siga los siguientes pasos:

* [Descargar e importar los recursos en AEM mediante el administrador de paquetes](assets/xmlandstringvariable.zip)
* [Explorar el modelo de flujo de trabajo ](http://localhost:4502/editor.html/conf/global/settings/workflow/models/vacationrequest.html) para comprender las variables que se utilizan en el flujo de trabajo
* [Configuración del servicio de correo electrónico](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [Abrir el formulario adaptable](http://localhost:4502/content/dam/formsanddocuments/applicationfortimeoff/jcr:content?wcmmode=disabled)
* Complete los detalles y envíe el formulario.

