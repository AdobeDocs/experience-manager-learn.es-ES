---
title: Variables en el flujo de trabajo de AEM[Part4]
description: Uso de variables de tipo XML, JSON, ArrayList o Document en un flujo de trabajo de AEM
version: Experience Manager 6.5
topic: Development
feature: Adaptive Forms, Workflow
role: Developer
level: Beginner
exl-id: 269e43f7-24cf-4786-9439-f51bfe91d39c
duration: 102
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '437'
ht-degree: 0%

---

# Variable ArrayList en el flujo de trabajo de AEM

Se han introducido variables de tipo ArrayList en AEM Forms 6.5. Un caso de uso común para utilizar la variable ArrayList es definir las rutas personalizadas que se utilizarán en AssignTask.

Para utilizar la variable ArrayList en un flujo de trabajo de AEM, debe crear un formulario adaptable que genere elementos repetidos en los datos enviados. Una práctica común es definir un esquema que contenga un elemento de matriz. Para los fines de este artículo, he creado un esquema JSON simple que contiene elementos de matriz. El caso de uso es cuando un empleado rellena un informe de gastos. En el informe de gastos, se captura el nombre del responsable del remitente y el nombre del responsable. Los nombres del administrador se almacenan en una matriz denominada managerchain. La captura de pantalla siguiente muestra el formulario de informe de gastos y los datos del envío de Forms adaptable.

![informe de gastos](assets/expensereport.jpg)

A continuación se muestran los datos del envío del formulario adaptable. El formulario adaptable se basó en el esquema JSON; los datos enlazados al esquema se almacenan en el elemento de datos afBoundData. La cadena de administración es una matriz y es necesario rellenar ArrayList con el elemento de nombre del objeto dentro de la matriz de cadena de administración.

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

Para inicializar la variable ArrayList de la cadena de subtipo puede utilizar el modo de asignación de puntos JSON o XPath. La siguiente captura de pantalla muestra cómo rellenar una variable ArrayList denominada CustomRoutes mediante la notación de puntos JSON. Asegúrese de señalar a un elemento de un objeto de matriz como se muestra en la captura de pantalla siguiente. Se está rellenando la ArrayList de CustomRoutes con los nombres del objeto de matriz managerchain.
A continuación, se utiliza el ArrayList de rutas personalizadas para rellenar las rutas en el componente AssignTask
![rutas personalizadas](assets/arraylist.jpg)
Una vez que la variable CustomRoutes ArrayList se inicializa con los valores de los datos enviados, las Rutas del componente AssignTask se rellenan utilizando la variable CustomRoutes. La captura de pantalla siguiente muestra las rutas personalizadas de una tarea asignada
![asingtask](assets/customactions.jpg)

Para probar este flujo de trabajo en su sistema, siga los siguientes pasos

* Descargue y guarde el archivo ArrayListVariable.zip en su sistema de archivos
* [Importe el archivo zip](assets/arraylistvariable.zip) mediante el Administrador de paquetes de AEM
* [Abrir el formulario TravelExpenseReport](http://localhost:4502/content/dam/formsanddocuments/helpx/travelexpensereport/jcr:content?wcmmode=disabled)
* Escriba un par de gastos y los nombres de los dos responsables
* Pulse el botón Enviar
* [Abrir la bandeja de entrada](http://localhost:4502/aem/inbox)
* Debería ver una nueva tarea titulada &quot;Asignar a administrador de gastos&quot;
* Abra el formulario asociado a la tarea
* Debería ver dos rutas personalizadas con los nombres de los responsables
  [Explorar ReviewExpenseReportWorkflow.](http://localhost:4502/editor.html/conf/global/settings/workflow/models/ReviewExpenseReport.html) Este flujo de trabajo utiliza la variable ArrayList, la variable de tipo JSON y el editor de reglas en el componente Or-Split
