---
title: Utilice oak-run.jar para administrar índices
description: el comando index de oak-run.jar consolida una serie de funciones para administrar los índices Oak en AEM, desde recopilar estadísticas de índices, ejecutar comprobaciones de coherencia de índices y reindexar los propios índices.
version: Experience Manager 6.4, Experience Manager 6.5
feature: Search
doc-type: Technical Video
topic: Performance
role: Developer
level: Experienced
exl-id: be49718e-f1f5-4ab2-9c9d-6430a52bb439
duration: 726
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '427'
ht-degree: 0%

---

# Utilice oak-run.jar para administrar índices

El comando index de [!DNL oak-run.jar] consolida una serie de características para administrar [!DNL Oak]200 índices en AEM, desde la recopilación de estadísticas de índices, la ejecución de comprobaciones de coherencia de índices y la reindexación de índices.

>[!NOTE]
>
>Dentro de este artículo y videos los términos indexación y reindexación se utilizan indistintamente y se consideran la misma operación.

## Fundamentos de comandos de índice [!DNL oak-run.jar]

>[!VIDEO](https://video.tv.adobe.com/v/40115?quality=12&learn=on&captions=spa)

* La versión de [[!DNL oak-run.jar]](https://repository.apache.org/service/local/artifact/maven/redirect?r=releases&g=org.apache.jackrabbit&a=oak-run&v=1.8.0) utilizada debe coincidir con la versión de Oak utilizada en la instancia de AEM.
* Al administrar índices mediante [!DNL oak-run.jar], se aprovecha el comando **[!DNL index]** con varios indicadores para admitir distintas operaciones.

   * `java -jar oak-run*.jar index ...`

## Estadísticas de índice

>[!VIDEO](https://video.tv.adobe.com/v/39297?quality=12&learn=on&captions=spa)

* `oak-run.jar` elimina todas las definiciones de índice, las estadísticas de índice importantes y el contenido de índice para el análisis sin conexión.
* La recopilación de estadísticas de índice es segura para ejecutarse en instancias de AEM en uso.

## Comprobación de coherencia de índice

>[!VIDEO](https://video.tv.adobe.com/v/36780?quality=12&learn=on&captions=spa)

* `oak-run.jar` determina rápidamente si los índices de lucene Oak están dañados.
* La comprobación de coherencia es segura para ejecutarse en la instancia de AEM en uso para los niveles 1 y 2 de comprobación de coherencia.

## Indexación de TarMK Online con [!DNL oak-run.jar] {#tarmkonlineindexingwithoakrunjar}

>[!VIDEO](https://video.tv.adobe.com/v/36782?quality=12&learn=on&captions=spa)

* La indización en línea de [!DNL TarMK] mediante [!DNL oak-run.jar] es más rápida que establecer `reindex=true` en el nodo `oak:queryIndexDefinition`. A pesar de este aumento de rendimiento, la indexación en línea con [!DNL oak-run.jar] todavía requiere una ventana de mantenimiento para realizar la indexación.

* La indexación en línea de [!DNL TarMK] mediante [!DNL oak-run.jar] debería **no** ejecutarse con instancias de AEM fuera de la ventana de mantenimiento de instancias de AEM.

## Indexación sin conexión de TarMK con oak-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/36781?quality=12&learn=on&captions=spa)

* La indización sin conexión de [!DNL TarMK] mediante [!DNL oak-run.jar] es el método de indización basado en [!DNL oak-run.jar] más sencillo para [!DNL TarMK], ya que requiere un solo comando [!DNL oak-run.jar], pero requiere que se cierre la instancia de AEM.

## Indexación fuera de banda de TarMK con oak-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/340809?quality=12&learn=on&captions=spa)

* La indexación fuera de banda en [!DNL TarMK] con [!DNL oak-run.jar] minimiza el impacto de la indexación en las instancias de AEM en uso.
* La indexación fuera de banda es el método de indexación recomendado para instalaciones de AEM en las que el tiempo de reindexación supera los períodos de mantenimiento disponibles.

## MongoMK Indexación en línea con oak-run.jar

* El índice en línea con [!DNL oak-run.jar] en [!DNL MongoMK] y [!DNL RDBMK] es el método recomendado para reindexar instalaciones de AEM [!DNL MongoMK] (y [!DNL RDBMK]). **No se debe usar ningún otro método para [!DNL MongoMK] o [!DNL RDBMK].**
* Esta indexación solo debe ejecutarse con una única instancia de AEM en el clúster.
* La indexación en línea de [!DNL MongoMK] se puede ejecutar con seguridad en un clúster de AEM en ejecución, ya que el recorrido del repositorio solo se producirá en un nodo [!DNL MongoDB] único, lo que permitirá que los demás sigan atendiendo solicitudes sin afectar de manera significativa al rendimiento.

El comando de índice [!DNL oak-run.jar] para realizar una indexación en línea de [!DNL MongoMK] es el [mismo que la indexación en línea de  [!DNL TarMK] con [!DNL oak-run.jar]](#tarmkonlineindexingwithoakrunjar) con la diferencia de que el parámetro del almacén de segmentos señala a la instancia de [!DNL MongoDB] que contiene el almacén de nodos.

```
java -jar oak-run*.jar index
 --reindex
 --index-paths=/oak:index/lucene
 --read-write
 --fds-path=/path/to/datastore mongodb://server:port/aem
```

## Materiales de apoyo

* [Descargar [!DNL oak-run.jar]](https://repository.apache.org/#nexus-search;gav~org.apache.jackrabbit~oak-run~~~~kw,versionexpand)
   * *Asegúrese de que la versión descargada coincida con la versión de Oak instalada en AEM como se ha descrito anteriormente*
* [Documentación del comando del índice Apache Jackrabbit Oak oak-run.jar](https://jackrabbit.apache.org/oak/docs/query/oak-run-indexing.html)
