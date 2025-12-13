---
title: Cómo habilitar el almacenamiento en caché de CDN
description: Obtenga información sobre cómo habilitar el almacenamiento en caché de respuestas HTTP en la CDN de AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
feature: Operations, CDN Cache
topic: Administration, Performance
role: Admin, Developer
level: Beginner
doc-type: Tutorial
last-substantial-update: 2023-11-17T00:00:00Z
jira: KT-14224
thumbnail: KT-14224.jpeg
exl-id: 544c3230-6eb6-4f06-a63c-f56d65c0ff4b
duration: 174
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '631'
ht-degree: 1%

---

# Cómo habilitar el almacenamiento en caché de CDN

Obtenga información sobre cómo habilitar el almacenamiento en caché de respuestas HTTP en la CDN de AEM as a Cloud Service. El almacenamiento en caché de las respuestas está controlado por `Cache-Control`, `Surrogate-Control` o `Expires` encabezados de caché de respuestas HTTP.

Estos encabezados de caché generalmente se establecen en configuraciones vhost de AEM Dispatcher mediante `mod_headers`, pero también se pueden establecer en código Java™ personalizado que se ejecuta en AEM Publish.

## Comportamiento de almacenamiento en caché predeterminado

Cuando NO hay configuraciones personalizadas presentes, se utilizan los valores predeterminados. En la siguiente captura de pantalla, puede ver el comportamiento de almacenamiento en caché predeterminado para AEM Publish y Author cuando se implementa un [Arquetipo de proyecto de AEM](https://github.com/adobe/aem-project-archetype) basado en `mynewsite` proyecto de AEM.

![Comportamiento de almacenamiento en caché predeterminado](../assets/how-to/aem-publish-default-cache-headers.png){width="800" zoomable="yes"}

Revise [Publicación de AEM - Duración predeterminada de la caché](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/caching/publish.html#cdn-cache-life) y [Autor de AEM - Duración predeterminada de la caché](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/caching/author.html?#default-cache-life) para obtener más información.

En resumen, AEM as a Cloud Service almacena en caché la mayoría de los tipos de contenido (HTML, JSON, JS, CSS y Assets) en AEM Publish y algunos tipos de contenido (JS, CSS) en AEM Author.

## Habilitar el almacenamiento en caché

Para cambiar el comportamiento predeterminado del almacenamiento en caché, puede actualizar los encabezados de caché de dos formas.

1. **Configuración de vhost de Dispatcher:** Solo disponible para publicación en AEM.
1. **Código Java™ personalizado:** disponible tanto para publicación como para autor en AEM.

Revisemos cada una de estas opciones.

### Configuración de vhost de Dispatcher

Esta opción es el método recomendado para habilitar el almacenamiento en caché, pero solo está disponible para AEM Publish. Para actualizar los encabezados de caché, utilice el módulo `mod_headers` y la directiva `<LocationMatch>` en el archivo vhost del servidor HTTP de Apache. La sintaxis general es la siguiente:

```
<LocationMatch "$URL$ || $URL_REGEX$">
    # Removes the response header of this name, if it exists. If there are multiple headers of the same name, all will be removed.
    Header unset Cache-Control
    Header unset Surrogate-Control
    Header unset Expires

    # Instructs the web browser and CDN to cache the response for 'max-age' value (XXX) seconds. The 'stale-while-revalidate' and 'stale-if-error' attributes controls the stale state treatment at CDN layer.
    Header set Cache-Control "max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX"
    
    # Instructs the CDN to cache the response for 'max-age' value (XXX) seconds. The 'stale-while-revalidate' and 'stale-if-error' attributes controls the stale state treatment at CDN layer.
    Header set Surrogate-Control "max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX"
    
    # Instructs the web browser and CDN to cache the response until the specified date and time.
    Header set Expires "Sun, 31 Dec 2023 23:59:59 GMT"
</LocationMatch>
```

A continuación se resume el propósito de cada **encabezado** y los **atributos** aplicables para el encabezado.

|                     | Navegador web | La red de distribución de contenido (CDN) | Descripción |
|---------------------|:-----------:|:---------:|:-----------:|
| Cache-Control | ✔ | ✔ | Este encabezado controla la duración del explorador web y la caché de la CDN. |
| Control de sustitución | ✘ | ✔ | Esta cabecera controla la duración de la caché de la CDN. |
| Caduca | ✔ | ✔ | Este encabezado controla la duración del explorador web y la caché de la CDN. |


- **max-age**: este atributo controla el TTL o el &quot;tiempo de vida&quot; del contenido de la respuesta en segundos.
- **stale-while-revalidate**: este atributo controla el tratamiento de _stale state_ del contenido de respuesta en la capa de CDN cuando la solicitud recibida se encuentra dentro del período especificado en segundos. El _estado obsoleto_ es el período de tiempo después de que caduque el TTL y antes de que se vuelva a validar la respuesta.
- **stale-if-error**: este atributo controla el tratamiento de _stale state_ del contenido de respuesta en la capa de CDN cuando el servidor de origen no está disponible y la solicitud recibida se encuentra dentro del período especificado en segundos.

Revise los detalles de [inactividad y revalidación](https://developer.fastly.com/learning/concepts/edge-state/cache/stale/) para obtener más información.

#### Ejemplo

Para aumentar la duración de la caché del explorador web y la red de distribución de contenido (CDN) de **tipo de contenido HTML** a _10 minutos_ sin un tratamiento de estado obsoleto, siga estos pasos:

1. En su proyecto de AEM, busque el archivo vhost deseado en el directorio `dispatcher/src/conf.d/available_vhosts`.
1. Actualice el archivo vhost (p. ej. `wknd.vhost`) de la siguiente manera:

   ```
   <LocationMatch "^/content/.*\.(html)$">
       # Removes the response header if present
       Header unset Cache-Control
   
       # Instructs the web browser and CDN to cache the response for max-age value (600) seconds.
       Header set Cache-Control "max-age=600"
   </LocationMatch>
   ```

   Los archivos vhost del directorio `dispatcher/src/conf.d/enabled_vhosts` son **symlinks** a los archivos del directorio `dispatcher/src/conf.d/available_vhosts`, así que asegúrese de crear enlaces simbólicos si no los hay.
1. Implemente los cambios de vhost en el entorno de AEM as a Cloud Service deseado mediante [Cloud Manager - Canalización de configuración de nivel web](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?#web-tier-config-pipelines) o [Comandos RDE](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/how-to-use.html?lang=en#deploy-apache-or-dispatcher-configuration).

Sin embargo, para tener valores diferentes para la duración de la caché del explorador web y la CDN, puede utilizar el encabezado `Surrogate-Control` del ejemplo anterior. Del mismo modo, para que la caché caduque en una fecha y hora específicas, puede utilizar el encabezado `Expires`. Además, con los atributos `stale-while-revalidate` y `stale-if-error`, puede controlar el tratamiento del estado antiguo del contenido de la respuesta. El proyecto WKND de AEM tiene una configuración de caché de CDN [tratamiento de estado obsoleto de referencia](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost#L150-L155).

Del mismo modo, también puede actualizar los encabezados de caché para otros tipos de contenido (JSON, JS, CSS y Assets).

### Código Java™ personalizado

Esta opción está disponible tanto para Publicación en AEM como para Autor. Sin embargo, no se recomienda habilitar el almacenamiento en caché en AEM Author y mantener el comportamiento de almacenamiento en caché predeterminado.

Para actualizar los encabezados de caché, utilice el objeto `HttpServletResponse` en el código Java™ personalizado (servlet Sling, filtro servlet Sling). La sintaxis general es la siguiente:

```java
// Instructs the web browser and CDN to cache the response for 'max-age' value (XXX) seconds. The 'stale-while-revalidate' and 'stale-if-error' attributes controls the stale state treatment at CDN layer.
response.setHeader("Cache-Control", "max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX");

// Instructs the CDN to cache the response for 'max-age' value (XXX) seconds. The 'stale-while-revalidate' and 'stale-if-error' attributes controls the stale state treatment at CDN layer.
response.setHeader("Surrogate-Control", "max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX");

// Instructs the web browser and CDN to cache the response until the specified date and time.
response.setHeader("Expires", "Sun, 31 Dec 2023 23:59:59 GMT");
```
