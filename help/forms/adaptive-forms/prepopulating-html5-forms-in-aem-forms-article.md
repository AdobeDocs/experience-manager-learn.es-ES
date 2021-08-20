---
title: Rellene previamente HTML5 Forms mediante el atributo data .
description: Rellene formularios HTML5 recuperando datos del origen del servidor.
feature: Formularios adaptables
version: 6.3,6.4,6.5.
topic: Desarrollo
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '497'
ht-degree: 0%

---


# Rellenar previamente HTML5 Forms mediante un atributo de datos {#prepopulate-html-forms-using-data-attribute}

Visite la página [AEM Forms samples](https://forms.enablementadobe.com/content/samples/samples.html?query=0) para ver un vínculo a una demostración en directo de esta capacidad.

Las plantillas XDP procesadas en formato HTML mediante AEM Forms se denominan HTML5 o Forms móvil. Un caso de uso común es rellenar previamente estos formularios cuando se están procesando.

Existen dos maneras de combinar datos con la plantilla xdp cuando se está procesando como HTML.

**dataRef**: Puede utilizar el parámetro dataRef en la dirección URL. Este parámetro especifica la ruta absoluta del archivo de datos que se combina con la plantilla. Este parámetro puede ser una URL a un servicio de descanso que devuelva los datos en formato XML.

**datos**: Este parámetro especifica los bytes de datos codificados UTF-8 que se combinan con la plantilla. Si se especifica este parámetro, el formulario HTML5 ignora el parámetro dataRef. Como práctica recomendada, se recomienda utilizar el método de los datos.

El método recomendado es establecer el atributo de datos en la solicitud con los datos con los que desea rellenar previamente el formulario.

slingRequest.setAttribute(&quot;data&quot;, contenido);

En este ejemplo, se configura el atributo de datos con el contenido. El contenido representa los datos con los que desea rellenar previamente el formulario. Normalmente, el &quot;contenido&quot; se recuperaría realizando una llamada REST a un servicio interno.

Para lograr este caso de uso, debe crear un perfil personalizado. Los detalles sobre la creación de perfiles personalizados están claramente documentados en la [documentación de AEM Forms aquí](https://helpx.adobe.com/aem-forms/6/html5-forms/custom-profile.html).

Una vez que cree su perfil personalizado, creará un archivo JSP que recuperará los datos realizando llamadas a su sistema back-end. Una vez recuperados los datos, utilizará slingRequest.setAttribute(&quot;data&quot;, content); rellenar previamente el formulario

Cuando se procesa el XDP, también se pueden pasar algunos parámetros al xdp y, según el valor del parámetro, se pueden recuperar los datos del sistema back-end.

[Por ejemplo, esta dirección URL tiene el parámetro name](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=john)

El JSP que escriba tendrá acceso al parámetro name a través de request.getParameter(&quot;name&quot;) . A continuación, puede pasar el valor de este parámetro al proceso backend para recuperar los datos necesarios.
Para que esta capacidad funcione en su sistema, siga los pasos que se indican a continuación:

* [Descargue e importe los recursos en AEM mediante el gestor de ](assets/prepopulatemobileform.zip)
paquetesEl paquete instalará lo siguiente

   * Perfil personalizado
   * XDP de muestra
   * Punto final del POST de ejemplo que devolverá datos para rellenar el formulario

>[!NOTE]
>
>Si desea rellenar el formulario llamando al proceso del área de trabajo, puede que desee incluir callWorkbenchProcess.jsp en su /apps/AEMFormsDemoListings/customprofiles/PrepopulateForm/html.jsp en lugar de setdata.jsp

* [Apunte su navegador favorito a esta dirección URL](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=Adobe%20Systems). El formulario debe rellenarse previamente con el valor del parámetro name
