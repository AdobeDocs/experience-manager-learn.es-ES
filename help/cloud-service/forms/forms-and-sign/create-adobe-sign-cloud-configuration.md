---
title: Crear Cloud Service de configuración de Acrobat Sign Cloud
description: Cree la integración de AEM Forms y Acrobat Sign con la configuración de los servicios en la nube.
solution: Experience Manager,Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-7428
thumbnail: 332437.jpg
badgeIntegration: label="Integración" type="positive"
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: a55773a5-0486-413f-ada6-bb589315f0b1
duration: 229
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '155'
ht-degree: 1%

---

# Crear configuración de nube de Acrobat Sign

AEM AEM La configuración de servicios en la nube de le permite crear servicios de integración entre y otras aplicaciones de la nube de.

AEM El siguiente vídeo le guiará por los pasos necesarios para crear la configuración de servicios en la nube para la integración con Acrobat Sign para que se integre de forma rápida y sencilla con el servicio en la nube para su integración con la plataforma de datos de

>[!VIDEO](https://video.tv.adobe.com/v/332437?quality=12&learn=on)

## Solución de problemas

Si se produce un error al configurar la clonación de nube de Adobe Sign, se pueden realizar los siguientes pasos para solucionar los problemas
* Asegúrese de que la URL de redireccionamiento especificada en la aplicación API de Acrobat Sign tenga el siguiente formato
&lt;your instance=&quot;&quot; name=&quot;&quot;>/libs/adobesign/cloudservices/adobesign/createcloudconfigwizard/cloudservices.html/conf/&lt;container>.
Por ejemplo: https://author-p24107-e32034.adobeaemcloud.com/libs/adobesign/cloudservices/adobesign/createcloudconfigwizard/cloudservices.html/conf/FormsCS. FormsCS es el nombre del contenedor que contendrá la configuración de la nube
* Asegúrese de que la URL de oAuth sea correcta
* Compruebe el ID de cliente y el secreto de cliente
* Pruebe el modo de ventana de incógnito

