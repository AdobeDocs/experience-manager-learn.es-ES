---
title: Integración de etiquetas de recopilación de datos de Experience Platform (Launch) y AEM
description: Las etiquetas de la recopilación de datos de Experience Platform son la solución de administración de etiquetas de próxima generación de Adobe y la mejor manera de implementar Adobe Analytics, Target, Audience Manager y muchas más soluciones. Obtenga información general sobre las etiquetas (anteriormente denominadas Launch) y la integración recomendada con Adobe Experience Manager.
topics: integrations
audience: administrator
solution: Experience Manager, Data Collection, Experience Platform
doc-type: technical video
activity: setup
version: Cloud Service
kt: 5979
thumbnail: 39090.jpg
topic: Integrations
role: Developer
level: Intermediate
last-substantial-update: 2022-07-10T00:00:00Z
exl-id: bdae56d8-96e7-4b05-9b8b-3c6c2e998bd8
source-git-commit: 18a72187290d26007cdc09c45a050df8f152833b
workflow-type: tm+mt
source-wordcount: '336'
ht-degree: 2%

---

# Integración de etiquetas y AEM de recopilación de datos del Experience Platform {#overview}

Aprenda a integrar el Experience Platform _Etiquetas de recopilación de datos_ (anteriormente conocido como Launch) con Adobe Experience Manager.

>[!NOTE]
>
>Adobe Experience Platform Launch se ha convertido en un conjunto de tecnologías de recopilación de datos en Adobe Experience Platform. Como resultado, se han implementado varios cambios terminológicos en la documentación del producto. Consulte lo siguiente [documento](https://experienceleague.adobe.com/docs/experience-platform/tags/term-updates.html) para una referencia consolidada de los cambios terminológicos.


Las etiquetas son la nueva generación de tecnología de administración de etiquetas de Adobe Experience Platform. Las etiquetas son la forma más sencilla de implementar Adobe Analytics, Target, Audience Manager y muchas más soluciones. Obtenga información general sobre las etiquetas y la integración recomendada con Adobe Experience Manager.

>[!VIDEO](https://video.tv.adobe.com/v/3417061?quality=12&learn=on)


## Requisitos previos

A la hora de integrar las etiquetas de recopilación de datos de Experience Platform, es necesario lo siguiente:

+ AEM acceso de administrador a AEM entorno as a Cloud Service
+ Un sitio de referencia como [WKND](https://github.com/adobe/aem-guides-wknd) implementada en ella.
+ Acceso a la solución de recopilación de datos de Adobe Experience Platform
+ Acceso del administrador del sistema a [Consola de Adobe Developer](https://developer.adobe.com/developer-console/)


## Pasos de alto nivel

+ En la recopilación de datos de Adobe Experience Platform, cree una propiedad de etiqueta y edítela a _Agregar regla_. Entonces _Agregar biblioteca_, seleccione la regla recién añadida, apruebe y publíquela.
+ Conectar AEM y etiquetas mediante la configuración de IMS existente (o nueva)
+ En AEM, cree una configuración de Launch cloud services, luego aplíquela a un sitio existente y, finalmente, verifique que la propiedad Etiquetas y sus bibliotecas se carguen en el sitio Publicado o Autor.

## Pasos siguientes

[Crear una propiedad de etiqueta](create-tag-property.md)

## Recursos adicionales {#additional-resources}

+ [Integraciones de Experience Platform con aplicaciones de Experience Cloud](https://experienceleague.adobe.com/docs/platform-learn/tutorials/intro-to-platform/integrations-with-experience-cloud-applications.html)
+ [Información general sobre etiquetas](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)
+ [Implementación del Experience Cloud en sitios web con etiquetas](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/overview.html)
