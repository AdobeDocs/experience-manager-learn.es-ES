---
title: Comprender Adobe Cloud Manager
description: Adobe Cloud Manager AEM proporciona una solución sencilla pero sólida que permite una administración sencilla, introspectivas y el autoservicio de entornos de.
sub-product: Experience Manager Cloud Manager, Experience Manager
doc-type: Feature Video
topic: Architecture
feature: Cloud Manager
role: Architect
level: Beginner
exl-id: 53279cbb-70c8-4319-b5bb-9a7d350a7f72
last-substantial-update: 2022-05-10T00:00:00Z
thumbnail: understand-cloud-manager.jpg
duration: 1011
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '464'
ht-degree: 15%

---

# Comprender Adobe Cloud Manager

Adobe Cloud Manager AEM proporciona una solución sencilla pero sólida que permite una administración sencilla, introspectivas y el autoservicio de entornos de.

## Información general de Cloud Manager

En esta serie de vídeos se analizan las funciones clave de los programas de Cloud Manager AEM para la inclusión de las siguientes

* [Programas](#programs)
* [Entornos](#environments)
* [Informes](#reports)
* [Canalización de producción de CI/CD](#cicd-production-pipeline)
* [Canalizaciones de no producción de CI/CD](#cicd-non-production-pipeline)
* [Actividad](#activity)

Para obtener una descripción general completa, consulte la [Guía del usuario de Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/introduction.html?lang=es).

## Programas {#programs}

[Los programas de Cloud Manager AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/getting-started/program-setup.html) representan conjuntos de entornos que admiten conjuntos lógicos de iniciativas empresariales, que normalmente corresponden a un contrato de nivel de servicio (SLA) adquirido. AEM Por ejemplo, un programa puede representar los recursos de la para apoyar los sitios web públicos globales, mientras que otro programa representa un DAM central interno.

>[!VIDEO](https://video.tv.adobe.com/v/26313?quality=12&learn=on)

## Entornos {#environments}

[Los entornos de Cloud Manager AEM AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/managing-environments.html) están compuestos por instancias de autor de la, de Publish de la y de Dispatcher. Los distintos entornos admiten diferentes funciones y se pueden utilizar con diferentes canalizaciones de CD/CI (descritas a continuación). Los entornos de Cloud Manager suelen tener un entorno de producción y un entorno de ensayo.

>[!VIDEO](https://video.tv.adobe.com/v/26318?quality=12&learn=on)

## Informes {#reports}

[Informes de Cloud Manager AEM AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/monitoring-environments.html) proporcionan una vista de los entornos del programa y de las instancias de la aplicación a través de un conjunto de gráficos que informan y hacen un seguimiento de las distintas métricas de cada instancia de la aplicación de datos de la aplicación de datos de la aplicación de datos de la aplicación de datos de usuario.

>[!VIDEO](https://video.tv.adobe.com/v/26315?quality=12&learn=on)

## Canalización de producción de CI/CD {#cicd-production-pipeline}

*[Usar la canalización de CI/CD en la serie de vídeos Cloud Manager de Adobe](./use-the-cicd-pipeline-in-cloud-manager-for-aem.md) proporciona una explicación detallada de la ejecución de la canalización de producción, incluida la exploración de implementaciones erróneas y exitosas.*

>[!NOTE]
>
> A lo largo de estos vídeos, los tiempos de compilación, prueba e implementación se han acelerado para reducir el tiempo del vídeo. AEM Una ejecución completa de la canalización suele tardar 45 minutos o más (incluidas las pruebas de rendimiento obligatorias de 30 minutos), según el tamaño del proyecto, el número de instancias de y los procesos de UAT.

### Configuración

La configuración de [Canalización de producción de CI/CD](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/pipelines/production-pipelines.html) define el déclencheur que inicia la canalización y los parámetros que controlan la implementación de la producción y los parámetros de prueba de rendimiento.

>[!VIDEO](https://video.tv.adobe.com/v/26314?quality=12&learn=on)

### Ejecución de canalización

La [canalización de producción de CI/CD](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/code-deployment.html?lang=es) se usa para generar e implementar código a través de Stage en el entorno de producción, lo que reduce el tiempo de respuesta al valor.

>[!VIDEO](https://video.tv.adobe.com/v/26317?quality=12&learn=on)

## Canalizaciones de no producción de CI/CD {#cicd-non-production-pipeline}

[Las canalizaciones de no producción de CI/CD](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/pipelines/production-pipelines.html) se dividen en dos categorías: canalizaciones de calidad de código y canalizaciones de implementación. La calidad del código canaliza todo el código de una rama Git para crearlo y evaluarlo con el análisis de calidad del código de Cloud Manager. Las canalizaciones de implementación admiten la implementación automatizada de código desde el repositorio Git a cualquier entorno que no sea de producción, es decir, cualquier entorno de AEM aprovisionado que no sea de fase o producción.

>[!VIDEO](https://video.tv.adobe.com/v/26316?quality=12&learn=on)

## Actividad {#activity}

Cloud Manager proporciona una vista consolidada de la actividad de un programa, que enumera todas las ejecuciones de la canalización de CI/CD, tanto de producción como de no producción, lo que permite ver la actividad actual y pasada, y se pueden revisar los detalles de cualquier actividad.

Cloud Manager también se integra por usuario con [Notificaciones de Adobe Experience Cloud](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/notifications.html), lo que proporciona una vista omnipresente de los eventos y las acciones de interés.

>[!VIDEO](https://video.tv.adobe.com/v/26319?quality=12&learn=on)
