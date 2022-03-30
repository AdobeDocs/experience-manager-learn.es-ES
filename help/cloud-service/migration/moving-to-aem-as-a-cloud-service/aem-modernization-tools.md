---
title: Uso de AEM herramientas de modernización para pasar a AEM as a Cloud Service
description: Descubra cómo AEM Herramientas de modernización se utilizan para actualizar un proyecto y contenido de AEM existente para que sean compatibles con AEM as a Cloud Service.
version: Cloud Service
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8629
thumbnail: 336965.jpeg
exl-id: 310f492c-0095-4015-81a4-27d76f288138
source-git-commit: 631fef25620c84e04c012c8337c9b76613e3ad46
workflow-type: tm+mt
source-wordcount: '438'
ht-degree: 2%

---


# Herramientas de modernización de AEM

Descubra cómo AEM Herramientas de modernización se utilizan para actualizar un contenido existente de AEM Sites para que sea compatible con AEM as a Cloud Service y se ajuste a las prácticas recomendadas.

>[!VIDEO](https://video.tv.adobe.com/v/336965/?quality=12&learn=on)

## Uso de AEM herramientas de modernización

![Ciclo de vida de las herramientas de modernización AEM](./assets/aem-modernization-tools.png)

AEM herramientas de modernización convierten automáticamente las páginas de AEM existentes compuestas de plantillas estáticas heredadas, componentes de base y parsys para utilizar enfoques modernos, como plantillas editables, AEM componentes WCM principales y contenedores de diseño.

## Actividades clave

+ Clone la producción de AEM 6.x para ejecutar AEM herramientas de modernización
+ Descargue e instale el [herramientas de modernización de AEM más recientes](https://github.com/adobe/aem-modernize-tools/releases/latest) sobre el clon de producción de AEM 6.x mediante el Administrador de paquetes

+ [Conversor de estructura de página](https://opensource.adobe.com/aem-modernize-tools/pages/structure/about.html) actualiza el contenido de una página existente de una plantilla estática a una plantilla editable asignada mediante contenedores de diseño
   + Definir reglas de conversión mediante la configuración OSGi
   + Ejecutar el conversor de estructura de página con páginas existentes

+ [Conversor de componentes](https://opensource.adobe.com/aem-modernize-tools/pages/component/about.html) actualiza el contenido de una página existente de una plantilla estática a una plantilla editable asignada mediante contenedores de diseño
   + Definir reglas de conversión mediante definiciones de nodos JCR/XML
   + Ejecutar la herramienta Conversor de componentes en páginas existentes

+ [Importador de políticas](https://opensource.adobe.com/aem-modernize-tools/pages/policy/about.html) crea directivas a partir de la configuración de diseño
   + Definir reglas de conversión utilizando definiciones de nodo JCR/XML
   + Ejecutar el Importador de directivas con definiciones de diseño existentes
   + Aplicar políticas importadas a componentes y contenedores de AEM

## Ejercicio práctico

Aplique sus conocimientos probando lo que ha aprendido con este ejercicio práctico.

Antes de intentar el ejercicio práctico, asegúrese de haber visto y comprendido el vídeo de arriba y los siguientes materiales:

+ [Pensar de forma diferente en AEM as a Cloud Service](./introduction.md)
+ [Modernización del repositorio](./repository-modernization.md)
+ [Contenido mutable e inmutable](../../developing/basics/mutable-immutable.md)
+ [AEM estructura del proyecto](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html?lang=es)

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
            <div style="font-size:1.25rem;font-weight:400;">Manos a mano con AEM modernización</div>
            <p style="margin:1rem 0">
                Explore el uso de AEM Herramientas de modernización para actualizar un sitio WKND heredado y ajustarlo a AEM prácticas recomendadas as a Cloud Service.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session2-migration#bootcamp---session-2-migration-methodology" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Pruebe las AEM herramientas de modernización</span>
            </a>
        </td>
    </tr>
</table>

## Otros recursos

+ [Descargar AEM herramientas de modernización](https://github.com/adobe/aem-modernize-tools/releases/latest)
+ [Documentación de las herramientas de modernización de AEM](https://opensource.adobe.com/aem-modernize-tools/)
+ [AEM Gems: presentación de AEM Modernization Suite](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/Introducing-the-AEM-Modernization-Suite.html)


1. Implemente el sitio heredado de wknd recién modernizado en el SDK de AEM local. AEM ASK está disponible para su descarga aquí:
+ [Portal de distribución de software](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html).
