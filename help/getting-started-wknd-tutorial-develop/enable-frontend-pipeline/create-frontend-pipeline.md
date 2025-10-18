---
title: Implementación mediante la canalización front-end
description: Obtenga información sobre cómo crear y ejecutar una canalización front-end que genere recursos front-end se implemente en la CDN integrada en AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
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
source-git-commit: 293157c296676ef1496e6f861ed8c2c24da7e068
workflow-type: tm+mt
source-wordcount: '685'
ht-degree: 100%

---

# Implementación mediante la canalización front-end

En este capítulo, creamos y ejecutamos una canalización front-end en Adobe Cloud Manager. Solo genera los archivos del módulo `ui.frontend` y los implementa en la CDN integrada en AEM as a Cloud Service. Por lo tanto, se aleja de la entrega de recursos del front-end que se basa en `/etc.clientlibs`.


## Objetivos {#objectives}

* Cree y ejecute una canalización front-end.
* Comprobar que los recursos front-end NO se entreguen desde `/etc.clientlibs` sino desde un nuevo nombre de host que comience por `https://static-`

## Uso de la canalización front-end

>[!VIDEO](https://video.tv.adobe.com/v/3409420?quality=12&learn=on)

## Requisitos previos {#prerequisites}

Este es un tutorial de varias partes y se da por hecho que los pasos descritos en [Actualizar proyecto estándar de AEM](./update-project.md) se han completado.

Asegúrese de que tiene [privilegios para crear e implementar canalizaciones en Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/requirements/users-and-roles.html?lang=es#role-definitions) y [acceso a un entorno de AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html?lang=es).

## Cambiar nombre de canalización existente

Cambie el nombre de la canalización existente de __Implementar en desarrollador__ a __Implementar WKND de pila completa en desarrollo__. Para ello, vaya al campo __Nombre de canalización que no es de producción__ de la pestaña __Configuración__. Esto sirve para hacer explícito si una canalización es de pila completa o front-end simplemente con ver su nombre.

![Cambiar nombre de canalización](assets/fullstack-wknd-deploy-dev-pipeline.png)


También en la pestaña __Código de fiente__, asegúrese de que los valores de los campos Repositorio y Rama de Git sean correctos y de que la rama tenga los cambios de contrato de canalización front-end.

![Canalización de configuración de código fuente](assets/fullstack-wknd-source-code-config.png)


## Crear una canalización front-end

Para generar e implementar __SOLAMENTE__ los recursos front-end desde el módulo `ui.frontend`, realice los pasos siguientes:

1. En la interfaz de usuario de Cloud Manager, en la sección __Canalizaciones__, haga clic en el botón __Añadir__ y, a continuación, seleccione __Añadir canalización que no sea de producción__ (o __Añadir canalización de producción__) según el entorno de AEM as a Cloud Service en el que desee realizar la implementación.

1. En el cuadro de diálogo __Añadir canalización que no sea de producción__, como parte de los pasos de __Configuración__, seleccione la opción __Canalización de implementación__, asígnele el nombre __Implementar WKND de front-end en desarrollo__ y haga clic en __Continuar__

![Crear configuraciones de canalización front-end](assets/create-frontend-pipeline-configs.png)

1. Como parte de los pasos de __Código fuente__, seleccione la opción __Código front-end__ y elija el entorno en __Entornos de implementación aptos__. En la sección __Código fuente__, asegúrese de que los valores de los campos Repositorio y Rama de Git sean correctos y de que la rama tenga los cambios de contrato de canalización front-end.
Y, __lo más importante__, que el valor del campo __Ubicación del código__ sea `/ui.frontend`. Por último, haga clic en __Guardar__.

![Crear código fuente de canalización front-end](assets/create-frontend-pipeline-source-code.png)


## Secuencia de implementación

* Ejecute primero la canalización __Implementar WKND de pila completa en desarrollo__, cuyo nombre se acaba de cambiar, para quitar los archivos clientlib de WKND del repositorio de AEM. Y lo más importante, prepare AEM para el contrato de canalización front-end añadiendo archivos de __configuración de Sling__ (`SiteConfig`, `HtmlPageItemsConfig`).

![Sitio WKND sin estilo](assets/unstyled-wknd-site.png)

>[!WARNING]
>
>Una vez finalizada la canalización __Implementar WKND de pila completa en desarrollo__, tendrá un sitio WKND __sin estilo__ que podría parecer dañado. Planee una interrupción o implemente durante las horas impares. Se trata de una interrupción única que debe planificar durante el cambio inicial desde el uso de una canalización de pila completa única hasta la canalización front-end.


* Finalmente, ejecute la canalización __Implementar WKND de front-end en desarrollo__ para generar solo el módulo `ui.frontend` e implementar los recursos front-end directamente en la CDN.

>[!IMPORTANT]
>
>Observará que el sitio WKND __sin estilo__ ha vuelto a la normalidad y esta vez la ejecución de la canalización __front-end__ fue mucho más rápida que la canalización de pila completa.

## Verificar cambios de estilo y nuevo paradigma de entrega

* Abra cualquier página del sitio WKND y podrá ver el color del texto con __Adobe Red__ y los archivos de recursos front-end (CSS, JS) se entregan desde la CDN. El nombre de host de solicitud de recursos comienza con `https://static-pXX-eYY.p123-e456.adobeaemcloud.com/$HASH_VALUE$/theme/site.css`, al igual que el archivo site.js o cualquier otro recurso estático al que haga referencia en el archivo `HtmlPageItemsConfig`.


![Sitio WKND de nuevo estilo](assets/newly-styled-wknd-site.png)



>[!TIP]
>
>El `$HASH_VALUE$` aquí es el mismo que se ve en el campo __HASH DE CONTENIDO__ de la canalización __Implementar WKND de front-end en desarrollo__. Se notifica a AEM de la URL de CDN del recurso front-end. El valor se almacena en `/conf/wknd/sling:configs/com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig/jcr:content` en la propiedad __prefixPath__.


![Correlación de valores hash](assets/hash-value-correlartion.png)



## Enhorabuena. {#congratulations}

Enhorabuena. Ha creado, ejecutado y verificado la canalización front-end que solo genera e implementa el módulo “ui.frontend” del proyecto de sitios WKND. Ahora, el equipo front-end puede iterar rápidamente en el diseño y el comportamiento de front-end del sitio, fuera del ciclo de vida completo del proyecto de AEM.

## Próximos pasos {#next-steps}

En el capítulo siguiente, [Consideraciones](considerations.md), revisará el impacto en el proceso de desarrollo del front-end y del back-end.
