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
source-git-commit: 43c021b051806380b3211f2d7357555622217b91
workflow-type: tm+mt
source-wordcount: '897'
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

    &quot;conf
    &lt;locationmatch url=&quot;&quot; url_regex=&quot;&quot;>
    # Quita el encabezado de respuesta de este nombre, si existe. Si hay varios encabezados con el mismo nombre, se eliminarán todos.
    Encabezado unset Cache-Control
    Encabezado unset Surrogate-Control
    Caduca el encabezado desconfigurado
    
    # Indica al explorador web y a la CDN que almacenen en caché la respuesta durante el valor &quot;max-age&quot; (XXX) segundos. Los atributos &quot;stale-while-revalidate&quot; y &quot;stale-if-error&quot; controlan el tratamiento del estado de obsolescencia en la capa CDN.
    Encabezado set Cache-Control &quot;max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX&quot;
    
    # Indica a la CDN que almacene en caché la respuesta durante el valor &quot;max-age&quot; (XXX) segundos. Los atributos &quot;stale-while-revalidate&quot; y &quot;stale-if-error&quot; controlan el tratamiento del estado de obsolescencia en la capa CDN.
    Encabezado set Surrogate-Control &quot;max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX&quot;
    
    # Indica al explorador web y a la CDN que almacenen en caché la respuesta hasta la fecha y la hora especificadas.
    Conjunto de encabezados Caduca &quot;Dom, 31 Dic 2023 23:59:59 GMT&quot;
    &lt;/locationmatch>
    &quot;

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

       &quot;conf
       &lt;locationmatch content=&quot;&quot;>*\.(html)$&quot;>
       # Quita el encabezado de respuesta si está presente.
       Encabezado unset Cache-Control
       
       # Indica al explorador web y a la CDN que almacenen en caché la respuesta del valor de edad máxima (600) segundos.
       Encabezado set Cache-Control &quot;max-age=600&quot;
       &lt;/locationmatch>
       &quot;
   Los archivos vhost de `dispatcher/src/conf.d/enabled_vhosts` Los directorios son **enlaces simbólicos** a los archivos de `dispatcher/src/conf.d/available_vhosts` , así que asegúrese de crear enlaces simbólicos si no están presentes.
1. AEM Implemente los cambios de vhost en el entorno as a Cloud Service deseado utilizando la variable [Cloud Manager: Canalización de configuración de nivel web](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?#web-tier-config-pipelines) o [Comandos de RDE](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/how-to-use.html?lang=en#deploy-apache-or-dispatcher-configuration).

Sin embargo, para tener valores diferentes para la vida útil del explorador web y la caché de CDN, puede utilizar el complemento `Surrogate-Control` encabezado en el ejemplo anterior. Del mismo modo, para que la caché caduque en una fecha y hora específicas, puede utilizar el `Expires` encabezado. Además, el uso de `stale-while-revalidate` y `stale-if-error` atributos, puede controlar el tratamiento del estado de obsolescencia del contenido de respuesta. AEM El proyecto de WKND de la tiene un [tratamiento de referencia del estado de obsolescencia](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost#L150-L155) Configuración de caché de CDN.

Del mismo modo, también puede actualizar los encabezados de caché para otros tipos de contenido (JSON, JS, CSS y Assets).

### Código Java™ personalizado

AEM Esta opción está disponible tanto para Publicación como para Autor de la. AEM Sin embargo, no se recomienda habilitar el almacenamiento en caché en el Autor de la y mantener el comportamiento de almacenamiento en caché predeterminado.

Para actualizar los encabezados de caché, utilice el `HttpServletResponse` en el código Java™ personalizado (servlet de Sling, filtro de servlet de Sling). La sintaxis general es la siguiente:

    &quot;java
    // Indica al explorador web y a la CDN que almacenen en caché la respuesta durante el valor &quot;max-age&quot; (XXX) segundos. Los atributos &quot;stale-while-revalidate&quot; y &quot;stale-if-error&quot; controlan el tratamiento del estado de obsolescencia en la capa CDN.
    response.setHeader(&quot;Cache-Control&quot;, &quot;max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX&quot;);
    
    // Indica a la CDN que almacene en caché la respuesta durante el valor &quot;max-age&quot; (XXX) segundos. Los atributos &quot;stale-while-revalidate&quot; y &quot;stale-if-error&quot; controlan el tratamiento del estado de obsolescencia en la capa CDN.
    response.setHeader(&quot;Surrogate-Control&quot;, &quot;max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX&quot;);
    
    // Indica al explorador web y a la CDN que almacenen en caché la respuesta hasta la fecha y hora especificadas.
    response.setHeader(&quot;Expires&quot;, &quot;Sun, 31 Dec 2023 23:59:59 GMT&quot;);
    &quot;
