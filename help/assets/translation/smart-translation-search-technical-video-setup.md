---
title: Configuración de la búsqueda de traducción inteligente con AEM Assets
description: La búsqueda de traducción inteligente permite el uso de términos de búsqueda que no estén en inglés para resolver el contenido en inglés. Para configurar AEM para la búsqueda de traducción inteligente, debe instalarse y configurarse el paquete OSGi de traducción de Apache Oak Search Machine, así como los paquetes de idioma Apache Joshua de código abierto y gratuito que contengan las reglas de traducción.
version: 6.3, 6.4, 6.5
feature: 'Búsqueda  '
topic: Administración de contenido
role: Desarrollador
level: Principiante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '872'
ht-degree: 1%

---


# Configuración de la búsqueda de traducción inteligente con AEM Assets{#set-up-smart-translation-search-with-aem-assets}

La búsqueda de traducción inteligente permite el uso de términos de búsqueda que no estén en inglés para resolver el contenido en inglés. Para configurar AEM para la búsqueda de traducción inteligente, debe instalarse y configurarse el paquete OSGi de traducción de Apache Oak Search Machine, así como los paquetes de idioma Apache Joshua de código abierto y gratuito que contengan las reglas de traducción.

>[!VIDEO](https://video.tv.adobe.com/v/21291/?quality=9&learn=on)

>[!NOTE]
>
>La búsqueda de traducción inteligente debe configurarse en cada instancia de AEM que la requiera.

1. Descargue e instale el paquete OSGi Oak Search Machine Translation
   * [Descargue el ](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22) paquete OSGi Oak Search Machine Translation que corresponde a la versión Oak de AEM.
   * Instale el paquete OSGi de traducción Oak Search Machine descargado en AEM a través de [ `/system/console/bundles`](http://localhost:4502/system/console/bundles).

2. Descargar y actualizar los paquetes de idioma de Apache Joshua
   * Descargue y descomprima los [paquetes de idioma Apache Joshua](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs) deseados.
   * Edite el archivo `joshua.config` y comente las dos líneas que comienzan por:

      ```
      feature-function = LanguageModel ...
      ```

   * Determine y registre el tamaño de la carpeta modelo del paquete de idioma, ya que esto influye en el espacio adicional que AEM necesitará.
   * Mover la carpeta del paquete de idioma Apache Joshua descomprimido (con las `joshua.config` ediciones) a

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

      * El tamaño de pila de falta de lenguaje previo de AEM + el tamaño del directorio del modelo redondeado hasta los 2 GB más cercanos
      * Por ejemplo: Si los paquetes de idiomas previos a la instalación de AEM requieren 8 GB de memoria para ejecutarse y la carpeta modelo del paquete de idioma es de 3,8 GB sin comprimir, el nuevo tamaño de pila es:

         El `8GB` + ( `3.75GB` original redondeado a la `2GB` más cercana, que es `4GB`) para un total de `12GB`
   * Verifique que el equipo tenga esta cantidad de memoria disponible adicional.
   * Actualice los scripts de inicio de AEM para ajustar el nuevo tamaño de pila

      * Ejemplo. `java -Xmx12g -jar cq-author-p4502.jar`
   * Reinicie AEM con el tamaño de pila aumentado.

   >[!NOTE]
   >
   >El espacio de memoria necesario para los paquetes de idiomas puede crecer, especialmente cuando se utilizan varios paquetes de idiomas.
   >
   >
   >Asegúrese siempre de que **la instancia tenga suficiente memoria** para dar cabida a los incrementos en el espacio asignado.
   >
   >
   >La **pila base siempre debe calcularse para soportar un rendimiento aceptable sin ningún paquete de idioma** instalado.

4. Registre los paquetes de idiomas a través de las configuraciones OSGi del proveedor de términos de consulta de texto completo de Apache Jackrabbit Oak Machine Translation

   * Para cada paquete de idioma, [cree una nueva configuración OSGi del proveedor de términos de consulta de texto completo Apache Jackrabbit Oak Machine Translation](http://localhost:4502/system/console/configMgr/org.apache.jackrabbit.oak.plugins.index.mt.MTFulltextQueryTermsProviderFactory) a través del administrador de configuración de la consola web de AEM.

      * `Joshua Config Path` es la ruta absoluta al archivo joshua.config. El proceso AEM debe poder leer todos los archivos de la carpeta del paquete de idioma.
      * `Node types` son los tipos de nodos candidatos cuya búsqueda de texto completo atraerá este paquete de idioma para su traducción.
      * `Minimum score` es la puntuación de confianza mínima para un término traducido que se utilizará.

         * Por ejemplo, hombre puede traducirse a la palabra en inglés &quot;man&quot; con una puntuación de confianza de `0.9` y también traducirse a la palabra en inglés &quot;human&quot; con una puntuación de confianza `0.2`. Si se ajusta la puntuación mínima a `0.3`, se conservaría el &quot;hombre&quot; en la traducción &quot;hombre&quot;, pero se descartaría el &quot;hombre&quot; en la traducción &quot;humana&quot;, ya que esta puntuación de traducción de `0.2` es inferior a la puntuación mínima de `0.3`.

5. Realizar una búsqueda de texto completo con recursos
   * Dado que dam:Asset es el tipo de nodo en el que se registra de nuevo este paquete de idioma, debemos buscar AEM Assets mediante la búsqueda de texto completo para validar esto.
   * Vaya a AEM > Assets y abra Omnisearch. Busque un término en el idioma cuyo paquete de idioma se haya instalado.
   * Si es necesario, ajuste la puntuación mínima en las configuraciones de OSGi para garantizar la precisión de los resultados.

6. Actualización de paquetes de idiomas
   * Los paquetes de idioma Apache Joshua están totalmente mantenidos por el proyecto Apache Joshua, y su actualización o corrección es discreción del proyecto Apache Joshua.
   * Si se actualiza un paquete de idioma, para instalar las actualizaciones en AEM, deben seguirse los pasos 2 a 4 anteriores, ajustando el tamaño de pila hacia arriba o hacia abajo según sea necesario.

      * Tenga en cuenta que cuando mueva el paquete de idioma descomprimido a la carpeta crx-quickstart/opt, mueva cualquier carpeta de paquete de idioma existente antes de copiarlo en el nuevo.
   * Si AEM no requiere un reinicio, entonces las configuraciones OSGi del proveedor de términos de consulta de texto completo de Apache Jackrabbit Oak Machien relevantes relacionadas con los paquetes de idioma actualizados deben volver a guardarse para que AEM procese los archivos actualizados.


## Actualización del índice damAssetLucene {#updating-damassetlucene-index}

Para que [AEM Smart Tags](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html) se vean afectados por la traducción inteligente de AEM, el índice `/oak   :index  /damAssetLucene` de AEM debe actualizarse para marcar las etiquetas predichas (el nombre del sistema para &quot;etiquetas inteligentes&quot;) para que formen parte del índice Lucene agregado del recurso.

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
* [Etiquetas inteligentes de AEM](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html)
* [Prácticas recomendadas para consultas e indexación](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/best-practices-for-queries-and-indexing.html)