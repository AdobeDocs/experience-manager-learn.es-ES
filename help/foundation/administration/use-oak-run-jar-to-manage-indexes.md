---
title: Utilice oak-run.jar para administrar índices
description: AEM El comando index de oak-run.jar consolida una serie de funciones para administrar los índices Oak en los entornos de trabajo, desde recopilar estadísticas de índices, ejecutar comprobaciones de coherencia de índices y reindexar los propios índices.
version: 6.4, 6.5
feature: Search
doc-type: Technical Video
topic: Performance
role: Developer
level: Experienced
exl-id: be49718e-f1f5-4ab2-9c9d-6430a52bb439
duration: 766
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '427'
ht-degree: 0%

---

# Utilice oak-run.jar para administrar índices

[!DNL oak-run.jar]El comando index de consolida una serie de funciones que se deben administrar [!DNL Oak]AEM 200 índices en la, desde la recopilación de estadísticas de índice, la ejecución de comprobaciones de coherencia de índices y la reindexación de índices.

>[!NOTE]
>
>Dentro de este artículo y videos los términos indexación y reindexación se utilizan indistintamente y se consideran la misma operación.

## [!DNL oak-run.jar] Conceptos básicos de comandos index

>[!VIDEO](https://video.tv.adobe.com/v/21475?quality=12&learn=on)

* La versión de [[!DNL oak-run.jar]](https://repository.apache.org/service/local/artifact/maven/redirect?r=releases&amp;g=org.apache.jackrabbit&amp;a=oak-run&amp;v=1.8.0) AEM El valor de utilizado debe coincidir con la versión de Oak utilizada en la instancia de.
* Administración de índices mediante [!DNL oak-run.jar] aprovecha el **[!DNL index]** con varios indicadores para admitir distintas operaciones.

   * `java -jar oak-run*.jar index ...`

## Estadísticas de índice

>[!VIDEO](https://video.tv.adobe.com/v/21477?quality=12&learn=on)

* `oak-run.jar` elimina todas las definiciones de índice, las estadísticas de índice importantes y el contenido de índice para el análisis sin conexión.
* AEM La recopilación de estadísticas de índice se puede ejecutar de forma segura en instancias de datos en uso de la.

## Comprobación de coherencia de índice

>[!VIDEO](https://video.tv.adobe.com/v/21476?quality=12&learn=on)

* `oak-run.jar` determina rápidamente si los índices de lucene Oak están dañados.
* AEM La comprobación de coherencia es segura para ejecutarse en la instancia de comprobación de coherencia en uso en los niveles 1 y 2 de la prueba de coherencia en el uso.

## TarMK Indexación en línea con [!DNL oak-run.jar] {#tarmkonlineindexingwithoakrunjar}

>[!VIDEO](https://video.tv.adobe.com/v/21479?quality=12&learn=on)

* Indexación en línea de [!DNL TarMK] usando [!DNL oak-run.jar] es más rápido que configurar `reindex=true` en el `oak:queryIndexDefinition` nodo. A pesar de este aumento de rendimiento, la indexación en línea mediante [!DNL oak-run.jar] aún requiere una ventana de mantenimiento para realizar la indexación.

* Indexación en línea de [!DNL TarMK] usando [!DNL oak-run.jar] debería **no** AEM AEM se ejecutará en instancias de fuera de la ventana de mantenimiento de instancias de.

## Indexación sin conexión de TarMK con oak-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21478?quality=12&learn=on)

* Indexación sin conexión de [!DNL TarMK] usando [!DNL oak-run.jar] es el más simple [!DNL oak-run.jar] enfoque de indexación basado para [!DNL TarMK] ya que requiere un único [!DNL oak-run.jar] AEM , sin embargo requiere que se cierre la instancia de la instancia de la.

## Indexación fuera de banda de TarMK con oak-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21480?quality=12&learn=on)

* Indexación fuera de banda en [!DNL TarMK] usando [!DNL oak-run.jar] AEM minimiza el impacto de la indexación en las instancias de en uso.
* AEM La indexación fuera de banda es el método de indexación recomendado para instalaciones en las que el tiempo de reindexación supera los períodos de mantenimiento disponibles.

## MongoMK Indexación en línea con oak-run.jar

* Índice en línea con [!DNL oak-run.jar] el [!DNL MongoMK] y [!DNL RDBMK] es el método recomendado para la reindexación [!DNL MongoMK] (y [!DNL RDBMK]AEM ) instalaciones de. **No se debe utilizar ningún otro método para [!DNL MongoMK] o [!DNL RDBMK].**
* AEM Esta indexación solo debe ejecutarse con una única instancia de en el clúster.
* Indexación en línea de [!DNL MongoMK] AEM es seguro ejecutarlo en un clúster de en ejecución, ya que el recorrido del repositorio solo se producirá en un único clúster [!DNL MongoDB] , lo que permite a los demás seguir atendiendo solicitudes sin afectar de forma significativa al rendimiento.

El [!DNL oak-run.jar] comando index para realizar una indexación en línea de [!DNL MongoMK] es el [igual que el [!DNL TarMK] Indexación en línea con [!DNL oak-run.jar]](#tarmkonlineindexingwithoakrunjar) con la diferencia de que el parámetro de almacén de segmentos apunta a [!DNL MongoDB] instancia de que contiene el almacén de nodos.

```
java -jar oak-run*.jar index
 --reindex
 --index-paths=/oak:index/lucene
 --read-write
 --fds-path=/path/to/datastore mongodb://server:port/aem
```

## Materiales de apoyo

* [Descargar [!DNL oak-run.jar]](https://repository.apache.org/#nexus-search;gav~org.apache.jackrabbit~oak-run~~~~kw,versionexpand)
   * *AEM Asegúrese de que la versión descargada coincida con la versión de Oak instalada en, tal como se ha descrito anteriormente*
* [Documentación del comando del índice Apache Jackrabbit Oak-run.jar](https://jackrabbit.apache.org/oak/docs/query/oak-run-indexing.html)
