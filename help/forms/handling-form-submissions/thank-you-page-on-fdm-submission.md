---
title: Mostrar el ID de envío al enviar el formulario
description: Mostrar la respuesta de un envío de modelo de datos de formulario en la página de agradecimiento
feature: Adaptive Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
jira: KT-13900
last-substantial-update: 2023-09-09T00:00:00Z
exl-id: 18648914-91cc-470d-8f27-30b750eb2f32
duration: 85
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '330'
ht-degree: 0%

---

# Personalizar página de agradecimiento

Cuando envía un formulario adaptable a un extremo REST, desea mostrar un mensaje de confirmación que permita al usuario saber que el envío del formulario se ha realizado correctamente. La respuesta del POST contiene detalles sobre el envío, como el ID de envío, y un mensaje de confirmación bien diseñado que incluye el ID de envío, lo que contribuye a mejorar la experiencia del usuario. Esta respuesta se puede mostrar en la página de agradecimiento configurada con el formulario adaptable.

La siguiente captura de pantalla muestra un formulario que se está enviando mediante la acción de envío del modelo de datos de formulario con una página de agradecimiento configurada

![página de agradecimiento](./assets/thank-you-page-fdm-submit.png)

El POST de un modelo de datos de formulario siempre devolverá un objeto JSON en la respuesta. Este JSON está disponible en la dirección URL de la página de agradecimiento como parámetro de consulta denominado _fdmSubmitResult_. Puede analizar este parámetro de consulta y mostrar los elementos JSON en la página de agradecimiento.
El siguiente código de ejemplo analiza la respuesta JSON para extraer el valor del campo numérico. A continuación, se construye el xml adecuado y se pasa en slingRequest para rellenar el formulario. Este código suele escribirse en el jsp del componente de página asociado a la plantilla del formulario adaptable.

```java
if(request.getParameter("fdmSubmitResult")!=null)
{
    String fdmSubmitResult =  request.getParameter("fdmSubmitResult");
    String status = request.getParameter("status");
    com.google.gson.JsonObject jsonObject = com.google.gson.JsonParser.parseString(fdmSubmitResult).getAsJsonObject();
    String caseNumber = jsonObject.get("result").getAsJsonObject().get("number").getAsString();
    slingRequest.setAttribute("data","<afData><afUnboundData><data><caseNumber>"+caseNumber+"</caseNumber><status>"+status+"</status></data></afUnboundData></afData>");
}
```

Se recomienda basar la página de agradecimiento en una nueva plantilla de formulario adaptable que le permite escribir el código personalizado para extraer la respuesta de los parámetros de consulta.

## Prueba de la solución

Cree un formulario adaptable y configure para enviar el formulario mediante la acción de envío del modelo de datos de formulario.
[Implementar la plantilla de formulario adaptable de ejemplo](assets/thank-you-page-template.zip)
Crear un formulario de agradecimiento basado en esta plantilla Asociar esta página de agradecimiento con el formulario principal Modificar el código jsp en la [createXml.jsp](http://localhost:4502/apps/thank-you-page-template/component/page/thankyoupage/createxml.jsp) para generar el xml necesario para rellenar previamente el formulario adaptable.
Obtenga una vista previa del formulario adaptable y envíelo.
La página de agradecimiento debe mostrarse y rellenarse previamente con los datos especificados en el XML
