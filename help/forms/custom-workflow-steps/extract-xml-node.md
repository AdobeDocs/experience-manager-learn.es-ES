---
title: Extraer nodo del xml de datos enviado
description: Paso de proceso personalizado para agregar al sistema de archivos el tamaño del documento de escritura en la carpeta de carga útil
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-9860
exl-id: 5282034f-275a-479d-aacb-fc5387da793d
source-git-commit: 012850e3fa80021317f59384c57adf56d67f0280
workflow-type: tm+mt
source-wordcount: '179'
ht-degree: 0%

---

# Extraer nodo del xml de datos enviado

Este paso de proceso personalizado es crear un nuevo documento xml extrayendo el nodo de otro documento xml. Debe utilizar esto cuando desee combinar los datos enviados con una plantilla xdp para generar pdf. Por ejemplo, al enviar un formulario adaptable, los datos que debe combinar con la plantilla xdp se encuentran dentro del elemento de datos. En este caso, necesitará crear otro documento xml extrayendo el elemento de datos adecuado.

La siguiente captura de pantalla muestra los argumentos que debe pasar al paso de proceso personalizado
![paso del proceso](assets/create-xml-process-step.png)
Los siguientes son los parámetros
* Data.xml: el archivo xml del que desea extraer el nodo
* datatomerge.xml - El nuevo xml creado con el nodo extraído
* /afData/afUnboundData/data: el nodo que se va a extraer


La siguiente captura de pantalla muestra el archivo datamerge.xml que se está creando en la carpeta de carga útil
![create-xml](assets/create-xml.png)

[El paquete personalizado se puede descargar desde aquí](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)
