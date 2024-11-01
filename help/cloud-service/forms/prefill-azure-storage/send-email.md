---
title: Enviar correo electrónico mediante SendGrid
description: Almacenar en déclencheur un mensaje de correo electrónico con un vínculo al formulario guardado
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-8474
duration: 25
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 0%

---

# Enviar correo electrónico

Una vez guardados los datos del formulario en Azure Blob Storage, se envía al usuario un mensaje de correo electrónico con un vínculo al formulario guardado. Este correo electrónico se envía mediante la API de REST de SendGrid.

El archivo swagger, el modelo de datos de formulario y la configuración de los servicios en la nube necesarios para enviar correos electrónicos se proporcionan como parte de los recursos del artículo.

Deberá crear una cuenta de SendGrid, una plantilla dinámica que pueda introducir los datos capturados en el formulario adaptable.


A continuación se muestra el fragmento de código HTML utilizado en la plantilla dinámica. El valor del parámetro blobID se pasa mediante el servicio de invocación del modelo de datos de formulario.

```html
You can  <a href='https://forms.enablementadobe.com/content/dam/formsanddocuments/azureportalstorage/creditcardapplication/jcr:content?wcmmode=disabled&ampguid={{blobID}}'>access your application here</a> and complete it.
```


