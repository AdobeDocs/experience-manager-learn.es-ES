---
title: Captura de mensajes de error en el servicio de modelo de datos de formulario como paso del flujo de trabajo
description: A partir de AEM Forms AEM 6.5.1, ahora podemos capturar los mensajes de error generados al utilizar invocar el servicio de modelo de datos de formulario como paso en el flujo de trabajo de la. Flujo de trabajo.
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 8cae155c-c393-4ac3-a412-bf14fc411aac
last-substantial-update: 2020-06-09T00:00:00Z
duration: 75
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '249'
ht-degree: 0%

---

# Capturar mensajes de error en el paso para invocar el servicio de modelo de datos de formulario

A partir de AEM Forms 6.5.1, ahora tenemos la opción de capturar mensajes de error y especificar opciones de validación. El paso para invocar el servicio de modelo de datos de formulario se ha mejorado para proporcionar las siguientes capacidades.

* Proporcionar una opción de validación de tres niveles (&quot;OFF&quot;, &quot;BASIC&quot; y &quot;FULL&quot;) para gestionar las excepciones encontradas al invocar el servicio del modelo de datos de formulario. Las tres opciones indican sucesivamente una versión más estricta de la comprobación de los requisitos específicos de la base de datos.
  ![validation-levels](assets/validation-level.PNG)

* Proporcionar una casilla de verificación para personalizar la ejecución del flujo de trabajo. Por lo tanto, los usuarios ahora tienen la flexibilidad de seguir adelante con la ejecución del flujo de trabajo, incluso si el paso para invocar el modelo de datos de formulario genera excepciones.

* Almacenar información importante de errores que surgen debido a excepciones de validación. Se han incorporado tres selectores de variables de tipo autocompletar para seleccionar variables relevantes para almacenar ErrorCode(String), ErrorMessage(String) y ErrorDetails(JSON). Sin embargo, ErrorDetails se establecería en null en caso de que la excepción no sea DermisValidationException.
  ![captura de mensajes de error](assets/fdm-error-details.PNG)

Con estos cambios, el paso para invocar el servicio de modelo de datos de formulario garantiza que los valores de entrada se ajusten a las restricciones de datos proporcionadas en el archivo swagger. Por ejemplo, se genera el siguiente mensaje de error cuando los valores accountId y balance no cumplen las restricciones de datos especificadas en el archivo swagger.

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
