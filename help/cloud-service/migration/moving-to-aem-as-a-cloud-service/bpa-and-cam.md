---
title: Configuración del proyecto de BPA y CAM
description: AEM Descubra cómo el Analizador de prácticas recomendadas y Cloud Acceleration Manager proporcionan una guía personalizada para migrar a la as a Cloud Service de la.
version: Cloud Service
feature: Developer Tools
topic: Migration, Upgrade
role: Developer
level: Experienced
jira: KT-8627
thumbnail: 336957.jpeg
exl-id: f8289dd4-b293-4b8f-b14d-daec091728c0
duration: 680
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '350'
ht-degree: 2%

---

# Analizador de prácticas recomendadas y Cloud Acceleration Manager

AEM Descubra cómo el Analizador de prácticas recomendadas (BPA) y Cloud Acceleration Manager (CAM) proporcionan una guía personalizada para migrar a as a Cloud Service. 

>[!VIDEO](https://video.tv.adobe.com/v/336957?quality=12&learn=on)

## Uso de BPA y CAM

![Diagrama de alto nivel de BPA y CAM](assets/bpa-cam-diagram.png)

AEM El paquete BPA debe instalarse en un clon del entorno de producción de la versión 6.x de la. AEM El BPA generará un informe que se puede cargar en el CAM, que proporcionará orientación sobre las actividades clave que deben llevarse a cabo para pasar a la fase as a Cloud Service de la.

## Actividades clave

+ Cree un clon del entorno de producción 6.x. A medida que migra contenido y refactoriza código, tener un clon de un entorno de producción es valioso para probar varias herramientas y cambios.
+ Descargue la herramienta BPA más reciente de [Portal de distribución de software](https://experience.adobe.com/#/downloads/content/software-distribution/es-es/aemcloud.html) AEM e instale en su entorno clonado de 6.x.
+ Utilice la herramienta BPA para generar un informe que se pueda cargar en Cloud Acceleration Manager (CAM). Se accede a CAM a través de [https://experience.adobe.com/](https://experience.adobe.com/) > **Experience Manager** > **Cloud Acceleration Manager**.
+ AEM Utilice CAM para proporcionar orientación sobre las actualizaciones que se deben realizar en la base de código y el entorno actuales para pasar a la versión as a Cloud Service de la.

## Ejercicio práctico

Aplique sus conocimientos probando lo que ha aprendido con este ejercicio práctico.

Antes de probar el ejercicio práctico, asegúrese de haber visto y entendido el vídeo anterior y los siguientes materiales:

+ [AEM Pensar de manera diferente sobre el as a Cloud Service](./introduction.md)
+ [AEM ¿Qué está as a Cloud Service?](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/introduction/what-is-aem-as-a-cloud-service.html?lang=en)
+ [Arquitectura de AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/introduction/architecture.html?lang=en)
+ [Contenido mutable e inmutable](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/mutable-immutable.html?lang=en)
+ [AEM Diferencias en el desarrollo para la as a Cloud Service AEM y la 6.x](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html#developing)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session1-differently#bootcamp---session-1-introduction-and-thinking-differently"><img alt="Repositorio de GitHub de ejercicios prácticos" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">Prácticas con el Analizador de prácticas recomendadas</div>
            <p style="margin:1rem 0">
                Explore el Analizador de prácticas recomendadas (BPA) y revise los resultados ejecutándolo con una base de código de WKND heredado que contenga infracciones de ejemplo.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session1-differently#bootcamp---session-1-introduction-and-thinking-differently" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Pruebe el Analizador de prácticas recomendadas</span>
            </a>
        </td>
    </tr>
</table>


## Otros recursos

+ [Descargar el Analizador de prácticas recomendadas](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=Best*+Practices*+Analyzer*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)