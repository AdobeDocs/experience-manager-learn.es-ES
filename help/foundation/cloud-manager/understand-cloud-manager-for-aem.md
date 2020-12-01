---
title: Comprender Adobe Cloud Manager
description: Adobe Cloud Manager proporciona una solución sencilla pero sólida que permite una administración sencilla, introspectiva y autoservicio de los entornos de AEM.
sub-product: cloud-manager, foundation
feature: pipelines, programs, projects, quality-gates, reports
topics: best-practices, cicd, development, operations, governance
doc-type: feature video
activity: understand
audience: developer, implementer, administrator, architect
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '531'
ht-degree: 4%

---


# Comprender Adobe Cloud Manager

Adobe Cloud Manager proporciona una solución sencilla pero sólida que permite una administración sencilla, introspectiva y autoservicio de los entornos de AEM.

## Información general de Cloud Manager

En esta serie de vídeos se analizan las funciones clave de Cloud Manager para AEM, entre las que se incluyen:

* [Programas](#programs)
* [Entornos](#environments)
* [Informes](#reports)
* [Canalización de producción de CI/CD](#cicd-production-pipeline)
* [Cerdos no productivos de CI/CD](#cicd-non-production-pipeline)
* [Actividad](#activity)

Para obtener una descripción general completa, consulte la [Guía del usuario del Administrador de nube](https://docs.adobe.com/content/help/es-ES/experience-manager-cloud-manager/using/introduction-to-cloud-manager.html).

## Programas {#programs}

[Los ](https://docs.adobe.com/content/help/es-ES/experience-manager-cloud-manager/using/getting-started/setting-up-program.html) programas de Cloud Manager representan conjuntos de entornos de AEM que admiten conjuntos lógicos de iniciativas comerciales, que normalmente corresponden a un acuerdo de nivel de servicio (SLA) adquirido. Por ejemplo, un Programa puede representar los recursos AEM para admitir los sitios Web públicos globales, mientras que otro Programa representa un DAM central interno.

>[!VIDEO](https://video.tv.adobe.com/v/26313/?quality=12&learn=on)

## Entornos {#environments}

[Los ](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/manage-your-environment.html) entornos de Cloud Manager están compuestos por instancias de AEM Author, AEM Publish y Dispatcher. Diferentes entornos admiten funciones y pueden participar utilizando diferentes canalizaciones CI/CD (se describe a continuación). Los entornos de Cloud Manager suelen tener un entorno de producción y un entorno de fase.

>[!VIDEO](https://video.tv.adobe.com/v/26318/?quality=12&learn=on)

## Informes {#reports}

[Los ](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/monitor-your-environments.html) informes de Cloud Manager proporcionan una vista en los Entornos y las instancias de AEM del Programa a través de un conjunto de gráficos que informan y rastrean una variedad de métricas para cada instancia de AEM.

>[!VIDEO](https://video.tv.adobe.com/v/26315/?quality=12&learn=on)

## Canalización de producción de CI/CD {#cicd-production-pipeline}

*[Utilice la canalización CI/CD de la serie de vídeo de Adobe Cloud ](./use-the-cicd-pipeline-in-cloud-manager-for-aem.md) Manager para profundizar en la ejecución de la canalización de producción, incluida la exploración de las implementaciones que han fallado y las que han tenido éxito.*

>[!NOTE]
>
> A lo largo de estos vídeos, los tiempos de compilación, prueba e implementación se han acelerado para reducir el tiempo del vídeo. La ejecución completa de la canalización suele tardar 45 minutos o más (incluida la prueba de rendimiento obligatoria de 30 minutos), según el tamaño del proyecto, el número de instancias de AEM y los procesos de UAT.

### Configuración

La configuración [CI/CD Production Pipeline](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/configuring-pipeline.html) define el activador que iniciará la canalización, los parámetros que controlan la implementación de producción y los parámetros de prueba de rendimiento.

>[!VIDEO](https://video.tv.adobe.com/v/26314/?quality=12&learn=on)

### Ejecución de canalización

La [tubería de producción de CI/CD](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/deploying-code.html) se utiliza para generar e implementar código a través de Stage en el entorno de producción, lo que reduce el tiempo de respuesta al valor.

>[!VIDEO](https://video.tv.adobe.com/v/26317/?quality=12&learn=on)

## Tuberías de no producción de CI/CD {#cicd-non-production-pipeline}

[Las ](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/configuring-pipeline.html#non-production--code-quality-only-pipelines) tuberías de CI/CD que no son de producción se dividen en dos categorías: canalizaciones de calidad del código y tuberías de implementación. La calidad del código canaliza todo el código desde una rama Git para crear y evaluarse en función del análisis de calidad del código del Administrador de nube. Las tuberías de implementación admiten la implementación automatizada de código desde el repositorio Git a cualquier entorno que no sea de producción, lo que significa cualquier entorno de AEM suministrado que no sea de fase o producción.

>[!VIDEO](https://video.tv.adobe.com/v/26316/?quality=12&learn=on)

## Actividad {#activity}

Cloud Manager ofrece una vista consolidada de la actividad de un Programa, enumerando todas las ejecuciones de la canalización de CD/CI, tanto de producción como de no producción, lo que permite la visibilidad de la actividad pasada y presente, y los detalles de cualquier actividad se pueden revisar.

Cloud Manager también se integra por usuario con [Notificaciones de Adobe Experience Cloud](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/notifications.html), lo que proporciona una vista omnipresente en eventos y acciones de interés.

>[!VIDEO](https://video.tv.adobe.com/v/26319/?quality=12&learn=on)
