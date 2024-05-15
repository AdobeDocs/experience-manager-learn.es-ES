---
title: Cómo deshabilitar el almacenamiento en caché de CDN
description: AEM Obtenga información sobre cómo deshabilitar el almacenamiento en caché de respuestas HTTP en la red de distribución de contenido (CDN) de as a Cloud Service de datos (CDN).
version: Cloud Service
feature: Operations, CDN Cache
topic: Administration, Performance
role: Admin, Architect, Developer
level: Beginner
doc-type: Tutorial
last-substantial-update: 2023-11-30T00:00:00Z
jira: KT-14224
thumbnail: KT-14224.jpeg
exl-id: 22b1869e-5bb5-437d-9cb5-2d27f704c052
duration: 100
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '400'
ht-degree: 0%

---

# Cómo deshabilitar el almacenamiento en caché de CDN

AEM Obtenga información sobre cómo deshabilitar el almacenamiento en caché de respuestas HTTP en la red de distribución de contenido (CDN) de as a Cloud Service de datos (CDN). El almacenamiento en caché de las respuestas está controlado por `Cache-Control`, `Surrogate-Control`, o `Expires` Encabezados de caché de respuesta HTTP.

AEM Estos encabezados de caché suelen configurarse en configuraciones de vhost de Dispatcher con el uso de `mod_headers`AEM , pero también se puede configurar en el código Java™ personalizado que se ejecuta en la propia publicación de la publicación de la.

## Comportamiento de almacenamiento en caché predeterminado

AEM Revise el comportamiento de almacenamiento en caché predeterminado para Publicación y Autor de cuando un [AEM Tipo de archivo del proyecto](./enable-caching.md#default-caching-behavior) AEM Se implementa el proyecto basado en el cliente.

## Deshabilitar almacenamiento en caché

AEM Desactivar el almacenamiento en caché puede tener un impacto negativo en el rendimiento de la instancia as a Cloud Service de la aplicación, por lo que debe tener cuidado al desactivar el comportamiento predeterminado del almacenamiento en caché.

Sin embargo, hay algunos escenarios en los que puede que desee deshabilitar el almacenamiento en caché, como:

- Desarrollar una nueva función y desea ver los cambios inmediatamente.
- El contenido es seguro (solo para usuarios autenticados) o dinámico (carro de compras, detalles de pedidos) y no debe almacenarse en caché.

Para deshabilitar el almacenamiento en caché, puede actualizar los encabezados de caché de dos formas.

1. **Configuración de vhost de Dispatcher:** AEM Solo disponible para publicación en el servidor de correo electrónico.
1. **Código Java™ personalizado:** AEM Disponible tanto para publicación como para autor de.

Revisemos cada una de estas opciones.

### Configuración de vhost de Dispatcher

AEM Esta opción es el método recomendado para deshabilitar el almacenamiento en caché; sin embargo, solo está disponible para Publicación de la. Para actualizar los encabezados de caché, utilice el `mod_headers` módulo y `<LocationMatch>` en el archivo vhost del servidor HTTP de Apache. La sintaxis general es la siguiente:

```
<LocationMatch "$URL$ || $URL_REGEX$">
    # Removes the response header of this name, if it exists. If there are multiple headers of the same name, all will be removed.
    Header unset Cache-Control
    Header unset Expires

    # Instructs the CDN to not cache the response.
    Header set Cache-Control "private"
</LocationMatch>
```

#### Ejemplos

Para deshabilitar el almacenamiento en caché de CDN de **Tipos de contenido CSS** para solucionar algunos problemas, siga estos pasos.

Tenga en cuenta que, para omitir la caché CSS existente, se requiere un cambio en el archivo CSS para generar una nueva clave de caché para el archivo CSS.

1. AEM En el proyecto de, busque el archivo vhsot deseado en `dispatcher/src/conf.d/available_vhosts` directorio.
1. Actualice el vhost (p. ej., `wknd.vhost`) como se indica a continuación:

   ```
   <LocationMatch "^/etc.clientlibs/.*\.(css)$">
       # Removes the response header of this name, if it exists. If there are multiple headers of the same name, all will be removed.
       Header unset Cache-Control
       Header unset Expires
   
       # Instructs the CDN to not cache the response.
       Header set Cache-Control "private"
   </LocationMatch>
   ```

   Los archivos vhost de `dispatcher/src/conf.d/enabled_vhosts` Los directorios son **enlaces simbólicos** a los archivos de `dispatcher/src/conf.d/available_vhosts` , así que asegúrese de crear enlaces simbólicos si no están presentes.
1. AEM Implemente los cambios de vhost en el entorno as a Cloud Service deseado utilizando la variable [Cloud Manager: Canalización de configuración de nivel web](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?#web-tier-config-pipelines) o [Comandos de RDE](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/how-to-use.html?lang=en#deploy-apache-or-dispatcher-configuration).

### Código Java™ personalizado

AEM Esta opción está disponible tanto para Publicación como para Autor de la. Para actualizar los encabezados de caché, utilice el `SlingHttpServletResponse` en el código Java™ personalizado (servlet de Sling, filtro de servlet de Sling). La sintaxis general es la siguiente:

```java
response.setHeader("Cache-Control", "private");
```
