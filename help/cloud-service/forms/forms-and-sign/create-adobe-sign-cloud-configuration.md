---
title: Crear Cloud Service de configuración de Acrobat Sign Cloud
description: Cree la integración de AEM Forms y Acrobat Sign mediante la configuración de los servicios de nube.
solution: Experience Manager,Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 7428
thumbnail: 332437.jpg
exl-id: a55773a5-0486-413f-ada6-bb589315f0b1
source-git-commit: bd41cd9d64253413e793479b5ba900c8e01c0eab
workflow-type: tm+mt
source-wordcount: '159'
ht-degree: 0%

---

# Crear la configuración de Acrobat Sign Cloud

La configuración de Cloud services en AEM le permite crear integración entre AEM y otras aplicaciones de la nube.

El siguiente vídeo le guiará por los pasos necesarios para crear la configuración de servicios de nube para integrar AEM con Acrobat Sign

>[!VIDEO](https://video.tv.adobe.com/v/332437?quality=12&learn=on)

## Solución de problemas

Si se produce un error al configurar la configuración de nube de Abobe Sign, se pueden realizar los siguientes pasos para solucionar problemas
* Asegúrese de que la URL de redirección especificada en la aplicación de API de Acrobat Sign tenga el formato siguiente
&lt;your instance=&quot;&quot; name=&quot;&quot;>/libs/adobesign/cloudservices/adobesign/createcloudconfigwizard/cloudservices.html/conf/&lt;container>.
Por ejemplo: https://author-p24107-e32034.adobeaemcloud.com/libs/adobesign/cloudservices/adobesign/createcloudconfigwizard/cloudservices.html/conf/FormsCS. FormsCS es el nombre del contenedor que va a contener la configuración de nube
* Asegúrese de que la URL oAuth sea correcta
* Compruebe el ID de cliente y el secreto del cliente
* Probar modo de ventana incógnito

