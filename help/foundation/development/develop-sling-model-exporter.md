---
title: Desarrollar exportadores de modelos de Sling en AEM
description: Este recorrido técnico recorre los pasos a través de la configuración de AEM para su uso con Sling Model Exporter, mejorando un modelo Sling existente usando el marco Exporter para la representación como JSON, y cómo usar las opciones Exporter y las anotaciones Jackson para personalizar aún más el resultado.
version: 6.4, 6.5
sub-product: foundation, content-services
feature: APIs
topics: content-delivery, development, headless
activity: develop
audience: developer
doc-type: technical video
topic: Development
role: Developer
level: Intermediate
exl-id: fc321ed1-5cf7-4bbe-adc6-c4905af7b43c
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '419'
ht-degree: 0%

---

# Desarrollar Exportadores De Modelo Sling

Este recorrido técnico recorre los pasos a través de la configuración de AEM para su uso con Sling Model Exporter, mejorando un modelo Sling existente usando el marco Exporter para la representación como JSON, y cómo usar las opciones Exporter y las anotaciones Jackson para personalizar aún más el resultado.

Sling Model Exporter se introdujo en los modelos Sling v1.3.0. Esta nueva función permite añadir nuevas anotaciones a los modelos Sling que definan cómo se puede exportar el modelo como un objeto Java diferente, o más comúnmente, serializado en un formato diferente, como JSON.

Apache Sling proporciona un exportador JSON de Jackson para cubrir el caso más común de exportación de modelos Sling como objetos JSON para su consumo por parte de consumidores web programáticos como otros servicios web y aplicaciones JavaScript.

## Configuración de AEM para Sling Model Exporter

>[!VIDEO](https://video.tv.adobe.com/v/16862/?quality=12&learn=on)

[!DNL Sling Model Exporter] es una función de la variable [!DNL Apache Sling] proyecto y no está directamente enlazado al ciclo de lanzamiento del producto de AEM. [!DNL Sling Model Exporter] es compatible con AEM 6.3 y posteriores.

## El caso de uso de [!DNL Sling Model Exporter]

>[!VIDEO](https://video.tv.adobe.com/v/16863/?quality=12&learn=on)

[!DNL Sling Model Exporter] es perfecto para aprovechar los modelos Sling que ya contienen lógica empresarial que admite representaciones de HTML a través de HTL (o anteriormente JSP) y exponer la misma representación empresarial que JSON para su consumo mediante servicios web programáticos o aplicaciones JavaScript.

## Creación de un exportador de modelo Sling

>[!VIDEO](https://video.tv.adobe.com/v/16864/?quality=12&learn=on)

Habilitación [!DNL Exporter] compatibilidad con [!DNL Sling Model] es tan fácil como agregar la variable `@Exporter` anotación en la clase Java.

## Aplicación de las opciones de Sling Model Exporter

>[!VIDEO](https://video.tv.adobe.com/v/16865/?quality=12&learn=on)

[!DNL Sling Model Exporter] admite pasar opciones de Exportador por modelo a la implementación de Exportador para impulsar cómo se usa la variable [!DNL Sling Model] finalmente se exporta. Estas opciones generalmente se aplican &quot;globalmente&quot; a la forma en que se usa la variable [!DNL Sling Model] se exporta en comparación con los puntos de datos que se pueden realizar mediante anotaciones en línea descritas a continuación.

[!DNL Jackson Exporter] las opciones incluyen:

* [Opciones de función de correspondencia](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [Opciones de las funciones de serialización](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

## Aplicando [!DNL Jackson] anotaciones

>[!VIDEO](https://video.tv.adobe.com/v/16866/?quality=12&learn=on)

Las implementaciones de los exportadores también pueden admitir anotaciones que se pueden aplicar en línea en la variable [!DNL Sling Model] , que puede proporcionar un nivel de control más preciso sobre cómo se exportan los datos.

* [[!DNL Jackson Exporter] anotaciones](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)

## Ver el código {#view-the-code}

[SampleSlingModelExporter.java](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleSlingModelExporter.java)

## Materiales de apoyo {#supporting-materials}

* [[!DNL Jackson Mapper] Función Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [[!DNL Jackson Serialization] Función Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

* [[!DNL Jackson Annotations] Documentos](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)
