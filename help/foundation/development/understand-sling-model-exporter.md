---
title: Explicación del exportador de modelos Sling en AEM
description: Apache Sling Models 1.3.0 presenta el Exportador de modelos Sling, una forma elegante de exportar o serializar objetos del modelo Sling en abstracciones personalizadas. Este artículo yuxtapone el caso de uso tradicional de utilizar modelos Sling para rellenar scripts HTL, con el uso del marco del exportador de modelos Sling para serializar un modelo Sling en JSON.
version: Experience Manager 6.4, Experience Manager 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
doc-type: Article
topic: Development
role: Developer
level: Beginner
exl-id: 03cdf5d1-3253-44c9-ae1f-ec5d3c562427
duration: 133
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '568'
ht-degree: 0%

---

# Comprender [!DNL Sling Model Exporter]

Apache [!DNL Sling Models] 1.3.0 presenta [!DNL Sling Model Exporter], una forma elegante de exportar o serializar objetos [!DNL Sling Model] en abstracciones personalizadas. Este artículo yuxtapone el caso de uso tradicional de usar [!DNL Sling Models] para rellenar scripts HTL, con el uso del marco [!DNL Sling Model Exporter] para serializar un [!DNL Sling Model] en JSON.

## Flujo de solicitud HTTP del modelo Sling tradicional

El caso de uso tradicional de [!DNL Sling Models] es proporcionar una abstracción empresarial para un recurso o una solicitud, lo que proporciona secuencias de comandos HTL (o, anteriormente JSP) una interfaz para acceder a funciones empresariales.

Hay patrones comunes que desarrollan [!DNL Sling Models] que representan componentes o páginas de AEM y que utilizan los objetos [!DNL Sling Model] para alimentar los scripts HTL con datos, con un resultado final de HTML que se muestra en el explorador.

### Flujo de solicitud HTTP del modelo Sling

![Flujo de solicitud del modelo Sling](./assets/understand-sling-model-exporter/sling-model-request-flow.png)

1. [!DNL HTTP GET]: se ha realizado una solicitud para un recurso en AEM.

   Ejemplo: `HTTP GET /content/my-resource.html`

1. En función de `sling:resourceType` del recurso de solicitud, se resuelve el script apropiado.

1. El script adapta la solicitud o el recurso al [!DNL Sling Model] deseado.

1. La secuencia de comandos utiliza el objeto [!DNL Sling Model] para generar la representación de HTML.

1. El HTML generado por el script se devuelve en la respuesta HTTP.

Este patrón tradicional funciona bien en el contexto de la generación de HTML, ya que [!DNL Sling Model] se puede aprovechar fácilmente mediante HTL. Crear datos más estructurados, como JSON o XML, es un esfuerzo mucho más tedioso, ya que HTL no se presta naturalmente a la definición de estos formatos.

## Flujo de solicitud HTTP [!DNL Sling Model Exporter]

Apache [!DNL Sling Model Exporter] viene con un exportador Jackson proporcionado por Sling que serializa automáticamente un objeto [!DNL Sling Model] &quot;ordinario&quot; en JSON. El exportador Jackson, aunque bastante configurable, inspecciona en su núcleo el objeto [!DNL Sling Model] y genera JSON utilizando cualquier método &quot;captador&quot; como claves JSON, y los valores devueltos por captador como valores JSON.

La serialización directa de [!DNL Sling Models] les permite atender solicitudes web normales con sus respuestas de HTML creadas mediante el flujo de solicitud tradicional de [!DNL Sling Model] (ver arriba), pero también exponer representaciones JSON que pueden consumir los servicios web o las aplicaciones JavaScript.

![Flujo de solicitud HTTP del exportador del modelo Sling](./assets/understand-sling-model-exporter/sling-model-exporter-request-flow.png)

*Este flujo describe el flujo utilizando el exportador Jackson proporcionado para generar la salida JSON. El uso de exportadores personalizados sigue el mismo flujo pero con su formato de salida.*

1. La solicitud HTTP GET se realiza para un recurso de AEM con el selector y la extensión registrados en el exportador de [!DNL Sling Model].

   Ejemplo: `HTTP GET /content/my-resource.model.json`

1. Sling resuelve el `sling:resourceType`, el selector y la extensión del recurso solicitado en un servlet de exportador de Sling generado dinámicamente, que se asigna al [!DNL Sling Model] con el exportador.
1. El servlet del exportador de Sling resuelto invoca el [!DNL Sling Model Exporter] contra el objeto [!DNL Sling Model] adaptado de la solicitud o el recurso (según lo determinado por los adaptables de los modelos de Sling).
1. El exportador serializa [!DNL Sling Model] en función de las anotaciones Opciones del exportador y Modelo Sling específico del exportador y devuelve el resultado al servlet Exportador de Sling.
1. El servlet del exportador de Sling devuelve la representación JSON de [!DNL Sling Model] en la respuesta HTTP.

>[!NOTE]
>
>Aunque el proyecto Apache Sling proporciona el exportador Jackson que serializa [!DNL Sling Models] en JSON, el marco del exportador también admite exportadores personalizados. Por ejemplo, un proyecto podría implementar un exportador personalizado que serialice un(a) [!DNL Sling Model] en XML.

>[!NOTE]
>
>No solo [!DNL Sling Model Exporter] *serializa* [!DNL Sling Models], sino que también puede exportarlos como objetos Java. La exportación a otros objetos Java no desempeña un papel en el flujo de solicitud HTTP y, por lo tanto, no aparece en el diagrama anterior.

## Materiales de apoyo

* [Documentación de Apache [!DNL Sling Model Exporter] Framework](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
