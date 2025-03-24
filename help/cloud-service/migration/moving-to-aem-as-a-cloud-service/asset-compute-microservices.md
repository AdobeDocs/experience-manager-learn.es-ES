---
title: AEM Assets Microservicios y paso a AEM as a Cloud Service
description: AEM Assets Descubra cómo los microservicios de Asset Compute de as a Cloud Service le permiten generar de forma automática y eficaz cualquier representación de sus recursos, sustituyendo esta función del flujo de trabajo tradicional de AEM.
version: Experience Manager as a Cloud Service
feature: Asset Compute Microservices
topic: Migration, Upgrade
role: Developer
level: Experienced
jira: KT-8635
thumbnail: 336990.jpeg
exl-id: 327e8663-086b-4b31-b159-a0cf30480b45
duration: 973
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '291'
ht-degree: 1%

---

# Microservicios de AEM Assets: paso a AEM as a Cloud Service

AEM Assets Descubra cómo los microservicios de Asset Compute de as a Cloud Service le permiten generar de forma automática y eficaz cualquier representación de sus recursos, sustituyendo esta función del flujo de trabajo tradicional de AEM.

>[!VIDEO](https://video.tv.adobe.com/v/336990?quality=12&learn=on)

## Herramienta de migración de flujos de trabajo

![Herramienta de migración del flujo de trabajo de recursos](./assets/asset-workflow-migration.png)

Como parte de la refactorización del código base, use la [herramienta de migración del flujo de trabajo de recursos](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/asset-workflow-migration-tool.html?lang=es) para migrar los flujos de trabajo existentes y usar los microservicios de Asset Compute en AEM as a Cloud Service.

## Actividades clave

+ Utilice la herramienta [Adobe I/O Workflow Migrator](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationworkflow-migrator) para migrar flujos de trabajo de procesamiento de recursos y utilizar los microservicios de Asset Compute.
+ Configure un [entorno de desarrollo local](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=es) e implemente los flujos de trabajo actualizados. Puede ser necesario un ajuste manual para flujos de trabajo complejos.
+ Continúe iterando en un entorno de desarrollo local utilizando AEM SDK hasta que el flujo de trabajo actualizado coincida con la paridad de características.
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
                Explore cómo definir y asignar perfiles de procesamiento de AEM Assets a carpetas y cargar recursos en AEM mediante el módulo CLI aem-upload.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session8-assets#cloud-acceleration-bootcamp---session-8-assets-and-microservices" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Pruebe la administración de recursos</span>
            </a>
        </td>
    </tr>
</table>
