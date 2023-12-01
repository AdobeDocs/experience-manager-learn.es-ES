---
title: AEM Desarrollar exportadores de modelos Sling en el sector de la
description: AEM Este tutorial técnico explica cómo configurar los parámetros para utilizarlos con el exportador de modelos Sling, mejorar un modelo Sling existente mediante el marco del exportador para representarlo como JSON y cómo utilizar las opciones del exportador y las anotaciones de Jackson para personalizar aún más el resultado.
version: 6.4, 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
doc-type: Technical Video
topic: Development
role: Developer
level: Intermediate
exl-id: fc321ed1-5cf7-4bbe-adc6-c4905af7b43c
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '419'
ht-degree: 0%

---

# Desarrollar exportadores de modelos Sling

AEM Este tutorial técnico explica cómo configurar los parámetros para utilizarlos con el exportador de modelos Sling, mejorar un modelo Sling existente mediante el marco del exportador para representarlo como JSON y cómo utilizar las opciones del exportador y las anotaciones de Jackson para personalizar aún más el resultado.

El exportador de modelos Sling se introdujo en los modelos Sling v1.3.0. Esta nueva función permite agregar nuevas anotaciones a los modelos Sling que definen cómo el modelo se puede exportar como un objeto Java diferente o, más comúnmente, serializarlo en un formato diferente como JSON.

Apache Sling proporciona un exportador JSON de Jackson para cubrir el caso más común de exportación de modelos Sling como objetos JSON para el consumo por consumidores web programáticos, como otros servicios web y aplicaciones JavaScript.

## AEM Configuración de la configuración para el exportador del modelo Sling

>[!VIDEO](https://video.tv.adobe.com/v/16862?quality=12&learn=on)

[!DNL Sling Model Exporter] es una característica de [!DNL Apache Sling] AEM proyecto y no está directamente enlazado al ciclo de lanzamiento de producto de la versión de la aplicación de la. [!DNL Sling Model Exporter] AEM es compatible con la versión 6.3 y posterior de la versión de.

## El caso de uso para [!DNL Sling Model Exporter]

>[!VIDEO](https://video.tv.adobe.com/v/16863?quality=12&learn=on)

[!DNL Sling Model Exporter] es perfecto para aprovechar los modelos Sling que ya contienen lógica empresarial y que admiten representaciones de HTML a través de HTL (o anteriormente JSP), y exponen la misma representación empresarial que JSON para que la consuman servicios web programáticos o aplicaciones JavaScript.

## Creación de un exportador de modelos Sling

>[!VIDEO](https://video.tv.adobe.com/v/16864?quality=12&learn=on)

Habilitando [!DNL Exporter] asistencia en un [!DNL Sling Model] es tan fácil como agregar el `@Exporter` Anotación en la clase Java.

## Aplicar las opciones del exportador del modelo Sling

>[!VIDEO](https://video.tv.adobe.com/v/16865?quality=12&learn=on)

[!DNL Sling Model Exporter] admite pasar las opciones del exportador por modelo a la implementación del exportador para impulsar la [!DNL Sling Model] finalmente se exporta. Estas opciones se aplican generalmente &quot;globalmente&quot; para mostrar el [!DNL Sling Model] se exporta, frente a por punto de datos, lo que se puede hacer mediante anotaciones en línea que se describen a continuación.

[!DNL Jackson Exporter] las opciones incluyen:

* [Opciones de función de asignador](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [Opciones de función de serialización](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

## Aplicando [!DNL Jackson] anotaciones

>[!VIDEO](https://video.tv.adobe.com/v/16866?quality=12&learn=on)

Las implementaciones de exportadores también pueden admitir anotaciones que se pueden aplicar en línea en la [!DNL Sling Model] , que puede proporcionar un nivel más preciso de control sobre cómo se exportan los datos.

* [[!DNL Jackson Exporter] anotaciones](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)

## Ver el código {#view-the-code}

[SampleSlingModelExporter.java](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleSlingModelExporter.java)

## Materiales de apoyo {#supporting-materials}

* [[!DNL Jackson Mapper] Javadoc de funciones](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [[!DNL Jackson Serialization] Javadoc de funciones](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

* [[!DNL Jackson Annotations] Documentos](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)
