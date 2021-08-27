---
title: 'Rellenar tabla de formulario adaptable '
description: Rellenar la tabla Formulario adaptable con los resultados de las invocaciones del servicio del Modelo de datos de formulario
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: User
level: Intermediate
source-git-commit: 0049c9fd864bd4dd4f8c33b1e40e94aad3ffc5b9
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 0%

---


# Rellenar tabla de formulario adaptable con los resultados de la invocación del servicio del modelo de datos de formulario

[Live form está alojado ](https://forms.enablementadobe.com/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
aquíEn este artículo se observa cómo rellenar la tabla de formularios adaptables recuperando datos de invocación del servicio del modelo de datos de formulario. Vamos a crear un programa de amortización en una tabla que enumera cada pago normal de una hipoteca a lo largo del tiempo. El modelo de datos de formulario devuelve los resultados de la amortización. El servicio del Modelo de datos de formulario se invoca en el suceso click del botón calcular como se muestra en la captura de pantalla. Los parámetros de entrada y salida de la invocación del servicio se asignan correctamente como se muestra en la captura de pantalla. El resultado se asigna a las columnas de Fila1
![click event](assets/amortization.PNG)

La fila 1 está configurada para crecer según los datos devueltos por la llamada de servicio. Observe la configuración de repetición que se especifica aquí. Un valor de -1 indica un número ilimitado de filas en la tabla
![Fila1](assets/rowconfiguration.PNG)

## Implemente esto en su servidor

[Instale Tomcat tal como se especifica ](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md)
[aquíImplemente el archivo SampleRest.war contenido en este ](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/assets/common-osgi-bundles/sample-rest.zip)
[archivo zipInstale los recursos  ](assets/amortizationschedule.zip) mediante AEM administrador de paquetes 
[Abra el ](http://localhost:4502/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
formulario de programación de amortizaciónIntroduzca el valor apropiado y haga clic en calcular programación de amortización debe rellenarse en el formulario

