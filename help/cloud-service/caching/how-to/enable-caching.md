---
title: Cómo habilitar el almacenamiento en caché de CDN
description: AEM Obtenga información sobre cómo habilitar el almacenamiento en caché de respuestas HTTP en la red de distribución de contenido (CDN) de as a Cloud Service de datos (CDN) de la.
version: Cloud Service
feature: Operations, CDN Cache
topic: Administration, Performance
role: Admin, Architect, Developer
level: Beginner
doc-type: Tutorial
last-substantial-update: 2023-11-17T00:00:00Z
jira: KT-14224
thumbnail: KT-14224.jpeg
exl-id: 544c3230-6eb6-4f06-a63c-f56d65c0ff4b
duration: 174
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '637'
ht-degree: 0%

---

# Cómo habilitar el almacenamiento en caché de CDN

AEM Obtenga información sobre cómo habilitar el almacenamiento en caché de respuestas HTTP en la red de distribución de contenido (CDN) de as a Cloud Service de datos (CDN) de la. El almacenamiento en caché de las respuestas está controlado por `Cache-Control`, `Surrogate-Control`, o `Expires` Encabezados de caché de respuesta HTTP.

AEM Estos encabezados de caché suelen configurarse en configuraciones de vhost de Dispatcher con el uso de `mod_headers`AEM , pero también se puede configurar en el código Java™ personalizado que se ejecuta en la propia publicación de la publicación de la.

## Comportamiento de almacenamiento en caché predeterminado

Cuando NO hay configuraciones personalizadas presentes, se utilizan los valores predeterminados. AEM En la siguiente captura de pantalla, puede ver el comportamiento de almacenamiento en caché predeterminado para Publicación y Autor de la cuando se crea una [AEM Tipo de archivo del proyecto](https://github.com/adobe/aem-project-archetype) basado `mynewsite` AEM Se ha implementado el proyecto de.

![Comportamiento de almacenamiento en caché predeterminado](../assets/how-to/aem-publish-default-cache-headers.png){width="800" zoomable="yes"}

Revise la [AEM Publicación - Duración predeterminada de la caché](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/caching/publish.html#cdn-cache-life) y [AEM Autor de la: duración predeterminada de la caché](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/caching/author.html?#default-cache-life) para obtener más información.

AEM En resumen, el as a Cloud Service de almacena en caché la mayoría de los tipos de contenido (HTML AEM AEM, JSON, JS, CSS y Assets) en la publicación de la publicación de la aplicación y algunos tipos de contenido (JS, CSS) en la publicación de la aplicación de autor de la aplicación (CSS) de la.

## Habilitar almacenamiento en caché

Para cambiar el comportamiento predeterminado del almacenamiento en caché, puede actualizar los encabezados de caché de dos formas.

1. **Configuración de vhost de Dispatcher:** AEM Solo disponible para publicación en el servidor de correo electrónico.
1. **Código Java™ personalizado:** AEM Disponible tanto para publicación como para autor de.

Revisemos cada una de estas opciones.

### Configuración de vhost de Dispatcher

AEM Esta opción es el método recomendado para habilitar el almacenamiento en caché; sin embargo, solo está disponible para publicación en la. Para actualizar los encabezados de caché, utilice el `mod_headers` módulo y `<LocationMatch>` en el archivo vhost del servidor HTTP de Apache. La sintaxis general es la siguiente:

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

A continuación se resume el propósito de cada **encabezado** y aplicable **atributos** para el encabezado.

|                     | Navegador web | La red de distribución de contenido (CDN) | Descripción |
|---------------------|:-----------:|:---------:|:-----------:|
| Cache-Control | ✔ | ✔ | Este encabezado controla la duración del explorador web y la caché de la CDN. |
| Control de sustitución | ✘ | ✔ | Esta cabecera controla la duración de la caché de la CDN. |
| Caduca | ✔ | ✔ | Este encabezado controla la duración del explorador web y la caché de la CDN. |


- **max-age**: Este atributo controla el TTL o el &quot;tiempo de vida&quot; del contenido de respuesta en segundos.
- **stale-while-revalidate**: este atributo controla el _estado antiguo_ tratamiento del contenido de respuesta en la capa de CDN cuando la solicitud recibida se encuentra dentro del período especificado en segundos. El _estado antiguo_ es el período de tiempo después de que caduque el TTL y antes de que se vuelva a validar la respuesta.
- **stale-if-error**: este atributo controla el _estado antiguo_ tratamiento del contenido de respuesta en la capa de CDN cuando el servidor de origen no está disponible y la solicitud recibida se encuentra dentro del período especificado en segundos.

Revise la [inmovilidad y revalidación](https://developer.fastly.com/learning/concepts/edge-state/cache/stale/) detalles para obtener más información.

#### Ejemplos

Para aumentar la vida útil del explorador web y la caché de CDN de **Tipo de contenido de HTML** hasta _10 minutos_ sin tratamiento de estado anticuado, siga estos pasos:

1. AEM En el proyecto de, busque el archivo vhsot deseado en `dispatcher/src/conf.d/available_vhosts` directorio.
1. Actualice el vhost (p. ej., `wknd.vhost`) como se indica a continuación:

   ```
   <LocationMatch "^/content/.*\.(html)$">
       # Removes the response header if present
       Header unset Cache-Control
   
       # Instructs the web browser and CDN to cache the response for max-age value (600) seconds.
       Header set Cache-Control "max-age=600"
   </LocationMatch>
   ```

   Los archivos vhost de `dispatcher/src/conf.d/enabled_vhosts` Los directorios son **enlaces simbólicos** a los archivos de `dispatcher/src/conf.d/available_vhosts` , así que asegúrese de crear enlaces simbólicos si no están presentes.
1. AEM Implemente los cambios de vhost en el entorno as a Cloud Service deseado utilizando la variable [Cloud Manager: Canalización de configuración de nivel web](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?#web-tier-config-pipelines) o [Comandos de RDE](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/how-to-use.html?lang=en#deploy-apache-or-dispatcher-configuration).

Sin embargo, para tener valores diferentes para la vida útil del explorador web y la caché de CDN, puede utilizar el complemento `Surrogate-Control` encabezado en el ejemplo anterior. Del mismo modo, para que la caché caduque en una fecha y hora específicas, puede utilizar el `Expires` encabezado. Además, el uso de `stale-while-revalidate` y `stale-if-error` atributos, puede controlar el tratamiento del estado de obsolescencia del contenido de respuesta. AEM El proyecto de WKND de la tiene un [tratamiento de referencia del estado de obsolescencia](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost#L150-L155) Configuración de caché de CDN.

Del mismo modo, también puede actualizar los encabezados de caché para otros tipos de contenido (JSON, JS, CSS y Assets).

### Código Java™ personalizado

AEM Esta opción está disponible tanto para Publicación como para Autor de la. AEM Sin embargo, no se recomienda habilitar el almacenamiento en caché en el Autor de la y mantener el comportamiento de almacenamiento en caché predeterminado.

Para actualizar los encabezados de caché, utilice el `HttpServletResponse` en el código Java™ personalizado (servlet de Sling, filtro de servlet de Sling). La sintaxis general es la siguiente:

```java
// Instructs the web browser and CDN to cache the response for 'max-age' value (XXX) seconds. The 'stale-while-revalidate' and 'stale-if-error' attributes controls the stale state treatment at CDN layer.
response.setHeader("Cache-Control", "max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX");

// Instructs the CDN to cache the response for 'max-age' value (XXX) seconds. The 'stale-while-revalidate' and 'stale-if-error' attributes controls the stale state treatment at CDN layer.
response.setHeader("Surrogate-Control", "max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX");

// Instructs the web browser and CDN to cache the response until the specified date and time.
response.setHeader("Expires", "Sun, 31 Dec 2023 23:59:59 GMT");
```
