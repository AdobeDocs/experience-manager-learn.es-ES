---
title: Configurar la búsqueda de traducción inteligente con AEM Assets
seo-title: Configurar la búsqueda de traducción inteligente con AEM Assets
description: La búsqueda de traducción inteligente permite el uso de términos de búsqueda que no están en inglés para resolver el contenido en inglés. Para configurar AEM para la búsqueda de traducción inteligente, se debe instalar y configurar el paquete OSGi de traducción de Apache Oak Search Machine, así como los paquetes de idioma relevantes de Apache Joshua libre y de código abierto que contienen las reglas de traducción.
seo-description: La búsqueda de traducción inteligente permite el uso de términos de búsqueda que no están en inglés para resolver el contenido en inglés. Para configurar AEM para la búsqueda de traducción inteligente, se debe instalar y configurar el paquete OSGi de traducción de Apache Oak Search Machine, así como los paquetes de idioma relevantes de Apache Joshua libre y de código abierto que contienen las reglas de traducción.
uuid: b0e8dab2-6bc4-4158-91a1-4b9811359798
discoiquuid: 4db1b4db-74f4-4646-b5de-cb891612cc90
topics: authoring, search, metadata, localization
audience: administrator, developer
doc-type: technical video
activity: setup
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '929'
ht-degree: 0%

---


# Configure la búsqueda de traducción inteligente con AEM Assets{#set-up-smart-translation-search-with-aem-assets}

La búsqueda de traducción inteligente permite el uso de términos de búsqueda que no están en inglés para resolver el contenido en inglés. Para configurar AEM para la búsqueda de traducción inteligente, se debe instalar y configurar el paquete OSGi de traducción de Apache Oak Search Machine, así como los paquetes de idioma relevantes de Apache Joshua libre y de código abierto que contienen las reglas de traducción.

>[!VIDEO](https://video.tv.adobe.com/v/21291/?quality=9&learn=on)

>[!NOTE]
>
>La búsqueda de traducción inteligente debe configurarse en cada instancia de AEM que lo requiera.

1. Descargar e instalar el paquete OSGi de Oak Search Machine Translation
   * [Descargue el ](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22) paquete OSGi de traducción Oak Search Machine que corresponde a AEM versión Oak.
   * Instale el paquete OSGi de traducción de Oak SearchMachine descargado en AEM mediante [ `/system/console/bundles`](http://localhost:4502/system/console/bundles).

2. Descargar y actualizar los paquetes de idioma de Apache Joshua
   * Descargue y descomprima los [paquetes de idioma Apache Joshua](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs) que desee.
   * Edite el archivo `joshua.config` y comente las dos líneas que comienzan por:

      ```
      feature-function = LanguageModel ...
      ```

   * Determine y registre el tamaño de la carpeta del modelo del paquete de idioma, ya que esto influye en la cantidad de espacio adicional que AEM necesitar.
   * Mover la carpeta del paquete de idioma Apache Joshua sin comprimir (con las `joshua.config` ediciones) a

      ```
      .../crx-quickstart/opt/<source_language-target_language>
      ```

      Por ejemplo:

      ```
       .../crx-quickstart/opt/es-en
      ```

3. Reiniciar AEM con asignación de memoria de montón actualizada
   * Detener AEM
   * Determinar el nuevo tamaño de pila necesario para AEM

      * AEM tamaño del montón de falta de lenguaje previo + el tamaño del directorio del modelo redondeado a los 2 GB más cercanos
      * Por ejemplo: Si los paquetes de idiomas previos a la instalación del AEM requieren 8 GB de pila para ejecutarse y la carpeta del modelo del paquete de idioma es de 3,8 GB sin comprimir, el nuevo tamaño del montón es:

         El `8GB` + original ( `3.75GB` redondeado al `2GB` más cercano, que es `4GB`) para un total de `12GB`
   * Verifique que el equipo tenga esta cantidad de memoria adicional disponible.
   * Actualice las secuencias de comandos de inicio AEM para ajustar el nuevo tamaño del montón

      * Ejemplo. `java -Xmx12g -jar cq-author-p4502.jar`
   * Reinicie AEM con el tamaño de pila aumentado.

   >[!NOTE]
   >
   >El espacio de montón necesario para los paquetes de idiomas puede aumentar, especialmente cuando se utilizan varios paquetes de idiomas.
   >
   >
   >Asegúrese siempre de que **la instancia tenga suficiente memoria** para dar cabida a los incrementos en el espacio del montón asignado.
   >
   >
   >El montón base **siempre debe calcularse para admitir un rendimiento aceptable sin ningún paquete de idioma** instalado.

4. Registre los paquetes de idiomas a través de las configuraciones OSGi del proveedor de términos de Consulta de texto completo de Apache Jackrabbit Oak Machine Translation

   * Para cada paquete de idioma, [cree una nueva configuración OSGi del proveedor de términos de Consulta de texto completo de Apache Jackrabbit Oak Machine Translation](http://localhost:4502/system/console/configMgr/org.apache.jackrabbit.oak.plugins.index.mt.MTFulltextQueryTermsProviderFactory) a través del administrador de configuración de la consola web de AEM.

      * `Joshua Config Path` es la ruta absoluta al archivo joshua.config. El proceso de AEM debe poder leer todos los archivos de la carpeta del paquete de idioma.
      * `Node types` son los tipos de nodos candidatos cuya búsqueda de texto completo atraerá este paquete de idioma para la traducción.
      * `Minimum score` es la puntuación de confianza mínima para un término traducido que se va a utilizar.

         * Por ejemplo, hombre puede traducir la palabra inglesa &quot;man&quot; con una puntuación de confianza de `0.9` y también traducir la palabra inglesa &quot;human&quot; con una puntuación de confianza `0.2`. Si se ajusta la puntuación mínima a `0.3`, se mantendrá la traducción &quot;hombre&quot; a &quot;hombre&quot;, pero se descartará la traducción &quot;hombre&quot; a &quot;hombre&quot;, ya que esta puntuación de `0.2` en la traducción es inferior a la puntuación mínima de `0.3`.

5. Realizar una búsqueda de texto completo con recursos
   * Dado que dam:Asset es el tipo de nodo en el que se registra este paquete de idioma, debemos buscar AEM Assets mediante la búsqueda de texto completo para validar esto.
   * Vaya a AEM > Recursos y abra Omniture Search. Busque un término en el idioma cuyo paquete de idioma se instaló.
   * Si es necesario, ajuste la puntuación mínima en las configuraciones de OSGi para garantizar la precisión de los resultados.

6. Actualización de paquetes de idiomas
   * Los paquetes de idioma Apache Joshua son mantenidos completamente por el proyecto Apache Joshua, y su actualización o corrección es discreción del proyecto Apache Joshua.
   * Si se actualiza un paquete de idioma, para instalar las actualizaciones en AEM, deben seguirse los pasos anteriores 2 a 4, ajustando el tamaño del montón hacia arriba o hacia abajo según sea necesario.

      * Tenga en cuenta que, al mover el paquete de idioma sin comprimir a la carpeta crx-quickstart/opt, mueva cualquier carpeta del paquete de idioma existente antes de copiar en el nuevo.
   * Si AEM no requiere un reinicio, entonces las configuraciones OSGi del proveedor de términos de Consulta de texto completo de traducción Apache Jackrabbit Oak que se relacionan con los paquetes de idioma actualizados deben ser reguardadas para que AEM procese los archivos actualizados.


## Actualizando el índice damAssetLucene {#updating-damassetlucene-index}

Para que [AEM etiquetas inteligentes](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html) se vean afectadas por AEM traducción inteligente, AEM índice `/oak   :index  /damAssetLucene` debe actualizarse para marcar las etiquetas predichas (el nombre del sistema de &quot;etiquetas inteligentes&quot;) como parte del índice Lucene acumulado del recurso.

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
* [Prácticas recomendadas para consultar e indexar](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/best-practices-for-queries-and-indexing.html)