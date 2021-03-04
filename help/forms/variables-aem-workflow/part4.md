---
title: Variables en el flujo de trabajo de AEM[Part4]
seo-title: Variables en el flujo de trabajo de AEM[Part4]
description: Uso de variables de tipo xml,json,arraylist,document en el flujo de trabajo de aem
seo-description: Uso de variables de tipo xml,json,arraylist,document en el flujo de trabajo de aem
feature: flujo de trabajo
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '472'
ht-degree: 0%

---


# Variable ArrayList en el flujo de trabajo de AEM

Se han introducido variables de tipo ArrayList en AEM Forms 6.5. Un caso de uso común para usar la variable ArrayList es definir las rutas personalizadas que se utilizarán en AssignTask.

Para utilizar la variable ArrayList en un flujo de trabajo de AEM, debe crear un formulario adaptable que genere elementos repetitivos en los datos enviados. Una práctica habitual es definir un esquema que contenga un elemento de matriz. A los efectos de este artículo, he creado un esquema JSON simple que contiene elementos de matriz. El caso de uso es que un empleado rellene un informe de gastos. En el informe de gastos, capturamos el nombre de administrador del remitente y el nombre de administrador del administrador. Los nombres del administrador se almacenan en una matriz denominada cadena de administración. La captura de pantalla siguiente muestra el formulario del informe de gastos y los datos del envío de formularios adaptables.

![informe de gastos](assets/expensereport.jpg)

A continuación se muestran los datos del envío del formulario adaptable. El formulario adaptable se basaba en el esquema JSON en el que los datos enlazados al esquema se almacenan en el elemento de datos del elemento afBoundData. La cadena de administración es una matriz y es necesario rellenar la ArrayList con el elemento name del objeto dentro de la matriz de la cadena de administración.

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

Para inicializar la variable ArrayList de la cadena de subtipo, puede utilizar la Notación de datos JSON o el modo de asignación XPath. La siguiente captura de pantalla muestra cómo rellenar una variable ArrayList llamada CustomRoutes usando la Notación de datos JSON. Asegúrese de que está señalando a un elemento en un objeto de matriz como se muestra en la captura de pantalla siguiente. Se está rellenando el objeto ArrayList de CustomRoutes con los nombres del objeto de matriz de cadena de administración.
A continuación, se utiliza la ArrayList de CustomRoutes para rellenar las rutas en el componente AssignTask
![rutas personalizadas](assets/arraylist.jpg)
Una vez que la variable ArrayList de CustomRoutes se inicializa con los valores de los datos enviados, las rutas del componente AssignTask se rellenan con la variable CustomRoutes. La captura de pantalla siguiente muestra las rutas personalizadas en una tarea Assign
![asingtask](assets/customactions.jpg)

Para probar este flujo de trabajo en su sistema, siga los siguientes pasos

* Descargue y guarde el archivo ArrayListVariable.zip en su sistema de archivos
* [Importe el ](assets/arraylistvariable.zip) archivo zip mediante el Administrador de paquetes de AEM
* [Abra el formulario Informe de gastos de viaje .](http://localhost:4502/content/dam/formsanddocuments/helpx/travelexpensereport/jcr:content?wcmmode=disabled)
* Introduzca un par de gastos y los nombres de los 2 gerentes
* Pulse el botón de envío
* [Abra la bandeja de entrada](http://localhost:4502/aem/inbox)
* Debería ver una nueva tarea titulada &quot;Asignar al administrador de gastos&quot;
* Abra el formulario asociado a la tarea.
* Debería ver dos rutas personalizadas con los nombres del administrador
   [Explorar el flujo de trabajo ReviewExpenseReportWorkflow.](http://localhost:4502/editor.html/conf/global/settings/workflow/models/ReviewExpenseReport.html) Este flujo de trabajo utiliza la variable ArrayList, la variable de tipo JSON, el editor de reglas en el componente Or-Split
