---
title: Rellenar previamente HTML5 Forms mediante el atributo de datos.
description: Rellene formularios de HTML 5 recuperando datos del origen del servidor.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: ab0f5282-383b-4be6-9c57-cded6ab37528
last-substantial-update: 2020-01-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '471'
ht-degree: 2%

---

# Rellenar previamente Forms de HTML5 mediante el atributo de datos {#prepopulate-html-forms-using-data-attribute}


Las plantillas XDP procesadas en formato de HTML mediante AEM Forms se denominan HTML5 o Mobile Forms. Un caso de uso común es rellenar previamente estos formularios cuando se están procesando.

Existen dos formas de combinar los datos con la plantilla xdp cuando se representan como HTML.

**dataRef**: Puede utilizar el parámetro dataRef en la dirección URL. Este parámetro especifica la ruta absoluta del archivo de datos que se combina con la plantilla. Este parámetro puede ser una URL a un servicio REST que devuelva los datos en formato XML.

**datos**: Este parámetro especifica los bytes de datos codificados UTF-8 que se combinan con la plantilla. Si se especifica este parámetro, el formulario HTML5 ignorará el parámetro dataRef. Como práctica recomendada, recomendamos utilizar el enfoque de datos.

El método recomendado es establecer el atributo de datos en la solicitud con los datos con los que desea rellenar previamente el formulario.

slingRequest.setAttribute(&quot;data&quot;, contenido);

En este ejemplo, configuramos el atributo de datos con el contenido. El contenido representa los datos con los que desea rellenar previamente el formulario. Normalmente, recuperaría el &quot;contenido&quot; realizando una llamada de REST a un servicio interno.

Para aplicar este caso de uso, debe crear un perfil personalizado. Los detalles sobre la creación de perfiles personalizados se documentan claramente en [Documentación de AEM Forms aquí](https://helpx.adobe.com/aem-forms/6/html5-forms/custom-profile.html).

Una vez creado el perfil personalizado, creará un archivo JSP que recuperará los datos realizando llamadas a su sistema back-end. Una vez recuperados los datos, utilizará slingRequest.setAttribute(&quot;data&quot;, content); para rellenar previamente el formulario

Cuando se procesa el XDP, también puede pasar algunos parámetros al xdp y, en función del valor del parámetro, puede recuperar los datos del sistema backend.

[Por ejemplo, esta URL tiene un parámetro de nombre](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=john)

El JSP que escriba tendrá acceso al parámetro name mediante request.getParameter(&quot;name&quot;) A continuación, puede pasar el valor de este parámetro al proceso backend para recuperar los datos necesarios.
Para que esta capacidad funcione en su sistema, siga los pasos que se mencionan a continuación:

* [AEM Descarga e importación de recursos en el administrador de paquetes mediante el uso de un administrador de paquetes](assets/prepopulatemobileform.zip)
El paquete instalará lo siguiente

   * CustomProfile
   * XDP de muestra
   * Extremo de POST de muestra que devolverá datos para rellenar el formulario

>[!NOTE]
>
>Si desea rellenar el formulario llamando al proceso de Workbench, es posible que desee incluir callWorkbenchProcess.jsp en su /apps/AEMFormsDemoListings/customprofiles/PrepopulateForm/html.jsp en lugar de setdata.jsp

* [Apunta tu navegador favorito a esta URL](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=Adobe%20Systems). El formulario debe rellenarse previamente con el valor del parámetro name
