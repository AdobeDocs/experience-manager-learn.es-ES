---
title: AEM AEM Uso de herramientas de modernización de la para pasar a la as a Cloud Service
description: AEM AEM AEM as a Cloud Service Descubra cómo se utilizan las herramientas de modernización de la para actualizar un proyecto y contenido existente para que sea compatible con la.
version: Cloud Service
topic: Migration, Upgrade
feature: Migration
role: Developer
level: Experienced
jira: KT-8629
thumbnail: 336965.jpeg
exl-id: 310f492c-0095-4015-81a4-27d76f288138
duration: 2540
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '385'
ht-degree: 1%

---


# Herramientas de modernización de AEM

AEM Descubra cómo se utilizan las herramientas de modernización de la para actualizar un contenido existente de AEM Sites AEM para que sea compatible con el as a Cloud Service y se ajuste a las prácticas recomendadas.

## Conversor todo en uno

>[!VIDEO](https://video.tv.adobe.com/v/338802?quality=12&learn=on)

## Conversión de página

>[!VIDEO](https://video.tv.adobe.com/v/338799?quality=12&learn=on)

## Conversión de componentes

>[!VIDEO](https://video.tv.adobe.com/v/338788?quality=12&learn=on)

## Importación de directivas

>[!VIDEO](https://video.tv.adobe.com/v/338797?quality=12&learn=on)

## AEM Uso De Herramientas De Modernización

![AEM Ciclo de vida de Herramientas de modernización](./assets/aem-modernization-tools.png)

AEM AEM AEM Las herramientas de modernización convierten automáticamente las páginas de la existentes compuestas de plantillas estáticas heredadas, componentes de base y parsys para utilizar enfoques modernos como plantillas editables, componentes principales de WCM y contenedores de diseño, entre otros.

## Actividades clave

+ AEM AEM Clonar producción de.x para ejecutar herramientas de modernización de la con
+ Descargue e instale [AEM Últimas herramientas de modernización de](https://github.com/adobe/aem-modernize-tools/releases/latest) AEM en el clon de producción de 6.x a través del Administrador de paquetes

+ [Conversor de estructura de página](https://opensource.adobe.com/aem-modernize-tools/pages/structure/about.html) actualiza el contenido de la página existente de una plantilla estática a una plantilla editable asignada mediante contenedores de diseño
   + Definir reglas de conversión mediante la configuración OSGi
   + Ejecutar el conversor de estructura de página para páginas existentes

+ [Conversor de componentes](https://opensource.adobe.com/aem-modernize-tools/pages/component/about.html) actualiza el contenido de la página existente de una plantilla estática a una plantilla editable asignada mediante contenedores de diseño
   + Defina las reglas de conversión a través de las definiciones/XML del nodo JCR
   + Ejecute la herramienta Conversor de componentes con páginas existentes

+ [Importador de directivas](https://opensource.adobe.com/aem-modernize-tools/pages/policy/about.html) crea directivas desde la configuración de diseño
   + Definir reglas de conversión mediante definiciones de nodo JCR/XML
   + Ejecutar el importador de directivas con definiciones de diseño existentes
   + AEM Aplicar directivas importadas a componentes y contenedores de la

## Ejercicio práctico

Aplique sus conocimientos probando lo que ha aprendido con este ejercicio práctico.

Antes de probar el ejercicio práctico, asegúrese de haber visto y entendido el vídeo anterior y los siguientes materiales:

+ [AEM Pensar de manera diferente sobre el as a Cloud Service](./introduction.md)
+ [Modernización de repositorios](./repository-modernization.md)
+ [Contenido mutable e inmutable](../../developing/basics/mutable-immutable.md)
+ [AEM estructura del proyecto de](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html?lang=es)

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
            <div style="font-size:1.25rem;font-weight:400;">AEM Práctica con la modernización de la</div>
            <p style="margin:1rem 0">
                AEM AEM Explore el uso de herramientas de modernización de para actualizar un sitio WKND heredado y ajustarlo a las prácticas recomendadas as a Cloud Service.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session2-migration#bootcamp---session-2-migration-methodology" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">AEM Pruebe las herramientas de modernización de la</span>
            </a>
        </td>
    </tr>
</table>

## Otros recursos

+ [AEM Descargar herramientas de modernización de](https://github.com/adobe/aem-modernize-tools/releases/latest)
+ [AEM Documentación de herramientas de modernización de](https://opensource.adobe.com/aem-modernize-tools/)
+ [AEM AEM Gems - Presentación de la suite de modernización de la](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/Introducing-the-AEM-Modernization-Suite.html)

1. AEM Implemente el sitio wknd-legacy recién modernizado en el SDK local de la. AEM ASK disponible para descargar aquí (en inglés):
   + [Portal de distribución de software](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html).
