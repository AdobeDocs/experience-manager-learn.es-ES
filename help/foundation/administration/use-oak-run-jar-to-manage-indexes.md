---
title: Usar oak-run.jar para administrar índices
description: El comando index de oak-run.jar consolida una serie de características para administrar índices Oak en AEM, desde la recopilación de estadísticas de índices, la ejecución de comprobaciones de coherencia de índices y la reindexación de los índices.
version: 6.4, 6.5
feature: oak
topics: search
activity: use
audience: architect, developer, implementer
doc-type: technical video
translation-type: tm+mt
source-git-commit: e19e177589df7ce6a56c0be3f9d590cbca2f8ce7
workflow-type: tm+mt
source-wordcount: '451'
ht-degree: 0%

---


# Usar oak-run.jar para administrar índices

[!DNL oak-run.jar]El comando de índice de Omniture consolida una serie de funciones para administrar [!DNL Oak]200 índices en AEM, desde la recopilación de estadísticas de índices, la ejecución de comprobaciones de coherencia de índices y la reindexación de los índices.

>[!NOTE]
>
>Dentro de este artículo y vídeos, los términos indexación y reindexación se utilizan de forma intercambiable y se consideran la misma operación.

## [!DNL oak-run.jar] conceptos básicos de comandos de índice

>[!VIDEO](https://video.tv.adobe.com/v/21475/?quality=9&learn=on)

* La versión de [[!DNL oak-run.jar]](https://repository.apache.org/service/local/artifact/maven/redirect?r=releases&amp;g=org.apache.jackrabbit&amp;a=oak-run&amp;v=1.8.0) utilizada debe coincidir con la versión de Oak utilizada en la instancia de AEM.
* La administración de índices mediante [!DNL oak-run.jar] el uso del **[!DNL index]** comando utiliza varios indicadores para admitir distintas operaciones.

   * `java -jar oak-run*.jar index ...`

## Estadísticas de índice

>[!VIDEO](https://video.tv.adobe.com/v/21477/?quality=12&learn=on)

* `oak-run.jar` muestra todas las definiciones de índice, las estadísticas de índice importantes y el contenido de índice para la análisis sin conexión.
* La recopilación de estadísticas de índice es segura para ejecutarse en instancias de AEM en uso.

## Comprobación de coherencia de índice

>[!VIDEO](https://video.tv.adobe.com/v/21476/?quality=12&learn=on)

* `oak-run.jar` determina rápidamente si los índices de roble de lucene están dañados.
* La comprobación de coherencia es segura para ejecutarse en AEM instancia en uso para comprobar la coherencia de los niveles 1 y 2.

## Indexación TarMK Online con [!DNL oak-run.jar] {#tarmkonlineindexingwithoakrunjar}

>[!VIDEO](https://video.tv.adobe.com/v/21479/?quality=12&learn=on)

* La indexación en línea del [!DNL TarMK] uso [!DNL oak-run.jar] es más rápida que la configuración `reindex=true` en el `oak:queryIndexDefinition` nodo. A pesar de este aumento de rendimiento, la indexación en línea mediante [!DNL oak-run.jar] sigue requiriendo una ventana de mantenimiento para realizar la indexación.

* La indexación en línea del [!DNL TarMK] uso [!DNL oak-run.jar] no **debe** ejecutarse en AEM instancias fuera de la ventana de mantenimiento de instancias de AEM.

## Indexación fuera de línea de TarMK con oak-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21478/?quality=12&learn=on)

* La indexación sin conexión de [!DNL TarMK] uso [!DNL oak-run.jar] es el método de indexación [!DNL oak-run.jar] basado más sencillo para [!DNL TarMK] ya que requiere un único [!DNL oak-run.jar] comando, pero requiere que se cierre la instancia de AEM.

## Indexación fuera de banda TarMK con oak-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21480/?quality=12&learn=on)

* La indexación fuera de banda al [!DNL TarMK] utilizar [!DNL oak-run.jar] minimiza el impacto de la indexación en las instancias de AEM en uso.
* La indización fuera de banda es el método de indexación recomendado para instalaciones AEM en las que el tiempo de reindexación supera las ventanas de mantenimiento disponibles.

## Indexación en línea de MongoMK con oak-run.jar

* Índice en línea con [!DNL oak-run.jar] on [!DNL MongoMK] y [!DNL RDBMK] es el método recomendado para la reindexación [!DNL MongoMK] (y [!DNL RDBMK]) de instalaciones AEM. **No se debe utilizar ningún otro método para[!DNL MongoMK]o[!DNL RDBMK].**
* Esta indexación solo debe ejecutarse en una única instancia de AEM del clúster.
* La indexación en línea de [!DNL MongoMK] es segura para ejecutarse con un clúster de AEM en ejecución, ya que la inversión del repositorio se producirá solamente en un [!DNL MongoDB] nodo único, lo que permitirá que los demás continúen sirviendo solicitudes sin un impacto significativo en el rendimiento.

El comando [!DNL oak-run.jar] index para realizar una indexación en línea de [!DNL MongoMK] es el [mismo que [!DNL TarMK] la indización en línea con [!DNL oak-run.jar]](#tarmkonlineindexingwithoakrunjar) la diferencia que el parámetro segment store señala a la [!DNL MongoDB] instancia que contiene el almacén de nodos.

```
java -jar oak-run*.jar index
 --reindex
 --index-paths=/oak:index/lucene
 --read-write
 --fds-path=/path/to/datastore mongodb://server:port/aem
```

## Materiales de apoyo

* [Descargar [!DNL oak-run.jar]](https://repository.apache.org/#nexus-search;gav~org.apache.jackrabbit~oak-run~~~~kw,versionexpand)
   * *Asegúrese de que la versión descargada coincide con la versión de Oak instalada en AEM como se describe anteriormente*
* [Documentación del comando del índice Apache Jackrabbit Oak oak run.jar](https://jackrabbit.apache.org/oak/docs/query/oak-run-indexing.html)
