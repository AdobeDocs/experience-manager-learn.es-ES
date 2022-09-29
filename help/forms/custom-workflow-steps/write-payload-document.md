---
title: Escribir el documento de carga útil en el sistema de archivos
description: Paso de proceso personalizado para agregar al sistema de archivos el tamaño del documento de escritura en la carpeta de carga útil
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-9859
exl-id: bab7c403-ba42-4a91-8c86-90b43ca6026c
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '140'
ht-degree: 0%

---

# Escribir el documento en el sistema de archivos

El caso de uso habitual es escribir los documentos generados en el flujo de trabajo en el sistema de archivos.
Este paso personalizado del proceso del flujo de trabajo facilita la escritura de los documentos del flujo de trabajo en el sistema de archivos.
El proceso personalizado toma los siguientes argumentos separados por comas

```java
ChangeBeneficiary.pdf,c:\confirmation
```

El primer argumento es el nombre del documento que desea guardar en el sistema de archivos. El segundo argumento es la ubicación de la carpeta en la que desea guardar el documento. Por ejemplo, en el caso de uso anterior, el documento se escribe en `c:\confirmation\ChangeBeneficiary.pdf`

La siguiente captura de pantalla muestra los argumentos que debe pasar al paso de proceso personalizado
![write-payload-file-system](assets/write-payload-file-system.png)

[El paquete personalizado se puede descargar desde aquí](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)
