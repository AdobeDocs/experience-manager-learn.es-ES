---
title: Utilice oak-run.jar para administrar índices
description: el comando index de oak-run.jar consolida una serie de funciones para administrar los índices Oak AEM en los entornos de trabajo, desde recopilar estadísticas de índices, ejecutar comprobaciones de coherencia de índices y reindexar los propios índices.
version: 6.4, 6.5
feature: Search
doc-type: Technical Video
topic: Performance
role: Developer
level: Experienced
exl-id: be49718e-f1f5-4ab2-9c9d-6430a52bb439
duration: 726
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '427'
ht-degree: 0%

---

# Utilice oak-run.jar para administrar índices

AEM El comando index de [!DNL oak-run.jar] consolida una serie de características para administrar [!DNL Oak]200 índices en el tiempo de ejecución, desde recopilar estadísticas de índice, ejecutar comprobaciones de coherencia de índice y volver a indexar los índices.

>[!NOTE]
>
>Dentro de este artículo y videos los términos indexación y reindexación se utilizan indistintamente y se consideran la misma operación.

## Fundamentos de comandos de índice [!DNL oak-run.jar]

>[!VIDEO](https://video.tv.adobe.com/v/21475?quality=12&learn=on)

* La versión de [[!DNL oak-run.jar]](https://repository.apache.org/service/local/artifact/maven/redirect?r=releases&amp;g=org.apache.jackrabbit&amp;a=oak-run&amp;v=1.8.0) usada debe coincidir con la versión de Oak AEM usada en la instancia de la instancia de la.
* Al administrar índices mediante [!DNL oak-run.jar], se aprovecha el comando **[!DNL index]** con varios indicadores para admitir distintas operaciones.

   * `java -jar oak-run*.jar index ...`

## Estadísticas de índice

>[!VIDEO](https://video.tv.adobe.com/v/21477?quality=12&learn=on)

* `oak-run.jar` elimina todas las definiciones de índice, las estadísticas de índice importantes y el contenido de índice para el análisis sin conexión.
* AEM La recopilación de estadísticas de índice se puede ejecutar de forma segura en instancias de datos en uso de la.

## Comprobación de coherencia de índice

>[!VIDEO](https://video.tv.adobe.com/v/21476?quality=12&learn=on)

* `oak-run.jar` determina rápidamente si los índices de lucene Oak están dañados.
* AEM La comprobación de coherencia es segura para ejecutarse en la instancia de comprobación de coherencia en uso en los niveles 1 y 2 de la prueba de coherencia en el uso.

## Indexación de TarMK Online con [!DNL oak-run.jar] {#tarmkonlineindexingwithoakrunjar}

>[!VIDEO](https://video.tv.adobe.com/v/21479?quality=12&learn=on)

* La indización en línea de [!DNL TarMK] mediante [!DNL oak-run.jar] es más rápida que establecer `reindex=true` en el nodo `oak:queryIndexDefinition`. A pesar de este aumento de rendimiento, la indexación en línea con [!DNL oak-run.jar] todavía requiere una ventana de mantenimiento para realizar la indexación.

* AEM AEM La indexación en línea de [!DNL TarMK] que usa [!DNL oak-run.jar] debería **no** ejecutarse contra instancias de fuera de la ventana de mantenimiento de instancias en proceso de.

## Indexación sin conexión de TarMK con oak-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21478?quality=12&learn=on)

* AEM La indización sin conexión de [!DNL TarMK] mediante [!DNL oak-run.jar] es el método de indización basado en [!DNL oak-run.jar] más sencillo para [!DNL TarMK], ya que requiere un solo comando [!DNL oak-run.jar], pero requiere que se cierre la instancia de la instancia de la.

## Indexación fuera de banda de TarMK con oak-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21480?quality=12&learn=on)

* AEM La indexación fuera de banda en [!DNL TarMK] con [!DNL oak-run.jar] minimiza el impacto de la indexación en instancias de la en uso.
* AEM La indexación fuera de banda es el método de indexación recomendado para instalaciones en las que el tiempo de reindexación supera los períodos de mantenimiento disponibles.

## MongoMK Indexación en línea con oak-run.jar

* AEM El índice en línea con [!DNL oak-run.jar] en [!DNL MongoMK] y [!DNL RDBMK] es el método recomendado para volver a indexar instalaciones de [!DNL MongoMK] (y [!DNL RDBMK]). **No se debe usar ningún otro método para [!DNL MongoMK] o [!DNL RDBMK].**
* AEM Esta indexación solo debe ejecutarse con una única instancia de en el clúster.
* AEM La indexación en línea de [!DNL MongoMK] se puede ejecutar con seguridad en un clúster de en ejecución, ya que el recorrido del repositorio se producirá en un solo nodo [!DNL MongoDB], lo que permitirá que los demás sigan atendiendo solicitudes sin afectar de manera significativa al rendimiento.

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
   * *Asegúrese de que la versión descargada coincida con la versión de Oak AEM instalada en el servidor, tal como se ha descrito anteriormente*.
* [Documentación del comando del índice Apache Jackrabbit Oak oak-run.jar](https://jackrabbit.apache.org/oak/docs/query/oak-run-indexing.html)
