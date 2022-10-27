---
title: Rellenar tabla de formulario adaptable
description: Rellenar la tabla Formulario adaptable con los resultados de las invocaciones del servicio del Modelo de datos de formulario
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: User
level: Intermediate
exl-id: 6e4b901a-6534-4c34-b315-2f2620b74247
last-substantial-update: 2019-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '240'
ht-degree: 0%

---

# Rellenar tabla de formulario adaptable con los resultados de la invocación del servicio del modelo de datos de formulario

[El formulario en directo está alojado aquí](https://forms.enablementadobe.com/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
En este artículo analizamos cómo rellenar la tabla de formularios adaptables recuperando datos de invocación del servicio del modelo de datos de formulario. Vamos a crear un programa de amortización en una tabla que enumera cada pago normal de una hipoteca a lo largo del tiempo. El modelo de datos de formulario devuelve los resultados de la amortización. El servicio del Modelo de datos de formulario se invoca en el suceso click del botón calcular como se muestra en la captura de pantalla. Los parámetros de entrada y salida de la invocación del servicio se asignan correctamente como se muestra en la captura de pantalla. El resultado se asigna a las columnas de Fila1
![click, suceso](assets/amortization.PNG)

La fila 1 está configurada para crecer según los datos devueltos por la llamada de servicio. Observe la configuración de repetición que se especifica aquí. Un valor de -1 indica un número ilimitado de filas en la tabla
![Fila1](assets/rowconfiguration.PNG)

## Implemente esto en su servidor

[Instale Tomcat tal como se especifica aquí](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md)
[Implemente el archivo SampleRest.war contenido en este archivo zip en Tomcat.](assets/sample-rest.zip)
[Instalación de los recursos ](assets/amortizationschedule.zip) uso de AEM administrador de paquetes
[Abrir el formulario de programación de amortización](http://localhost:4502/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
Introduzca el valor adecuado y haga clic en calcular Amortización El programa debe rellenarse en el formulario
