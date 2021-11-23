---
title: Búsqueda e indexación en AEM as a Cloud Service
description: Obtenga información sobre los índices de búsqueda de AEM as a Cloud Service, cómo convertir AEM 6 definiciones de índice y cómo implementar índices.
version: Cloud Service
feature: Search
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8634
thumbnail: 336963.jpeg
exl-id: f752df86-27d4-4dbf-a3cb-ee97b7d9a17e
source-git-commit: 1dcb66bc3535231c89f3e7fc127688fcf96f2b61
workflow-type: tm+mt
source-wordcount: '309'
ht-degree: 2%

---

# Búsqueda e indexación

Obtenga información sobre los índices de búsqueda de AEM as a Cloud Service, cómo convertir AEM 6 definiciones de índice para que sean compatibles con AEM as a Cloud Service y cómo implementar índices para AEM as a Cloud Service.

>[!VIDEO](https://video.tv.adobe.com/v/336963/?quality=12&learn=on)

## Herramienta Conversor de índices

![Herramienta Conversor de índices](./assets/index-converter.png)

Como parte de la refactorización de la base de código, utilice la variable [Herramienta Conversor de índices](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) para convertir definiciones de índice Oak personalizadas a AEM definiciones de índice compatibles as a Cloud Service.

## Actividades clave

+ Utilice la variable [Migración del flujo de trabajo de Adobe I/O](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) herramienta para migrar flujos de trabajo de procesamiento de recursos para utilizar los microservicios de Asset compute.
+ Configure un [entorno de desarrollo local](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=es) e implemente los índices personalizados. Asegúrese de que los índices actualizados estén actualizados.
+ Implemente la base de código actualizada en un entorno de desarrollo as a Cloud Service AEM y continúe validando.
+ Si se modifica un índice predeterminado **SIEMPRE** copie la definición de índice más reciente de un entorno as a Cloud Service AEM que se ejecute en la última versión. Modifique la definición del índice copiado para adaptarla a sus necesidades.

## Ejercicio práctico

Aplique sus conocimientos probando lo que ha aprendido con este ejercicio práctico.

Antes de intentar el ejercicio práctico, asegúrese de haber visto y comprendido el vídeo de arriba y los siguientes materiales:

+ [Pensar de forma diferente en AEM as a Cloud Service](./introduction.md)
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
            <div style="font-size:1.25rem;font-weight:400;">Manos con índices</div>
            <p style="margin:1rem 0">
                Explore la definición e implementación de índices Oak para AEM as a Cloud Service.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session7-indexes#cloud-acceleration-bootcamp---session-7-search-and-indexing" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Pruebe la indexación</span>
            </a>
        </td>
    </tr>
</table>
