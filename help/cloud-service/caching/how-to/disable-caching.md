---
title: Cómo deshabilitar el almacenamiento en caché de CDN
description: Obtenga información sobre cómo deshabilitar el almacenamiento en caché de respuestas HTTP en la CDN de AEM as a Cloud Service.
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

Obtenga información sobre cómo deshabilitar el almacenamiento en caché de respuestas HTTP en la CDN de AEM as a Cloud Service. El almacenamiento en caché de las respuestas está controlado por `Cache-Control`, `Surrogate-Control` o `Expires` encabezados de caché de respuestas HTTP.

AEM Estos encabezados de caché generalmente se establecen en configuraciones de host de Dispatcher AEM usando `mod_headers`, pero también se pueden establecer en el código Java™ personalizado que se ejecuta en el propio Publish.

## Comportamiento de almacenamiento en caché predeterminado

AEM Revise el comportamiento de almacenamiento en caché predeterminado para el autor y el Publish AEM AEM de la de datos cuando se implemente un proyecto basado en [Arquetipo de proyecto de](./enable-caching.md#default-caching-behavior).

## Deshabilitar almacenamiento en caché

Desactivar el almacenamiento en caché puede tener un impacto negativo en el rendimiento de la instancia de AEM as a Cloud Service, por lo que tenga cuidado al desactivar el comportamiento predeterminado del almacenamiento en caché.

Sin embargo, hay algunos escenarios en los que puede que desee deshabilitar el almacenamiento en caché, como:

- Desarrollar una nueva función y desea ver los cambios inmediatamente.
- El contenido es seguro (solo para usuarios autenticados) o dinámico (carro de compras, detalles de pedidos) y no debe almacenarse en caché.

Para deshabilitar el almacenamiento en caché, puede actualizar los encabezados de caché de dos formas.

1. **Configuración de vhost de Dispatcher AEM Publish:** Solo disponible para la configuración de vhost de.
1. AEM **Código Java™ personalizado:** Disponible tanto para Publish como para Autor de la aplicación de la aplicación de manera personalizada.

Revisemos cada una de estas opciones.

### Configuración de vhost de Dispatcher

AEM Esta opción es el método recomendado para deshabilitar el almacenamiento en caché; sin embargo, solo está disponible para el Publish de la. Para actualizar los encabezados de caché, utilice el módulo `mod_headers` y la directiva `<LocationMatch>` en el archivo vhost del servidor HTTP de Apache. La sintaxis general es la siguiente:

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

Para deshabilitar el almacenamiento en caché de CDN de los **tipos de contenido CSS** para algunos fines de solución de problemas, siga estos pasos.

Tenga en cuenta que, para omitir la caché CSS existente, se requiere un cambio en el archivo CSS para generar una nueva clave de caché para el archivo CSS.

1. AEM En el proyecto de la, busque el archivo vhsot deseado en el directorio `dispatcher/src/conf.d/available_vhosts`.
1. Actualice el archivo vhost (p. ej. `wknd.vhost`) de la siguiente manera:

   ```
   <LocationMatch "^/etc.clientlibs/.*\.(css)$">
       # Removes the response header of this name, if it exists. If there are multiple headers of the same name, all will be removed.
       Header unset Cache-Control
       Header unset Expires
   
       # Instructs the CDN to not cache the response.
       Header set Cache-Control "private"
   </LocationMatch>
   ```

   Los archivos vhost del directorio `dispatcher/src/conf.d/enabled_vhosts` son **symlinks** a los archivos del directorio `dispatcher/src/conf.d/available_vhosts`, así que asegúrese de crear enlaces simbólicos si no los hay.
1. Implemente los cambios de vhost en el entorno de AEM as a Cloud Service deseado mediante [Cloud Manager - Canalización de configuración de nivel web](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?#web-tier-config-pipelines) o [Comandos RDE](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/how-to-use.html?lang=en#deploy-apache-or-dispatcher-configuration).

### Código Java™ personalizado

AEM Esta opción está disponible tanto para Publish como para Autor de la. Para actualizar los encabezados de caché, utilice el objeto `SlingHttpServletResponse` en el código Java™ personalizado (servlet Sling, filtro servlet Sling). La sintaxis general es la siguiente:

```java
response.setHeader("Cache-Control", "private");
```
