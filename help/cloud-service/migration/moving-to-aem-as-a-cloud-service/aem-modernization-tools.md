---
title: Uso de las herramientas de modernización de AEM para pasar a AEM as a Cloud Service
description: Descubra cómo se utilizan las herramientas de modernización de AEM para actualizar un proyecto y contenido de AEM existente para que sea compatible con AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
topic: Migration, Upgrade
feature: Migration
role: Developer
level: Experienced
jira: KT-8629
thumbnail: 336965.jpeg
exl-id: 310f492c-0095-4015-81a4-27d76f288138
duration: 2502
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '385'
ht-degree: 1%

---


# Herramientas de modernización de AEM

Descubra cómo se utilizan las herramientas de modernización de AEM para actualizar un contenido de AEM Sites existente para que sea compatible con AEM as a Cloud Service y se ajuste a las prácticas recomendadas.

## Conversor todo en uno

>[!VIDEO](https://video.tv.adobe.com/v/338802?quality=12&learn=on)

## Conversión de página

>[!VIDEO](https://video.tv.adobe.com/v/338799?quality=12&learn=on)

## Conversión de componentes

>[!VIDEO](https://video.tv.adobe.com/v/338788?quality=12&learn=on)

## Importación de directivas

>[!VIDEO](https://video.tv.adobe.com/v/338797?quality=12&learn=on)

## Uso de las herramientas de modernización de AEM

![Ciclo de vida de las herramientas de modernización AEM](./assets/aem-modernization-tools.png)

Las herramientas de modernización de AEM convierten automáticamente las páginas de AEM existentes compuestas de plantillas estáticas heredadas, componentes de base y parsys para utilizar enfoques modernos como plantillas editables, componentes de WCM principales de AEM y contenedores de diseño.

## Actividades clave

+ Clone la producción de AEM 6.x para ejecutar las herramientas de modernización de AEM con
+ Descargue e instale las [últimas herramientas de modernización de AEM](https://github.com/adobe/aem-modernize-tools/releases/latest) en el clon de producción de AEM 6.x a través del Administrador de paquetes

+ [Conversor de estructura de página](https://opensource.adobe.com/aem-modernize-tools/pages/structure/about.html) actualiza el contenido de página existente de una plantilla estática a una plantilla editable asignada mediante contenedores de diseño
   + Definir reglas de conversión mediante la configuración OSGi
   + Ejecutar el conversor de estructura de página para páginas existentes

+ [Conversor de componentes](https://opensource.adobe.com/aem-modernize-tools/pages/component/about.html) actualiza el contenido de página existente de una plantilla estática a una plantilla editable asignada mediante contenedores de diseño
   + Defina las reglas de conversión a través de las definiciones/XML del nodo JCR
   + Ejecute la herramienta Conversor de componentes con páginas existentes

+ [Importador de directivas](https://opensource.adobe.com/aem-modernize-tools/pages/policy/about.html) crea directivas a partir de la configuración de diseño
   + Definir reglas de conversión mediante definiciones de nodo JCR/XML
   + Ejecutar el importador de directivas con definiciones de diseño existentes
   + Aplicar directivas importadas a componentes y contenedores de AEM

## Ejercicio práctico

Aplique sus conocimientos probando lo que ha aprendido con este ejercicio práctico.

Antes de probar el ejercicio práctico, asegúrese de haber visto y entendido el vídeo anterior y los siguientes materiales:

+ [Pensar de forma diferente sobre AEM as a Cloud Service](./introduction.md)
+ [Modernización de repositorios](./repository-modernization.md)
+ [Contenido mutable e inmutable](../../developing/basics/mutable-immutable.md)
+ [Estructura del proyecto AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html?lang=es)

Además, asegúrese de haber completado el ejercicio práctico anterior:

+ [Ejercicio práctico de BPA y CAM](./bpa-and-cam.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session2-migration#bootcamp---session-2-migration-methodology"><img alt="Repositorio de GitHub de ejercicios prácticos" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">Práctica con la modernización de AEM</div>
            <p style="margin:1rem 0">
                Explore cómo utilizar las herramientas de modernización de AEM para actualizar un sitio WKND heredado y ajustarlo a las prácticas recomendadas de AEM as a Cloud Service.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session2-migration#bootcamp---session-2-migration-methodology" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Pruebe las herramientas de modernización de AEM</span>
            </a>
        </td>
    </tr>
</table>

## Otros recursos

+ [Descargar herramientas de modernización de AEM](https://github.com/adobe/aem-modernize-tools/releases/latest)
+ [Documentación de herramientas de modernización de AEM](https://opensource.adobe.com/aem-modernize-tools/)
+ [Gems de AEM: presentación del conjunto de modernización de AEM](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/Introducing-the-AEM-Modernization-Suite.html)

1. Implemente el sitio wknd-legacy recién modernizado en el SDK local de AEM. AEM PREGUNTAR se puede descargar aquí:
   + [Portal de distribución de software](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html).
