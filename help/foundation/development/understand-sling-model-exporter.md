---
title: Comprender el modelo de exportación de Sling en AEM
description: Apache Sling Models 1.3.0 presenta Sling Model Exporter, una forma elegante de exportar o serializar objetos del Modelo Sling en abstracciones personalizadas. Este artículo yuxtapone el caso de uso tradicional del uso de modelos Sling para rellenar secuencias de comandos HTL, con el aprovechamiento del marco de exportación del modelo Sling para serializar un modelo Sling en JSON.
version: 6.3, 6.4, 6.5
sub-product: fundación, servicios de contenido
feature: sling-models, sling-model-exporter
topics: development, content-delivery, headless
activity: understand
audience: developer, architect
doc-type: article
translation-type: tm+mt
source-git-commit: 63295cbc353a796959ba2e98e3e21c188f596372
workflow-type: tm+mt
source-wordcount: '571'
ht-degree: 0%

---


# Comprender [!DNL Sling Model Exporter]

Apache [!DNL Sling Models] 1.3.0 presenta [!DNL Sling Model Exporter], una manera elegante de exportar o serializar objetos [!DNL Sling Model] en abstracciones personalizadas. Este artículo yuxtapone el caso de uso tradicional de utilizar [!DNL Sling Models] para rellenar scripts HTML, con el uso del módulo [!DNL Sling Model Exporter] para serializar un [!DNL Sling Model] en JSON.

## Flujo de solicitud HTTP del modelo Sling tradicional

El caso de uso tradicional de [!DNL Sling Models] es proporcionar una abstracción comercial para un recurso o una solicitud, que proporciona secuencias de comandos HTL (o, anteriormente, JSP) una interfaz para acceder a funciones comerciales.

Los patrones comunes están desarrollando [!DNL Sling Models] que representan AEM Componentes o Páginas, y usando los objetos [!DNL Sling Model] para alimentar las secuencias de comandos HTML con datos, con un resultado final de HTML que se muestra en el explorador.

### Flujo de solicitud HTTP del modelo Sling

![Flujo de solicitud de modelo Sling](./assets/understand-sling-model-exporter/sling-model-request-flow.png)

1. [!DNL HTTP GET] La solicitud se realiza para un recurso en AEM.

   Ejemplo: `HTTP GET /content/my-resource.html`

1. Según el `sling:resourceType` del recurso de solicitud, se resuelve la secuencia de comandos adecuada.

1. La secuencia de comandos adapta la solicitud o el recurso al [!DNL Sling Model] deseado.

1. La secuencia de comandos utiliza el objeto [!DNL Sling Model] para generar la representación HTML.

1. El HTML generado por la secuencia de comandos se devuelve en la respuesta HTTP.

Este patrón tradicional funciona bien en el contexto de la generación de HTML, ya que el [!DNL Sling Model] se puede aprovechar fácilmente a través de HTL. Crear datos más estructurados como JSON o XML es un esfuerzo mucho más tedioso ya que HTL no se presta naturalmente a la definición de estos formatos.

## [!DNL Sling Model Exporter] Flujo de solicitud HTTP

Apache [!DNL Sling Model Exporter] viene con un Sling proporcionado por Jackson Exporter que serializa automáticamente un objeto [!DNL Sling Model] &quot;ordinario&quot; en JSON. El exportador Jackson, aunque bastante configurable, en su núcleo inspecciona el objeto [!DNL Sling Model] y genera JSON usando cualquier método &quot;getter&quot; como claves JSON, y los valores devueltos por captador como valores JSON.

La serialización directa de [!DNL Sling Models] les permite dar servicio a ambas solicitudes Web normales con sus respuestas HTML creadas mediante el flujo de solicitud tradicional [!DNL Sling Model] (ver arriba), pero también exponer representaciones JSON que pueden ser consumidas por servicios Web o aplicaciones JavaScript.

![Flujo de solicitud HTTP de Sling Model Exporter](./assets/understand-sling-model-exporter/sling-model-exporter-request-flow.png)

*Este flujo describe el flujo usando el exportador Jackson proporcionado para producir salida JSON. El uso de exportadores personalizados sigue el mismo flujo pero con su formato de salida.*

1. La solicitud de GET HTTP se realiza para un recurso en AEM con el selector y la extensión registrados con el exportador de [!DNL Sling Model].

   Ejemplo: `HTTP GET /content/my-resource.model.json`

1. Sling resuelve el `sling:resourceType` selector del recurso solicitado y la extensión a un servlet Sling Exporter generado dinámicamente, que se asigna a [!DNL Sling Model] con Exporter.
1. El servlet Sling Exporter resuelto invoca el [!DNL Sling Model Exporter] contra el objeto [!DNL Sling Model] adaptado de la solicitud o recurso (según lo determinado por los adaptables de los modelos Sling).
1. El exportador serializa el [!DNL Sling Model] en función de las anotaciones del modelo de Sling específico del exportador y las opciones del exportador y devuelve el resultado al servlet Sling Exporter.
1. El servlet Sling Exporter devuelve la representación JSON de [!DNL Sling Model] en la respuesta HTTP.

>[!NOTE]
>
>Mientras que el proyecto Apache Sling proporciona el Jackson Exporter que serializa [!DNL Sling Models] a JSON, el marco de trabajo Exportador también admite exportadores personalizados. Por ejemplo, un proyecto podría implementar un exportador personalizado que serialice un [!DNL Sling Model] en XML.

>[!NOTE]
>
>No sólo [!DNL Sling Model Exporter] *serializa* [!DNL Sling Models], sino que también puede exportarlos como objetos Java. La exportación a otros objetos de Java no juega un papel en el flujo de solicitud HTTP y, por lo tanto, no aparece en el diagrama anterior.

## Materiales de apoyo

* [Documentación de  [!DNL Sling Model Exporter] ApacheFramework](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
