---
title: AEM Búsqueda e indexación en as a Cloud Service
description: AEM Obtenga información acerca de los índices de búsqueda de los as a Cloud Service AEM de, cómo convertir las definiciones de índice de 6 y cómo implementar los índices.
version: Cloud Service
feature: Search
topic: Migration, Upgrade
role: Developer
level: Experienced
jira: KT-8634
thumbnail: 336963.jpeg
exl-id: f752df86-27d4-4dbf-a3cb-ee97b7d9a17e
duration: 1231
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '297'
ht-degree: 0%

---

# Búsqueda e indexación

AEM Obtenga información acerca de los índices de búsqueda de as a Cloud Service AEM AEM, cómo convertir las definiciones de índice de 6 de forma que sean compatibles con el as a Cloud Service AEM y cómo implementar índices para que se puedan usar en las definiciones de índice de as a Cloud Service.

>[!VIDEO](https://video.tv.adobe.com/v/336963?quality=12&learn=on)

## Herramienta Conversor de índices

![Herramienta Conversor de índices](./assets/index-converter.png)

Como parte de la refactorización del código base, utilice el [Herramienta convertidor de índices](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) AEM para convertir definiciones de índice de Oak personalizadas en definiciones de índice compatibles con el as a Cloud Service.

Revise la [documentación del convertidor de índices](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/refactoring-tools/index-converter.html) para obtener el conjunto completo y actual de funcionalidades de Index Converter.

## Actividades clave

+ Utilice el [Migrador de flujo de trabajo Adobe I/O](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) herramienta para migrar flujos de trabajo de procesamiento de recursos para utilizar los microservicios de Asset compute.
+ Configuración de un [entorno de desarrollo local](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=es) e implementar los índices personalizados. Asegúrese de que los índices actualizados estén actualizados.
+ AEM Implemente la base de código actualizada en un entorno de desarrollo as a Cloud Service de y siga validando.
+ Si se modifica un índice predeterminado **SIEMPRE** AEM copie la última definición de índice de un entorno as a Cloud Service que se ejecute en la última versión. Modifique la definición del índice copiado para adaptarla a sus necesidades.

## Ejercicio práctico

Aplique sus conocimientos probando lo que ha aprendido con este ejercicio práctico.

Antes de probar el ejercicio práctico, asegúrese de haber visto y entendido el vídeo anterior y los siguientes materiales:

+ [AEM Pensar de manera diferente sobre el as a Cloud Service](./introduction.md)
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
                AEM Explore la definición e implementación de índices Oak para as a Cloud Service.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session7-indexes#cloud-acceleration-bootcamp---session-7-search-and-indexing" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Pruebe la indexación</span>
            </a>
        </td>
    </tr>
</table>
