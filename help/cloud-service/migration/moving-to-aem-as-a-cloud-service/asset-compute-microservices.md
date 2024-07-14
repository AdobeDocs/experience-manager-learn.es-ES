---
title: Microservicios de AEM Assets y paso a AEM as a Cloud Service
description: Descubra cómo los microservicios de asset compute AEM de AEM Assets as a Cloud Service le permiten generar de forma automática y eficaz cualquier representación de sus recursos, sustituyendo esta función del flujo de trabajo tradicional de la.
version: Cloud Service
feature: Asset Compute Microservices
topic: Migration, Upgrade
role: Developer
level: Experienced
jira: KT-8635
thumbnail: 336990.jpeg
exl-id: 327e8663-086b-4b31-b159-a0cf30480b45
duration: 973
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '291'
ht-degree: 1%

---

# Microservicios de AEM Assets: paso a AEM as a Cloud Service

Descubra cómo los microservicios de asset compute AEM de AEM Assets as a Cloud Service le permiten generar de forma automática y eficaz cualquier representación de sus recursos, sustituyendo esta función del flujo de trabajo tradicional de la.

>[!VIDEO](https://video.tv.adobe.com/v/336990?quality=12&learn=on)

## Herramienta de migración de flujos de trabajo

![Herramienta de migración del flujo de trabajo de recursos](./assets/asset-workflow-migration.png)

Como parte de la refactorización del código base, use la [herramienta de migración del flujo de trabajo de recursos](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/asset-workflow-migration-tool.html?lang=es) para migrar los flujos de trabajo existentes y usar los microservicios de Asset compute en AEM as a Cloud Service.

## Actividades clave

+ Utilice la herramienta [Migrador de flujo de trabajo de Adobe I/O](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationworkflow-migrator) para migrar flujos de trabajo de procesamiento de recursos y utilizar los microservicios de Asset compute.
+ Configure un [entorno de desarrollo local](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=es) e implemente los flujos de trabajo actualizados. Puede ser necesario un ajuste manual para flujos de trabajo complejos.
+ AEM Siga iterando en un entorno de desarrollo local mediante el SDK de la hasta que el flujo de trabajo actualizado coincida con la paridad de características.
+ Implemente el código base actualizado en un entorno de desarrollo de AEM as a Cloud Service y continúe validando.

## Ejercicio práctico

Aplique sus conocimientos probando lo que ha aprendido con este ejercicio práctico.

Antes de probar el ejercicio práctico, asegúrese de haber visto y entendido el vídeo anterior y los siguientes materiales:

+ [Pensar de forma diferente sobre AEM as a Cloud Service](./introduction.md)
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
