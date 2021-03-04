---
title: Adición de elementos al componente de grupo de opciones
seo-title: Adición de elementos al componente de grupo de opciones
description: Agregar elementos al componente de grupo de opciones de forma dinámica
seo-description: Agregar elementos al componente de grupo de opciones de forma dinámica
feature: formularios adaptables
topics: authoring
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '528'
ht-degree: 0%

---



# Adición dinámica de elementos al componente de grupo de opciones

AEM Forms 6.5 ha introducido la capacidad de agregar elementos de forma dinámica a un componente de grupo de opciones de formularios adaptables como Casilla de verificación, Botón de radio y Lista de imágenes.

[Esta funcionalidad está disponible en vivo en el servidor de muestras](https://forms.enablementadobe.com/content/samples/samples.html?query=0). Busque elementos de la casilla de verificación dinámica en la tarjeta y haga clic en &quot;Probar&quot;.


Puede agregar elementos mediante el editor visual, así como el editor de código, según el caso de uso.

**Uso del editor visual:** puede rellenar los elementos del grupo de opciones a partir de los resultados de una llamada de función o de una llamada de servicio. Por ejemplo, puede configurar los elementos del grupo de opciones consumiendo la respuesta de una llamada a la API de REST.

En la captura de pantalla siguiente, estamos configurando las opciones de Periodo(años) de préstamo en los resultados de una llamada de servicio llamada getLoanPeriods.

![Editor de reglas](assets/ruleeditor.png)

**Uso del editor** de código: Cuando desee definir los elementos del grupo de opciones de forma dinámica en función de los valores introducidos en el formulario. Por ejemplo, el siguiente fragmento de código establece los elementos de la casilla de verificación en los valores introducidos en los campos de nombre de solicitante y cónyuge del formulario adaptable.

En el fragmento de código, se configuran los elementos de WorkingMembers que es un componente de casilla de verificación. La matriz de los elementos se está creando dinámicamente recuperando los valores de los campos de texto de nombre de solicitante y cónyuge de los formularios adaptables

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

**Adición de elementos mediante el editor de reglas**

>[!VIDEO](https://video.tv.adobe.com/v/26847?quality=12&learn=on)

**Adición de elementos mediante el editor de código**

>[!VIDEO](https://video.tv.adobe.com/v/26848?quality=12&learn=on)

Para probar esto en su sistema:

**Uso del editor de código para agregar elementos**

* [Descargar los recursos](assets/usingthecodeeditor.zip)
* [Abrir Formularios Y Documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Haga clic en &quot;Crear&quot; | Cargar archivo&quot; y cargar el archivo que descargó en el paso anterior
* [Vista previa de los formularios](http://localhost:4502/content/dam/formsanddocuments/simpleform/jcr:content?wcmmode=disabled)
* Especifique el nombre del solicitante y seleccione el estado civil del matrimonio
* Escriba el nombre del cónyuge
* Haga clic en Siguiente
* Debería ver casillas de verificación rellenadas con el nombre del solicitante y con el nombre del cónyuge si el estado civil está casado

**Uso del editor visual para añadir elementos**

* [Descargar los recursos](assets/usingthevisualeditor.zip)
* Instale Tomcat si todavía no lo tiene. [Las instrucciones para instalar tomcat están disponibles aquí](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html)
* [Implementar el archivo SampleRest.war en Tomcat](https://forms.enablementadobe.com/content/DemoServerBundles/SampleRest.war)
* [Abrir Formularios Y Documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Haga clic en &quot;Crear&quot; | Cargar archivo&quot; y cargar el archivo que descargó en el paso anterior
* [Vista previa de los formularios](http://localhost:4502/content/dam/formsanddocuments/amortizationschedule/jcr:content?wcmmode=disabled)
* Introduzca Loan amount y tab fuera del campo. Esto activará la regla que muestra el campo período de préstamo.
* Seleccione el período de préstamo adecuado (los artículos del período de préstamo se rellenan desde la llamada de reposo)
* Seleccione la tasa de interés y haga clic en &quot;Obtener programa de amortización&quot;
* La tabla de amortización debe rellenarse. La programación de amortización se obtiene mediante una llamada a REST.

>[!NOTE]
> Se supone que tomcat se ejecuta en el puerto 8080 y AEM en el puerto 4502
