---
title: Extraer respuesta del envío personalizado
description: Extraer respuesta personalizada al enviar correctamente el formulario
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-13520
exl-id: e5f76d6a-2ea8-4909-9cfb-b673077cf8fd
duration: 16
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '51'
ht-degree: 0%

---

# Extraiga el objeto json de la respuesta

Cuando envía formularios adaptables sin encabezado a un controlador de envío personalizado, los datos devueltos por el controlador de envío se pueden extraer y mostrar en la aplicación de React. El siguiente fragmento de código muestra

```javascript
// associate event handler for the onSubmitSuccess event
<AdaptiveForm mappings={extendMappings} onSubmitSuccess={onSuccess} formJson={selectedForm}/>
```

```javascript
// extract the json returned by the custom submit service
const onSuccess=(action) =>{
        let body = action.payload?.body;
        let FirstName = JSON.parse(body.metadata.json).firstName;
        setThankYouMessage(FirstName+" your request has been received");
        
      }
```
