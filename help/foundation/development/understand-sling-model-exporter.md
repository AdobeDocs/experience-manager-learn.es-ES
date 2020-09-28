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

Apache [!DNL Sling Models] 1.3.0 introduce [!DNL Sling Model Exporter], una forma elegante de exportar o serializar [!DNL Sling Model] objetos en abstracciones personalizadas. Este artículo yuxtapone el caso de uso tradicional de usar [!DNL Sling Models] para rellenar scripts HTML, con el aprovechamiento del [!DNL Sling Model Exporter] marco para serializar un [!DNL Sling Model] en JSON.

## Flujo de solicitud HTTP del modelo Sling tradicional

El caso de uso tradicional para [!DNL Sling Models] es proporcionar una abstracción comercial para un recurso o una solicitud, que proporciona secuencias de comandos HTL (o, anteriormente, JSP) una interfaz para acceder a funciones comerciales.

Se están desarrollando patrones comunes [!DNL Sling Models] que representan AEM Componentes o Páginas, y que utilizan los [!DNL Sling Model] objetos para alimentar las secuencias de comandos HTML con datos, con un resultado final de HTML que se muestra en el explorador.

### Flujo de solicitud HTTP del modelo Sling

![Flujo de solicitud de modelo Sling](./assets/understand-sling-model-exporter/sling-model-request-flow.png)

1. [!DNL HTTP GET] La solicitud se realiza para un recurso en AEM.

   Ejemplo: `HTTP GET /content/my-resource.html`

1. En función de la secuencia de comandos `sling:resourceType`del recurso de solicitud, se resuelve la secuencia de comandos adecuada.

1. La secuencia de comandos adapta la solicitud o el recurso a la [!DNL Sling Model].

1. La secuencia de comandos utiliza el [!DNL Sling Model] objeto para generar la representación HTML.

1. El HTML generado por la secuencia de comandos se devuelve en la respuesta HTTP.

Este patrón tradicional funciona bien en el contexto de la generación de HTML, ya que [!DNL Sling Model] se puede aprovechar fácilmente mediante HTL. Crear datos más estructurados como JSON o XML es un esfuerzo mucho más tedioso ya que HTL no se presta naturalmente a la definición de estos formatos.

## [!DNL Sling Model Exporter] Flujo de solicitud HTTP

Apache [!DNL Sling Model Exporter] viene con un Sling proporcionado por Jackson Exporter que serializa automáticamente un objeto &quot;ordinario&quot; [!DNL Sling Model] en JSON. El exportador Jackson, aunque bastante configurable, en su núcleo inspecciona el [!DNL Sling Model] objeto y genera JSON usando cualquier método &quot;getter&quot; como claves JSON, y los valores de devolución del captador como valores JSON.

La serialización directa de [!DNL Sling Models] les permite dar servicio a ambas solicitudes Web normales con sus respuestas HTML creadas mediante el flujo de [!DNL Sling Model] solicitudes tradicional (ver arriba), pero también exponer las representaciones JSON que pueden ser consumidas por servicios Web o aplicaciones JavaScript.

![Flujo de solicitud HTTP de Sling Model Exporter](./assets/understand-sling-model-exporter/sling-model-exporter-request-flow.png)

*Este flujo describe el flujo usando el exportador Jackson proporcionado para producir salida JSON. El uso de exportadores personalizados sigue el mismo flujo pero con su formato de salida.*

1. La solicitud de GET HTTP se realiza para un recurso en AEM con el selector y la extensión registrados con el Exportador [!DNL Sling Model]de.

   Ejemplo: `HTTP GET /content/my-resource.model.json`

1. Sling resuelve el selector `sling:resourceType`, selector y extensión del recurso solicitado en un servlet Sling Exporter generado dinámicamente, que se asigna al [!DNL Sling Model] con Exporter.
1. El servlet Sling Exporter resuelto invoca el [!DNL Sling Model Exporter] objeto [!DNL Sling Model] adaptado de la solicitud o recurso (según lo determinado por los adaptables de los modelos Sling).
1. El exportador serializa el modelo [!DNL Sling Model] en función de las anotaciones de Opciones del exportador y del modelo de Sling específico del exportador y devuelve el resultado al servlet de Sling Exporter.
1. El servlet Sling Exporter devuelve la representación JSON del [!DNL Sling Model] en la respuesta HTTP.

>[!NOTE]
>
>Mientras que el proyecto Apache Sling proporciona a Jackson Exporter que se serializa [!DNL Sling Models] a JSON, el marco de trabajo Exporter también admite exportadores personalizados. Por ejemplo, un proyecto podría implementar un exportador personalizado que serialice un [!DNL Sling Model] en XML.

>[!NOTE]
>
>No sólo [!DNL Sling Model Exporter] serializa ** [!DNL Sling Models], sino que también puede exportarlas como objetos Java. La exportación a otros objetos de Java no juega un papel en el flujo de solicitud HTTP y, por lo tanto, no aparece en el diagrama anterior.

## Materiales de apoyo

* [Documentación de [!DNL Sling Model Exporter] ApacheFramework](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
