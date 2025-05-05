---
title: Búsqueda e indexación en AEM as a Cloud Service
description: Obtenga información sobre los índices de búsqueda de AEM as a Cloud Service, cómo convertir definiciones de índice de AEM 6 y cómo implementar índices.
version: Experience Manager as a Cloud Service
feature: Search
topic: Migration, Upgrade
role: Developer
level: Experienced
jira: KT-8634
thumbnail: 336963.jpeg
exl-id: f752df86-27d4-4dbf-a3cb-ee97b7d9a17e
duration: 1231
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '297'
ht-degree: 0%

---

# Búsqueda e indexación

Obtenga información sobre los índices de búsqueda de AEM as a Cloud Service, cómo convertir las definiciones de índice de AEM 6 para que sean compatibles con AEM as a Cloud Service y cómo implementar índices en AEM as a Cloud Service.

>[!VIDEO](https://video.tv.adobe.com/v/336963?quality=12&learn=on)

## Herramienta Conversor de índices

![Herramienta de conversión de índices](./assets/index-converter.png)

Como parte de la refactorización del código base, use la [herramienta de conversión de índices](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) para convertir definiciones de índices Oak personalizadas en definiciones de índices compatibles con AEM as a Cloud Service.

Revise la [documentación del convertidor de índices](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/refactoring-tools/index-converter.html?lang=es) para ver el conjunto completo y actual de funcionalidades del convertidor de índices.

## Actividades clave

+ Utilice la herramienta [Adobe I/O Workflow Migrator](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) para migrar flujos de trabajo de procesamiento de recursos y utilizar los microservicios de Asset Compute.
+ Configure un [entorno de desarrollo local](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=es) e implemente los índices personalizados. Asegúrese de que los índices actualizados estén actualizados.
+ Implemente el código base actualizado en un entorno de desarrollo de AEM as a Cloud Service y continúe validando.
+ Si modifica un índice predeterminado **SIEMPRE**, copie la definición de índice más reciente de un entorno de AEM as a Cloud Service que se ejecute en la última versión. Modifique la definición del índice copiado para adaptarla a sus necesidades.

## Ejercicio práctico

Aplique sus conocimientos probando lo que ha aprendido con este ejercicio práctico.

Antes de probar el ejercicio práctico, asegúrese de haber visto y entendido el vídeo anterior y los siguientes materiales:

+ [Pensar de forma diferente sobre AEM as a Cloud Service](./introduction.md)
+ [Modernización del repositorio](./repository-modernization.md)

Además, asegúrese de haber completado el ejercicio práctico anterior:

+ [Ejercicio práctico de la herramienta de transferencia de contenido](./content-migration/content-transfer-tool.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session7-indexes#cloud-acceleration-bootcamp---session-7-search-and-indexing"><img alt="Repositorio de GitHub de ejercicios prácticos" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">Prácticas con índices</div>
            <p style="margin:1rem 0">
                Explore la definición e implementación de índices de Oak en AEM as a Cloud Service.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session7-indexes#cloud-acceleration-bootcamp---session-7-search-and-indexing" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Probar la indexación</span>
            </a>
        </td>
    </tr>
</table>
