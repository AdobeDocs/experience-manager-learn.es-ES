---
title: Uso de la canalización de CI/CD en Adobe Cloud Manager
description: Adobe Cloud Manager proporciona una canalización de CD/CI de autoservicio sencilla pero flexible que permite a los equipos de proyectos de AEM implementar código de forma rápida, segura y coherente en todos los entornos de AEM alojados en AMS. En esta serie de vídeos se analiza la configuración y ejecución de la canalización de CI/CD de Cloud Manager en los casos de error y de éxito.
sub-product: cloud-manager, foundation
topics: cicd, performance, best-practices, development, governance
doc-type: feature video
activity: understand
audience: all
topic: Arquitectura
role: Desarrollador
level: Principiante
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '316'
ht-degree: 0%

---


# Uso de la canalización de CI/CD en Adobe Cloud Manager

Adobe Cloud Manager proporciona una canalización de CD/CI de autoservicio sencilla pero flexible que permite a los equipos de proyectos de AEM implementar código de forma rápida, segura y coherente en todos los entornos de AEM alojados en AMS. En esta serie de vídeos se analiza la configuración y ejecución de la canalización de CI/CD de Cloud Manager en los casos de error y de éxito.

## Introducción

Introducción rápida a Cloud Manager y a los programas de Cloud Manager.

>[!NOTE]
>
>A lo largo de estos vídeos, los tiempos de compilación, prueba e implementación se han acelerado para reducir el tiempo de vídeo. Una ejecución completa de la canalización suele tardar 45 minutos o más (incluida la prueba de rendimiento obligatoria de 30 minutos), según el tamaño del proyecto, el número de instancias de AEM y los procesos de UAT.

>[!VIDEO](https://video.tv.adobe.com/v/23082/?quality=12&learn=on)

## Configuración de la canalización CI/CD

En este vídeo se analiza la configuración de la canalización para el programa en Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/23083/?quality=12&learn=on)

## Ejecución de canalización fallida

En este vídeo se explora la ejecución de la canalización CI/CD mediante el código que falla en las comprobaciones de calidad necesarias de Cloud Manager, utilizando la rama del repositorio **[!DNL yellow]**.

>[!VIDEO](https://video.tv.adobe.com/v/23084/?quality=12&learn=on)

## Ejecución correcta de la canalización

En este vídeo se explora la ejecución correcta de la canalización CI/CD mediante el código que pasa las comprobaciones de calidad necesarias de Cloud Manager, utilizando la rama del repositorio **[!DNL master]**.

Este vídeo también afecta a la consola [!UICONTROL Activity] en Cloud Manager, que permite volver a entrar en ejecuciones activas o revisar las ejecuciones completadas o fallidas.

>[!VIDEO](https://video.tv.adobe.com/v/23085/?quality=12&learn=on)

## Materiales de apoyo

* [Guía del usuario de Cloud Manager](https://helpx.adobe.com/experience-manager/cloud-manager/user-guide.html)
* [Descargar  [!DNL SonarQube] paquetes de análisis de código](https://helpx.adobe.com/experience-manager/cloud-manager/using/understand-your-test-results.html#CodeQualityTesting)
   * *XLSX disponible en la parte inferior de la sección vinculada*
* [[!DNL SonarQube] Índice de reglas Java](https://rules.sonarsource.com/java/)
