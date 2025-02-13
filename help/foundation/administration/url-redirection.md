---
title: Redirecciones de URL
description: Obtenga información acerca de las distintas opciones para realizar una redirección de URL en AEM.
version: 6.4, 6.5, Cloud Service
topic: Development, Administration
feature: Operations, Dispatcher
role: Developer, Architect
level: Intermediate
jira: KT-11466
last-substantial-update: 2024-10-22T00:00:00Z
index: y
doc-type: Article
exl-id: 8e64f251-e5fd-4add-880e-9d54f8e501a6
duration: 164
source-git-commit: bc2f4655631f28323a39ed5b4c7878613296a0ba
workflow-type: tm+mt
source-wordcount: '961'
ht-degree: 0%

---

# Redirecciones de URL

La redirección de URL es un aspecto común como parte de la operación del sitio web. Los arquitectos y administradores tienen el desafío de encontrar la mejor solución para cómo y dónde administrar las redirecciones URL que proporcionan flexibilidad y un tiempo de implementación de redireccionamiento rápido.

Familiarícese con la infraestructura de [AEM (6.x) aka AEM Classic](https://experienceleague.adobe.com/en/docs/experience-manager-learn/dispatcher-tutorial/chapter-2) y [AEM as a Cloud Service](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/overview/architecture). Las principales diferencias son las siguientes:

1. AEM as a Cloud Service tiene [CDN integrado](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn), pero los clientes pueden proporcionar un CDN (BYOCDN) delante de un CDN administrado por AEM.
1. AEM 6.x, ya sea local o Adobe Systems Managed Services (AMS), no incluye un CDN administrado por AEM, y los clientes deben traer el suyo propio.

Los otros servicios AEM (AEM Autor/Publish y Dispatcher) son conceptualmente similares entre AEM 6.x y AEM como Cloud Service.

AEM URL redirección soluciones son las siguientes:

|                                                   | Administrado e implementado como AEM código de proyecto | Capacidad de cambio por equipo marketing/contenido | Compatible con AEM as Cloud Service | Dónde se produce la ejecución de redirección |
|---------------------------------------------------|:-----------------------:|:---------------------:|:---------------------:| :---------------------:|
| [En Edge a través de CDN administrada por AEM](#at-edge-via-aem-managed-cdn) | ✔ | ✘ | ✔ | Edge/CDN (integrado) |
| [En Edge a través de bring your own CDN (BYOCDN)](#at-edge-via-bring-your-own-cdn) | ✘ | ✘ | ✔ | Edge/CDN (BYOCDN) |
| [Apache `mod_rewrite` reglas como Dispatcher config](#apache-mod_rewrite-module) | ✔ | ✘ | ✔ | Dispatcher |
| [ACS Commons - Administrador de mapas de redireccionamiento](#redirect-map-manager) | ✘ | ✔ | ✔ | Dispatcher |
| [ACS Commons - Administrador de redireccionamiento](#redirect-manager) | ✘ | ✔ | ✔ | AEM/Dispatcher |
| [La propiedad de página `Redirect`](#the-redirect-page-property) | ✘ | ✔ | ✔ | AEM |


## Opciones de solución

Las siguientes son opciones de solución en el orden de estar más cerca del explorador del visitante del sitio web.

### En Edge a través de una CDN administrada por AEM {#at-edge-via-aem-managed-cdn}

Esta opción solo está disponible para AEM como cliente Cloud Service.

El [CDN administrado por AEM](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn) proporciona una solución de redirección en el nivel Edge, lo que reduce los viajes de ida y vuelta al origen. La [característica Redirecciones por parte del cliente permite configurar las reglas de](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-configuring-traffic#client-side-redirectors) redirección en el código y el implementar del proyecto de AEM mediante la [canalización](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#deploy-rules-through-cloud-manager) de configuración. El tamaño del archivo de configuración (`cdn.yaml`) de CDN no debe superar los 100 KB.

La administración de redirecciones en el nivel de Edge o CDN tiene ventajas de rendimiento.

### En Edge, lleve su propia CDN

Algunos servicios de CDN proporcionan soluciones de redirección en el nivel de Edge, lo que reduce los viajes de ida y vuelta al origen. Consulte [Redirector de Edge de Akamai](https://techdocs.akamai.com/cloudlets/docs/what-edge-redirector), [Funciones de AWS CloudFront](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cloudfront-functions.html). Consulte con su proveedor de servicios de CDN la capacidad de redirección a nivel de Edge.

La administración de redirecciones en el nivel de Edge o CDN tiene ventajas de rendimiento, pero no se administran como parte de AEM, sino como proyectos discretos. Un proceso bien definido para administrar e implementar reglas de redirección es crucial para evitar problemas.


### módulo Apache `mod_rewrite`

Una solución común utiliza [el módulo Apache mod_rewrite](https://httpd.apache.org/docs/current/mod/mod_rewrite.html). El [arquetipo](https://github.com/adobe/aem-project-archetype) del proyecto AEM proporciona una estructura de proyecto Dispatcher tanto para AEM 6.x](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.ams#file-structure) como [[para AEM como proyecto Cloud Service](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud#file-structure). Las reglas de reescritura predeterminadas (inmutables) y personalizadas se definen en la carpeta y el motor de reescritura está activado para `virtualhosts` que escuche en puerto `80` `conf.d/rewrites` a través `conf.d/dispatcher_vhost.conf` del archivo. Un ejemplo implementación está disponible en el [proyecto](https://github.com/adobe/aem-guides-wknd/tree/main/dispatcher/src/conf.d/rewrites) AEM WKND Sites.

En AEM como Cloud Service, estas reglas de redirección se administran como parte del código AEM y se implementan a través de la canalización](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines) de configuración de nivel web de Cloud Manager [o [la canalización](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines) de pila completa. Por lo tanto, su AEM proceso específico del proyecto está en juego para administrar, implementar y rastrear las reglas redirección.

La mayoría de los servicios de CDN almacenan en caché las redirecciones HTTP 301 y 302 según sus encabezados `Cache-Control` o `Expires`. Ayuda a evitar la acción de ida y vuelta después de la redirección inicial que se origina en Apache/Dispatcher.


### ACS AEM Commons

Hay dos características disponibles en [ACS AEM Commons](https://adobe-consulting-services.github.io/acs-aem-commons/) para administrar las redirecciones de URL. Tenga en cuenta que ACS AEM Commons es un proyecto de código abierto operado por la comunidad y no es compatible con Adobe.

#### Administrador de redireccionamiento de mapas

[Administrador de mapas de redireccionamiento](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-map-manager/index.html) ayuda a los administradores de AEM a mantener y publicar fácilmente los archivos de [Apache RewriteMap](https://httpd.apache.org/docs/2.4/rewrite/rewritemap.html) sin tener que acceder directamente al servidor web Apache ni requerir el reinicio del servidor web Apache. Esta función permite a los usuarios de permisos crear, actualizar y eliminar reglas de redireccionamiento desde una consola en AEM, sin la ayuda del equipo de desarrollo ni de una implementación de AEM. El administrador de mapas de redireccionamiento es compatible con **AEM as a Cloud Service** (consulte la estrategia de [redireccionamientos de URL sin canalizaciones](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/pipeline-free-url-redirects) y el [tutorial](https://experienceleague.adobe.com/en/docs/experience-manager-learn/foundation/administration/implementing-pipeline-free-url-redirects#acs-commons---redirect-map-manager) relacionado) y con **AEM 6.x**.

#### Administrador de redireccionamiento

[Redirect Manager](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/index.html) permite a los usuarios en AEM mantener y publicar redireccionamientos fácilmente desde AEM. El implementación se basa en el filtro de servlets Java™, por lo tanto, el consumo típico de recursos de JVM. Esta característica también elimina la dependencia de los AEM equipo de desarrollo y de las implementaciones AEM. Redirect Manager es **AEM como Cloud Service** y **compatible con AEM 6.x** . Mientras que el solicitud redirigido inicial debe Visita el servicio de Publish de AEM generar la caché 301/302 (la mayoría) de las CDN 301/302 de forma predeterminada, lo que permite que las solicitudes posteriores se redirijan al borde / CDN.

[Redirect Manager](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/index.html) también soporta [la estrategia de redirecciones de URL de gratuito](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/pipeline-free-url-redirects) canalización para **AEM como Cloud Service** compilando [redirecciones en un archivo](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/subpages/rewritemap.html) de texto para [Apache RewriteMap](https://httpd.apache.org/docs/2.4/rewrite/rewritemap.html), por lo que permite actualizar las redirecciones utilizadas en el servidor web Apache sin acceder directamente a ellas ni requerir su reinicio. Consulte los [tutorial](https://experienceleague.adobe.com/en/docs/experience-manager-learn/foundation/administration/implementing-pipeline-free-url-redirects#acs-commons---redirect-manager) para obtener más información. En este escenario, el redirección inicial solicitud llega al servidor web Apache y no AEM Publish servicio.

### El `Redirect` Página Propiedad

El Propiedad de Página predeterminado (OOTB) `Redirect` del pestaña de Avanzadas [](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/sites-console/page-properties.html) permite a contenido autores definir la ubicación redirección para el Página actual. Esta solución es perfecta para escenarios de redirección por Página y no tiene una ubicación central para vista y administrar las Página redirecciones.

## ¿Qué solución es la adecuada para implementación

A continuación se presentan algunos criterios para determinar la solución correcta. Además, el proceso de TI y marketing de su organización debería ayudarle a elegir la solución correcta.

1. Permite al equipo de marketing o a los superusuarios administrar las reglas de redireccionamiento sin el equipo de desarrollo de AEM ni las implementaciones de AEM.
1. Proceso para administrar, verificar, rastrear y revertir los cambios o la mitigación de riesgos.
1. Disponibilidad de _experiencia en la materia_ para **At Edge a través de la solución CDN Service**.
