---
title: Extraer nodo del XML de datos enviado
description: Paso de proceso personalizado para agregar al sistema de archivos el documento de escritura que reside en la carpeta de carga útil
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-9860
exl-id: 5282034f-275a-479d-aacb-fc5387da793d
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '179'
ht-degree: 0%

---

# Extraer nodo del XML de datos enviado

Este paso de proceso personalizado es crear un nuevo documento xml al extraer el nodo de otro documento xml. Debe utilizarlo cuando desee combinar los datos enviados con la plantilla xdp para generar un pdf. Por ejemplo, cuando envía un formulario adaptable, los datos que necesita combinar con la plantilla xdp se encuentran dentro del elemento de datos. En este caso, deberá crear otro documento xml extrayendo el elemento de datos correspondiente.

La siguiente captura de pantalla muestra los argumentos que debe pasar al paso de proceso personalizado
![paso del proceso](assets/create-xml-process-step.png)
Los siguientes son los parámetros
* Data.xml: el archivo XML del que desea extraer el nodo
* datatomerge.xml: el nuevo xml creado con el nodo extraído
* /afData/afUnboundData/data: nodo que se va a extraer


La siguiente captura de pantalla muestra el datamerge.xml que se crea en la carpeta de carga útil
![create-xml](assets/create-xml.png)

[El paquete personalizado se puede descargar desde aquí](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)
