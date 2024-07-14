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
duration: 932
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 0%

---

# Desarrollar exportadores de modelos Sling

AEM Este tutorial técnico explica cómo configurar los parámetros para utilizarlos con el exportador de modelos Sling, mejorar un modelo Sling existente mediante el marco del exportador para representarlo como JSON y cómo utilizar las opciones del exportador y las anotaciones de Jackson para personalizar aún más el resultado.

El exportador de modelos Sling se introdujo en los modelos Sling v1.3.0. Esta nueva función permite agregar nuevas anotaciones a los modelos Sling que definen cómo el modelo se puede exportar como un objeto Java diferente o, más comúnmente, serializarlo en un formato diferente como JSON.

Apache Sling proporciona un exportador JSON de Jackson para cubrir el caso más común de exportación de modelos Sling como objetos JSON para el consumo por consumidores web programáticos, como otros servicios web y aplicaciones de JavaScript.

## AEM Configuración de la configuración para el exportador del modelo Sling

>[!VIDEO](https://video.tv.adobe.com/v/16862?quality=12&learn=on)

AEM [!DNL Sling Model Exporter] es una característica del proyecto [!DNL Apache Sling] y no está directamente enlazada al ciclo de lanzamiento de producto de la versión de la versión de la versión de la versión de la versión de la versión de la versión de producto de la. AEM [!DNL Sling Model Exporter] es compatible con la versión 6.3 y versiones posteriores de.

## Caso de uso de [!DNL Sling Model Exporter]

>[!VIDEO](https://video.tv.adobe.com/v/16863?quality=12&learn=on)

[!DNL Sling Model Exporter] es perfecto para aprovechar los modelos Sling que ya contienen lógica empresarial y que admiten representaciones de HTML a través de HTL (o anteriormente JSP), y exponen la misma representación empresarial que JSON para que la consuman los servicios web programáticos o las aplicaciones JavaScript.

## Creación de un exportador de modelos Sling

>[!VIDEO](https://video.tv.adobe.com/v/16864?quality=12&learn=on)

Habilitar la compatibilidad con [!DNL Exporter] en [!DNL Sling Model] es tan fácil como agregar la anotación `@Exporter` a la clase Java.

## Aplicar las opciones del exportador del modelo Sling

>[!VIDEO](https://video.tv.adobe.com/v/16865?quality=12&learn=on)

[!DNL Sling Model Exporter] admite pasar opciones del exportador por modelo a la implementación del exportador para controlar cómo se exporta finalmente [!DNL Sling Model]. Estas opciones generalmente se aplican &quot;globalmente&quot; a cómo se exporta [!DNL Sling Model], frente a por punto de datos que se puede realizar mediante anotaciones en línea que se describen a continuación.

[!DNL Jackson Exporter] opciones incluyen:

* [Opciones de la función de asignación](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [Opciones de característica de serialización](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

## Aplicando [!DNL Jackson] anotaciones

>[!VIDEO](https://video.tv.adobe.com/v/16866?quality=12&learn=on)

Las implementaciones de exportadores también pueden admitir anotaciones que se pueden aplicar en línea en la clase [!DNL Sling Model], lo que puede proporcionar un nivel más preciso de control sobre cómo se exportan los datos.

* [[!DNL Jackson Exporter] anotaciones](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)

## Ver el código {#view-the-code}

[SampleSlingModelExporter.java](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleSlingModelExporter.java)

## Materiales de apoyo {#supporting-materials}

* [[!DNL Jackson Mapper] Javadoc de característica](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [[!DNL Jackson Serialization] Javadoc de característica](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

* [[!DNL Jackson Annotations] Documentos](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)
