---
title: Comprender Adobe Cloud Manager
description: Adobe Cloud Manager proporciona una solución sencilla pero sólida que permite una administración sencilla, introspectiva y autoservicio de los entornos AEM.
sub-product: cloud-manager, foundation
topics: best-practices, cicd, development, operations, governance
doc-type: feature video
activity: understand
audience: developer, implementer, administrator, architect
topic: Arquitectura
role: Architect
level: Beginner
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '516'
ht-degree: 3%

---


# Comprender Adobe Cloud Manager

Adobe Cloud Manager proporciona una solución sencilla pero sólida que permite una administración sencilla, introspectiva y autoservicio de los entornos AEM.

## Información general de Cloud Manager

Esta serie de vídeos explora las funciones clave de Cloud Manager para AEM, entre las que se incluyen:

* [Programas](#programs)
* [Entornos](#environments)
* [Informes](#reports)
* [Canalización de producción de CI/CD](#cicd-production-pipeline)
* [Canalizaciones no productivas CI/CD](#cicd-non-production-pipeline)
* [Actividad](#activity)

Para obtener una descripción general completa, consulte la [Guía del usuario de Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/introduction-to-cloud-manager.html?lang=es).

## Programas {#programs}

[Los ](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/getting-started/setting-up-program.html) programas de Cloud Manager representan conjuntos de entornos AEM que admiten conjuntos lógicos de iniciativas empresariales, que normalmente corresponden a un contrato de nivel de servicio (SLA) adquirido. Por ejemplo, un programa puede representar los recursos AEM para apoyar los sitios web públicos globales, mientras que otro programa representa un DAM central interno.

>[!VIDEO](https://video.tv.adobe.com/v/26313/?quality=12&learn=on)

## Entornos {#environments}

[Los ](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/manage-your-environment.html) entornos de Cloud Manager están compuestos por instancias de AEM Author, AEM Publish y Dispatcher. Los distintos entornos admiten funciones y se pueden utilizar con diferentes canalizaciones de CD/CI (descritas a continuación). Los entornos de Cloud Manager suelen tener un entorno de producción y un entorno de ensayo.

>[!VIDEO](https://video.tv.adobe.com/v/26318/?quality=12&learn=on)

## Informes {#reports}

[Los ](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/monitor-your-environments.html) informes de Cloud Manager proporcionan una vista de los entornos y las instancias de AEM del programa a través de un conjunto de gráficos que informan y rastrean una variedad de métricas para cada instancia de AEM.

>[!VIDEO](https://video.tv.adobe.com/v/26315/?quality=12&learn=on)

## Canalización de producción de CI/CD {#cicd-production-pipeline}

*[El uso de la canalización CI/CD en la serie de vídeos de Adobe Cloud ](./use-the-cicd-pipeline-in-cloud-manager-for-aem.md) Manager proporciona una inmersión profunda en la ejecución de la canalización de producción, incluida la exploración de las implementaciones fallidas y correctas.*

>[!NOTE]
>
> A lo largo de estos vídeos, los tiempos de compilación, prueba e implementación se han acelerado para reducir el tiempo de vídeo. Una ejecución completa de la canalización suele tardar 45 minutos o más (incluida la prueba de rendimiento obligatoria de 30 minutos), según el tamaño del proyecto, el número de instancias de AEM y los procesos de UAT.

### Configuración

La configuración [CI/CD Production Pipeline](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/configuring-pipeline.html) define el déclencheur que iniciará la canalización, los parámetros que controlan la implementación de producción y los parámetros de prueba de rendimiento.

>[!VIDEO](https://video.tv.adobe.com/v/26314/?quality=12&learn=on)

### Ejecución de canalización

La [Canalización de producción de CI/CD](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/deploying-code.html) se utiliza para generar e implementar código a través de Stage en el entorno de producción, lo que reduce el tiempo de respuesta al valor.

>[!VIDEO](https://video.tv.adobe.com/v/26317/?quality=12&learn=on)

## Canalizaciones no productivas CI/CD {#cicd-non-production-pipeline}

[Las ](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/configuring-pipeline.html#non-production--code-quality-only-pipelines) canalizaciones no productivas de CI/CD se dividen en dos categorías, las canalizaciones de calidad del código y las canalizaciones de implementación. La calidad del código canaliza todo el código de una rama de Git para crearlo y evaluarlo en relación con el análisis de calidad del código de Cloud Manager. Las canalizaciones de implementación admiten la implementación automatizada de código desde el repositorio Git a cualquier entorno que no sea de producción, es decir, cualquier entorno de AEM aprovisionado que no sea de fase o producción.

>[!VIDEO](https://video.tv.adobe.com/v/26316/?quality=12&learn=on)

## Actividad {#activity}

Cloud Manager proporciona una vista consolidada de la actividad de un programa, que enumera todas las ejecuciones de canalización de CI/CD, tanto de producción como de no producción, lo que permite ver la actividad actual y pasada, y se pueden revisar los detalles de cualquier actividad.

Cloud Manager también se integra a nivel de usuario con [Adobe Experience Cloud Notifications](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/notifications.html), lo que proporciona una vista omnipresente de los eventos y las acciones de interés.

>[!VIDEO](https://video.tv.adobe.com/v/26319/?quality=12&learn=on)
