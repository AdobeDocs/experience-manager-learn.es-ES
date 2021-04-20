---
title: Uso de las API de geolocalización en formularios adaptables
seo-title: Uso de las API de geolocalización en formularios adaptables
description: Rellene los campos de dirección del formulario con la api de geolocalización
seo-description: Rellene los campos de dirección del formulario con la api de geolocalización
uuid: 5a461659-6873-4ea1-9f37-8296e5a9d895
feature: Adaptive Forms
topics: integrations
audience: developer
doc-type: article
activity: develop
version: 6.3,6.4,6.5
discoiquuid: 3400251b-aee0-4d69-994b-e1643fabc868
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '434'
ht-degree: 1%

---


# Uso de las API de geolocalización en formularios adaptables{#using-geolocation-api-s-in-adaptive-forms}

Visite la página [AEM Forms samples](https://forms.enablementadobe.com/content/samples/samples.html?query=0) para ver un vínculo a una demostración en directo de esta capacidad.

En este artículo analizaremos el uso de la API de geolocalización de Google para rellenar los campos de un formulario adaptable. Este caso de uso se utiliza comúnmente cuando desea rellenar los campos de dirección actuales en un formulario.

Se siguieron los siguientes pasos para utilizar la API de geolocalización en formularios adaptables.

1. [Obtenga ](https://developers.google.com/maps/documentation/javascript/get-api-key) clave de API de Google para utilizar la plataforma Google Maps. Puede obtener una clave de prueba que sea válida durante 1 año.

1. El fragmento de formulario adaptable se creó con campos para contener la dirección actual

1. Se invocó la API de geolocalización en el suceso click del objeto image del formulario adaptable

1. Los datos JSON devueltos por la llamada de API se analizaron y los valores de los campos del formulario adaptable se establecieron en consecuencia.

```javascript
navigator.geolocation.getCurrentPosition(showPosition);
function showPosition(position) 
{
console.log(" I am inside the showPosition in fragment");
console.log("Latitude: " + position.coords.latitude + "Longitude " + position.coords.longitude);
var url = "https://maps.googleapis.com/maps/api/geocode/json?latlng="+position.coords.latitude+","+position.coords.longitude+"&key=<your_api_key>";
  console.log(url);
  
  $.getJSON(url,function (data, textStatus){
    
    var location=data.results[0].formatted_address;
    console.log(location);
    
    for(i=0;i<data.results[0].address_components.length;i++)
        {
          if(data.results[0].address_components[i].types[0] == "street_number")
            {
              streetNumber.value = data.results[0].address_components[i].long_name;
            }
          if(data.results[0].address_components[i].types[0] == "route")
            {
              streetName.value = data.results[0].address_components[i].long_name;
            }
            if(data.results[0].address_components[i].types[0] == "postal_code")
            {
              
              zipCode.value = data.results[0].address_components[i].long_name;
            }
            if(data.results[0].address_components[i].types[0] == "locality")
            {
              
              city.value = data.results[0].address_components[i].long_name;
            }
          if(data.results[0].address_components[i].types[0] == "administrative_area_level_1")
            {
              
              state.value = data.results[0].address_components[i].long_name;
            }
        }
    
  });
}
```

![Campos rellenados con la api de geoloacción](assets/capture-4.gif)

En la línea 1 utilizamos la API de geolocalización HTML para obtener la ubicación actual. Una vez obtenida la ubicación actual, pasamos la ubicación actual a la función showPosition.

En la función showPosition, se utiliza la API de Google para recuperar los detalles de dirección de la latitud y longitud dadas.

A continuación, se analiza el JSON devuelto por la API para establecer los campos del formulario adaptable.

>[!NOTE]
>
>Para realizar pruebas, puede utilizar el protocolo HTTP con localhost en la dirección URL.
>
>Para el servidor de producción, deberá habilitar SSL para el servidor AEM para obtener esta capacidad.
>
>La muestra asociada a este artículo se ha probado con la dirección de EE. UU. Si desea utilizar esta capacidad en otras ubicaciones geográficas, es posible que tenga que modificar el análisis de JSON.

Para obtener esta capacidad en su servidor, siga los siguientes pasos

* Instale e inicie el servidor de AEM Forms.

>!![NOTE] Esta funcionalidad se probó en AEM Forms 6.3 y posterior
* [Obtener clave de API de Google](https://developers.google.com/maps/documentation/javascript/get-api-key).
* [Importe los recursos relacionados con este artículo en AEM.](assets/geolocationapi.zip)
* [Abra el fragmento Formulario adaptable en modo de edición.](http://localhost:4502/editor.html/content/forms/af/currentaddressfragment.html)
* Abra el editor de reglas para el componente Opción de imagen .
* Reemplace &lt;your_api_key> por la clave de API de Google.
* Guarde los cambios.
* [Obtener una vista previa del formulario](http://localhost:4502/content/dam/formsanddocuments/currentaddressfragment/jcr:content?wcmmode=disabled).
* Haga clic en el icono &quot;geolocalización&quot;.
* El formulario debe rellenarse con la ubicación actual.
