---
title: Integración de etiquetas en Adobe Experience Platform y AEM
description: Las etiquetas en la recopilación de datos de Experience Platform son la solución de administración de etiquetas de última generación de Adobe y la mejor manera de implementar Adobe Analytics, Target, Audience Manager y muchas más soluciones. Obtenga información general sobre las etiquetas en Adobe Experience Platform y la integración recomendada con Adobe Experience Manager.
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
duration: 230
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '270'
ht-degree: 100%

---

# Integración de etiquetas de recopilación de datos de Experience Platform y AEM {#overview}

Obtenga información sobre cómo integrar las etiquetas en Adobe Experience Platform con Adobe Experience Manager.

Las etiquetas son la tecnología de administración de etiquetas de última generación de Adobe Experience Platform. Las etiquetas proporcionan la forma más sencilla de implementar Adobe Analytics, Target, Audience Manager y muchas más soluciones. Obtenga información general sobre las etiquetas y la integración recomendada con Adobe Experience Manager.

>[!VIDEO](https://video.tv.adobe.com/v/3417061?quality=12&learn=on)

## Requisitos previos

Se requiere lo siguiente al integrar etiquetas de recopilación de datos de Experience Platform.

+ Acceso de administrador de AEM a AEM as a Cloud Service
+ Se ha implementado un sitio de referencia como [WKND]( https://github.com/adobe/aem-guides-wknd).
+ Acceso a la solución de recopilación de datos de Adobe Experience Platform
+ Acceso del administrador del sistema a [Adobe Developer Console](https://developer.adobe.com/developer-console/)


## Pasos de alto nivel

+ En la recopilación de datos de Adobe Experience Platform, cree una propiedad de etiqueta y edítela en _Añadir regla_. A continuación, _agregue la biblioteca_, seleccione la regla recién agregada, apruébela y publíquela.
+ Conecte AEM y las etiquetas mediante la configuración de IMS existente (o nueva)
+ En AEM, cree una configuración de servicio en la nube de etiquetas, aplíquela a un sitio existente y, finalmente, compruebe que la propiedad de etiqueta y sus bibliotecas se cargan en el sitio Publicado o de Autor.

## Siguientes pasos

[Crear una propiedad de etiqueta](create-tag-property.md)

## Recursos adicionales {#additional-resources}

+ [Integraciones de Experience Platform con aplicaciones de Experience Cloud](https://experienceleague.adobe.com/docs/platform-learn/tutorials/intro-to-platform/integrations-with-experience-cloud-applications.html?lang=es)
+ [Información general sobre las etiquetas](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html?lang=es)
+ [Implementación de Experience Cloud en sitios web con etiquetas](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/overview.html?lang=es)
