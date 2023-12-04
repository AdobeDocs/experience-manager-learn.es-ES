---
title: AEM Almacenamiento en caché del servicio de publicación
description: AEM Información general sobre el almacenamiento en caché del servicio Publicación as a Cloud Service de.
version: Cloud Service
feature: Dispatcher, Developer Tools
topic: Performance
role: Architect, Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-08-28T00:00:00Z
jira: KT-13858
thumbnail: KT-13858.jpeg
exl-id: 1a1accbe-7706-4f9b-bf63-755090d03c4c
duration: 333
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '1134'
ht-degree: 1%

---

# AEM Publish

AEM AEM El servicio Publicación de tiene dos capas principales de almacenamiento en caché, la CDN as a Cloud Service AEM de la red de distribución de contenido (CDN) y la Dispatcher de. Opcionalmente, se puede colocar una CDN administrada por el cliente delante de la CDN as a Cloud Service de la red de distribución de contenido (CDN) de la red de distribución de contenido (CDN) de la red AEM. AEM La CDN as a Cloud Service proporciona una entrega de contenido de vanguardia, lo que garantiza que las experiencias se entreguen con baja latencia a usuarios de todo el mundo. AEM AEM AEM Dispatcher proporciona almacenamiento en caché directamente delante de la publicación de la publicación, y se utiliza para mitigar la carga innecesaria en la publicación de la propia publicación de la.

![AEM Diagrama de información general de publicación en caché](./assets/publish/publish-all.png){align="center"}

## La red de distribución de contenido (CDN)

AEM El almacenamiento en caché de CDN de as a Cloud Service está controlado por encabezados de caché de respuesta HTTP y su objetivo es almacenar en caché el contenido para optimizar el equilibrio entre actualización y rendimiento. AEM La CDN se encuentra entre el usuario final y el Dispatcher de y se utiliza para almacenar en caché el contenido lo más cerca posible del usuario final, lo que garantiza una experiencia de rendimiento.

![AEM CDN de publicación de](./assets/publish/publish-cdn.png){align="center"}

Configurar cómo la CDN almacena en caché el contenido se limita a establecer encabezados de caché en las respuestas HTTP. AEM Estos encabezados de caché suelen configurarse en configuraciones de vhost de Dispatcher con el uso de `mod_headers`AEM , pero también se puede configurar en el código Java™ personalizado que se ejecuta en la propia publicación de la publicación de la.

### ¿Cuándo se almacenan en caché las solicitudes/respuestas HTTP?

AEM La red de distribución de contenido (CDN) as a Cloud Service solo almacena en caché respuestas HTTP y deben cumplirse todos los criterios siguientes:

+ El estado de la solicitud HTTP es `2xx` o `3xx`
+ El método de solicitud HTTP es `GET` o `HEAD`
+ Al menos uno de los siguientes encabezados de respuesta HTTP está presente: `Cache-Control`, `Surrogate-Control`, o  `Expires`
+ La respuesta HTTP puede ser cualquier tipo de contenido, incluidos HTML, JSON, CSS, JS y archivos binarios.

De forma predeterminada, las respuestas HTTP no almacenadas en caché por [AEM Dispatcher](#aem-dispatcher) Elimine automáticamente cualquier encabezado de caché de respuesta HTTP para evitar el almacenamiento en caché en CDN. Este comportamiento se puede sobrescribir cuidadosamente utilizando `mod_headers` con el `Header always set ...` cuando sea necesario.

### ¿Qué se almacena en caché?

AEM CDN as a Cloud Service almacena en caché lo siguiente:

+ cuerpo de respuesta HTTP
+ Encabezados de respuesta HTTP

Normalmente, una solicitud/respuesta HTTP para una sola URL se almacena en caché como un solo objeto. Sin embargo, la CDN puede gestionar el almacenamiento en caché de varios objetos para una sola URL, cuando la variable `Vary` se establece en la respuesta HTTP. Evitar especificar `Vary` en encabezados cuyos valores no tienen un conjunto de valores estrictamente controlados, ya que esto puede provocar muchos errores de caché, lo que reduce la proporción de visitas de caché. AEM Para admitir el almacenamiento en caché de solicitudes variables en Dispatcher de la, [revise la documentación de almacenamiento en caché de variantes](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/advanced/variant-caching.html).

### Duración de caché{#cdn-cache-life}

AEM La CDN de publicación de datos está basada en TTL (tiempo de vida), lo que significa que la duración de la caché está determinada por la variable `Cache-Control`, `Surrogate-Control`, o `Expires` Encabezados de respuesta HTTP. Si el proyecto no establece los encabezados de almacenamiento en caché de respuestas HTTP, y la variable [criterios de idoneidad](#when-are-http-requestsresponses-cached) Cuando se cumplen, la Adobe establece una duración predeterminada de 10 minutos (600 segundos).

Así es como los encabezados de caché influyen en la duración de la caché de la CDN:

+ [`Cache-Control`](https://developer.fastly.com/reference/http/http-headers/Cache-Control/) El encabezado de respuesta HTTP indica al explorador web y a la CDN cuánto tiempo deben almacenarse en caché las respuestas. El valor se expresa en segundos. Por ejemplo, `Cache-Control: max-age=3600` indica al explorador web que almacene en caché la respuesta durante una hora. CDN ignora este valor si `Surrogate-Control` El encabezado de respuesta HTTP también está presente.
+ [`Surrogate-Control`](https://developer.fastly.com/reference/http/http-headers/Surrogate-Control/) AEM El encabezado de respuesta HTTP indica al CDN de la cuánto tiempo debe almacenar la respuesta en caché. El valor se expresa en segundos. Por ejemplo, `Surrogate-Control: max-age=3600` indica a la CDN que almacene en caché la respuesta durante una hora.
+ [`Expires`](https://developer.fastly.com/reference/http/http-headers/Expires/) AEM El encabezado de respuesta HTTP indica al CDN (y al explorador web) de la cuánto tiempo es válida la respuesta en caché. El valor es una fecha. Por ejemplo, `Expires: Sat, 16 Sept 2023 09:00:00 EST` indica al explorador web que almacene en caché la respuesta hasta la fecha y hora especificadas.

Uso `Cache-Control` para controlar la duración de la caché cuando sea la misma tanto para el explorador como para CDN. Uso `Surrogate-Control` cuando el explorador web debe almacenar la respuesta en caché durante un tiempo diferente al de la CDN.

#### Duración predeterminada de la caché

AEM Si una respuesta HTTP cumple los requisitos para el almacenamiento en caché de Dispatcher de [por cualificadores anteriores](#when-are-http-requestsresponses-cached), los siguientes son los valores predeterminados a menos que haya una configuración personalizada.

| Tipo de contenido | Duración predeterminada de la caché de CDN |
|:------------ |:---------- |
| [HTML/JSON/XML](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#html-text) | 5 minutos |
| [Recursos (imágenes, vídeos, documentos, etc.)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#images) | 10 minutos |
| [Consultas persistentes (JSON)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html?publish-instances) | 2 horas |
| [Bibliotecas de cliente (JS/CSS)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#client-side-libraries) | 30 días |
| [Otros](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#other-content) | No almacenado en caché |

### Personalizar las reglas de caché

[Configuración de cómo la CDN almacena en caché el contenido](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#disp) se limita a establecer encabezados de caché en respuestas HTTP. AEM Estos encabezados de caché suelen configurarse en Dispatcher de la interfaz de usuario de `vhost` configuraciones que utilizan `mod_headers`AEM , pero también se puede configurar en el código Java™ personalizado que se ejecuta en la propia publicación de la publicación de la.

## Dispatcher de AEM

![AEM AEM Publicación de Dispatcher](./assets/publish/publish-dispatcher.png){align="center"}

### ¿Cuándo se almacenan en caché las solicitudes/respuestas HTTP?

Las respuestas HTTP para las solicitudes HTTP correspondientes se almacenan en caché cuando se cumplen los siguientes criterios:

+ El método de solicitud HTTP es `GET` o `HEAD`
   + `HEAD` Las solicitudes HTTP solo almacenan en caché los encabezados de respuesta HTTP. No tienen cuerpos de respuesta.
+ El estado de respuesta HTTP es `200`
+ La respuesta HTTP NO es para un archivo binario.
+ La ruta de la URL de solicitud HTTP termina con una extensión, por ejemplo: `.html`, `.json`, `.css`, `.js`, etc.
+ AEM La solicitud HTTP no contiene autorización y no está autenticada por los usuarios de la aplicación de la autenticación de.
   + Sin embargo, el almacenamiento en caché de solicitudes autenticadas [se puede habilitar globalmente](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#caching-when-authentication-is-used) o selectivamente mediante [almacenamiento en caché con permisos confidenciales](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/permissions-cache.html?lang=es).
+ La solicitud HTTP no contiene parámetros de consulta.
   + Sin embargo, configurar [Parámetros de consulta ignorados](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=es#ignoring-url-parameters) permite que las solicitudes HTTP con los parámetros de consulta ignorados se almacenen en caché o se proporcionen desde la caché.
+ Ruta de la solicitud HTTP [coincide con una regla de permitir Dispatcher y no coincide con una regla de denegación](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#specifying-the-documents-to-cache).
+ AEM La respuesta HTTP no tiene ninguno de los siguientes encabezados de respuesta HTTP establecidos por Publicación de la:

   + `no-cache`
   + `no-store`
   + `must-revalidate`

### ¿Qué se almacena en caché?

AEM Dispatcher almacena en caché lo siguiente:

+ cuerpo de respuesta HTTP
+ Encabezados de respuesta HTTP especificados en el [configuración de encabezados de caché](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#caching-http-response-headers). Consulte la configuración predeterminada que se incluye con [AEM Tipo de archivo del proyecto](https://github.com/adobe/aem-project-archetype/blob/develop/src/main/archetype/dispatcher.cloud/src/conf.dispatcher.d/available_farms/default.farm#L106-L113).
   + `Cache-Control`
   + `Content-Disposition`
   + `Content-Type`
   + `Expires`
   + `Last-Modified`
   + `X-Content-Type-Options`

### Duración de caché

AEM Dispatcher almacena en caché las respuestas HTTP mediante los siguientes métodos:

+ Hasta que la invalidación se active mediante mecanismos como la publicación o cancelación de la publicación del contenido.
+ TTL (tiempo de vida) cuando se establece explícitamente [configurado en la configuración de Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#configuring-time-based-cache-invalidation-enablettl). Consulte la configuración predeterminada en la [AEM Tipo de archivo del proyecto](https://github.com/adobe/aem-project-archetype/blob/develop/src/main/archetype/dispatcher.cloud/src/conf.dispatcher.d/available_farms/default.farm#L122-L127) revisando la `enableTTL` configuración.

#### Duración predeterminada de la caché

AEM Si una respuesta HTTP cumple los requisitos para el almacenamiento en caché de Dispatcher de [por cualificadores anteriores](#when-are-http-requestsresponses-cached-1), los siguientes son los valores predeterminados a menos que haya una configuración personalizada.

| Tipo de contenido | Duración predeterminada de la caché de CDN |
|:------------ |:---------- |
| [HTML/JSON/XML](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#html-text) | Hasta la invalidación |
| [Recursos (imágenes, vídeos, documentos, etc.)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#images) | Nunca |
| [Consultas persistentes (JSON)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html?publish-instances) | 1 minuto |
| [Bibliotecas de cliente (JS/CSS)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#client-side-libraries) | 30 días |
| [Otros](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#other-content) | Hasta la invalidación |

### Personalizar las reglas de caché

AEM La caché de Dispatcher se puede configurar mediante la variable [Configuración de Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache) incluyendo:

+ Qué se almacena en caché
+ Qué partes de la caché se invalidan al publicar/cancelar la publicación
+ Qué parámetros de consulta de solicitud HTTP se omiten al evaluar la caché
+ Qué encabezados de respuesta HTTP se almacenan en caché
+ Habilitar o deshabilitar el almacenamiento en caché de TTL
+ ... y mucho más

Uso de `mod_headers` para establecer los encabezados de caché `vhost` AEM La configuración de no afectará al almacenamiento en caché de Dispatcher (basado en TTL), ya que se añaden a la respuesta HTTP después de que Dispatcher procese la respuesta de forma independiente. AEM Para afectar al almacenamiento en caché de Dispatcher a través de encabezados de respuesta HTTP, se requiere código Java™ personalizado que se ejecute en Publicación de la aplicación que establezca los encabezados de respuesta HTTP adecuados.
