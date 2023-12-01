---
title: Implementación mediante la canalización front-end
description: AEM Obtenga información sobre cómo crear y ejecutar una canalización front-end que genere recursos front-end e implemente en la red de distribución de contenido (CDN) integrada en as a Cloud Service.
version: Cloud Service
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
jira: KT-10689
mini-toc-levels: 1
index: y
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: d6da05e4-bd65-4625-b9a4-cad8eae3c9d7
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '701'
ht-degree: 0%

---

# Implementación mediante la canalización front-end

En este capítulo, creamos y ejecutamos una canalización front-end en Adobe Cloud Manager. Solo genera los archivos de `ui.frontend` AEM y las implementa en la CDN integrada en el as a Cloud Service de la. Por lo tanto, se aleja de la  `/etc.clientlibs` entrega de recursos front-end basada en.


## Objetivos {#objectives}

* Cree y ejecute una canalización front-end.
* Compruebe que los recursos front-end NO se entregan desde `/etc.clientlibs` pero desde un nuevo nombre de host que empiece por `https://static-`

## Uso de la canalización front-end

>[!VIDEO](https://video.tv.adobe.com/v/3409420?quality=12&learn=on)

## Requisitos previos {#prerequisites}

Este es un tutorial de varias partes y se da por hecho que los pasos descritos en la sección [AEM Actualizar proyecto de estándar](./update-project.md) se han completado.

Asegúrese de que tiene [privilegios para crear e implementar canalizaciones en Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/requirements/users-and-roles.html?lang=en#role-definitions) y [AEM acceso a un entorno as a Cloud Service de](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html).

## Cambiar nombre de canalización existente

Cambie el nombre de la canalización existente de __Implementar en desarrollo__ hasta  __Implementación de WKND de FullStack en Desarrollo__ al ir a la __Configuración__ de la pestaña __Nombre de la canalización que no es de producción__ field. Esto sirve para hacer explícito si una canalización es de pila completa o front-end con solo mirar su nombre.

![Cambiar nombre de canalización](assets/fullstack-wknd-deploy-dev-pipeline.png)


También en el __Código fuente__ , asegúrese de que los valores de los campos Repositorio y Rama de Git son correctos y de que la rama tiene los cambios de contrato de canalización front-end.

![Canalización de configuración de código fuente](assets/fullstack-wknd-source-code-config.png)


## Creación de una canalización front-end

Hasta __SOLO__ generar e implementar los recursos front-end desde el `ui.frontend` , realice los siguientes pasos:

1. En la interfaz de usuario de Cloud Manager, en __Canalizaciones__ , haga clic en __Añadir__ botón, luego seleccione __Agregar canalización que no sea de producción__ (o __Agregar canalización de producción__ AEM ) en función del entorno as a Cloud Service en el que desee implementarlo.

1. En el __Agregar canalización que no sea de producción__ diálogo, como parte de la __Configuración__ pasos, seleccione la __Canalización de implementación__ , asígnele el nombre __Implementación de WKND de FrontEnd en Desarrollo__ y haga clic en __Continuar__

![Creación de configuraciones de canalización front-end](assets/create-frontend-pipeline-configs.png)

1. Como parte de __Código fuente__ pasos, seleccione la __Código front-end__ y seleccione el entorno de __Entornos de implementación aptos__. En el __Código fuente__ Las secciones garantizan que los valores de los campos Repositorio y Rama de Git sean correctos y que la rama tenga los cambios de contrato de canalización front-end.
Y __lo más importante__ para el __Ubicación del código__ campo el valor es `/ui.frontend` y, finalmente, haga clic en __Guardar__.

![Crear código fuente de canalización front-end](assets/create-frontend-pipeline-source-code.png)


## Secuencia de implementación

* Ejecute primero el recién renombrado __Implementación de WKND de FullStack en Desarrollo__ AEM canalización para quitar los archivos clientlib de WKND del repositorio de la. AEM Y lo más importante, prepare el para el contrato de canalización front-end añadiendo __Configuración de Sling__ archivos (`SiteConfig`, `HtmlPageItemsConfig`).

![Sitio WKND sin estilo](assets/unstyled-wknd-site.png)

>[!WARNING]
>
>Después, la variable __Implementación de WKND de FullStack en Desarrollo__ finalización de la canalización tendrá un __sin estilo__ Sitio WKND, que puede aparecer dañado. Planee una interrupción o implemente durante las horas impares, se trata de una interrupción única que debe planificar durante el cambio inicial desde el uso de una canalización de pila completa única hasta la canalización front-end.


* Finalmente, ejecute el __Implementación de WKND de FrontEnd en Desarrollo__ canalización solo para generar `ui.frontend` e implemente los recursos front-end directamente en la CDN.

>[!IMPORTANT]
>
>Observará que la variable __sin estilo__ El sitio WKND ha vuelto a la normalidad y esta vez __FrontEnd__ la ejecución de la canalización fue mucho más rápida que la canalización de pila completa.

## Verificar cambios de estilo y nuevo paradigma de entrega

* Abra cualquier página del sitio WKND y podrá ver el color del texto __Rojo Adobe__ y los archivos de recursos front-end (CSS, JS) se envían desde la CDN. El nombre de host de la solicitud de recursos empieza por `https://static-pXX-eYY.p123-e456.adobeaemcloud.com/$HASH_VALUE$/theme/site.css` y del mismo modo el site.js o cualquier otro recurso estático al que haga referencia en la `HtmlPageItemsConfig` archivo.


![Sitio WKND de nuevo estilo](assets/newly-styled-wknd-site.png)



>[!TIP]
>
>El `$HASH_VALUE$` Esto es lo mismo que se ve en el __Implementación de WKND de FrontEnd en Desarrollo__  la canalización es __HASH DE CONTENIDO__ field. AEM Se le notifica de la URL de CDN del recurso front-end, el valor se almacena en `/conf/wknd/sling:configs/com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig/jcr:content` bajo __prefixPath__ propiedad.


![Correlación de valor hash](assets/hash-value-correlartion.png)



## Enhorabuena. {#congratulations}

Ha creado, ejecutado y verificado la canalización front-end que solo crea e implementa el módulo &quot;ui.frontend&quot; del proyecto WKND Sites. AEM Ahora, su equipo front-end puede iterar rápidamente en el diseño y el comportamiento del front-end del sitio, fuera del ciclo de vida completo del proyecto de la.

## Pasos siguientes {#next-steps}

En el capítulo siguiente, [Consideraciones](considerations.md), revisará el impacto en el proceso de desarrollo del front-end y del back-end.
