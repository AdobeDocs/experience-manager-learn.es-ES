---
title: 'Rellenar tabla de formulario adaptable '
seo-title: Rellenar tabla de formulario adaptable
description: Rellene la tabla Formulario adaptable con los resultados de las invocaciones del servicio Modelo de datos de formulario
seo-description: Rellene la tabla Formulario adaptable con los resultados de las invocaciones del servicio Modelo de datos de formulario
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: 449202af47b6bbcd9f860d5c5391d1f7096d489e
workflow-type: tm+mt
source-wordcount: '254'
ht-degree: 0%

---


# Rellenar tabla de formulario adaptable con los resultados de la invocación del servicio del modelo de datos de formulario

[El formulario activo se aloja aquí](https://forms.enablementadobe.com/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)En este artículo analizamos el rellenado de la tabla de formularios adaptables recuperando datos de la invocación del servicio del modelo de datos de formulario. Vamos a crear un programa de amortización en una tabla que lista cada pago regular en una hipoteca con el paso del tiempo. El modelo de datos de formulario devuelve los resultados de la amortización. El servicio del Modelo de datos de formulario se invoca en el botón hacer clic en el evento de cálculo, como se muestra en la captura de pantalla. Los parámetros de entrada y salida de la invocación del servicio se asignan correctamente, como se muestra en la captura de pantalla. El resultado se asigna a las columnas del evento de clic Fila1![.](assets/amortization.PNG)

Fila1 está configurada para crecer según los datos devueltos por la llamada de servicio. Observe la configuración de repetición que se especifica aquí. Un valor de -1 indica un número ilimitado de filas en la tabla![Fila1](assets/rowconfiguration.PNG)

## Implementar esto en el servidor

[Instalar Tomcat como se especifica aquí](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md)[Implementar el archivo](https://forms.enablementadobe.com/content/DemoServerBundles/SampleRest.war)SampleRest.warInstalar los recursos[mediante AEM administrador ](assets/amortizationschedule.zip) de paquetes Abrir el formulario[](http://localhost:4502/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)Programa de amortizaciónIntroduzca el valor adecuado y haga clic en calculateLa programación de amortización se debe rellenar en el formulario

