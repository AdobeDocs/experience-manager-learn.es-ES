---
title: AEM Configuración de Dispatcher al pasar a as a Cloud Service
description: AEM AEM Obtenga información acerca de los cambios más importantes en la herramienta de conversión de Dispatcher para la creación de informes de Dispatcher para el as a Cloud Service de la, y cómo utilizar el SDK de herramientas de Dispatcher.
version: Cloud Service
feature: Dispatcher
topic: Migration, Upgrade
role: Developer
level: Experienced
jira: KT-8633
thumbnail: 336962.jpeg
exl-id: 81397b21-b4f3-4024-a6da-a9b681453eff
duration: 1634
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 4%

---


# Dispatcher

AEM AEM Obtenga información acerca de Dispatcher de para as a Cloud Service AEM, centrándose en los cambios importantes de Dispatcher para 6, la herramienta de conversión de Dispatcher y cómo utilizar el SDK de herramientas de Dispatcher.

>[!VIDEO](https://video.tv.adobe.com/v/336962?quality=12&learn=on)

## Dispatcher Converter

![Dispatcher Converter](./assets/dispatcher-converter-diagram.png)

Como parte de la refactorización del código base, utilice el [AEM Conversor de Dispatcher de](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/dispatcher-transformation-utility-tools.html) para refactorizar las configuraciones locales o de Adobe existentes de Managed Services AEM Dispatcher a una configuración de Dispatcher compatible as a Cloud Service de la aplicación.

## Actividades clave

+ Utilice el [Herramienta Dispatcher Converter de Adobe I/O](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#aio-aem-migrationdispatcher-converter) para migrar una configuración de Dispatcher existente.
+ Haga referencia al módulo de Dispatcher desde el [AEM Tipo de archivo del proyecto](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud) como práctica recomendada.
+ [Configurar las herramientas locales de Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html?lang=es) para validar dispatcher, antes de realizar la prueba en un entorno de Cloud Service.

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
                AEM AEM Explore el uso de las herramientas de Dispatcher del SDK de la para validar configuraciones de Dispatcher, así como ejecutar Dispatcher localmente mediante Docker.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session5-dispatcher#cloud-acceleration-bootcamp---session-5-dispatcher" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Pruebe las herramientas de Dispatcher</span>
            </a>
        </td>
    </tr>
</table>
