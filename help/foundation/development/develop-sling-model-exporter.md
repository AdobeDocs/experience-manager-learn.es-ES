---
title: Desarrollar exportadores de modelos Sling en AEM
description: Esta caminata técnica a través de la configuración de AEM para su uso con Sling Model Exporter, mejorando un modelo Sling existente usando el marco Exporter para la representación como JSON, y cómo utilizar las opciones Exporter y las anotaciones Jackson para personalizar aún más la salida.
version: 6.3, 6.4, 6.5
sub-product: fundación, servicios de contenido
feature: sling-models, sling-model-exporter
topics: content-delivery, development, headless
activity: develop
audience: developer
doc-type: technical video
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '421'
ht-degree: 0%

---


# Desarrollar exportadores de modelos Sling

Esta caminata técnica a través de la configuración de AEM para su uso con Sling Model Exporter, mejorando un modelo Sling existente usando el marco Exporter para la representación como JSON, y cómo utilizar las opciones Exporter y las anotaciones Jackson para personalizar aún más la salida.

Sling Model Exporter se introdujo en Sling Models v1.3.0. Esta nueva función permite añadir nuevas anotaciones a los modelos Sling que definen cómo se puede exportar el modelo como un objeto Java diferente, o más comúnmente, serializado en un formato diferente como JSON.

Apache Sling proporciona un exportador JSON de Jackson para cubrir el caso más común de exportar modelos Sling como objetos JSON para consumo por parte de consumidores web programáticos como otros servicios web y aplicaciones JavaScript.

## Configuración de AEM para Sling Model Exporter

>[!VIDEO](https://video.tv.adobe.com/v/16862/?quality=12&learn=on)

[!DNL Sling Model Exporter] es una característica del [!DNL Apache Sling] proyecto y no está directamente vinculada al ciclo de lanzamiento del producto AEM. [!DNL Sling Model Exporter] es compatible con AEM 6.3 y posterior.

## El caso de uso de [!DNL Sling Model Exporter]

>[!VIDEO](https://video.tv.adobe.com/v/16863/?quality=12&learn=on)

[!DNL Sling Model Exporter] es ideal para aprovechar los modelos Sling que ya contienen lógica empresarial que admite representaciones HTML a través de HTL (o anteriormente JSP) y que exponen la misma representación empresarial que JSON para el consumo mediante servicios Web programáticos o aplicaciones JavaScript.

## Creación de un exportador de modelo Sling

>[!VIDEO](https://video.tv.adobe.com/v/16864/?quality=12&learn=on)

Habilitar la [!DNL Exporter] compatibilidad en una clase [!DNL Sling Model] es tan fácil como agregar la `@Exporter` anotación a la clase Java.

## Aplicación de las opciones de exportación del modelo de ventas

>[!VIDEO](https://video.tv.adobe.com/v/16865/?quality=12&learn=on)

[!DNL Sling Model Exporter] admite el paso de las opciones de Exportador por modelo a la implementación de Exportador para impulsar cómo [!DNL Sling Model] se exporta finalmente. Estas opciones generalmente se aplican &quot;globalmente&quot; a la forma en que [!DNL Sling Model] se exportan, en comparación con los puntos de datos que se pueden realizar mediante anotaciones en línea descritas a continuación.

[!DNL Jackson Exporter] las opciones incluyen:

* [Opciones de la función Mapper](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [Opciones de las funciones de serialización](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

## Aplicación de [!DNL Jackson] anotaciones

>[!VIDEO](https://video.tv.adobe.com/v/16866/?quality=12&learn=on)

Las implementaciones de los exportadores también pueden admitir anotaciones que se pueden aplicar en línea en la [!DNL Sling Model] clase, lo que puede proporcionar un nivel de control más preciso sobre cómo se exportan los datos.

* [[!DNL Jackson Exporter] anotaciones](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)

## Vista del código {#view-the-code}

[SampleSlingModelExporter.java](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleSlingModelExporter.java)

## Materiales de apoyo {#supporting-materials}

* [[!DNL Jackson Mapper] Función Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [[!DNL Jackson Serialization] Función Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

* [[!DNL Jackson Annotations] Documentos](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)
