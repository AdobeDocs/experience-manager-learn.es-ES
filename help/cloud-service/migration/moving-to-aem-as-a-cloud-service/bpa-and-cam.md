---
title: Configure su proyecto BPA y CAM
description: Descubra cómo Best Practice Analyzer y Cloud Acceleration Manager proporcionan una guía personalizada para migrar a AEM as a Cloud Service.
version: Cloud Service
feature: Developer Tools
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8627
thumbnail: 336957.jpeg
exl-id: f8289dd4-b293-4b8f-b14d-daec091728c0
source-git-commit: 1ddf6154d50a341d9a0fd4234392c37ced878a73
workflow-type: tm+mt
source-wordcount: '234'
ht-degree: 2%

---

# Analizador de prácticas recomendadas y Cloud Acceleration Manager

Descubra cómo Best Practice Analyzer (BPA) y Cloud Acceleration Manager (CAM) proporcionan una guía personalizada para migrar a AEM as a Cloud Service. 

>[!VIDEO](https://video.tv.adobe.com/v/336957/?quality=12&learn=on)

## Evaluar preparación

![Diagrama de alto nivel de BPA y CAM](assets/bpa-cam-diagram.png)

El paquete BPA debe instalarse en un clon del entorno de producción AEM 6.x. El BPA generará un informe que luego podrá cargarse en el CAM, que proporcionará orientación sobre las actividades clave que deben llevarse a cabo para pasar a AEM as a Cloud Service.

### Actividades clave

* Haga un clon de su entorno de producción 6.x. A medida que migra contenido y refactoriza código, tener un clon de un entorno de producción será valioso para probar varias herramientas y cambios.
* Descargue la herramienta de BPA más reciente de [Portal de distribución de software](https://experience.adobe.com/#/downloads/content/software-distribution/es-ES/aemcloud.html) e instale en su entorno clonado con AEM 6.x.
* Utilice la herramienta BPA para generar un informe que se pueda cargar en Cloud Acceleration Manager (CAM). Se accede a CAM a través de [https://experience.adobe.com/](https://experience.adobe.com/) > **Experience Manager** > **Cloud Acceleration Manager**.
* Utilice CAM para proporcionar instrucciones sobre las actualizaciones que se deben realizar en la base de código y el entorno actuales para poder pasar a AEM as a Cloud Service.
