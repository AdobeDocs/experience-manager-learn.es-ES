---
title: Uso de oak-run.jar para administrar índices
description: el comando index de oak-run.jar consolida una serie de características para administrar índices Oak en AEM, desde la recopilación de estadísticas de índice, la ejecución de comprobaciones de consistencia de índice y la reindexación de índices.
version: 6.4, 6.5
feature: Search
topics: search
activity: use
audience: architect, developer, implementer
doc-type: technical video
topic: Performance
role: Developer
level: Experienced
exl-id: be49718e-f1f5-4ab2-9c9d-6430a52bb439
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '449'
ht-degree: 0%

---

# Uso de oak-run.jar para administrar índices

[!DNL oak-run.jar]El comando index de consolida una serie de funciones para administrar [!DNL Oak]200 índices en AEM, desde la recopilación de estadísticas de índices, la ejecución de comprobaciones de coherencia de índices y la reindexación/indexación de los propios índices.

>[!NOTE]
>
>Dentro de este artículo y vídeos, los términos de indexación y reindexación se utilizan de forma intercambiable y se consideran la misma operación.

## [!DNL oak-run.jar] Conceptos básicos del comando index

>[!VIDEO](https://video.tv.adobe.com/v/21475?quality=12&learn=on)

* La versión de [[!DNL oak-run.jar]](https://repository.apache.org/service/local/artifact/maven/redirect?r=releases&amp;g=org.apache.jackrabbit&amp;a=oak-run&amp;v=1.8.0) usado debe coincidir con la versión de Oak utilizada en la instancia de AEM.
* Administración de índices mediante [!DNL oak-run.jar] aprovecha el **[!DNL index]** con varios indicadores para admitir diferentes operaciones.

   * `java -jar oak-run*.jar index ...`

## Estadísticas de índice

>[!VIDEO](https://video.tv.adobe.com/v/21477?quality=12&learn=on)

* `oak-run.jar` genera todas las definiciones de índice, estadísticas de índice importantes y contenido de índice para análisis sin conexión.
* La recopilación de estadísticas de índice es segura de ejecutar en instancias de AEM en uso.

## Comprobación de coherencia del índice

>[!VIDEO](https://video.tv.adobe.com/v/21476?quality=12&learn=on)

* `oak-run.jar` determina rápidamente si los índices de lucene Oak están corruptos.
* La comprobación de consistencia es segura para ejecutarse en la instancia de AEM en uso para los niveles de comprobación de consistencia 1 y 2.

## Indexación TarMK en línea con [!DNL oak-run.jar] {#tarmkonlineindexingwithoakrunjar}

>[!VIDEO](https://video.tv.adobe.com/v/21479?quality=12&learn=on)

* Indexación en línea de [!DNL TarMK] using [!DNL oak-run.jar] es más rápido que configurar `reindex=true` en el `oak:queryIndexDefinition` nodo . A pesar de este aumento de rendimiento, la indexación en línea mediante [!DNL oak-run.jar] todavía requiere una ventana de mantenimiento para realizar la indexación.

* Indexación en línea de [!DNL TarMK] using [!DNL oak-run.jar] should **not** se ejecute con instancias AEM fuera de la ventana de mantenimiento de instancias de AEM.

## Indexación fuera de línea TarMK con oak-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21478?quality=12&learn=on)

* Indexación sin conexión de [!DNL TarMK] using [!DNL oak-run.jar] es el más sencillo [!DNL oak-run.jar] enfoque de indexación basado para [!DNL TarMK] ya que requiere un [!DNL oak-run.jar] , sin embargo, requiere que la instancia de AEM se cierre.

## Indexación fuera de banda TarMK con oak-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21480?quality=12&learn=on)

* Indexación fuera de banda en [!DNL TarMK] using [!DNL oak-run.jar] minimiza el impacto de la indexación en las instancias de AEM en uso.
* La indexación fuera de banda es el método de indexación recomendado para instalaciones AEM en las que el tiempo de reindexación supera las ventanas de mantenimiento disponibles.

## Indexación en línea de MongoMK con oak-run.jar

* Índice en línea con [!DNL oak-run.jar] en [!DNL MongoMK] y [!DNL RDBMK] es el método recomendado para la reindexación [!DNL MongoMK] (y [!DNL RDBMK]) AEM instalaciones. **No se debe utilizar ningún otro método para [!DNL MongoMK] o [!DNL RDBMK].**
* Esta indexación solo debe ejecutarse en una instancia de AEM única del clúster.
* Indexación en línea de [!DNL MongoMK] es seguro ejecutarlo contra un clúster AEM en ejecución, ya que la travesía del repositorio se producirá solo en un único [!DNL MongoDB] , lo que permite que los demás continúen sirviendo solicitudes sin un impacto significativo en el rendimiento.

La variable [!DNL oak-run.jar] comando index para realizar una indexación en línea de [!DNL MongoMK] es la variable [igual que la variable [!DNL TarMK] Indexación en línea con [!DNL oak-run.jar]](#tarmkonlineindexingwithoakrunjar) con la diferencia de que el parámetro del almacén de segmentos señala a la variable [!DNL MongoDB] que contiene el almacén de nodos.

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
