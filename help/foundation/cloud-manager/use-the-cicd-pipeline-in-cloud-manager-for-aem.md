---
title: Uso de la canalización de CI/CD en Adobe Cloud Manager
description: Adobe Cloud Manager proporciona una canalización de CD/CI de autoservicio sencilla pero flexible que permite a los equipos de AEM proyectos implementar código de forma rápida, segura y consistente en todos los entornos AEM alojados en AMS. En esta serie de vídeos se analiza la configuración y ejecución de la canalización de CI/CD de Cloud Manager en los casos de error y de éxito.
sub-product: Experience Manager Cloud Manager, Experience Manager
topics: cicd, performance, best-practices, development, governance
doc-type: feature video
activity: understand
audience: all
topic: Architecture
role: Developer
level: Beginner
exl-id: d5d59ef5-9343-4ac2-9053-a010decdb9b6
last-substantial-update: 2022-08-15T00:00:00Z
thumbnail: cm-pipeline.jpg
source-git-commit: 2f02a4e202390434de831ce1547001b2cef01562
workflow-type: tm+mt
source-wordcount: '312'
ht-degree: 0%

---

# Uso de la canalización de CI/CD en Adobe Cloud Manager

Adobe Cloud Manager proporciona una canalización de CD/CI de autoservicio sencilla pero flexible que permite a los equipos de AEM proyectos implementar código de forma rápida, segura y consistente en todos los entornos AEM alojados en AMS. En esta serie de vídeos se analiza la configuración y ejecución de la canalización de CI/CD de Cloud Manager en los casos de error y de éxito.

## Introducción

Introducción rápida a Cloud Manager y a los programas de Cloud Manager.

>[!NOTE]
>
>A lo largo de estos vídeos, los tiempos de compilación, prueba e implementación se han acelerado para reducir el tiempo que tarda el vídeo. Una ejecución completa de la canalización suele tardar 45 minutos o más (incluida la prueba de rendimiento obligatoria de 30 minutos), según el tamaño del proyecto, el número de instancias de AEM y los procesos de UAT.

>[!VIDEO](https://video.tv.adobe.com/v/23082/?quality=12&learn=on)

## Configuración de la canalización CI/CD

En este vídeo se analiza la configuración de la canalización para el programa en Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/23083/?quality=12&learn=on)

## Ejecución de canalización fallida

En este vídeo se explora la ejecución de la canalización de CI/CD mediante el código que falla en las comprobaciones de calidad necesarias de Cloud Manager, utilizando la variable **[!DNL yellow]** ramificación del repositorio.

>[!VIDEO](https://video.tv.adobe.com/v/23084/?quality=12&learn=on)

## Ejecución correcta de la canalización

En este vídeo se analiza la ejecución correcta de la canalización de CI/CD mediante el código que pasa las comprobaciones de calidad necesarias de Cloud Manager, utilizando la variable **[!DNL master]** ramificación del repositorio.

Este vídeo también afecta al [!UICONTROL Actividad] en Cloud Manager, que permite volver a entrar en ejecuciones activas, o revisar ejecuciones completadas o fallidas.

>[!VIDEO](https://video.tv.adobe.com/v/23085/?quality=12&learn=on)

## Materiales de apoyo

* [Guía del usuario de Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/introduction.html)
* [Descarga del análisis de código [!DNL SonarQube] reglas](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/code-quality-testing.html)
   * *XLSX disponible en la parte inferior de la sección vinculada*
* [[!DNL SonarQube] Índice de reglas de Java™](https://rules.sonarsource.com/java/)
