---
title: Uso de AEM Forms con el bot de chat
description: Analizar datos de ChatBot
feature: Adaptive Forms
version: 6.5
jira: KT-15344
topic: Development
role: User
level: Intermediate
exl-id: 3c304b0a-33f8-49ed-a576-883df4759076
duration: 22
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '97'
ht-degree: 2%

---

# Analizar datos de ChatBot

AEM Se usó un webhook de [ChatBot](https://www.chatbot.com/help/webhooks/what-are-webhooks/) para enviar los datos de ChatBot a un servlet de.
Los datos capturados en el ChatBot están en formato JSON con los datos introducidos por el usuario en el objeto de atributos como se muestra a continuación
![chatbot-data](assets/chat-bot-data.png)

Para combinar los datos con la plantilla XDP, se debe crear el siguiente XML. Observe el elemento raíz del xml, que debe coincidir con el elemento raíz de la plantilla XDP para que los datos se combinen correctamente.


```xml
<topmostSubForm>
    <f1_01>David Smith</f1_01>
    <signmethod>ESIGN</signmethod>
    <corporation>1</corporation>
    <f1_08>San Jose, CA, 95110</f1_08>
    <f1_07>345 Park Avenue</f1_07>
    <ssn>123-45-6789</ssn>
    <form_name>W-9</form_name>
</topmostSubForm>
```

![xdp-template](assets/xdp-template.png)

## Siguientes pasos

[Combinar datos con la plantilla XDP](./merge-data-with-template.md)
