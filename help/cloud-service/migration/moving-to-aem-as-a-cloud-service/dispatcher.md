---
title: Configuración de Dispatcher al moverse a AEM as a Cloud Service
description: Obtenga información sobre cambios importantes en AEM Dispatcher para AEM as a Cloud Service, la herramienta de conversión de Dispatcher y cómo utilizar el SDK de herramientas de Dispatcher.
version: Cloud Service
feature: Dispatcher
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8633
thumbnail: 336962.jpeg
exl-id: 81397b21-b4f3-4024-a6da-a9b681453eff
source-git-commit: 1dcb66bc3535231c89f3e7fc127688fcf96f2b61
workflow-type: tm+mt
source-wordcount: '277'
ht-degree: 3%

---


# Dispatcher

Obtenga información sobre AEM Dispatcher para AEM as a Cloud Service, centrándose en los cambios notables de Dispatcher para AEM 6, la herramienta de conversión de Dispatcher y cómo utilizar el SDK de herramientas de Dispatcher.

>[!VIDEO](https://video.tv.adobe.com/v/336962/?quality=12&learn=on)

## Dispatcher Converter

![Dispatcher Converter](./assets/dispatcher-converter-diagram.png)

Como parte de la refactorización de la base de código, utilice la variable [AEM Dispatcher Converter](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/dispatcher-transformation-utility-tools.html) para refactorizar las configuraciones existentes in situ o Adobe Managed Services de Dispatcher a AEM configuración as a Cloud Service compatible de Dispatcher.

## Actividades clave

+ Utilice la variable [Herramienta Dispatcher Converter de Adobe I/O](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#aio-aem-migrationdispatcher-converter) para migrar una configuración de Dispatcher existente.
+ Haga referencia al módulo de Dispatcher desde el [Tipo de archivo del proyecto AEM](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud) como práctica recomendada.
+ [Configuración de las herramientas locales de Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html) para validar Dispatcher, antes de realizar pruebas en un entorno de Cloud Service.

## Ejercicio práctico

Aplique sus conocimientos probando lo que ha aprendido con este ejercicio práctico.

Antes de intentar el ejercicio práctico, asegúrese de haber visto y comprendido el vídeo de arriba y los siguientes materiales:

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
            <div style="font-size:1.25rem;font-weight:400;">Uso de las herramientas de Dispatcher</div>
            <p style="margin:1rem 0">
                Explore el uso de las herramientas de Dispatcher del SDK de AEM para validar las configuraciones de Dispatcher, así como la ejecución AEM Dispatcher localmente mediante Docker.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session5-dispatcher#cloud-acceleration-bootcamp---session-5-dispatcher" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Probar las herramientas de Dispatcher</span>
            </a>
        </td>
    </tr>
</table>
