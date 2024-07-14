---
title: Configuración de Dispatcher al pasar a AEM as a Cloud Service
description: AEM Obtenga información acerca de los cambios más importantes que se han realizado en Dispatcher para AEM as a Cloud Service, la herramienta de conversión de Dispatcher y cómo utilizar el SDK de herramientas de Dispatcher.
version: Cloud Service
feature: Dispatcher
topic: Migration, Upgrade
role: Developer
level: Experienced
jira: KT-8633
thumbnail: 336962.jpeg
exl-id: 81397b21-b4f3-4024-a6da-a9b681453eff
duration: 1618
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 4%

---


# Dispatcher

AEM Obtenga información acerca de Dispatcher para AEM as a Cloud Service, centrándose en los cambios más importantes de Dispatcher AEM para la 6, la herramienta de conversión de Dispatcher y cómo utilizar el SDK de herramientas de Dispatcher.

>[!VIDEO](https://video.tv.adobe.com/v/336962?quality=12&learn=on)

## Dispatcher Converter

![Dispatcher Converter](./assets/dispatcher-converter-diagram.png)

AEM Como parte de la refactorización del código base, use [Dispatcher Converter](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/dispatcher-transformation-utility-tools.html) para refactorizar las configuraciones locales o de Adobe de Managed Services Dispatcher existentes a la configuración de Dispatcher compatible con AEM as a Cloud Service.

## Actividades clave

+ Use la [herramienta Dispatcher Converter de Adobe I/O](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#aio-aem-migrationdispatcher-converter) para migrar una configuración de Dispatcher existente.
+ Como práctica recomendada, haga referencia al módulo de Dispatcher AEM desde el [Arquetipo de proyecto de](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud).
+ [Configure las herramientas locales de Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html?lang=es) para validar Dispatcher antes de realizar pruebas en un entorno de Cloud Service.

## Ejercicio práctico

Aplique sus conocimientos probando lo que ha aprendido con este ejercicio práctico.

Antes de probar el ejercicio práctico, asegúrese de haber visto y entendido el vídeo anterior y los siguientes materiales:

+ [Herramientas de modernización de AEM](./aem-modernization-tools.md)
+ [Incorporación](./onboarding.md)
+ [Cloud Manager](./cloud-manager.md)

Además, asegúrese de haber completado el ejercicio práctico anterior:

+ [Ejercicio práctico de Cloud Manager](./cloud-manager.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session5-dispatcher#cloud-acceleration-bootcamp---session-5-dispatcher"><img alt="Repositorio de GitHub de ejercicios prácticos" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">Prácticas con las herramientas de Dispatcher</div>
            <p style="margin:1rem 0">
                AEM Explore el uso de las herramientas Dispatcher del SDK de la para validar configuraciones de Dispatcher AEM, así como para ejecutar Dispatcher localmente mediante Docker.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session5-dispatcher#cloud-acceleration-bootcamp---session-5-dispatcher" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Pruebe las herramientas de Dispatcher</span>
            </a>
        </td>
    </tr>
</table>
