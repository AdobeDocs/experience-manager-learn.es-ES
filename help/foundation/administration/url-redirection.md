---
title: Redirecciones de URL
description: Obtenga información acerca de las distintas opciones para realizar una redirección de URL en AEM.
version: Experience Manager 6.4, Experience Manager 6.5, Experience Manager as a Cloud Service
topic: Development, Administration
feature: Operations, Dispatcher
role: Developer
level: Intermediate
jira: KT-11466
last-substantial-update: 2024-10-22T00:00:00Z
index: y
doc-type: Article
exl-id: 8e64f251-e5fd-4add-880e-9d54f8e501a6
duration: 164
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '943'
ht-degree: 0%

---

# Redirecciones de URL

La redirección de URL es un aspecto común como parte de la operación del sitio web. Los arquitectos y administradores tienen el desafío de encontrar la mejor solución para cómo y dónde administrar las redirecciones URL que proporcionan flexibilidad y un tiempo de implementación de redireccionamiento rápido.

Familiarícese con la infraestructura de [AEM (6.x) aka AEM Classic](https://experienceleague.adobe.com/en/docs/experience-manager-learn/dispatcher-tutorial/chapter-2) y [AEM as a Cloud Service](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/overview/architecture). Las principales diferencias son las siguientes:

1. AEM as a Cloud Service tiene [CDN integrado](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn), pero los clientes pueden proporcionar un CDN (BYOCDN) delante de un CDN administrado por AEM.
1. AEM 6.x, ya sea local o Adobe Managed Services (AMS), no incluye una CDN administrada por AEM y los clientes deben traer la suya propia.

Los demás servicios de AEM (AEM Author/Publish y Dispatcher) son conceptualmente similares entre AEM 6.x y AEM as a Cloud Service.

Las soluciones de redireccionamiento de URL de AEM son las siguientes:

|                                                   | Administrado e implementado como código de proyecto de AEM | Posibilidad de cambio por equipo de marketing/contenido | Compatible con AEM as Cloud Service | Dónde se produce la ejecución de redirección |
|---------------------------------------------------|:-----------------------:|:---------------------:|:---------------------:| :---------------------:|
| [En Edge a través de CDN administrada por AEM](#at-edge-via-aem-managed-cdn) | ✔ | ✘ | ✔ | Edge/CDN (integrado) |
| [En Edge mediante trae tu propia CDN (BYOCDN)](#at-edge-via-bring-your-own-cdn) | ✘ | ✘ | ✔ | Edge/CDN (BYOCDN) |
| [Apache `mod_rewrite` reglas como Dispatcher config](#apache-mod_rewrite-module) | ✔ | ✘ | ✔ | Dispatcher |
| [ACS Commons - Administrador de mapas de redireccionamiento](#redirect-map-manager) | ✘ | ✔ | ✔ | Dispatcher |
| [ACS Commons - Administrador de redireccionamiento](#redirect-manager) | ✘ | ✔ | ✔ | AEM/Dispatcher |
| [La propiedad de página `Redirect`](#the-redirect-page-property) | ✘ | ✔ | ✔ | AEM |


## Opciones de solución

Las siguientes son opciones de solución en el orden de estar más cerca del explorador del visitante del sitio web.

### En Edge a través de una CDN administrada por AEM {#at-edge-via-aem-managed-cdn}

Esta opción solo está disponible para clientes de AEM as a Cloud Service.

La [CDN administrada por AEM](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn) proporciona una solución de redirección en el nivel de Edge, lo que reduce los viajes de ida y vuelta al origen. La función [Redirecciones del lado del servidor](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-configuring-traffic#server-side-redirectors) le permite configurar las reglas de redireccionamiento en el código del proyecto de AEM e implementarlas mediante la [Canalización de configuración](https://experienceleague.adobe.com/es/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#deploy-rules-through-cloud-manager). El tamaño del archivo de configuración de CDN (`cdn.yaml`) no debe exceder los 100 KB.

La administración de redirecciones en el nivel de Edge o CDN tiene ventajas de rendimiento.

### En Edge, lleve su propia CDN

Algunos servicios de CDN proporcionan soluciones de redirección en el nivel de Edge, lo que reduce los viajes de ida y vuelta al origen. Consulte [Redirector de Edge de Akamai](https://techdocs.akamai.com/cloudlets/docs/what-edge-redirector), [Funciones de AWS CloudFront](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cloudfront-functions.html). Consulte con su proveedor de servicios de CDN la capacidad de redirección a nivel de Edge.

La administración de redirecciones en el nivel de Edge o CDN tiene ventajas de rendimiento, pero no se administran como parte de AEM, sino como proyectos discretos. Un proceso bien definido para administrar e implementar reglas de redirección es crucial para evitar problemas.


### Módulo Apache `mod_rewrite`

Una solución común usa [módulo Apache mod_rewrite](https://httpd.apache.org/docs/current/mod/mod_rewrite.html). El [tipo de archivo del proyecto AEM](https://github.com/adobe/aem-project-archetype) proporciona una estructura de proyecto Dispatcher tanto para el proyecto [AEM 6.x](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.ams#file-structure) como para el proyecto [AEM as a Cloud Service](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud#file-structure). Las reglas de reescritura predeterminadas (inmutables) y personalizadas se definen en la carpeta `conf.d/rewrites` y el motor de reescritura está activado para `virtualhosts` que escucha en el puerto `80` a través del archivo `conf.d/dispatcher_vhost.conf`. Hay una implementación de ejemplo disponible en [AEM WKND Sites Project](https://github.com/adobe/aem-guides-wknd/tree/main/dispatcher/src/conf.d/rewrites).

En AEM as a Cloud Service, estas reglas de redireccionamiento se administran como parte del código AEM y se implementan a través de la [canalización de configuración de nivel web](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines) o la [canalización de pila completa](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines) de Cloud Manager. Por lo tanto, el proceso específico del proyecto de AEM está en juego para administrar, implementar y rastrear las reglas de redirección.

La mayoría de los servicios de CDN almacenan en caché las redirecciones HTTP 301 y 302 según sus encabezados `Cache-Control` o `Expires`. Ayuda a evitar la acción de ida y vuelta después de la redirección inicial que se origina en Apache/Dispatcher.


### ACS AEM Commons

Hay dos características disponibles en [ACS AEM Commons](https://adobe-consulting-services.github.io/acs-aem-commons/) para administrar las redirecciones de URL. Tenga en cuenta que ACS AEM Commons es un proyecto de código abierto operado por la comunidad y no es compatible con Adobe.

#### Administrador de redireccionamiento de mapas

[Administrador de mapas de redireccionamiento](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-map-manager/index.html) ayuda a los administradores de AEM a mantener y publicar fácilmente los archivos de [Apache RewriteMap](https://httpd.apache.org/docs/2.4/rewrite/rewritemap.html) sin tener que acceder directamente al servidor web Apache ni requerir el reinicio del servidor web Apache. Esta función permite a los usuarios de permisos crear, actualizar y eliminar reglas de redireccionamiento desde una consola en AEM, sin la ayuda del equipo de desarrollo ni de una implementación de AEM. El administrador de mapas de redireccionamiento es compatible con **AEM as a Cloud Service** (consulte la estrategia de [redireccionamientos de URL sin canalizaciones](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/pipeline-free-url-redirects) y el [tutorial](https://experienceleague.adobe.com/en/docs/experience-manager-learn/foundation/administration/implementing-pipeline-free-url-redirects#acs-commons---redirect-map-manager) relacionado) y con **AEM 6.x**.

#### Administrador de redireccionamiento

[Administrador de redireccionamiento](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/index.html) permite a los usuarios de AEM mantener y publicar fácilmente redirecciones de AEM. La implementación se basa en el filtro de servlet Java™, por lo que el consumo de recursos de JVM es típico. Esta función también elimina la dependencia del equipo de desarrollo de AEM y de las implementaciones de AEM. El Administrador de redireccionamiento es compatible con **AEM as a Cloud Service** y **AEM 6.x**. Mientras que la solicitud de redirección inicial debe acceder al servicio de publicación de AEM para generar la caché 301/302 (la mayoría) de los CDN 301/302 de forma predeterminada, lo que permite que las solicitudes posteriores se redirijan al perímetro/CDN.

[Redirect Manager](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/index.html) también admite la estrategia [Redirecciones de URL sin canalizaciones](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/pipeline-free-url-redirects) para **AEM as a Cloud Service** al [compilar las redirecciones en un archivo de texto](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/subpages/rewritemap.html) para [Apache RewriteMap](https://httpd.apache.org/docs/2.4/rewrite/rewritemap.html), de modo que permite actualizar las redirecciones usadas en el servidor web Apache sin tener que acceder directamente a él ni reiniciar. Consulte el [tutorial](https://experienceleague.adobe.com/en/docs/experience-manager-learn/foundation/administration/implementing-pipeline-free-url-redirects#acs-commons---redirect-manager) para obtener más información. En esta situación, la solicitud de redirección inicial afecta al servidor web Apache y no al servicio de publicación de AEM.

### La propiedad de página `Redirect`

La propiedad de página `Redirect` predeterminada (OOTB) de la [pestaña Avanzadas](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/sites-console/page-properties.html) permite a los autores de contenido definir la ubicación de redireccionamiento de la página actual. Esta solución es mejor para los escenarios de redireccionamiento por página y no tiene una ubicación central para ver y administrar las redirecciones de página.

## Qué solución es adecuada para la implementación

A continuación se presentan algunos criterios para determinar la solución correcta. Además, el proceso de TI y marketing de su organización debería ayudarle a elegir la solución correcta.

1. Permite al equipo de marketing o a los superusuarios administrar las reglas de redireccionamiento sin el equipo de desarrollo de AEM ni las implementaciones de AEM.
1. Proceso para administrar, verificar, rastrear y revertir los cambios o la mitigación de riesgos.
1. Disponibilidad de _experiencia en la materia_ para **At Edge a través de la solución CDN Service**.
