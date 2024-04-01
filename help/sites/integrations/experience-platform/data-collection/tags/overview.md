---
title: Integración de etiquetas en Adobe Experience Platform AEM y
description: Las etiquetas en la recopilación de datos de Experience Platform son la solución de administración de etiquetas de próxima generación de Adobe y la mejor manera de implementar Adobe Analytics, Target, Audience Manager y muchas más soluciones. Obtenga información general sobre las etiquetas en Adobe Experience Platform y la integración recomendada con Adobe Experience Manager.
solution: Experience Manager, Data Collection, Experience Platform
jira: KT-5979
thumbnail: 39090.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
last-substantial-update: 2022-07-10T00:00:00Z
badgeIntegration: label="Integración" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: bdae56d8-96e7-4b05-9b8b-3c6c2e998bd8
duration: 256
source-git-commit: adf3fe30474bcfe5fc1a1e2a8a3d49060067726d
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 2%

---

# Integración de etiquetas y etiquetas de recopilación de datos de Experience PlatformAEM {#overview}

Obtenga información sobre cómo integrar las etiquetas en Adobe Experience Platform con Adobe Experience Manager.

Las etiquetas son la nueva generación de tecnología de administración de etiquetas de Adobe Experience Platform. Las etiquetas proporcionan la forma más sencilla de implementar Adobe Analytics, Target, Audience Manager y muchas más soluciones. Obtenga información general sobre las etiquetas y la integración recomendada con Adobe Experience Manager.

>[!VIDEO](https://video.tv.adobe.com/v/3417061?quality=12&learn=on)

## Requisitos previos

Se requiere lo siguiente al integrar las etiquetas de recopilación de datos de Experience Platform.

+ AEM AEM Acceso de administrador de a un entorno as a Cloud Service
+ Un sitio de referencia como [WKND](https://github.com/adobe/aem-guides-wknd) implementado en él.
+ Acceso a la solución de recopilación de datos de Adobe Experience Platform
+ Acceso de administrador del sistema a [Consola de Adobe Developer](https://developer.adobe.com/developer-console/)


## Pasos de alto nivel

+ En Recopilación de datos de Adobe Experience Platform, cree una propiedad Tag y edítela en _Agregar regla_. Entonces _Añadir biblioteca_, seleccione la regla recién agregada, apruébela y publíquela.
+ AEM Conexión de etiquetas y etiquetas mediante la configuración de IMS existente (o nueva)
+ AEM En, cree una configuración de servicio en la nube de etiquetas, luego aplíquela a un sitio existente y, finalmente, compruebe que la propiedad Etiquetas y sus bibliotecas se cargan en el sitio Publicado o Autor.

## Pasos siguientes

[Crear una propiedad de etiqueta](create-tag-property.md)

## Recursos adicionales {#additional-resources}

+ [Integraciones de Experience Platform con aplicaciones de Experience Cloud](https://experienceleague.adobe.com/docs/platform-learn/tutorials/intro-to-platform/integrations-with-experience-cloud-applications.html)
+ [Información general sobre etiquetas](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)
+ [Implementación del Experience Cloud de en sitios web con etiquetas](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/overview.html)
