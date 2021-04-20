---
title: Uso de oak-run.jar para administrar índices
description: el comando index de oak-run.jar consolida una serie de características para administrar índices Oak en AEM, desde la recopilación de estadísticas de índice, la ejecución de comprobaciones de coherencia de índice y la reindexación de los propios índices.
version: 6.4, 6.5
feature: Search
topics: search
activity: use
audience: architect, developer, implementer
doc-type: technical video
topic: Performance
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '453'
ht-degree: 0%

---


# Uso de oak-run.jar para administrar índices

[!DNL oak-run.jar]El comando de índice de aEM consolida una serie de funciones para administrar  [!DNL Oak]200 índices en AEM, desde la recopilación de estadísticas de índice, la ejecución de comprobaciones de coherencia de índice y la reindexación de índices por sí mismos.

>[!NOTE]
>
>Dentro de este artículo y vídeos, los términos de indexación y reindexación se utilizan de forma intercambiable y se consideran la misma operación.

## [!DNL oak-run.jar] Conceptos básicos del comando index

>[!VIDEO](https://video.tv.adobe.com/v/21475/?quality=9&learn=on)

* La versión de [[!DNL oak-run.jar]](https://repository.apache.org/service/local/artifact/maven/redirect?r=releases&amp;g=org.apache.jackrabbit&amp;a=oak-run&amp;v=1.8.0) utilizada debe coincidir con la versión de Oak utilizada en la instancia de AEM.
* La administración de índices mediante [!DNL oak-run.jar] aprovecha el comando **[!DNL index]** con varios indicadores para admitir diferentes operaciones.

   * `java -jar oak-run*.jar index ...`

## Estadísticas de índice

>[!VIDEO](https://video.tv.adobe.com/v/21477/?quality=12&learn=on)

* `oak-run.jar` genera todas las definiciones de índice, estadísticas de índice importantes y contenido de índice para análisis sin conexión.
* La recopilación de estadísticas de índice es segura de ejecutar en instancias de AEM en uso.

## Comprobación de coherencia del índice

>[!VIDEO](https://video.tv.adobe.com/v/21476/?quality=12&learn=on)

* `oak-run.jar` determina rápidamente si los índices de lucene Oak están corruptos.
* La comprobación de coherencia es segura para ejecutarse en la instancia de AEM en uso para los niveles de comprobación de coherencia 1 y 2.

## Indexación TarMK en línea con [!DNL oak-run.jar] {#tarmkonlineindexingwithoakrunjar}

>[!VIDEO](https://video.tv.adobe.com/v/21479/?quality=12&learn=on)

* La indexación en línea de [!DNL TarMK] mediante [!DNL oak-run.jar] es más rápida que establecer `reindex=true` en el nodo `oak:queryIndexDefinition`. A pesar de este aumento de rendimiento, la indexación en línea mediante [!DNL oak-run.jar] sigue requiriendo una ventana de mantenimiento para realizar la indexación.

* La indexación en línea de [!DNL TarMK] mediante [!DNL oak-run.jar] debe **no** ejecutarse contra instancias de AEM fuera de la ventana de mantenimiento de instancias de AEM.

## Indexación fuera de línea TarMK con oak-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21478/?quality=12&learn=on)

* La indexación sin conexión de [!DNL TarMK] mediante [!DNL oak-run.jar] es el método de indexación basado en [!DNL oak-run.jar] más sencillo para [!DNL TarMK], ya que requiere un único comando [!DNL oak-run.jar], aunque requiere que se cierre la instancia de AEM.

## Indexación fuera de banda TarMK con oak-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21480/?quality=12&learn=on)

* La indexación fuera de banda en [!DNL TarMK] mediante [!DNL oak-run.jar] minimiza el impacto de la indexación en las instancias de AEM en uso.
* La indexación fuera de banda es el método de indexación recomendado para instalaciones AEM donde el tiempo de reindexación supera las ventanas de mantenimiento disponibles.

## Indexación en línea de MongoMK con oak-run.jar

* El índice en línea con [!DNL oak-run.jar] en [!DNL MongoMK] y [!DNL RDBMK] es el método recomendado para la reindexación de instalaciones de AEM [!DNL MongoMK] (y [!DNL RDBMK]). **No se debe utilizar ningún otro método para  [!DNL MongoMK] o  [!DNL RDBMK].**
* Esta indexación solo debe ejecutarse contra una única instancia de AEM en el clúster.
* La indexación en línea de [!DNL MongoMK] es segura de ejecutar con un clúster AEM en ejecución, ya que la travesía del repositorio se producirá solo en un nodo [!DNL MongoDB] único, lo que permitirá que los demás continúen sirviendo solicitudes sin un impacto significativo en el rendimiento.

El comando de índice [!DNL oak-run.jar] para realizar una indexación en línea de [!DNL MongoMK] es el [mismo que la [!DNL TarMK] Indexación en línea con [!DNL oak-run.jar]](#tarmkonlineindexingwithoakrunjar) con la diferencia de que el parámetro del almacén de segmentos señala a la instancia [!DNL MongoDB] que contiene el almacén de nodos.

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
* [Documentación de comandos del índice Apache Jackrabbit Oak oak run.jar](https://jackrabbit.apache.org/oak/docs/query/oak-run-indexing.html)
