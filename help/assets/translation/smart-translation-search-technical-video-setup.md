---
title: Configuración de la búsqueda de traducción inteligente con AEM Assets
description: La búsqueda de traducción inteligente permite el uso de términos de búsqueda que no estén en inglés para resolver el contenido en inglés. Para configurar AEM para la búsqueda de traducción inteligente, el paquete OSGi Apache Oak Search Machine Translation debe estar instalado y configurado, así como los paquetes de idioma Apache Joshua de código abierto y gratuito que contengan las reglas de traducción.
version: 6.4, 6.5
feature: Search
topic: Content Management
role: Developer
level: Beginner
exl-id: 7be8c3d5-b944-4421-97b3-bd5766c1b1b5
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '867'
ht-degree: 0%

---

# Configuración de la búsqueda de traducción inteligente con AEM Assets{#set-up-smart-translation-search-with-aem-assets}

La búsqueda de traducción inteligente permite el uso de términos de búsqueda que no estén en inglés para resolver el contenido en inglés. Para configurar AEM para la búsqueda de traducción inteligente, el paquete OSGi Apache Oak Search Machine Translation debe estar instalado y configurado, así como los paquetes de idioma Apache Joshua de código abierto y gratuito que contengan las reglas de traducción.

>[!VIDEO](https://video.tv.adobe.com/v/21291/?quality=9&learn=on)

>[!NOTE]
>
>La búsqueda de traducción inteligente debe configurarse en cada instancia de AEM que la requiera.

1. Descargue e instale el paquete OSGi Oak Search Machine Translation
   * [Descargar el paquete OSGi de Oak Search Machine Translation](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22) que corresponde a AEM versión de Oak.
   * Instale el paquete OSGi de traducción Oak Search Machine descargado en AEM mediante [ `/system/console/bundles`](http://localhost:4502/system/console/bundles).

2. Descargar y actualizar los paquetes de idioma de Apache Joshua
   * Descargue y descomprima el [Paquete de idioma Apache Joshua](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs).
   * Edite el `joshua.config` y comente las 2 líneas que comienzan por:

      ```
      feature-function = LanguageModel ...
      ```

   * Determine y registre el tamaño de la carpeta modelo del paquete de idioma, ya que esto influye en el espacio adicional que AEM necesitar.
   * Mueva la carpeta del paquete de idioma Apache Joshua descomprimido (con el `joshua.config` ediciones) a

      ```
      .../crx-quickstart/opt/<source_language-target_language>
      ```

      Por ejemplo:

      ```
       .../crx-quickstart/opt/es-en
      ```

3. Reiniciar AEM con asignación de memoria de pila actualizada
   * Detener AEM
   * Determine el nuevo tamaño de pila necesario para AEM

      * AEM tamaño de pila de falta de lenguaje previo + el tamaño del directorio modelo redondeado hasta los 2 GB más cercanos
      * Por ejemplo: Si los paquetes prelingüísticos de la instalación de AEM requieren 8 GB de memoria para ejecutarse y la carpeta modelo del paquete de idioma es de 3,8 GB sin comprimir, el nuevo tamaño de pila es:

         El original `8GB` + ( `3.75GB` redondeado hacia arriba hasta la más cercana `2GB`, que es `4GB`) para un total de `12GB`
   * Verifique que el equipo tenga esta cantidad de memoria disponible adicional.
   * Actualizar AEM scripts de inicio para ajustar el nuevo tamaño de pila

      * Ejemplo. `java -Xmx12g -jar cq-author-p4502.jar`
   * Reinicie AEM con el tamaño de pila aumentado.

   >[!NOTE]
   >
   >El espacio de memoria necesario para los paquetes de idiomas puede crecer, especialmente cuando se utilizan varios paquetes de idiomas.
   >
   >
   >Asegúrese siempre de que **la instancia tiene suficiente memoria** para dar cabida a los aumentos del espacio asignado.
   >
   >
   >La variable **la pila base siempre debe calcularse para soportar un rendimiento aceptable sin paquetes de idiomas** instalado.

4. Registre los paquetes de idiomas a través de las configuraciones OSGi del proveedor de términos de consulta de texto completo de Apache Jackrabbit Oak Machine Translation

   * Para cada paquete de idioma, [crear una nueva configuración OSGi del proveedor de términos de consulta de texto completo Apache Jackrabbit Oak Machine Translation](http://localhost:4502/system/console/configMgr/org.apache.jackrabbit.oak.plugins.index.mt.MTFulltextQueryTermsProviderFactory) a través del administrador de configuración de la consola web de AEM.

      * `Joshua Config Path` es la ruta absoluta al archivo joshua.config. El proceso de AEM debe poder leer todos los archivos de la carpeta del paquete de idioma.
      * `Node types` son los tipos de nodos candidatos cuya búsqueda de texto completo atraerá este paquete de idioma para su traducción.
      * `Minimum score` es la puntuación de confianza mínima para un término traducido que se utilizará.

         * Por ejemplo, el hombre puede traducirse a la palabra inglesa &quot;man&quot; con una nota de confianza de `0.9` y también traducir a inglés la palabra &quot;humano&quot; con una puntuación de confianza `0.2`. Ajuste de la puntuación mínima a `0.3`, mantendría el &quot;hombre&quot; en la traducción &quot;hombre&quot;, pero descartaría el &quot;hombre&quot; en la traducción &quot;humana&quot; como esta nota de traducción de `0.2` es menor que la puntuación mínima de `0.3`.

5. Realizar una búsqueda de texto completo con recursos
   * Dado que dam:Asset es el tipo de nodo en el que se registra de nuevo este paquete de idioma, debemos buscar AEM Assets mediante la búsqueda de texto completo para validar esto.
   * Vaya a AEM > Recursos y abra Omnisearch. Busque un término en el idioma cuyo paquete de idioma se haya instalado.
   * Si es necesario, ajuste la puntuación mínima en las configuraciones de OSGi para garantizar la precisión de los resultados.

6. Actualización de paquetes de idiomas
   * Los paquetes de idioma Apache Joshua están totalmente mantenidos por el proyecto Apache Joshua, y su actualización o corrección es discreción del proyecto Apache Joshua.
   * Si se actualiza un paquete de idioma, para instalar las actualizaciones en AEM, deben seguirse los pasos 2 a 4 anteriores, ajustando el tamaño del montículo hacia arriba o hacia abajo según sea necesario.

      * Tenga en cuenta que cuando mueva el paquete de idioma descomprimido a la carpeta crx-quickstart/opt, mueva cualquier carpeta de paquete de idioma existente antes de copiarlo en el nuevo.
   * Si AEM no requiere un reinicio, entonces el proveedor de términos de consulta de texto completo de Apache Jackrabbit Oak Machien pertinente OSGi que pertenecen a los paquetes de idioma actualizados debe ser re-guardado para que AEM procese los archivos actualizados.


## Actualización del índice damAssetLucene {#updating-damassetlucene-index}

Para [Etiquetas inteligentes AEM](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html) para que se vea afectada por AEM traducción inteligente, AEM `/oak   :index  /damAssetLucene` El índice debe actualizarse para marcar las etiquetas predichas (el nombre del sistema para &quot;Etiquetas inteligentes&quot;) para que formen parte del índice Lucene agregado del recurso.

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

* [Paquete OSGi de traducción de Apache Oak Search Machine](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22)
* [Paquetes de idioma Apache Joshua](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)
* [Etiquetas inteligentes AEM](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html)
* [Prácticas recomendadas para consultas e indexación](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/best-practices-for-queries-and-indexing.html)
