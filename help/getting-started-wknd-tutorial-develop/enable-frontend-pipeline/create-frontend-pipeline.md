---
title: Implementación mediante la canalización front-end
description: Obtenga información sobre cómo crear y ejecutar una canalización front-end que genere recursos front-end e implemente en la CDN integrada en AEM as a Cloud Service.
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
duration: 225
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '685'
ht-degree: 0%

---

# Implementación mediante la canalización front-end

En este capítulo, creamos y ejecutamos una canalización front-end en Adobe Cloud Manager. Solo genera los archivos del módulo `ui.frontend` y los implementa en la CDN integrada en AEM as a Cloud Service. Por lo tanto, se aleja de la entrega de recursos del front-end basado en `/etc.clientlibs`.


## Objetivos {#objectives}

* Cree y ejecute una canalización front-end.
* Compruebe que los recursos front-end NO se entreguen desde `/etc.clientlibs` sino desde un nuevo nombre de host que comience por `https://static-`

## Uso de la canalización front-end

>[!VIDEO](https://video.tv.adobe.com/v/3409420?quality=12&learn=on)

## Requisitos previos {#prerequisites}

AEM Este es un tutorial de varias partes y se supone que se han completado los pasos descritos en [Actualizar proyecto estándar](./update-project.md).

Asegúrese de que tiene [privilegios para crear e implementar canalizaciones en Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/requirements/users-and-roles.html?lang=en#role-definitions) y [acceso a un entorno de AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html).

## Cambiar nombre de canalización existente

Cambie el nombre de la canalización existente de __Implementar en desarrollador__ a __Implementar WKND de pila completa en desarrollador__ yendo al campo __Nombre de canalización que no es de producción__ de la pestaña __Configuración__. Esto sirve para hacer explícito si una canalización es de pila completa o front-end con solo mirar su nombre.

![Cambiar nombre de canalización](assets/fullstack-wknd-deploy-dev-pipeline.png)


También en la ficha __Código Source__, asegúrese de que los valores de los campos Repositorio y Rama de Git sean correctos y de que la rama tenga los cambios de contrato de canalización front-end.

![Canalización de configuración de código Source](assets/fullstack-wknd-source-code-config.png)


## Creación de una canalización front-end

Para __SOLAMENTE__ generar e implementar los recursos front-end desde el módulo `ui.frontend`, realice los siguientes pasos:

1. En la interfaz de usuario de Cloud Manager, en la sección __Canalizaciones__, haga clic en el botón __Agregar__ y, a continuación, seleccione __Agregar canalización que no sea de producción__ (o __Agregar canalización de producción__) según el entorno de AEM as a Cloud Service en el que desee implementar.

1. En el cuadro de diálogo __Agregar canalización que no sea de producción__, como parte de los pasos de __Configuración__, seleccione la opción __Canalización de implementación__, asígnele el nombre __Implementación de WKND de FrontEnd en Desarrollo__ y haga clic en __Continuar__

![Crear configuraciones de canalización front-end](assets/create-frontend-pipeline-configs.png)

1. Como parte de los pasos de __Código Source__, seleccione la opción __Código front-end__ y elija el entorno entre __Entornos de implementación aptos__. En la sección __Código Source__, asegúrese de que los valores de los campos Repositorio y Rama de Git sean correctos y de que la rama tenga los cambios de contrato de canalización front-end.
Y __lo más importante__ para el campo __Ubicación del código__ el valor es `/ui.frontend` y, finalmente, haz clic en __Guardar__.

![Crear código Source de canalización front-end](assets/create-frontend-pipeline-source-code.png)


## Secuencia de implementación

* AEM Ejecute primero la canalización __FullStack WKND Deploy to Dev__, cuyo nombre se acaba de cambiar, para quitar los archivos clientlib de WKND del repositorio de. AEM Y lo más importante, prepare el contrato de canalización de front-end para la preparación de la agregando __archivos de configuración de Sling__ (`SiteConfig`, `HtmlPageItemsConfig`).

![Sitio WKND sin estilo](assets/unstyled-wknd-site.png)

>[!WARNING]
>
>Una vez finalizada la canalización __FullStack WKND Deploy to Dev__, tendrá un sitio WKND __sin estilo__, que podría parecer dañado. Planee una interrupción o implemente durante las horas impares, se trata de una interrupción única que debe planificar durante el cambio inicial desde el uso de una canalización de pila completa única hasta la canalización front-end.


* Finalmente, ejecute la canalización __FrontEnd WKND Deploy to Dev__ para generar solo el módulo `ui.frontend` e implementar los recursos front-end directamente en la CDN.

>[!IMPORTANT]
>
>Observará que el sitio WKND __sin estilo__ ha vuelto a la normalidad y esta vez la ejecución de la canalización __FrontEnd__ fue mucho más rápida que la canalización de pila completa.

## Verificar cambios de estilo y nuevo paradigma de entrega

* Abra cualquier página del sitio WKND y podrá ver el color del texto con __Adobe rojo__ y los archivos de recursos front-end (CSS, JS) se entregan desde la red de distribución de contenido (CDN). El nombre de host de solicitud de recursos comienza con `https://static-pXX-eYY.p123-e456.adobeaemcloud.com/$HASH_VALUE$/theme/site.css`, así como el site.js o cualquier otro recurso estático al que haga referencia en el archivo `HtmlPageItemsConfig`.


![Sitio WKND de nuevo estilo](assets/newly-styled-wknd-site.png)



>[!TIP]
>
>El `$HASH_VALUE$` aquí es el mismo que se ve en el campo __HASH DE CONTENIDO__ de la canalización __FrontEnd WKND Deploy to Dev__. AEM Se le notifica la dirección URL de CDN del recurso front-end, el valor se almacena en `/conf/wknd/sling:configs/com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig/jcr:content` en la propiedad __prefixPath__.


![Correlación de valor hash](assets/hash-value-correlartion.png)



## Enhorabuena. {#congratulations}

Ha creado, ejecutado y verificado la canalización front-end que solo crea e implementa el módulo &quot;ui.frontend&quot; del proyecto WKND Sites. AEM Ahora, su equipo front-end puede iterar rápidamente en el diseño y el comportamiento del front-end del sitio, fuera del ciclo de vida completo del proyecto de la.

## Pasos siguientes {#next-steps}

En el capítulo siguiente, [Consideraciones](considerations.md), revisará el impacto en el proceso de desarrollo del front-end y del back-end.
