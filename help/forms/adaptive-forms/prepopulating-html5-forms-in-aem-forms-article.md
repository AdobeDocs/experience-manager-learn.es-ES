---
title: Rellene previamente HTML5 Forms mediante el atributo de datos.
seo-title: Rellene previamente HTML5 Forms mediante el atributo de datos.
description: Rellene formularios HTML5 recuperando datos del origen back-end.
seo-description: Rellene formularios HTML5 recuperando datos del origen back-end.
feature: integrations
topics: mobile-forms
audience: developer
doc-type: article
activity: implement
version: 6.3,6.4,6.5.
uuid: 889d2cd5-fcf2-4854-928b-0c2c0db9dbc2
discoiquuid: 3aa645c9-941e-4b27-a538-cca13574b21c
translation-type: tm+mt
source-git-commit: 1e615d1c51fa0c4c0db335607c29a8c284874c8d
workflow-type: tm+mt
source-wordcount: '510'
ht-degree: 0%

---


# Rellenar previamente HTML5 Forms mediante el atributo de datos {#prepopulate-html-forms-using-data-attribute}

Visite la página de ejemplos [de](https://forms.enablementadobe.com/content/samples/samples.html?query=0) AEM Forms para ver un vínculo a una demostración en directo de esta capacidad.

Las plantillas XDP procesadas en formato HTML con AEM Forms se denominan HTML5 o Forms móvil. Un caso de uso común es rellenar previamente estos formularios cuando se procesan.

Existen dos formas de combinar datos con la plantilla xdp cuando se procesa como HTML.

**dataRef**: Puede utilizar el parámetro dataRef en la dirección URL. Este parámetro especifica la ruta absoluta del archivo de datos que se combina con la plantilla. Este parámetro puede ser una URL a un servicio de descanso que devuelve los datos en formato XML.

**data**: Este parámetro especifica los bytes de datos codificados UTF-8 que se combinan con la plantilla. Si se especifica este parámetro, el formulario HTML5 omite el parámetro dataRef. Como práctica recomendada, recomendamos utilizar el enfoque de datos.

El método recomendado es establecer el atributo de datos en la solicitud con los datos con los que se desea rellenar previamente el formulario.

slingRequest.setAttribute(&quot;data&quot;, content);

En este ejemplo, estamos configurando el atributo de datos con el contenido. El contenido representa los datos con los que se desea rellenar previamente el formulario. Normalmente, se obtiene el &quot;contenido&quot; mediante una llamada REST a un servicio interno.

Para lograr este caso de uso debe crear un perfil personalizado. Los detalles sobre la creación de perfiles personalizados están claramente documentados en la documentación de [AEM Forms aquí](https://helpx.adobe.com/aem-forms/6/html5-forms/custom-profile.html).

Una vez creado el perfil personalizado, creará un archivo JSP que recuperará los datos haciendo llamadas al sistema back-end. Una vez que se hayan recuperado los datos, se utilizará slingRequest.setAttribute(&quot;data&quot;, contenido); para rellenar previamente el formulario

Cuando se procesa el XDP, también puede pasar algunos parámetros al xdp y, en función del valor del parámetro, puede recuperar los datos del sistema back-end.

[Por ejemplo: esta dirección URL tiene el parámetro name](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=john)

El JSP que escriba tendrá acceso al parámetro name a través de request.getParameter(&quot;name&quot;). A continuación, puede pasar el valor de este parámetro al proceso de back-end para recuperar los datos necesarios.
Para que esta capacidad funcione en su sistema, siga los pasos que se mencionan a continuación:

* [Descargar e importar los recursos en AEM mediante el administrador de paquetes](assets/prepopulatemobileform.zip)El paquete instalará lo siguiente

   * CustomProfile
   * XDP de muestra
   * Extremo de POST de ejemplo que devolverá datos para rellenar el formulario

>[!NOTE]
>
>Si desea rellenar el formulario llamando al proceso del área de trabajo, es posible que desee incluir callWorkbenchProcess.jsp en el archivo /apps/AEMFormsDemoListings/customprofiles/PrepopulateForm/html.jsp en lugar del archivo setdata.jsp

* [Apunta tu navegador favorito a esta dirección URL](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=Adobe%20Systems). El formulario debe rellenarse previamente con el valor del parámetro name
