---
title: Variables en AEM flujo de trabajo[Parte 4]
seo-title: Variables en AEM flujo de trabajo[Parte 4]
description: Uso de variables de tipo xml,json,arraylist,documento en el flujo de trabajo de aem
seo-description: Uso de variables de tipo xml,json,arraylist,documento en el flujo de trabajo de aem
feature: workflow
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
translation-type: tm+mt
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '471'
ht-degree: 0%

---


# Variable ArrayList en AEM flujo de trabajo

Las variables de tipo ArrayList se han introducido en AEM Forms 6.5. Un caso de uso común para utilizar la variable ArrayList es definir las rutas personalizadas que se utilizarán en la tarea Asignar.

Para utilizar la variable ArrayList en un flujo de trabajo AEM, debe crear un formulario adaptable que genere elementos repetitivos en los datos enviados. Una práctica común es definir un esquema que contenga un elemento de matriz. A los efectos de este artículo, he creado un esquema JSON sencillo que contiene elementos de matriz. El caso de uso es el de un empleado que rellena un informe de gastos. En el informe de gastos, capturamos el nombre del administrador y el nombre del administrador del remitente. Los nombres del administrador se almacenan en una matriz denominada cadena de administración. La captura de pantalla siguiente muestra el formulario de informe de gastos y los datos del envío de Forms adaptable.

![informe de gastos](assets/expensereport.jpg)

A continuación se muestran los datos del envío del formulario adaptable. El formulario adaptable se basaba en el esquema JSON cuando los datos enlazados al esquema se almacenan en el elemento de datos del elemento afBoundData. La cadena de administración es una matriz y necesitamos rellenar ArrayList con el elemento name del objeto dentro de la matriz de cadena de administración.

```json
{
    "afData": {
        "afUnboundData": {
            "data": {
                "numericbox_2762582281554154833426": 700
            }
        },
        "afBoundData": {
            "data": {
                "Employee": {
                    "Name": "Conrad Simms",
                    "Department": "IT",
                    "managerchain": [{
                        "name": "Gloria Rios"
                    }, {
                        "name": "John Jacobs"
                    }]
                },
                "expense": [{
                    "description": "Hotel",
                    "amount": 300
                }, {
                    "description": "Air Fare",
                    "amount": 400
                }]
            }
        },
        "afSubmissionInfo": {
            "computedMetaInfo": {},
            "stateOverrides": {},
            "signers": {},
            "afPath": "/content/dam/formsanddocuments/helpx/travelexpensereport",
            "afSubmissionTime": "20190402102953"
            }
        }
}
```

Para inicializar la variable ArrayList de la cadena de subtipo, puede utilizar el modo de asignación JSON Dot Notation o XPath. La siguiente captura de pantalla muestra cómo rellenar una variable ArrayList denominada CustomRoutes mediante la notación de punto JSON. Asegúrese de que está apuntando a un elemento en un objeto de matriz como se muestra en la captura de pantalla siguiente. Se rellena el objeto ArrayList de CustomRoutes con los nombres del objeto de matriz de cadena de administración.
A continuación, se utiliza la ArrayList de CustomRoutes para rellenar las rutas en el componente AsignarTarea
![rutas personalizadas](assets/arraylist.jpg)
Una vez inicializada la variable ArrayList de CustomRoutes con los valores de los datos enviados, las rutas del componente AsignarTarea se rellenan mediante la variable CustomRoutes. La siguiente captura de pantalla muestra las rutas personalizadas de una tarea Asignar
![asingtask](assets/customactions.jpg)

Para probar este flujo de trabajo en el sistema, siga los pasos siguientes

* Descargue y guarde el archivo ArrayListVariable.zip en el sistema de archivos
* [Importación del ](assets/arraylistvariable.zip) archivo zip mediante el Administrador de paquetes de AEM
* [Abrir el formulario TravelCostReport](http://localhost:4502/content/dam/formsanddocuments/helpx/travelexpensereport/jcr:content?wcmmode=disabled)
* Introduzca un par de gastos y los nombres de los dos gerentes
* Pulse el botón de envío
* [Abra la bandeja de entrada](http://localhost:4502/aem/inbox)
* Debe ver una nueva tarea titulada &quot;Asignar al administrador de gastos&quot;
* Abrir el formulario asociado a la tarea
* Debe ver dos rutas personalizadas con los nombres del administrador
   [Explore el flujo de trabajo de ReviewCostReportWorkflow.](http://localhost:4502/editor.html/conf/global/settings/workflow/models/ReviewExpenseReport.html) Este flujo de trabajo utiliza la variable ArrayList, la variable de tipo JSON, el editor de reglas en el componente Or-Split
