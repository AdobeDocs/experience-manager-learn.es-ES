---
title: AEM Prácticas recomendadas de indización en la
description: AEM Obtenga información acerca de las prácticas recomendadas de indexación en la.
version: 6.4, 6.5, Cloud Service
sub-product: Experience Manager, Experience Manager Sites
feature: Search
doc-type: Article
topic: Development
role: Developer, Architect
level: Beginner
duration: 389
last-substantial-update: 2024-01-04T00:00:00Z
jira: KT-14745
thumbnail: KT-14745.jpeg
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '1418'
ht-degree: 0%

---


# AEM Prácticas recomendadas de indización en la

Obtenga información acerca de las prácticas recomendadas de indexación en Adobe Experience Manager AEM (). Apache [Jackrabbit Oak](https://jackrabbit.apache.org/oak/docs/query/query.html) AEM activa la búsqueda de contenido en la y los siguientes son puntos clave:

- AEM De forma predeterminada, proporciona varios índices para admitir la funcionalidad de búsqueda y consulta, por ejemplo `damAssetLucene`, `cqPageLucene` y más.
- Todas las definiciones de índice se almacenan en el repositorio en `/oak:index` nodo.
- AEM El as a Cloud Service solo admite índices de Oak Lucene.
- AEM La configuración del índice debe administrarse en la base de código del proyecto de e implementarse mediante canalizaciones de CI/CD de Cloud Manager.
- Si hay varios índices disponibles para una consulta determinada, la variable **se utiliza un índice con el coste estimado más bajo**.
- Si no hay ningún índice disponible para una consulta determinada, se atraviesa el árbol de contenido para encontrar el contenido coincidente. Sin embargo, el límite predeterminado mediante `org.apache.jackrabbit.oak.query.QueryEngineSettingsService` es para atravesar solo 10 000 nodos.
- Los resultados de una consulta son **filtrado por fin** para garantizar que el usuario actual tenga acceso de lectura. Esto significa que los resultados de la consulta pueden ser menores que el número de nodos indexados.
- La reindexación del repositorio después de los cambios de definición de índice requiere tiempo y depende del tamaño del repositorio.

AEM Para disponer de una funcionalidad de búsqueda eficiente y correcta que no afecte al rendimiento de la instancia de la instancia de, es importante comprender las prácticas recomendadas de indexación.

## Índice personalizado frente a OOTB

A veces, debe crear índices personalizados para satisfacer los requisitos de búsqueda. Sin embargo, siga estas directrices antes de crear índices personalizados:

- Comprenda los requisitos de búsqueda y compruebe si los índices OOTB pueden admitir los requisitos de búsqueda. Uso **Herramienta de rendimiento de consultas**, disponible en [SDK local](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) y AEM CS a través de Developer Console o `https://author-pXXXX-eYYYY.adobeaemcloud.com/ui#/aem/libs/granite/operations/content/diagnosistools/queryPerformance.html?appId=aemshell`.

- Defina una consulta óptima, utilice el [optimización de consultas](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/operations/query-and-indexing-best-practices.html?#optimizing-queries) diagrama de flujo y [Hoja de características clave de consulta JCR](https://experienceleague.adobe.com/docs/experience-manager-65/assets/JCR_query_cheatsheet-v1.1.pdf?lang=en) como referencia.

- Si los índices OOTB no admiten los requisitos de búsqueda, tiene dos opciones. Sin embargo, revise las [Sugerencias para crear índices eficientes](https://experienceleague.adobe.com/docs/experience-manager-65/content/implementing/deploying/practices/best-practices-for-queries-and-indexing.html?#should-i-create-an-index)
   - Personalice el índice OOTB: opción preferida ya que es fácil de mantener y actualizar.
   - Índice totalmente personalizado: Solo si la opción anterior no funciona.

### Personalización del índice OOTB

- Entrada **AEM CS**, al personalizar el índice OOTB use **\&lt;ootbindexname>-\&lt;productversion>-custom-\&lt;customversion>** convención de nomenclatura. Por ejemplo, `cqPageLucene-custom-1` o `damAssetLucene-8-custom-1`. Esto ayuda a combinar la definición de índice personalizada cada vez que se actualiza el índice OOTB. Consulte [Cambios en los índices predeterminados de](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/operations/indexing.html?#changes-to-out-of-the-box-indexes) para obtener más información.

- Entrada **AEM.X**, el nombre anterior _no funciona_, sin embargo, actualice el índice OOTB con propiedades adicionales en la `indexRules` nodo.

- AEM Copie siempre la definición de índice OOTB más reciente de la instancia de mediante el Administrador de paquetes CRX DE (/crx/packmgr/), cambie el nombre y agregue personalizaciones dentro del archivo XML.

- AEM Almacenar la definición del índice en el proyecto de en `ui.apps/src/main/content/jcr_root/_oak_index` e implementarlo mediante las canalizaciones de CI/CD de Cloud Manager. Consulte [Implementación de definiciones de índice personalizadas](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/operations/indexing.html?#deploying-custom-index-definitions) para obtener más información.

### Índice totalmente personalizado

La creación de un índice totalmente personalizado debe ser la última opción y solo si la opción anterior no funciona.

- Al crear un índice totalmente personalizado, utilice **\&lt;prefix>.\&lt;customindexname>-\&lt;version>-custom-\&lt;customversion>** convención de nomenclatura. Por ejemplo, `wknd.adventures-1-custom-1`. Esto ayuda a evitar conflictos de nombres. Aquí, `wknd` es el prefijo y `adventures` es el nombre del índice personalizado. AEM Esta convención es aplicable tanto a la versión 6.X como a la versión 6.X de AEM CS y ayuda a prepararse para una migración futura a AEM CS.

- AEMCS solo admite índices Lucene, por lo que, para prepararse para una migración futura a AEMCS, utilice siempre índices Lucene. Consulte [Índices Lucene frente a índices de propiedades](https://experienceleague.adobe.com/docs/experience-manager-65/content/implementing/deploying/practices/best-practices-for-queries-and-indexing.html?#lucene-or-property-indexes) para obtener más información.

- Evite crear un índice personalizado en el mismo tipo de nodo que el índice OOTB. En su lugar, personalice el índice OOTB con propiedades adicionales en la `indexRules` nodo. Por ejemplo, no cree un índice personalizado en `dam:Asset` tipo de nodo, pero personalizar el OOTB `damAssetLucene` índice. _Ha sido una causa raíz común de problemas funcionales y de rendimiento_.

- Además, evite añadir varios tipos de nodos, por ejemplo `cq:Page` y `cq:Tag` en las reglas de indexación (`indexRules`) nodo. En su lugar, cree índices independientes para cada tipo de nodo.

- AEM Como se ha mencionado en la sección anterior, almacene la definición del índice en el proyecto de en `ui.apps/src/main/content/jcr_root/_oak_index` e implementarlo mediante las canalizaciones de CI/CD de Cloud Manager. Consulte [Implementación de definiciones de índice personalizadas](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/operations/indexing.html?#deploying-custom-index-definitions) para obtener más información.

- Las directrices de definición de índice son:
   - El tipo de nodo (`jcr:primaryType`) debe ser `oak:QueryIndexDefinition`
   - El tipo de índice (`type`) debe ser `lucene`
   - La propiedad asíncrona (`async`) debe ser `async,nrt`
   - Uso `includedPaths` y evitar `excludedPaths` propiedad. Siempre establecido `queryPaths` con el mismo valor que `includedPaths` valor.
   - Para aplicar la restricción de ruta, utilice `evaluatePathRestrictions` y establézcalo en. `true`.
   - Uso `tags` para etiquetar el índice y, mientras consulta, especifique este valor de etiquetas para utilizar el índice. La sintaxis general de la consulta es `<query> option(index tag <tagName>)`.

  ```xml
  /oak:index/wknd.adventures-1-custom-1
      - jcr:primaryType = "oak:QueryIndexDefinition"
      - type = "lucene"
      - compatVersion = 2
      - async = ["async", "nrt"]
      - includedPaths = ["/content/wknd"]
      - queryPaths = ["/content/wknd"]
      - evaluatePathRestrictions = true
      - tags = ["customAdvSearch"]
  ...
  ```

### Ejemplos

Para comprender las prácticas recomendadas, veamos algunos ejemplos.

#### Uso incorrecto de la propiedad de etiquetas

La siguiente imagen muestra la definición de índice personalizada y OOTB, destacando el `tags` propiedad, ambos índices utilizan el mismo `visualSimilaritySearch` valor.

![Uso incorrecto de la propiedad de etiquetas](./assets/understand-indexing-best-practices/incorrect-tags-property.png)

##### Análisis

Se trata de un uso incorrecto del `tags` en el índice personalizado. El motor de consultas de Oak elige el índice personalizado por encima del índice OOTB, la causa del coste estimado más bajo.

La forma correcta es personalizar el índice OOTB y agregar propiedades adicionales en la variable `indexRules` nodo. Consulte [Personalización del índice OOTB](#customize-the-ootb-index) para obtener más información.

#### Índice en el `dam:Asset` tipo de nodo

La siguiente imagen muestra un índice personalizado para `dam:Asset` tipo de nodo con `includedPaths` establecida en una ruta específica.

![Índice en dam: Tipo de nodo de recurso](./assets/understand-indexing-best-practices/index-for-damAsset-type.png)

##### Análisis

Si realiza una búsqueda omnidireccional en Assets, devolverá resultados incorrectos, ya que el índice personalizado tiene un coste estimado más bajo.

No cree ningún índice personalizado en `dam:Asset` tipo de nodo, pero personalizar el OOTB `damAssetLucene` índice con propiedades adicionales en `indexRules` nodo.

#### Varios tipos de nodo en reglas de indexación

La siguiente imagen muestra un índice personalizado con varios tipos de nodos bajo la etiqueta `indexRules` nodo.

![Varios tipos de nodos bajo las reglas de indexación](./assets/understand-indexing-best-practices/multiple-nodetypes-in-index.png)

##### Análisis

No se recomienda agregar varios tipos de nodo en un solo índice, sin embargo, puede indexar tipos de nodo en el mismo índice si los tipos de nodo están estrechamente relacionados, por ejemplo, `cq:Page` y `cq:PageContent`.

Una solución válida es personalizar el OOTB `cqPageLucene` y `damAssetLucene` , agregue propiedades adicionales en el `indexRules` nodo.

#### Ausencia de `queryPaths` propiedad

La siguiente imagen muestra un índice personalizado (que no sigue también la convención de nomenclatura) sin `queryPaths` propiedad.

![Falta de la propiedad queryPaths](./assets/understand-indexing-best-practices/absense-of-queryPaths-prop.png)

##### Análisis

Siempre establecido `queryPaths` con el mismo valor que `includedPaths` valor. Además, para aplicar la restricción de ruta, establezca `evaluatePathRestrictions` propiedad a `true`.

#### Consulta con etiqueta de índice

La siguiente imagen muestra un índice personalizado con `tags` y cómo utilizarla durante la consulta.

![Consulta con etiqueta de índice](./assets/understand-indexing-best-practices/tags-prop-usage.png)

```
/jcr:root/content/dam//element(*,dam:Asset)[(jcr:content/@contentFragment = 'true' and jcr:contains(., '/content/sitebuilder/test/mysite/live/ja-jp/mypage'))]order by @jcr:created descending option (index tag assetPrefixNodeNameSearch)
```

##### Análisis

Muestra cómo establecer valores no conflictivos y correctos `tags` valor de la propiedad en el índice y utilícelo durante la consulta. La sintaxis general de la consulta es `<query> option(index tag <tagName>)`. Consulte también [Etiqueta de índice de opción de consulta](https://jackrabbit.apache.org/oak/docs/query/query-engine.html#query-option-index-tag)

#### Índice personalizado

La siguiente imagen muestra un índice personalizado con `suggestion` para lograr la funcionalidad de búsqueda avanzada.

![Índice personalizado](./assets/understand-indexing-best-practices/custom-index-with-suggestion-node.png)

##### Análisis

Es un caso de uso válido crear un índice personalizado para [búsqueda avanzada](https://jackrabbit.apache.org/oak/docs/query/lucene.html#advanced-search-features) funcionalidad. Sin embargo, el nombre del índice debe seguir el **\&lt;prefix>.\&lt;customindexname>-\&lt;version>-custom-\&lt;customversion>** convención de nomenclatura.


## Herramientas útiles

Revisemos algunas herramientas que pueden ayudarle a definir, analizar y optimizar los índices.

### Herramienta de creación de índices

El [Generador de definiciones de índice de Oak](https://oakutils.appspot.com/generate/index) la herramienta ayuda **para generar la definición del índice** en función de las consultas de entrada. Es un buen punto de partida para crear un índice personalizado.

### Herramienta Analizar índice

El [Analizador de definición de índice](https://oakutils.appspot.com/analyze/index) la herramienta ayuda **para analizar la definición del índice** y proporciona recomendaciones para mejorar la definición del índice.

### Herramienta de rendimiento de consultas

El OOTB _Herramienta de rendimiento de consultas_ disponible en [SDK local](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) y AEM CS a través de Developer Console o `https://author-pXXXX-eYYYY.adobeaemcloud.com/ui#/aem/libs/granite/operations/content/diagnosistools/queryPerformance.html?appId=aemshell` ayuda **para analizar el rendimiento de la consulta** y [Hoja de características clave de consulta JCR](https://experienceleague.adobe.com/docs/experience-manager-65/assets/JCR_query_cheatsheet-v1.1.pdf?lang=en) para definir la consulta óptima.

### Herramientas y sugerencias para la resolución de problemas

AEM La mayoría de las siguientes opciones se aplican a la solución de problemas local y a la versión 6.X de la.

- Administrador de índices disponible en `http://host:port/libs/granite/operations/content/diagnosistools/indexManager.html` para obtener información de índice como tipo, última actualización, tamaño.

- Registro detallado de paquetes Java™ relacionados con la indexación y la consulta de Oak como `org.apache.jackrabbit.oak.plugins.index`, `org.apache.jackrabbit.oak.query`, y `com.day.cq.search` mediante `http://host:port/system/console/slinglog` para solucionar problemas.

- MBean de JMX de _IndexStats_ tipo disponible en `http://host:port/system/console/jmx` para obtener información de índice como estado, progreso o estadísticas relacionadas con la indexación asincrónica. También proporciona lo siguiente _FailingIndexStats_, si no hay resultados aquí, significa que no hay índices corruptos. AsyncIndexerService marca como dañado cualquier índice que no se actualice durante 30 minutos (configurable) y detiene su indexación. Si una consulta no da los resultados esperados, es útil que los desarrolladores comprueben esto antes de continuar con la reindexación, ya que esta es computacionalmente costosa y lleva tiempo.

- MBean de JMX de _LuceneIndex_ tipo disponible en `http://host:port/system/console/jmx` para estadísticas de índice de Lucene como tamaño, número de documentos por definición de índice.

- MBean de JMX de _QueryStat_ tipo disponible en `http://host:port/system/console/jmx` para estadísticas de consulta de Oak, incluidas consultas lentas y populares con detalles como consulta, tiempo de ejecución.

## Recursos adicionales

Consulte la siguiente documentación para obtener más información:

- [Consultas e indexación de Oak](https://experienceleague.adobe.com/docs/experience-manager-65/content/implementing/deploying/deploying/queries-and-indexing.html)
- [Prácticas recomendadas de consulta e indexación](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/operations/query-and-indexing-best-practices.html)
- [Prácticas recomendadas para consultas e indexación](https://experienceleague.adobe.com/docs/experience-manager-65/content/implementing/deploying/practices/best-practices-for-queries-and-indexing.html)
