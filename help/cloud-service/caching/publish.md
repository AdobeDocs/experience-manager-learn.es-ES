---
title: AEM Almacenamiento en caché del servicio Publish
description: Información general sobre el almacenamiento en caché del servicio AEM as a Cloud Service Publish.
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
duration: 240
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1134'
ht-degree: 2%

---

# Publicación de AEM

AEM El servicio de Publish tiene dos capas principales de almacenamiento en caché, la CDN de AEM as a Cloud Service AEM y la Dispatcher de la. Opcionalmente, se puede colocar una CDN administrada por el cliente delante de la CDN de AEM as a Cloud Service. La CDN de AEM as a Cloud Service proporciona una entrega de contenido perimetral, lo que garantiza que las experiencias se entreguen con baja latencia a usuarios de todo el mundo. AEM Dispatcher AEM proporciona almacenamiento en caché directamente delante de Publish AEM, y se utiliza para mitigar la carga innecesaria en el propio Publish, lo que se utiliza para reducir la carga innecesaria de la aplicación en la que se almacena en caché.

AEM ![Diagrama de información general sobre el almacenamiento en caché de Publish](./assets/publish/publish-all.png){align="center"}

## La red de distribución de contenido (CDN)

El almacenamiento en caché de CDN de AEM as a Cloud Service está controlado por encabezados de caché de respuesta HTTP y su objetivo es almacenar en caché el contenido para optimizar el equilibrio entre actualización y rendimiento. AEM La CDN se encuentra entre el usuario final y el Dispatcher de la, y se utiliza para almacenar en caché el contenido lo más cerca posible del usuario final, lo que garantiza una experiencia de rendimiento.

AEM ![CDN DE Publish ](./assets/publish/publish-cdn.png){align="center"}

Configurar cómo la CDN almacena en caché el contenido se limita a establecer encabezados de caché en las respuestas HTTP. AEM Estos encabezados de caché generalmente se establecen en configuraciones de host de Dispatcher AEM usando `mod_headers`, pero también se pueden establecer en el código Java™ personalizado que se ejecuta en el propio Publish.

### ¿Cuándo se almacenan en caché las solicitudes/respuestas HTTP?

AEM as a Cloud Service CDN almacena en caché solo las respuestas HTTP y deben cumplirse todos los criterios siguientes:

+ El estado de respuesta HTTP es `2xx` o `3xx`
+ El método de solicitud HTTP es `GET` o `HEAD`
+ Al menos uno de los siguientes encabezados de respuesta HTTP está presente: `Cache-Control`, `Surrogate-Control` o `Expires`
+ La respuesta HTTP puede ser cualquier tipo de contenido, incluidos HTML, JSON, CSS, JS y archivos binarios.

AEM De manera predeterminada, las respuestas HTTP no almacenadas en caché por [Dispatcher](#aem-dispatcher) quitan automáticamente cualquier encabezado de caché de respuestas HTTP para evitar el almacenamiento en caché en CDN. Este comportamiento se puede reemplazar cuidadosamente usando `mod_headers` con la directiva `Header always set ...` cuando sea necesario.

### ¿Qué se almacena en caché?

AEM as a Cloud Service CDN almacena en caché lo siguiente:

+ cuerpo de respuesta HTTP
+ Encabezados de respuesta HTTP

Normalmente, una solicitud/respuesta HTTP para una sola URL se almacena en caché como un solo objeto. Sin embargo, la CDN puede administrar el almacenamiento en caché de varios objetos para una sola dirección URL, cuando el encabezado `Vary` está establecido en la respuesta HTTP. Evite especificar `Vary` en encabezados cuyos valores no tengan un conjunto de valores estrictamente controlados, ya que esto puede provocar muchos errores de caché, lo que reduce la proporción de visitas de caché. AEM Para admitir el almacenamiento en caché de distintas solicitudes en Dispatcher, [revise la documentación de almacenamiento en caché de variantes](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/advanced/variant-caching.html).

### Duración de caché{#cdn-cache-life}

AEM La CDN de Publish se basa en TTL (time-to-live), lo que significa que la duración de la caché está determinada por los encabezados de respuesta HTTP `Cache-Control`, `Surrogate-Control` o `Expires`. Si el proyecto no establece los encabezados de almacenamiento en caché de respuestas HTTP y se cumplen los [criterios de idoneidad](#when-are-http-requestsresponses-cached), la Adobe establece una duración predeterminada de caché de 10 minutos (600 segundos).

Así es como los encabezados de caché influyen en la duración de la caché de la CDN:

+ El encabezado de respuesta HTTP [`Cache-Control`](https://developer.fastly.com/reference/http/http-headers/Cache-Control/) indica al explorador web y a la CDN cuánto tiempo deben almacenar la respuesta en caché. El valor se expresa en segundos. Por ejemplo, `Cache-Control: max-age=3600` indica al explorador web que almacene en caché la respuesta durante una hora. CDN ignora este valor si el encabezado de respuesta HTTP `Surrogate-Control` también está presente.
+ AEM El encabezado de respuesta HTTP [`Surrogate-Control`](https://developer.fastly.com/reference/http/http-headers/Surrogate-Control/) indica al CDN de la cuánto tiempo debe almacenar la respuesta en caché. El valor se expresa en segundos. Por ejemplo, `Surrogate-Control: max-age=3600` indica a la red de distribución de contenido (CDN) que almacene en caché la respuesta durante una hora.
+ AEM El encabezado de respuesta HTTP [`Expires`](https://developer.fastly.com/reference/http/http-headers/Expires/) indica a la CDN (y al explorador web) de la cuánto tiempo es válida la respuesta almacenada en caché. El valor es una fecha. Por ejemplo, `Expires: Sat, 16 Sept 2023 09:00:00 EST` indica al explorador web que almacene en caché la respuesta hasta la fecha y hora especificadas.

Utilice `Cache-Control` para controlar la duración de la caché cuando sea la misma tanto para el explorador como para CDN. Utilice `Surrogate-Control` cuando el explorador web deba almacenar la respuesta en caché durante un tiempo diferente al de la CDN.

#### Duración predeterminada de la caché

AEM Si una respuesta HTTP cumple los requisitos para el almacenamiento en caché de Dispatcher de [por los calificadores anteriores](#when-are-http-requestsresponses-cached), los siguientes son los valores predeterminados a menos que haya una configuración personalizada.

| Tipo de contenido | Duración predeterminada de la caché de CDN |
|:------------ |:---------- |
| [HTML/JSON/XML](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#html-text) | 5 minutos |
| [Assets (imágenes, vídeos, documentos, etc.)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#images) | 10 minutos |
| [Consultas persistentes (JSON)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html?publish-instances) | 2 horas |
| [Bibliotecas de cliente (JS/CSS)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#client-side-libraries) | 30 días |
| [Otros](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#other-content) | No almacenado en caché |

### Personalizar las reglas de caché

[Configurar cómo la CDN almacena en caché el contenido](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#disp) se limita a establecer encabezados de caché en respuestas HTTP. AEM Estos encabezados de caché suelen configurarse en configuraciones de Dispatcher AEM `vhost` de la aplicación que utilizan `mod_headers`, pero también pueden configurarse en el código Java™ personalizado que se ejecuta en el propio Publish de la aplicación de caché de la aplicación.

## Dispatcher de AEM

AEM ![Publish AEM Dispatcher](./assets/publish/publish-dispatcher.png){align="center"}

### ¿Cuándo se almacenan en caché las solicitudes/respuestas HTTP?

Las respuestas HTTP para las solicitudes HTTP correspondientes se almacenan en caché cuando se cumplen los siguientes criterios:

+ El método de solicitud HTTP es `GET` o `HEAD`
   + `HEAD` solicitudes HTTP solo almacenan en caché los encabezados de respuesta HTTP. No tienen cuerpos de respuesta.
+ El estado de respuesta HTTP es `200`
+ La respuesta HTTP NO es para un archivo binario.
+ La ruta de la dirección URL de la solicitud HTTP termina con una extensión, por ejemplo: `.html`, `.json`, `.css`, `.js`, etc.
+ AEM La solicitud HTTP no contiene autorización y no está autenticada por los usuarios de la aplicación de la autenticación de.
   + Sin embargo, el almacenamiento en caché de las solicitudes autenticadas [se puede habilitar globalmente](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#caching-when-authentication-is-used) o selectivamente mediante el almacenamiento en caché con permisos confidenciales [3}.](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/permissions-cache.html?lang=es)
+ La solicitud HTTP no contiene parámetros de consulta.
   + Sin embargo, configurar [Parámetros de consulta ignorados](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=es#ignoring-url-parameters) permite que las solicitudes HTTP con los parámetros de consulta ignorados se almacenen en caché o se proporcionen desde la caché.
+ La ruta de acceso de la solicitud HTTP [ coincide con una regla de permiso de Dispatcher y no coincide con una regla de denegación ](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#specifying-the-documents-to-cache).
+ AEM La respuesta HTTP no tiene ninguno de los siguientes encabezados de respuesta HTTP establecidos por el Publish de la:

   + `no-cache`
   + `no-store`
   + `must-revalidate`

### ¿Qué se almacena en caché?

AEM Dispatcher almacena en caché lo siguiente:

+ cuerpo de respuesta HTTP
+ Encabezados de respuesta HTTP especificados en la [configuración de encabezados de caché](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#caching-http-response-headers) de Dispatcher. AEM Consulte la configuración predeterminada que se envía con el [Arquetipo de proyecto de la aplicación ](https://github.com/adobe/aem-project-archetype/blob/develop/src/main/archetype/dispatcher.cloud/src/conf.dispatcher.d/available_farms/default.farm#L106-L113).
   + `Cache-Control`
   + `Content-Disposition`
   + `Content-Type`
   + `Expires`
   + `Last-Modified`
   + `X-Content-Type-Options`

### Duración de caché

AEM Dispatcher almacena en caché las respuestas HTTP mediante los siguientes métodos:

+ Hasta que la invalidación se active mediante mecanismos como la publicación o cancelación de la publicación del contenido.
+ TTL (tiempo de vida) cuando se [configura explícitamente en la configuración de Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#configuring-time-based-cache-invalidation-enablettl). AEM Vea la configuración predeterminada en el [Tipo de archivo del proyecto de](https://github.com/adobe/aem-project-archetype/blob/develop/src/main/archetype/dispatcher.cloud/src/conf.dispatcher.d/available_farms/default.farm#L122-L127) revisando la configuración de `enableTTL`.

#### Duración predeterminada de la caché

AEM Si una respuesta HTTP cumple los requisitos para el almacenamiento en caché de Dispatcher de [por los calificadores anteriores](#when-are-http-requestsresponses-cached-1), los siguientes son los valores predeterminados a menos que haya una configuración personalizada.

| Tipo de contenido | Duración predeterminada de la caché de CDN |
|:------------ |:---------- |
| [HTML/JSON/XML](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#html-text) | Hasta la invalidación |
| [Assets (imágenes, vídeos, documentos, etc.)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#images) | Nunca |
| [Consultas persistentes (JSON)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html?publish-instances) | 1 minuto |
| [Bibliotecas de cliente (JS/CSS)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#client-side-libraries) | 30 días |
| [Otros](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#other-content) | Hasta la invalidación |

### Personalizar las reglas de caché

AEM La caché de Dispatcher se puede configurar mediante la [configuración de Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache), que incluye:

+ Qué se almacena en caché
+ Qué partes de la caché se invalidan al publicar/cancelar la publicación
+ Qué parámetros de consulta de solicitud HTTP se omiten al evaluar la caché
+ Qué encabezados de respuesta HTTP se almacenan en caché
+ Habilitar o deshabilitar el almacenamiento en caché de TTL
+ ... y mucho más

Si se usa `mod_headers` para establecer encabezados de caché, la configuración de `vhost` no afectará al almacenamiento en caché de Dispatcher AEM (basado en TTL), ya que se agregarán a la respuesta HTTP después de que Dispatcher procese la respuesta de forma independiente. Para afectar al almacenamiento en caché de Dispatcher AEM a través de encabezados de respuesta HTTP, se requiere código Java™ personalizado que se ejecute en Publish y que establezca los encabezados de respuesta HTTP adecuados.
