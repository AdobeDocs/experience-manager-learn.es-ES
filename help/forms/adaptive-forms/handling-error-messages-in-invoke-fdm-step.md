---
title: Captura de mensajes de error en el servicio del modelo de datos de formulario como paso en el flujo de trabajo
seo-title: Captura de mensajes de error en el servicio del modelo de datos de formulario como paso en el flujo de trabajo
description: A partir de AEM Forms 6.5.1, ahora podemos capturar mensajes de error generados al utilizar invocar el servicio del modelo de datos de formulario como un paso en AEM flujo de trabajo. Flujo de trabajo.
seo-description: A partir de AEM Forms 6.5.1, ahora podemos capturar mensajes de error generados al utilizar invocar el servicio del modelo de datos de formulario como un paso en AEM flujo de trabajo. Flujo de trabajo.
feature: workflow
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.5.1,6.5.2
translation-type: tm+mt
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '286'
ht-degree: 0%

---


# Captura de mensajes de error en el paso del servicio Invocar modelo de datos de formulario

A partir de AEM Forms 6.5.1, ahora tenemos la opción de capturar mensajes de error y especificar opciones de validación. Se ha mejorado el paso Invocar el servicio del modelo de datos de formulario para proporcionar las siguientes capacidades.

* Proporciona una opción para la validación de tres niveles (&quot;OFF&quot;, &quot;BASIC&quot; y &quot;FULL&quot;) para controlar las excepciones encontradas al invocar el servicio del modelo de datos de formulario. Las 3 opciones indican sucesivamente una versión más estricta de la comprobación de los requisitos específicos de la base de datos.
   ![niveles de validación](assets/validation-level.PNG)

* Proporcionar una casilla de verificación para personalizar la ejecución del flujo de trabajo. Por lo tanto, el usuario tendrá la flexibilidad de continuar con la ejecución del flujo de trabajo, incluso si el paso Invocar modelo de datos de formulario genera excepciones.

* Almacenamiento de información importante sobre el error que se produce debido a las excepciones de validación. Se han incorporado tres selectores de variables de tipo Autocomplete para seleccionar variables relevantes para almacenar el errorCode(String), ErrorMessage(String) y ErrorDetails(JSON). No obstante, ErrorDetails se establecería en nulo si la excepción no es DermisValidationException.
   ![capturar mensajes de error](assets/fdm-error-details.PNG)

Con estos cambios, el paso Invocar el servicio del modelo de datos de formulario garantizará que los valores de entrada se ajusten a las restricciones de datos proporcionadas en el archivo swagger. Por ejemplo, se generará el siguiente mensaje de error cuando los valores accountId y balance no sean compatibles con las restricciones de datos especificadas en el archivo swagger.

```json
{

"errorCode": "AEM-FDM-001-049"

"errorMessage": "Input validations failed during operation execution"

"violations": {

"/accountId": ["numeric instance is greater than the required maximum (maximum: 20, found: 97)"],

"/newAccount/balance": ["instance type (string) does not match any allowed primitive type (allowed: [\"integer\",\"number\"])"]

}

}
```


