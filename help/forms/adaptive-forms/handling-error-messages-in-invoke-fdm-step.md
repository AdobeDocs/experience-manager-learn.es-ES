---
title: Captura de mensajes de error en el servicio del modelo de datos de formulario como paso en el flujo de trabajo
seo-title: Captura de mensajes de error en el servicio del modelo de datos de formulario como paso en el flujo de trabajo
description: A partir de AEM Forms 6.5.1, ahora podemos capturar mensajes de error generados al utilizar invocar el servicio del modelo de datos de formulario como un paso en AEM Workflow. Flujo de trabajo.
seo-description: A partir de AEM Forms 6.5.1, ahora podemos capturar mensajes de error generados al utilizar invocar el servicio del modelo de datos de formulario como un paso en AEM Workflow. Flujo de trabajo.
feature: Flujo de trabajo
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.5.1,6.5.2
topic: Desarrollo
role: Desarrollador
level: Intermedio
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '290'
ht-degree: 1%

---


# Captura de mensajes de error al invocar el paso del servicio del modelo de datos de formulario

A partir de AEM Forms 6.5.1, ahora tenemos la opción de capturar mensajes de error y especificar opciones de validación. El paso Invocar el servicio del modelo de datos de formulario se ha mejorado para proporcionar las siguientes capacidades.

* Se proporciona una opción para la validación de tres niveles (&quot;OFF&quot;, &quot;BASIC&quot; y &quot;FULL&quot;) para gestionar las excepciones que se encuentran al invocar el servicio del modelo de datos de formulario. Las 3 opciones indican sucesivamente una versión más estricta de la comprobación de los requisitos específicos de la base de datos.
   ![niveles de validación](assets/validation-level.PNG)

* Proporcionar una casilla de verificación para personalizar la ejecución del flujo de trabajo. Por lo tanto, el usuario tendrá la flexibilidad de continuar con la ejecución del flujo de trabajo, incluso si el paso Invocar el modelo de datos de formulario genera excepciones.

* Almacenamiento de información importante sobre Error producido debido a excepciones de validación. Se han incorporado tres selectores de variables de tipo Autocomplete para seleccionar variables relevantes y almacenar los parámetros ErrorCode(String), ErrorMessage(String) y ErrorDetails(JSON). Sin embargo, ErrorDetails se establecería en nulo si la excepción no es DermisValidationException.
   ![captura de mensajes de error](assets/fdm-error-details.PNG)

Con estos cambios, el paso Invocar el servicio del modelo de datos de formulario garantizará que los valores de entrada se ajusten a las restricciones de datos proporcionadas en el archivo de intercambio. Por ejemplo, se generará el siguiente mensaje de error cuando los valores accountId y balance no cumplan las restricciones de datos especificadas en el archivo swagger.

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


