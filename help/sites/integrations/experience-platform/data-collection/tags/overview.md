---
title: Integración de etiquetas de recopilación de datos de Experience Platform AEM (Launch) y
description: Las etiquetas en la recopilación de datos de Experience Platform son la solución de administración de etiquetas de próxima generación de Adobe y la mejor manera de implementar Adobe Analytics, Target, Audience Manager y muchas más soluciones. Obtenga información general sobre las etiquetas (anteriormente, Launch) y la integración recomendada con Adobe Experience Manager.
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
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '345'
ht-degree: 3%

---

# Integración de etiquetas y etiquetas de recopilación de datos de Experience PlatformAEM {#overview}

Aprenda a integrar el Experience Platform _Etiquetas de recopilación de datos_ (anteriormente conocido como Launch) con Adobe Experience Manager.

>[!NOTE]
>
>Adobe Experience Platform Launch se ha convertido en un conjunto de tecnologías de recopilación de datos en Adobe Experience Platform. Como resultado, se han implementado varios cambios terminológicos en la documentación del producto. Consulte lo siguiente [documento](https://experienceleague.adobe.com/docs/experience-platform/tags/term-updates.html) para obtener una referencia consolidada de los cambios terminológicos.


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
+ AEM En, cree una configuración de servicios en la nube de Launch, luego aplíquela a un sitio existente y, finalmente, compruebe que la propiedad Etiquetas y sus bibliotecas se cargan en el sitio Publicado o Autor.

## Pasos siguientes

[Crear una propiedad de etiqueta](create-tag-property.md)

## Recursos adicionales {#additional-resources}

+ [Integraciones de Experience Platform con aplicaciones de Experience Cloud](https://experienceleague.adobe.com/docs/platform-learn/tutorials/intro-to-platform/integrations-with-experience-cloud-applications.html)
+ [Información general sobre etiquetas](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)
+ [Implementación del Experience Cloud de en sitios web con etiquetas](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/overview.html)
