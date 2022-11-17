---
title: Redirecciones de URL
description: Obtenga información sobre las distintas opciones para realizar una redirección de URL en AEM.
version: 6.4, 6.5, Cloud Service
topic: Development, Administration
feature: Operations, Dispatcher
role: Developer, Architect
level: Intermediate
kt: 11466
last-substantial-update: 2022-10-14T00:00:00Z
index: y
source-git-commit: 00ea3a8e6b69cd99cf293093d38b59df51f6a26d
workflow-type: tm+mt
source-wordcount: '876'
ht-degree: 2%

---


# Redirecciones de URL

La redirección de URL es un aspecto común como parte de la operación del sitio web. Los arquitectos y administradores tienen el desafío de encontrar la mejor solución para administrar las redirecciones de URL y saber cómo hacerlo, lo que proporciona flexibilidad y un tiempo de implementación de redireccionamiento rápido.

Asegúrese de estar familiarizado con el [AEM (6.x) aka AEM Classic](https://experienceleague.adobe.com/docs/experience-manager-learn/dispatcher-tutorial/chapter-2.html#the-%E2%80%9Clegacy%E2%80%9D-setup) y [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/overview/architecture.html#runtime-architecture) infraestructura. Las principales diferencias son:

1. AEM as a Cloud Service ha [CDN integrado](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html?lang=es), sin embargo, los clientes pueden proporcionar una CDN (BYOCDN) delante de la CDN administrada por AEM.
1. AEM 6.x, ya sea on-premise o Adobe Managed Services (AMS) no incluye una CDN administrada por AEM, y los clientes deben traer sus propios servicios.

Los otros servicios de AEM (AEM Author/Publish y Dispatcher) son conceptualmente similares entre AEM 6.x y AEM as a Cloud Service.

AEM soluciones de redireccionamiento de URL son las siguientes:

|  | Administrado e implementado como código AEM proyecto | Capacidad de cambio por equipo de contenido/marketing | AEM como compatible con el Cloud Service | Donde ocurre la ejecución de redirección |
|---------------------------------------------------|:-----------------------:|:---------------------:|:---------------------:| :---------------------:|
| [En Edge al traer su propia CDN](#at-edge-via-bring-your-own-cdn) | ü | ü | š | Edge/CDN |
| [Apache `mod_rewrite` reglas como configuración de Dispatcher ](#apache-mod_rewrite-module) | š | ü | š | Dispatcher |
| [ACS Commons - Administrador de mapas de redireccionamiento](#redirect-map-manager) | ü | š | ü | Dispatcher |
| [ACS Commons - Administrador de redireccionamiento](#redirect-manager) | ü | š | š | AEM |
| [La variable `Redirect` propiedad de página](#the-redirect-page-property) | ü | š | š | AEM |


## Opciones de solución

Las siguientes son opciones de solución en orden a estar más cerca del explorador del visitante del sitio web.

### En Edge al traer su propia CDN

Algunos servicios de CDN proporcionan soluciones de redirección a nivel de Edge, reduciendo así los viajes de ida y vuelta al origen. Consulte [Redirector perimetral de Akamai](https://techdocs.akamai.com/cloudlets/docs/what-edge-redirector), [Funciones de AWS CloudFront](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cloudfront-functions.html). Consulte con su proveedor de servicios de CDN la capacidad de redirección a nivel de Edge.

La administración de redirecciones a nivel de Edge o CDN tiene ventajas de rendimiento, pero no se administran como parte de AEM, sino como proyectos más bien discretos. Un proceso bien pensado para administrar e implementar reglas de redireccionamiento es crucial para evitar problemas.


### Apache `mod_rewrite` módulo

Una solución común utiliza [Módulo mod_rewrite de Apache](https://httpd.apache.org/docs/current/mod/mod_rewrite.html). La variable [Tipo de archivo del proyecto AEM](https://github.com/adobe/aem-project-archetype) proporciona una estructura de proyecto de Dispatcher para ambos [AEM 6.x](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.ams#file-structure) y [AEM as a Cloud Service](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud#file-structure) proyecto. Las reglas de reescritura predeterminadas (inmutables) y personalizadas se definen en la variable `conf.d/rewrites` carpeta y el motor de reescritura está activado para `virtualhosts` que escucha en el puerto `80` via `conf.d/dispatcher_vhost.conf` archivo. Hay un ejemplo de implementación disponible en la sección [AEM proyecto de WKND Sites](https://github.com/adobe/aem-guides-wknd/tree/main/dispatcher/src/conf.d/rewrites).

En AEM as a Cloud Service, estas reglas de redireccionamiento se administran como parte del código AEM y se implementan mediante Cloud Manager [Canalización de configuración de nivel web](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html#web-tier-config-pipelines) o [Canalización de pila completa](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html#full-stack-pipeline). Por lo tanto, el proceso específico del proyecto de AEM está en juego para administrar, implementar y rastrear las reglas de redireccionamiento.

La mayoría de los servicios de CDN almacenan en caché las redirecciones HTTP 301 y 302 en función de sus `Cache-Control` o `Expires` encabezados. Esto ayuda a evitar el viaje de ida y vuelta después de la redirección inicial originada en Apache/Dispatcher.


### ACS AEM Commons

Hay dos funciones disponibles en [ACS AEM Commons](https://adobe-consulting-services.github.io/acs-aem-commons/) para administrar las redirecciones URL. Tenga en cuenta que ACS AEM Commons es un proyecto de código abierto operado por la comunidad y no apoyado por el Adobe.

#### Administrador de mapas de redireccionamiento

[Administrador de mapas de redireccionamiento](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-map-manager/index.html) permite a los administradores de AEM 6.x mantener y publicar fácilmente [Apache RewriteMap](https://httpd.apache.org/docs/2.4/rewrite/rewritemap.html) archivos sin acceder directamente al servidor web Apache ni requerir un reinicio del servidor web Apache. Esta función permite a los usuarios de permisos crear, actualizar y eliminar reglas de redireccionamiento desde una consola en AEM, sin la ayuda del equipo de desarrollo ni una implementación de AEM. El Administrador de mapas de redireccionamiento es **NO AEM compatible as a Cloud Service**.

#### Administrador de redireccionamiento

[Administrador de redireccionamiento](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/index.html) permite a los usuarios en AEM mantener y publicar fácilmente redirecciones desde AEM. La implementación se basa en el filtro de servlet Java™, por lo que el consumo de recursos típico de JVM. Esta función también elimina la dependencia del equipo de desarrollo de AEM y de las implementaciones de AEM. El Administrador de redireccionamiento es ambas **AEM as a Cloud Service** y **AEM 6.x** compatible. Mientras que la solicitud de redireccionamiento inicial debe visitar el servicio AEM Publish para generar la caché 301/302 (la mayoría) de las CDNs 301/302 de forma predeterminada, lo que permite redirigir las solicitudes posteriores al edge/CDN.

### La variable `Redirect` propiedad de página

El valor predeterminado (OOTB) `Redirect` propiedad de página de [Ficha Avanzadas](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/fundamentals/page-properties.html#advanced) permite a los autores de contenido definir la ubicación de redireccionamiento de la página actual. Esta solución es mejor para los casos de redireccionamiento por página y no tiene una ubicación central para ver y administrar los redireccionamientos de página.

## Qué solución es la adecuada para la implementación

A continuación se presentan algunos criterios para determinar la solución correcta. Además, el proceso de TI y marketing de su organización debería ayudarle a elegir la solución adecuada.

1. Habilitar al equipo de marketing o a los superusuarios para que administren las reglas de redireccionamiento sin el equipo de desarrollo de AEM y las implementaciones de AEM.
1. El proceso para administrar, verificar, rastrear y revertir los cambios o la mitigación de riesgos.
1. Disponibilidad de _Experimento en materia de asunto_ para **En Edge a través del servicio CDN** solución.

