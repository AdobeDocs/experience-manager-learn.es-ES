---
title: Implementación de SPA para AEM GraphQL
description: Conozca SPA opciones de implementación con respecto a AEM GraphQL, sin encabezado.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10587
thumbnail: KT-10587.jpg
mini-toc-levels: 2
source-git-commit: c7d2e69a9039cfbaa43d6d9b65b9fa6f69378716
workflow-type: tm+mt
source-wordcount: '528'
ht-degree: 0%

---


# Implementación de una SPA

En esta sección, analizaremos un método para implementar SPA (React, Vue, Angular, etc.) que invoque las API de GraphQL AEM cargar los datos, sin embargo, antes de que esto permitamos comprender los artefactos de alto nivel que uno tiene que implementar.

**SPA Generar artefactos de aplicaciones:**

Los archivos producidos por el marco de SPA, normalmente **HTML, CSS, JS**, también son artefactos de compilación estáticos. En el caso de la aplicación React, los artefactos de la `build` y para artefactos de Vue desde el `dist` directorio.
La solicitud a su SPA (por ejemplo, https://HOST/my-aem-spa.html) se servirá utilizando estos artefactos de compilación.

**API de AEM GraphQL:**

Claramente, este extremo de la API de GraphQL (`/graphql/execute.json/<PROJECT-CONFIG>/<PERSISTED-QUERY-NAME>`) debe alojarse en el dominio AEM.

En resumen, la arquitectura de implementación tiene dos partes *1. SPA 2. AEM capa de API de GraphQL*, así que revisemos las opciones de implementación para estas dos partes.


## Opciones de implementación

| Opción de implementación | URL de SPA | URL de la API de AEM GraphQL | ¿Se requiere la configuración de CORS? |
| ---------|---------- | ---------|---------- |
| **Mismo dominio** | https://**HOST**/my-aem-spa.html | https://**HOST**/graphql/execute.json/... | ü |
| **Dominio diferente** | https://**SPA-HOST**/my-aem-spa.html | https://**AEM-HOST**/graphql/execute.json/... | š |

**Mismo dominio:**\
Ambas *Capa de API de SPA y AEM GraphQL* en esta opción se implementan en el **Mismo dominio**. Significa solicitud a SPA URI `/my-aem-spa.html` Capa de la API de &amp; GraphQL `/graphql/execute.json/` se proporcionan desde exactamente el mismo dominio.

**Dominio diferente:**\
Ambas *Capa de API de SPA y AEM GraphQL* en esta opción se implementa en **Dominio diferente**. Significa solicitud a SPA URI `/my-aem-spa.html` se sirve desde un **dominio diferente** capa de la API de GraphQL `/graphql/execute.json/` solicitudes. Tenga en cuenta que, como parte de esta opción de implementación, debe [configurar CORS](cors.md) en la instancia de AEM.

>[!NOTE]
>
>DEBE configurar correctamente CORS en AEM instancia, [vea los pasos aquí](cors.md).

### Implementación en el mismo dominio

Al implementar en el mismo dominio, el dominio podría ser un **dominio de AEM principal** (En AEM dominio) o **dominio de SPA principal** (Fuera AEM dominio) y SPA entrantes, las solicitudes de API de AEM GraphQL se pueden dividir en cualquiera de los componentes de implementación, como CDN ([Fly](https://docs.fastly.com/en/guides/routing-assets-to-different-origins), Akamai, [CloudFront](https://aws.amazon.com/premiumsupport/knowledge-center/cloudfront-distribution-serve-content/)), [HTTPD con proxy inverso](https://httpd.apache.org/docs/2.4/howto/reverse_proxy.html). En otras palabras, se siguen implementando los artefactos de compilación SPA y las API de GraphQL AEM en diferentes servidores, pero para los usuarios finales, estos se entregan desde un único dominio y detrás de la escena enrutados a un destino o servidor de origen diferente.

Además, SPA artefactos de compilación se pueden alojar en AEM *pero no se recomiendan.*

| Misma implementación de dominio | División CDN | HTTPD + proxy inverso | AEM artefactos SPA alojados |
| ---------|---------- | ---------|---------- |
| **EN Dominio AEM** | š | š | š |
| **OFF AEM dominio** | š | š | **N/D** |


**HTTPD + proxy inverso**

Un ejemplo de configuración será como el siguiente

>[!TIP]
>
> Las siguientes configuraciones son ejemplos. Asegúrese de ajustarlos para adaptarlos a los requisitos del proyecto.

EN AEM DOMINIO

    &quot;
    ProxyPass &quot;/${YOUR-SPA-URI}&quot; &quot;http://${SPA-HOST}/&quot;
    ProxyPassReverse &quot;/${YOUR-SPA-URI}&quot; &quot;http://${SPA-HOST}/&quot;
    &quot;

OFF AEM dominio

    &quot;
    ProxyPass &quot;/graphql/execute.json/&quot; &quot;http://${AEM-HOST}/&quot;
    ProxyPassReverse &quot;/graphql/execute.json/&quot; &quot;http://${AEM-HOST}/&quot;
    &quot;




### Implementación en un dominio diferente

En este escenario, SPA artefactos de compilación se implementan en un dominio diferente al dominio de las API de AEM GraphQL y, para los usuarios finales, se entregan desde dos dominios separados, de modo que [Configuración CORS](cors.md) debe estar en AEM.

**SPA solicitudes de aplicaciones a través de distintos dominios**

![Entrega de dominios SPA diferentes](assets/spa/different-domain-spa-delivery.png)


**Encabezado de respuesta CORS en AEM API de GraphQL**

![Encabezado de respuesta CORS AEM API de GraphQL](assets/spa/CORS-response-header-aem-graphql-api.png)


