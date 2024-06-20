---
title: Redirecciones de URL
description: AEM Obtenga información acerca de las distintas opciones para realizar redirecciones de URL en la dirección de correo electrónico de.
version: 6.4, 6.5, Cloud Service
topic: Development, Administration
feature: Operations, Dispatcher
role: Developer, Architect
level: Intermediate
jira: KT-11466
last-substantial-update: 2022-10-14T00:00:00Z
index: y
doc-type: Article
exl-id: 8e64f251-e5fd-4add-880e-9d54f8e501a6
duration: 164
source-git-commit: 3cc9b4fa0a30d36638a8c28a73663ffa455ba4a3
workflow-type: tm+mt
source-wordcount: '877'
ht-degree: 0%

---

# Redirecciones de URL

La redirección de URL es un aspecto común como parte de la operación del sitio web. Los arquitectos y administradores tienen el desafío de encontrar la mejor solución para cómo y dónde administrar las redirecciones URL que proporcionan flexibilidad y un tiempo de implementación de redireccionamiento rápido.

Asegúrese de estar familiarizado con el [AEM AEM (6.x) también conocido como Classic](https://experienceleague.adobe.com/en/docs/experience-manager-learn/dispatcher-tutorial/chapter-2) y [AEM as a Cloud Service](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/overview/architecture) infraestructura. Las principales diferencias son las siguientes:

1. AEM ha as a Cloud Service [CDN integrada](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn)AEM Sin embargo, los clientes pueden proporcionar una CDN (BYOCDN) delante de una CDN administrada por el usuario de CDN que es administrada por el usuario de CDN.
1. AEM.x si Managed Services AEM local o de Adobe (AMS) no incluye una CDN administrada por el cliente, y los clientes deben traer la suya propia.

AEM AEM Los demás servicios de la (Author/Publish AEM AEM y Dispatcher) son conceptualmente similares entre la versión 6.x y la versión as a Cloud Service de la.

AEM Las soluciones de redireccionamiento de URL son las siguientes:

|                                                   | AEM Gestionado e implementado como código de proyecto de | Posibilidad de cambio por equipo de marketing/contenido | AEM compatible con el Cloud Service de | Dónde se produce la ejecución de redirección |
|---------------------------------------------------|:-----------------------:|:---------------------:|:---------------------:| :---------------------:|
| [AEM En Edge a través de CDN gestionada por](#at-edge-via-aem-managed-cdn) | ✔ | ✘ | ✔ | Edge/CDN (integrado) |
| [En Edge a través de su propia CDN (BYOCDN)](#at-edge-via-bring-your-own-cdn) | ✘ | ✘ | ✔ | Edge/CDN (BYOCDN) |
| [Apache `mod_rewrite` reglas como configuración de Dispatcher](#apache-mod_rewrite-module) | ✔ | ✘ | ✔ | Dispatcher |
| [ACS Commons - Administrador de mapas de redireccionamiento](#redirect-map-manager) | ✘ | ✔ | ✘ | Dispatcher |
| [ACS Commons - Administrador de redireccionamiento](#redirect-manager) | ✘ | ✔ | ✔ | AEM |
| [El `Redirect` propiedad de página](#the-redirect-page-property) | ✘ | ✔ | ✔ | AEM |


## Opciones de solución

Las siguientes son opciones de solución en el orden de estar más cerca del explorador del visitante del sitio web.

### AEM En Edge a través de CDN gestionada por {#at-edge-via-aem-managed-cdn}

AEM Esta opción solo está disponible para clientes as a Cloud Service de la aplicación

El [AEM CDN administrado por el usuario](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn) proporciona una solución de redirección en el nivel de Edge, lo que reduce los viajes de ida y vuelta al origen. El [Redirecciones del lado del cliente](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-configuring-traffic#client-side-redirectors) AEM Esta función le permite configurar las reglas de redireccionamiento en el código de proyecto de la e implementarlas mediante [Configurar canalización](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#deploy-rules-through-cloud-manager). El archivo de configuración de CDN (`cdn.yaml`) el tamaño no debe exceder los 100 KB.

La administración de redirecciones en el nivel de Edge o CDN tiene ventajas de rendimiento.

### En Edge mediante traiga su propia CDN

Algunos servicios de CDN proporcionan soluciones de redirección en el nivel de Edge, lo que reduce los viajes de ida y vuelta al origen. Consulte [Redirector de Akamai Edge](https://techdocs.akamai.com/cloudlets/docs/what-edge-redirector), [Funciones de AWS CloudFront](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cloudfront-functions.html). Consulte con su proveedor de servicios de CDN para la capacidad de redirección a nivel de Edge.

AEM La administración de redirecciones en el nivel de Edge o CDN tiene ventajas de rendimiento, pero no se administran como parte de proyectos de, sino discretos. Un proceso bien definido para administrar e implementar reglas de redirección es crucial para evitar problemas.


### Apache `mod_rewrite` módulo

Una solución común utiliza [Módulo Apache mod_rewrite](https://httpd.apache.org/docs/current/mod/mod_rewrite.html). El [AEM Tipo de archivo del proyecto](https://github.com/adobe/aem-project-archetype) proporciona una estructura de proyecto de Dispatcher para ambos [AEM.x](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.ams#file-structure) y [AEM as a Cloud Service](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud#file-structure) proyecto. Las reglas de reescritura predeterminadas (inmutables) y personalizadas se definen en la variable `conf.d/rewrites` y el motor de reescritura está activado para `virtualhosts` que escucha en el puerto `80` mediante `conf.d/dispatcher_vhost.conf` archivo. Hay un ejemplo de implementación disponible en la variable [AEM Proyecto de sitios de WKND](https://github.com/adobe/aem-guides-wknd/tree/main/dispatcher/src/conf.d/rewrites).

AEM En as a Cloud Service AEM, estas reglas de redireccionamiento se administran como parte del código de y se implementan mediante Cloud Manager [Canalización de configuración de nivel web](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines) o [Canalización de pila completa](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines). AEM Por lo tanto, el proceso específico del proyecto de la está en juego para administrar, implementar y rastrear las reglas de redirección.

La mayoría de los servicios de CDN almacenan en caché las redirecciones HTTP 301 y 302 según su `Cache-Control` o `Expires` encabezados. Ayuda a evitar la ida y vuelta después de la redirección inicial que se origina en Apache/Dispatcher.


### AEM ACS Commons

Hay dos funciones disponibles en [AEM ACS Commons](https://adobe-consulting-services.github.io/acs-aem-commons/) para administrar las redirecciones URL. AEM Tenga en cuenta que ACS Commons es un proyecto de código abierto operado por la comunidad y no es compatible con el Adobe.

#### Administrador de redireccionamiento de mapas

[Administrador de redireccionamiento de mapas](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-map-manager/index.html) AEM ayuda a los administradores de 6.x a mantener y publicar fácilmente [Apache RewriteMap](https://httpd.apache.org/docs/2.4/rewrite/rewritemap.html) archivos sin acceder directamente al servidor web Apache o que requieran que se reinicie el servidor web Apache. AEM AEM Esta función permite a los usuarios de permisos crear, actualizar y eliminar reglas de redireccionamiento de una consola en, sin la ayuda del equipo de desarrollo ni de una implementación de la aplicación. El administrador de mapas de redirección está **AEM NO as a Cloud Service compatible**.

#### Administrador de redireccionamiento

[Administrador de redireccionamiento](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/index.html) AEM AEM permite a los usuarios de la red de mantener y publicar fácilmente redirecciones de los recursos de la red de trabajo de la red de. La implementación se basa en el filtro de servlet Java™, por lo que el consumo de recursos de JVM es típico. AEM AEM Esta función también elimina la dependencia del equipo de desarrollo de la y de las implementaciones de la misma. El Gestor de redireccionamiento es **AEM as a Cloud Service** y **AEM.x** compatible. AEM Mientras que la solicitud de redirección inicial debe acceder al servicio de Publish de la para generar la caché 301/302 (la mayoría) de los CDN 301/302 de forma predeterminada, lo que permite que las solicitudes posteriores se redirijan al perímetro/CDN.

### El `Redirect` propiedad de página

El listo para usar (OOTB) `Redirect` propiedad de página del [Pestaña Avanzadas](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/sites-console/page-properties.html) permite a los autores de contenido definir la ubicación de redireccionamiento de la página actual. Esta solución es mejor para los escenarios de redireccionamiento por página y no tiene una ubicación central para ver y administrar las redirecciones de página.

## Qué solución es adecuada para la implementación

A continuación se presentan algunos criterios para determinar la solución correcta. Además, el proceso de TI y marketing de su organización debería ayudarle a elegir la solución correcta.

1. AEM AEM Permitir que el equipo de marketing o los superusuarios administren las reglas de redireccionamiento sin el equipo de desarrollo de la ni las implementaciones de la misma.
1. Proceso para administrar, verificar, rastrear y revertir los cambios o la mitigación de riesgos.
1. Disponibilidad de _Experiencia en la materia_ para **En Edge a través del servicio CDN** solución.
