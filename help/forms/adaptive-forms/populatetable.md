---
title: 'Rellenar tabla de formulario adaptable '
seo-title: Rellenar tabla de formulario adaptable
description: Rellenar la tabla Formulario adaptable con los resultados de las invocaciones del servicio del Modelo de datos de formulario
seo-description: Rellenar la tabla Formulario adaptable con los resultados de las invocaciones del servicio del Modelo de datos de formulario
feature: Formularios adaptables
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Desarrollo
role: User
level: Intermediate
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '257'
ht-degree: 1%

---


# Rellenar tabla de formulario adaptable con los resultados de la invocación del servicio del modelo de datos de formulario

[Live form está alojado ](https://forms.enablementadobe.com/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
aquíEn este artículo se observa cómo rellenar la tabla de formularios adaptables recuperando datos de invocación del servicio del modelo de datos de formulario. Vamos a crear un programa de amortización en una tabla que enumera cada pago normal de una hipoteca a lo largo del tiempo. El modelo de datos de formulario devuelve los resultados de la amortización. El servicio del Modelo de datos de formulario se invoca en el suceso click del botón calcular como se muestra en la captura de pantalla. Los parámetros de entrada y salida de la invocación del servicio se asignan correctamente como se muestra en la captura de pantalla. El resultado se asigna a las columnas de Fila1
![click event](assets/amortization.PNG)

La fila 1 está configurada para crecer según los datos devueltos por la llamada de servicio. Observe la configuración de repetición que se especifica aquí. Un valor de -1 indica un número ilimitado de filas en la tabla
![Fila1](assets/rowconfiguration.PNG)

## Implemente esto en su servidor

[Instale Tomcat como se especifica ](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md)
[aquíImplemente el ](https://forms.enablementadobe.com/content/DemoServerBundles/SampleRest.war)
[archivo SampleRest.warInstale los recursos  ](assets/amortizationschedule.zip) mediante AEM administrador de paquetes 
[Abra el ](http://localhost:4502/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
formulario de programación de amortizaciónIntroduzca el valor apropiado y haga clic en calcular programación de amortización debe rellenarse en el formulario

