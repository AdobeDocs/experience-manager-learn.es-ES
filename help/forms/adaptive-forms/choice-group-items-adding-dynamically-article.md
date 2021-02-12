---
title: Añadir elementos al componente de grupo de opciones
seo-title: Añadir elementos al componente de grupo de opciones
description: Añadir elementos para elegir el componente de grupo de forma dinámica
seo-description: Añadir elementos para elegir el componente de grupo de forma dinámica
feature: adaptive-forms
topics: authoring
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
translation-type: tm+mt
source-git-commit: ecbd4d21c5f41b2bc6db3b409767b767f00cc5d1
workflow-type: tm+mt
source-wordcount: '527'
ht-degree: 0%

---



# Añadir elementos dinámicamente a un componente de grupo de opciones

AEM Forms 6.5 ha introducido la capacidad de agregar elementos dinámicamente a un componente de grupo de opciones de Forms adaptable, como Casilla de verificación, Botón de radio y Lista de imágenes.

[Esta capacidad está disponible en directo en el servidor](https://forms.enablementadobe.com/content/samples/samples.html?query=0) de ejemplos. Busque la tarjeta de elementos de la casilla de verificación dinámica y haga clic en &quot;Probar&quot;


Puede agregar elementos mediante el editor visual y el editor de código, según el caso de uso.

**Uso del editor visual:** puede rellenar los elementos del grupo de opciones a partir de los resultados de una llamada de función o de servicio. Por ejemplo, puede configurar los elementos del grupo de opciones consumiendo la respuesta de una llamada de API REST.

En la captura de pantalla de abajo, estamos configurando las opciones de Período(años) de préstamo a los resultados de una llamada de servicio llamada getLoanPeriods.

![Editor de reglas](assets/ruleeditor.png)

**Uso del editor** de código: Cuando desee definir los elementos del grupo de opciones de forma dinámica en función de los valores introducidos en el formulario. Por ejemplo, el siguiente fragmento de código establece los elementos de la casilla de verificación en los valores introducidos en los campos de nombre del solicitante y cónyuge del formulario adaptable.

En el fragmento de código, estamos configurando los elementos de WorkingMembers, que es un componente de casilla de verificación. La matriz de los elementos se está generando dinámicamente mediante la captura de los valores de los campos de texto solicitanteName y cónyuge de los formularios adaptables

```javascript
 
 if(MaritalStatus.value=="Married")
  {
WorkingMembers.items =["spouse="+spouse.value,"applicant="+applicantName.value];
  }
else
  {
    WorkingMembers.items =["applicant="+applicantName.value];
  }
```

Los datos presentados son los siguientes

```xml
<afUnboundData>

<data>

<applicantName>John Jacobs</applicantName>

<MaritalStatus>Married</MaritalStatus>

<spouse>Gloria Rios</spouse>

<WorkingMembers>spouse,applicant</WorkingMembers>

</data>

</afUnboundData>
```

**Añadir elementos mediante el editor de reglas**

>[!VIDEO](https://video.tv.adobe.com/v/26847?quality=12&learn=on)

**Añadir elementos mediante el editor de código**

>[!VIDEO](https://video.tv.adobe.com/v/26848?quality=12&learn=on)

Para probar esto en el sistema:

**Uso del editor de código para agregar elementos**

* [Descargar los recursos](assets/usingthecodeeditor.zip)
* [Abrir Forms Y Documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Haga clic en &quot;Crear&quot; | Carga de archivos&quot; y carga el archivo descargado en el paso anterior
* [Previsualización de formularios](http://localhost:4502/content/dam/formsanddocuments/simpleform/jcr:content?wcmmode=disabled)
* Introduzca el nombre del solicitante y seleccione el estado civil del matrimonio
* Escriba el nombre del cónyuge
* Haga clic en Siguiente
* Debe ver la casilla de verificación rellenada con el nombre del solicitante y con el nombre del cónyuge si el estado civil está casado

**Uso del editor visual para añadir elementos**

* [Descargar los recursos](assets/usingthevisualeditor.zip)
* Instale Tomcat si todavía no lo tiene. [Las instrucciones para instalar tomcat están disponibles aquí](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html)
* [Implementar el archivo SampleRest.war en Tomcat](https://forms.enablementadobe.com/content/DemoServerBundles/SampleRest.war)
* [Abrir Forms Y Documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Haga clic en &quot;Crear&quot; | Carga de archivos&quot; y carga el archivo descargado en el paso anterior
* [Previsualización de formularios](http://localhost:4502/content/dam/formsanddocuments/amortizationschedule/jcr:content?wcmmode=disabled)
* Introduzca el importe de préstamo y el tabulador fuera del campo. Esto déclencheur la regla que muestra el campo del período de préstamo.
* Seleccione el período de préstamo apropiado(Los artículos para el período de préstamo se rellenan desde la llamada de descanso)
* Seleccione la tasa de interés y haga clic en &quot;Obtener programa de amortización&quot;
* La tabla de amortización debe rellenarse. El programa de amortización se obtiene mediante una llamada REST.

>[!NOTE]
> Se supone que tomcat se ejecuta en el puerto 8080 y AEM en el puerto 4502
