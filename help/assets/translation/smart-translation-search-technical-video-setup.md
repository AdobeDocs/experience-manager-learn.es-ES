---
title: Configuración de la búsqueda de traducción inteligente con AEM Assets
description: La búsqueda inteligente de traducción permite el uso de términos de búsqueda que no estén en inglés para resolverlos en contenido en inglés. AEM Para configurar los paquetes de traducción inteligente, se debe instalar y configurar el paquete OSGi de traducción automática de Apache Oak Search, así como los paquetes de idioma Apache Joshua gratuitos y de código abierto pertinentes que contienen las reglas de traducción.
version: 6.4, 6.5
feature: Search
topic: Content Management
role: Developer
level: Beginner
doc-type: Technical Video
exl-id: 7be8c3d5-b944-4421-97b3-bd5766c1b1b5
duration: 603
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '811'
ht-degree: 0%

---

# Configuración de la búsqueda de traducción inteligente con AEM Assets{#set-up-smart-translation-search-with-aem-assets}

La búsqueda inteligente de traducción permite el uso de términos de búsqueda que no estén en inglés para resolverlos en contenido en inglés. AEM Para configurar los paquetes de traducción inteligente, se debe instalar y configurar el paquete OSGi de traducción automática de Apache Oak Search, así como los paquetes de idioma Apache Joshua gratuitos y de código abierto pertinentes que contienen las reglas de traducción.

>[!VIDEO](https://video.tv.adobe.com/v/21291?quality=12&learn=on)

>[!NOTE]
>
>AEM La búsqueda inteligente de traducciones debe configurarse en cada instancia de traducción que la requiera.

1. Descargue e instale el paquete OSGi de traducción automática de búsqueda de Oak
   * [Descargue el paquete OSGi de traducción automática de búsqueda de Oak AEM](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22) que corresponde a la versión de Oak de la aplicación en la que se va a realizar la búsqueda de.
   * Instale el paquete OSGi de traducción automática de búsqueda de Oak AEM descargado en el servidor de correo electrónico a través de [`/system/console/bundles`](http://localhost:4502/system/console/bundles).

2. Descargar y actualizar los paquetes de idioma de Apache Joshua
   * Descargue y descomprima los [paquetes de idioma Apache Joshua](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs) que desee.
   * Edite el archivo `joshua.config` y comente las 2 líneas que comienzan por:

     ```
     feature-function = LanguageModel ...
     ```

   * AEM Determine y registre el tamaño de la carpeta del modelo del paquete de idioma, ya que esto influye en la cantidad de espacio de pila adicional que requerirá la aplicación de un módulo de almacenamiento de datos en un solo espacio de trabajo.
   * Mueva la carpeta del paquete de idioma Apache Joshua descomprimida (con las `joshua.config` ediciones) a

     ```
     .../crx-quickstart/opt/<source_language-target_language>
     ```

     Por ejemplo:

     ```
      .../crx-quickstart/opt/es-en
     ```

3. AEM Reiniciar con asignación de memoria de montón actualizada
   * AEM Detener la
   * AEM Determinar el nuevo tamaño de pila necesario para la

      * AEM el tamaño de la pila sin idioma previo + el tamaño del directorio del modelo redondeado al 2 GB más cercano
      * AEM Por ejemplo: Si la instalación de paquetes de idioma previos requiere 8 GB de pila para ejecutarse y la carpeta del modelo del paquete de idioma tiene 3,8 GB sin comprimir, el nuevo tamaño de pila es:

        El `8GB` + original ( `3.75GB` redondeado al `2GB` más cercano, que es `4GB`) para un total de `12GB`

   * Compruebe que el equipo tiene esta cantidad de memoria disponible adicional.
   * AEM Actualizar los scripts de inicio de la aplicación para ajustar el nuevo tamaño de la pila

      * Ejemplo: `java -Xmx12g -jar cq-author-p4502.jar`

   * AEM Reinicie con el tamaño de pila aumentado.

   >[!NOTE]
   >
   >El espacio de pila necesario para los paquetes de idioma puede aumentar, especialmente cuando se utilizan varios paquetes de idioma.
   >
   >
   >Asegúrese siempre de que **la instancia tenga suficiente memoria** para dar cabida a los aumentos en el espacio asignado en la pila.
   >
   >
   >El montón base **siempre debe calcularse para admitir un rendimiento aceptable sin tener instalados paquetes de idioma**.

4. Registre los paquetes de idiomas a través de las configuraciones OSGi del proveedor de términos de consulta de texto completo de traducción automática de Apache Jackrabbit Oak

   * Para cada paquete de idioma, [cree una nueva configuración OSGi del proveedor de términos de consulta de texto completo de traducción automática de Apache Jackrabbit Oak AEM](http://localhost:4502/system/console/configMgr/org.apache.jackrabbit.oak.plugins.index.mt.MTFulltextQueryTermsProviderFactory) a través del administrador de configuración de la consola web de la consola web.

      * `Joshua Config Path` es la ruta absoluta al archivo joshua.config. AEM El proceso de la debe poder leer todos los archivos de la carpeta del paquete de idioma.
      * `Node types` son los tipos de nodo candidatos cuya búsqueda de texto completo involucrará este paquete de idioma para su traducción.
      * `Minimum score` es la puntuación de confianza mínima para un término traducido que se va a usar.

         * Por ejemplo, hombre (en español significa &quot;hombre&quot;) puede traducirse a la palabra inglesa &quot;man&quot; con una puntuación de confianza de `0.9` y también a la palabra inglesa &quot;human&quot; con una puntuación de confianza `0.2`. Si ajusta la puntuación mínima a `0.3`, se mantendría la traducción de &quot;hombre&quot; a &quot;hombre&quot;, pero se descartaría la traducción de &quot;hombre&quot; a &quot;humano&quot;, ya que esta puntuación de traducción de `0.2` es menor que la puntuación mínima de `0.3`.

5. Realizar una búsqueda de texto completo en recursos
   * Dado que dam: Asset es el tipo de nodo en el que se registra de nuevo este paquete de idioma, debemos buscar AEM Assets mediante la búsqueda de texto completo para validarlo.
   * AEM Vaya a > Assets y abra Omnisearch. Busque un término en el idioma en el que se instaló el paquete de idioma.
   * Si es necesario, ajuste la puntuación mínima en las configuraciones de OSGi para garantizar la precisión de los resultados.

6. Actualizar paquetes de idioma
   * Los paquetes de idiomas de Apache Joshua son mantenidos íntegramente por el proyecto Apache Joshua, y su actualización o corrección es a discreción del proyecto Apache Joshua.
   * AEM Si se actualiza un paquete de idioma, para instalar las actualizaciones en el, se deben seguir los pasos anteriores 2-4, ajustando el tamaño de la pila hacia arriba o hacia abajo según sea necesario.

      * Tenga en cuenta que cuando mueva el paquete de idioma descomprimido a la carpeta crx-quickstart/opt, mueva cualquier carpeta de paquete de idioma existente antes de copiar la nueva.

   * AEM Si no requiere un reinicio, las configuraciones relevantes del proveedor de términos de consulta de texto completo de Apache Jackrabbit Oak AEM Machine Translation OSGi que pertenecen a los paquetes de idiomas actualizados deben volver a guardarse para que los procesos de procesamiento de los archivos actualizados se procesen de forma.

## Actualizando el índice damAssetLucene {#updating-damassetlucene-index}

AEM AEM AEM Para que [Etiquetas inteligentes](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html) se vean afectadas por la Traducción inteligente de los recursos, debe actualizarse el índice de `/oak   :index  /damAssetLucene` de los recursos para que se indiquen las etiquetas predichas (el nombre del sistema para Etiquetas inteligentes) y que formen parte del índice Lucene agregado de recursos. Para ello, debe actualizar el índice de Etiquetas inteligentes.

En `/oak:index/damAssetLucene/indexRules/dam:Asset/properties/predicatedTags`, asegúrese de que la configuración sea la siguiente:

```xml
 <damAssetLucene jcr:primaryType="oak:QueryIndexDefinition">
        <indexRules jcr:primaryType="nt:unstructured">
            <dam:Asset jcr:primaryType="nt:unstructured">
                <properties jcr:primaryType="nt:unstructured">
                    ...
                    <predictedTags
                        jcr:primaryType="nt:unstructured"
                        isRegexp="{Boolean}true"
                        name="jcr:content/metadata/predictedTags/*/name"
                        useInSpellheck="{Boolean}true"
                        useInSuggest="{Boolean}true"
                        analyzed="{Boolean}true"
                        nodeScopeIndex="{Boolean}true"/>
```

## Recursos adicionales{#additional-resources}

* [Paquete OSGi de traducción automática de búsqueda de Apache Oak](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22)
* [Paquetes de idioma Apache Joshua](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)
* AEM [Etiquetas Inteligentes De](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html)
* [Prácticas recomendadas para consultar e indexar](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/best-practices-for-queries-and-indexing.html)
