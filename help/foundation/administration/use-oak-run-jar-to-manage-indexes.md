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
source-wordcount: '449'
ht-degree: 0%

---


# Usar oak-run.jar para administrar índices

[!DNL oak-run.jar]El comando de índice de Omniture consolida una serie de funciones para administrar  [!DNL Oak]200 índices en AEM, desde la recopilación de estadísticas de índices, la ejecución de comprobaciones de coherencia de índices y la reindexación de los índices.

>[!NOTE]
>
>Dentro de este artículo y vídeos, los términos indexación y reindexación se utilizan de forma intercambiable y se consideran la misma operación.

## [!DNL oak-run.jar] conceptos básicos de comandos de índice

>[!VIDEO](https://video.tv.adobe.com/v/21475/?quality=9&learn=on)

* La versión de [[!DNL oak-run.jar]](https://repository.apache.org/service/local/artifact/maven/redirect?r=releases&amp;g=org.apache.jackrabbit&amp;a=oak-run&amp;v=1.8.0) utilizada debe coincidir con la versión de Oak utilizada en la instancia de AEM.
* La administración de índices mediante [!DNL oak-run.jar] aprovecha el comando **[!DNL index]** con varios indicadores para admitir diferentes operaciones.

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

* La indexación en línea de [!DNL TarMK] mediante [!DNL oak-run.jar] es más rápida que la configuración `reindex=true` en el nodo `oak:queryIndexDefinition`. A pesar de este aumento en el rendimiento, la indexación en línea mediante [!DNL oak-run.jar] aún requiere una ventana de mantenimiento para realizar la indexación.

* La indexación en línea de [!DNL TarMK] que utiliza [!DNL oak-run.jar] debe **no** ejecutarse con instancias de AEM fuera de la ventana de mantenimiento de instancias de AEM.

## Indexación fuera de línea de TarMK con oak-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21478/?quality=12&learn=on)

* La indexación sin conexión de [!DNL TarMK] mediante [!DNL oak-run.jar] es el método de indexación basado en [!DNL oak-run.jar] más sencillo para [!DNL TarMK], ya que requiere un único comando [!DNL oak-run.jar], pero requiere que se cierre la instancia de AEM.

## Indexación fuera de banda TarMK con oak-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21480/?quality=12&learn=on)

* La indexación fuera de banda en [!DNL TarMK] mediante [!DNL oak-run.jar] minimiza el impacto de la indexación en instancias de AEM en uso.
* La indización fuera de banda es el método de indexación recomendado para instalaciones AEM en las que el tiempo de reindexación supera las ventanas de mantenimiento disponibles.

## Indexación en línea de MongoMK con oak-run.jar

* El índice en línea con [!DNL oak-run.jar] en [!DNL MongoMK] y [!DNL RDBMK] es el método recomendado para volver a indexar [!DNL MongoMK] (y [!DNL RDBMK]) las instalaciones AEM. **No se debe utilizar ningún otro método para  [!DNL MongoMK] o  [!DNL RDBMK].**
* Esta indexación solo debe ejecutarse en una única instancia de AEM del clúster.
* La indexación en línea de [!DNL MongoMK] es segura para ejecutarse en un clúster de AEM en ejecución, ya que la inversión del repositorio se producirá solamente en un nodo [!DNL MongoDB] único, lo que permitirá que los demás continúen dando servicio a las solicitudes sin un impacto significativo en el performance.

El comando de índice [!DNL oak-run.jar] para realizar una indexación en línea de [!DNL MongoMK] es el [mismo que la indexación en línea [!DNL TarMK] con [!DNL oak-run.jar]](#tarmkonlineindexingwithoakrunjar) con la diferencia de que el parámetro de almacén de segmentos señala a la instancia [!DNL MongoDB] que contiene el almacén de nodos.

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
