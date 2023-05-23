---
title: Microservicios de AEM Assets AEM y paso a la as a Cloud Service
description: Descubra cómo los microservicios de asset compute AEM de AEM Assets as a Cloud Service le permiten generar de forma automática y eficaz cualquier representación de sus recursos, sustituyendo esta función del flujo de trabajo tradicional de la.
version: Cloud Service
feature: Asset Compute Microservices
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8635
thumbnail: 336990.jpeg
exl-id: 327e8663-086b-4b31-b159-a0cf30480b45
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '323'
ht-degree: 5%

---

# AEM Assets AEM Microservices: paso a la as a Cloud Service de la

Descubra cómo los microservicios de asset compute AEM de AEM Assets as a Cloud Service le permiten generar de forma automática y eficaz cualquier representación de sus recursos, sustituyendo esta función del flujo de trabajo tradicional de la.

>[!VIDEO](https://video.tv.adobe.com/v/336990?quality=12&learn=on)

## Herramienta de migración de flujos de trabajo

![Herramienta de migración del flujo de trabajo de recursos](./assets/asset-workflow-migration.png)

Como parte de la refactorización del código base, utilice el [Herramienta Migración de flujo de trabajo de recursos](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/asset-workflow-migration-tool.html?lang=es) para migrar los flujos de trabajo existentes y utilizar los microservicios de Asset compute AEM en as a Cloud Service.

## Actividades clave

+ Utilice el [Migrador de flujo de trabajo Adobe I/O](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationworkflow-migrator) para migrar flujos de trabajo de procesamiento de recursos y utilizar los microservicios de Asset compute.
+ Configuración de un [entorno de desarrollo local](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=es) e implementar los flujos de trabajo actualizados. Puede ser necesario un ajuste manual para flujos de trabajo complejos.
+ AEM Siga iterando en un entorno de desarrollo local mediante el SDK de la hasta que el flujo de trabajo actualizado coincida con la paridad de características.
+ AEM Implemente la base de código actualizada en un entorno de desarrollo as a Cloud Service de y siga validando.

## Ejercicio práctico

Aplique sus conocimientos probando lo que ha aprendido con este ejercicio práctico.

Antes de probar el ejercicio práctico, asegúrese de haber visto y entendido el vídeo anterior y los siguientes materiales:

+ [AEM Pensar de manera diferente sobre el as a Cloud Service](./introduction.md)
+ [Incorporación](./onboarding.md)

Además, asegúrese de haber completado el ejercicio práctico anterior:

+ [Ejercicio práctico de búsqueda e indexación](./search-and-indexing.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session8-assets#cloud-acceleration-bootcamp---session-8-assets-and-microservices"><img alt="Repositorio de GitHub de ejercicios prácticos" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">Prácticas con la carga de recursos</div>
            <p style="margin:1rem 0">
                Explore cómo definir y asignar perfiles de procesamiento de AEM Assets AEM a carpetas y cargar recursos a las carpetas mediante el módulo CLI de npm de carga de Aem.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session8-assets#cloud-acceleration-bootcamp---session-8-assets-and-microservices" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Pruebe la administración de recursos</span>
            </a>
        </td>
    </tr>
</table>
