---
title: Configuración de la búsqueda de traducción inteligente con AEM Assets
description: La búsqueda inteligente de traducción permite el uso de términos de búsqueda que no estén en inglés para resolverlos en contenido en inglés. AEM Para configurar la para la búsqueda de traducción inteligente, se debe instalar y configurar el paquete OSGi de traducción automática de Apache Oak Search, así como los paquetes de idioma gratuitos y de código abierto pertinentes de Apache Joshua que contienen las reglas de traducción.
version: 6.4, 6.5
feature: Search
topic: Content Management
role: Developer
level: Beginner
doc-type: Technical Video
exl-id: 7be8c3d5-b944-4421-97b3-bd5766c1b1b5
duration: 634
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '811'
ht-degree: 0%

---

# Configuración de la búsqueda de traducción inteligente con AEM Assets{#set-up-smart-translation-search-with-aem-assets}

La búsqueda inteligente de traducción permite el uso de términos de búsqueda que no estén en inglés para resolverlos en contenido en inglés. AEM Para configurar la para la búsqueda de traducción inteligente, se debe instalar y configurar el paquete OSGi de traducción automática de Apache Oak Search, así como los paquetes de idioma gratuitos y de código abierto pertinentes de Apache Joshua que contienen las reglas de traducción.

>[!VIDEO](https://video.tv.adobe.com/v/21291?quality=12&learn=on)

>[!NOTE]
>
>AEM La búsqueda inteligente de traducciones debe configurarse en cada instancia de traducción que la requiera.

1. Descargue e instale el paquete OSGi de traducción automática de Oak Search
   * [Descargue el paquete OSGi de traducción automática de búsqueda Oak](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22) AEM que corresponde a la versión de Oak de la.
   * AEM Instale el paquete OSGi de traducción automática de búsqueda de Oak descargado en la aplicación de correo electrónico de OSGi a través de la [`/system/console/bundles`](http://localhost:4502/system/console/bundles).

2. Descargar y actualizar los paquetes de idioma de Apache Joshua
   * Descargue y descomprima el archivo deseado [Paquetes de idiomas de Apache Joshua](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs).
   * Edite el `joshua.config` archivar y comentar las 2 líneas que comienzan por:

     ```
     feature-function = LanguageModel ...
     ```

   * AEM Determine y registre el tamaño de la carpeta del modelo del paquete de idioma, ya que esto influye en la cantidad de espacio de pila adicional que requerirá la aplicación de un módulo de almacenamiento de datos en un solo espacio de trabajo.
   * Mueva la carpeta del paquete de idioma Apache Joshua descomprimida (con el `joshua.config` ediciones) a

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

      * AEM Tamaño de pila de falta de idioma previo + el tamaño del directorio del modelo redondeado al 2 GB más cercano
      * AEM Por ejemplo: Si la instalación de paquetes de idioma previos requiere 8 GB de pila para ejecutarse y la carpeta del modelo del paquete de idioma tiene 3,8 GB sin comprimir, el nuevo tamaño de pila es:

        El original `8GB` + ( `3.75GB` redondeado hacia arriba al más cercano `2GB`, que es `4GB`) para un total de `12GB`

   * Compruebe que el equipo tiene esta cantidad de memoria disponible adicional.
   * AEM Actualizar scripts de inicio de la aplicación para ajustar el nuevo tamaño de la pila

      * Ejemplo: `java -Xmx12g -jar cq-author-p4502.jar`

   * AEM Reinicie con el tamaño de pila aumentado.

   >[!NOTE]
   >
   >El espacio de pila necesario para los paquetes de idioma puede aumentar, especialmente cuando se utilizan varios paquetes de idioma.
   >
   >
   >Asegúrese siempre de **la instancia tiene memoria suficiente** para dar cabida a los incrementos en el espacio de pila asignado.
   >
   >
   >El **el montón base siempre se debe calcular para admitir un rendimiento aceptable sin ningún paquete de idioma** instalado.

4. Registre los paquetes de idiomas a través de Apache Jackrabbit Oak Traducción automática Términos de consulta de texto completo Proveedor Configuraciones OSGi

   * Para cada paquete de idioma, [crear un nuevo Apache Jackrabbit Oak Traducción automática Consulta de texto completo Términos Proveedor Configuración OSGi](http://localhost:4502/system/console/configMgr/org.apache.jackrabbit.oak.plugins.index.mt.MTFulltextQueryTermsProviderFactory) AEM a través del administrador de configuración de la consola web de la.

      * `Joshua Config Path` es la ruta absoluta al archivo joshua.config. AEM El proceso de la debe poder leer todos los archivos de la carpeta del paquete de idioma.
      * `Node types` son los tipos de nodo candidatos cuya búsqueda de texto completo involucrará a este paquete de idioma para su traducción.
      * `Minimum score` es la puntuación de confianza mínima para un término traducido para que se utilice.

         * Por ejemplo, hombre (hombre en español) puede traducirse a la palabra inglesa &quot;man&quot; con una puntuación de confianza de `0.9` y también traducir a la palabra &quot;humano&quot; con una puntuación de confianza `0.2`. Ajuste de la puntuación mínima a `0.3`, mantendría la traducción de &quot;hombre&quot; a &quot;hombre&quot;, pero descartaría la traducción de &quot;hombre&quot; a &quot;humano&quot; ya que la puntuación de traducción de `0.2` es menor que la puntuación mínima de `0.3`.

5. Realizar una búsqueda de texto completo en recursos
   * Dado que dam: Asset es el tipo de nodo en el que se registra de nuevo este paquete de idioma, debemos buscar AEM Assets mediante la búsqueda de texto completo para validarlo.
   * AEM Vaya a > Assets y abra Omnisearch. Busque un término en el idioma en el que se instaló el paquete de idioma.
   * Si es necesario, ajuste la puntuación mínima en las configuraciones de OSGi para garantizar la precisión de los resultados.

6. Actualizar paquetes de idioma
   * Los paquetes de idiomas de Apache Joshua son mantenidos íntegramente por el proyecto Apache Joshua, y su actualización o corrección es a discreción del proyecto Apache Joshua.
   * AEM Si se actualiza un paquete de idioma, para instalar las actualizaciones en el, se deben seguir los pasos anteriores 2-4, ajustando el tamaño de la pila hacia arriba o hacia abajo según sea necesario.

      * Tenga en cuenta que cuando mueva el paquete de idioma descomprimido a la carpeta crx-quickstart/opt, mueva cualquier carpeta de paquete de idioma existente antes de copiar la nueva.

   * AEM AEM Si no requiere un reinicio, las configuraciones relevantes del proveedor de términos de consulta de texto completo de Apache Jackrabbit Oak Machine que pertenecen a los paquetes de idiomas actualizados deben volver a guardarse para que procese los archivos actualizados, de modo que se procesen los archivos actualizados.

## Actualizando el índice damAssetLucene {#updating-damassetlucene-index}

Para que [AEM Etiquetas inteligentes de la](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html) AEM AEM para verse afectado por la traducción inteligente de la, `/oak   :index  /damAssetLucene` El índice debe actualizarse para marcar las etiquetas predichas (el nombre del sistema para &quot;etiquetas inteligentes&quot;) para que formen parte del índice Lucene agregado del recurso.

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

* [Paquete OSGi de traducción automática de Apache Oak](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22)
* [Paquetes de idiomas de Apache Joshua](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)
* [AEM Etiquetas inteligentes de la](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html)
* [Prácticas recomendadas para consultar e indexar](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/best-practices-for-queries-and-indexing.html)
