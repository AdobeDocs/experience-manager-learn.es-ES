---
title: Rellene Forms adaptable utilizando parámetros de consulta.
description: Rellene Forms adaptable con datos de parámetros de consulta.
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Experienced
kt: 11470
last-substantial-update: 2020-11-12T00:00:00Z
source-git-commit: fad7630d2d91d03b98a3982f73a689ef48700319
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 0%

---

# Rellenar previamente Forms adaptable utilizando parámetros de consulta

Uno de nuestros clientes tenía el requisito de rellenar el formulario adaptable utilizando los parámetros de consulta. Por ejemplo, en la siguiente dirección URL, los campos Nombre y Apellido del formulario adaptable se establecen en John y Doe respectivamente

```html
https://forms.enablementadobe.com/content/forms/af/testingxml.html?FirstName=John&LastName=Doe
```

Para lograr este caso de uso, se creó una nueva plantilla de formulario adaptable y se asoció a un componente de página. En este componente de página tenemos un jsp para obtener los parámetros de consulta y crear una estructura xml que se pueda utilizar para rellenar el formulario adaptable.

Los detalles sobre la creación de una nueva plantilla de formulario adaptable y un componente de página son [explicado en este vídeo.](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/storing-and-retrieving-form-data/part5.html?lang=en)

El siguiente es el código que se utilizó en la página jsp

```java
java.util.Enumeration enumeration = request.getParameterNames();
String dataXml = "<afData><afUnboundData><data>";
while (enumeration.hasMoreElements())
{
   String parameterName = (String) enumeration.nextElement();
   dataXml = dataXml + "<" + parameterName + ">" + request.getParameter(parameterName) + "</" + parameterName + ">";

}

dataXml = dataXml + "</data></afUnboundData></afData>";
//System.out.println("The data xml is "+dataXml);
slingRequest.setAttribute("data", dataXml);
```

>[!NOTE]
>
>Si el formulario utiliza un esquema, la estructura del xml será diferente y deberá crear el xml en consecuencia.


## Implementar los recursos en el sistema

* [Descargar e instalar la plantilla de formulario adaptable mediante el Administrador de paquetes](assets/populate-with-xml.zip)
* [Descargue e instale el formulario adaptable de ejemplo](assets/populate-af-with-query-paramters-form.zip)

* [Vista previa del formulario adaptable](http://localhost:4502/content/dam/formsanddocuments/testingxml/jcr:content?wcmmode=disabled&amp;FirstName=John&amp;LastName=Doe)
Debería ver el formulario adaptable rellenado con el valor John y Doe
