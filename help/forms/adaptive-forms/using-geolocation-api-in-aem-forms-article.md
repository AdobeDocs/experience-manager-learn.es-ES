---
title: Uso de las API de geolocalización en Forms adaptable
description: Rellene los campos de dirección del formulario con las API de geolocalización
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
exl-id: 50db6155-ee83-4ddb-9e3a-56e8709222db
last-substantial-update: 2020-03-20T00:00:00Z
duration: 88
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '366'
ht-degree: 0%

---

# Uso de las API de geolocalización en Forms adaptable{#using-geolocation-api-s-in-adaptive-forms}

En este artículo, analizaremos el uso de la API de geolocalización de Google para rellenar campos de un formulario adaptable. Este caso de uso se utiliza comúnmente cuando desea rellenar los campos de dirección actuales en un formulario.

Se han seguido los siguientes pasos para utilizar la API de geolocalización en Forms adaptable.

1. [Obtener clave API](https://developers.google.com/maps/documentation/javascript/get-api-key) de Google para usar la plataforma Google Maps. Puede obtener una clave de prueba con una validez de 1 año.

1. El fragmento de formulario adaptable se creó con campos para contener la dirección actual

1. La API de geolocalización se invocó en el evento de clic del objeto de imagen del formulario adaptable

1. Se analizaron los datos JSON devueltos por la llamada de API y los valores de los campos del formulario adaptable se establecieron en consecuencia.

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

![Los campos se rellenan con la API de geolocalización](assets/capture-4.gif)

En la línea 1 utilizamos la API de geolocalización de HTML para obtener la ubicación actual. Una vez obtenida la ubicación actual, pasamos la ubicación actual a la función showPosition.

En la función showPosition, utilizamos la API de Google para recuperar los detalles de dirección de la latitud y longitud dadas.

El JSON devuelto por la API se analiza a continuación para establecer los campos del formulario adaptable.

>[!NOTE]
>
>Para realizar pruebas, puede utilizar el protocolo HTTP con localhost en la dirección URL.
>
>Para el servidor de producción, deberá habilitar SSL en el servidor de AEM para obtener esta capacidad.
>
>La muestra asociada con este artículo se ha probado con la dirección de EE. UU. Si desea utilizar esta capacidad en otras ubicaciones geográficas, es posible que tenga que modificar el análisis de JSON.

Para poner esta capacidad en su servidor, siga los siguientes pasos

* Instale e inicie el servidor de AEM Forms.
> Esta capacidad se probó en AEM Forms 6.3 y versiones posteriores
>* [Obtener clave API de Google](https://developers.google.com/maps/documentation/javascript/get-api-key).
>* [Importe los recursos relacionados con este artículo en AEM.](assets/geolocationapi.zip)
>* [Abrir el fragmento de formulario adaptable en modo de edición.](http://localhost:4502/editor.html/content/forms/af/currentaddressfragment.html)
>* Abra el editor de reglas para el componente Opción de imagen.
>* Reemplace la &lt;your_api_key> por la clave de API de Google.
>* Guarde los cambios.
>* [Vista previa del formulario](http://localhost:4502/content/dam/formsanddocuments/currentaddressfragment/jcr:content?wcmmode=disabled).
>* Haga clic en el icono &quot;geolocalización&quot;.
>* El formulario debe rellenarse con la ubicación actual.
