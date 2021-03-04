---
title: Explicación del exportador de modelos de Sling en AEM
description: Apache Sling Models 1.3.0 presenta Sling Model Exporter, una forma elegante de exportar o serializar objetos del Modelo Sling en abstracciones personalizadas. Este artículo yuxtapone el caso de uso tradicional de usar modelos Sling para rellenar secuencias de comandos HTL, con el uso del marco de Sling Model Exporter para serializar un modelo Sling en JSON.
version: 6.3, 6.4, 6.5
sub-product: fundación, content-services
feature: API
topics: development, content-delivery, headless
activity: understand
audience: developer, architect
doc-type: article
topic: Desarrollo
role: Desarrollador
level: Principiante
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '575'
ht-degree: 0%

---


# Comprender [!DNL Sling Model Exporter]

Apache [!DNL Sling Models] 1.3.0 presenta [!DNL Sling Model Exporter], una forma elegante de exportar o serializar objetos [!DNL Sling Model] en abstracciones personalizadas. Este artículo yuxtapone el caso de uso tradicional de usar [!DNL Sling Models] para rellenar scripts HTL, con el uso del marco [!DNL Sling Model Exporter] para serializar un [!DNL Sling Model] en JSON.

## Flujo de solicitud HTTP del modelo tradicional de Sling

El caso de uso tradicional de [!DNL Sling Models] es proporcionar una abstracción del negocio para un recurso o solicitud, que proporciona scripts HTL (o, anteriormente JSP) una interfaz para acceder a funciones del negocio.

Los patrones comunes están desarrollando [!DNL Sling Models] que representan los componentes o páginas de AEM y utilizando los objetos [!DNL Sling Model] para alimentar los scripts HTL con datos, con un resultado final de HTML que se muestra en el explorador.

### Flujo de solicitud HTTP del modelo Sling

![Flujo de solicitud de modelo Sling](./assets/understand-sling-model-exporter/sling-model-request-flow.png)

1. [!DNL HTTP GET] La solicitud se realiza para un recurso en AEM.

   Ejemplo: `HTTP GET /content/my-resource.html`

1. En función del `sling:resourceType` del recurso de solicitud, se resuelve el script correspondiente.

1. El script adapta la solicitud o el recurso al [!DNL Sling Model] deseado.

1. La secuencia de comandos utiliza el objeto [!DNL Sling Model] para generar la representación HTML.

1. El HTML generado por el script se devuelve en la respuesta HTTP.

Este patrón tradicional funciona bien en el contexto de la generación de HTML, ya que el [!DNL Sling Model] se puede aprovechar fácilmente mediante HTL. Crear datos más estructurados como JSON o XML es un esfuerzo mucho más tedioso, ya que HTL no se presta naturalmente a la definición de estos formatos.

## [!DNL Sling Model Exporter] Flujo de solicitud HTTP

Apache [!DNL Sling Model Exporter] viene con un exportador Jackson proporcionado por Sling que serializa automáticamente un objeto [!DNL Sling Model] &quot;ordinario&quot; en JSON. El exportador Jackson, aunque es bastante configurable, en su núcleo inspecciona el objeto [!DNL Sling Model] y genera JSON utilizando cualquier método &quot;getter&quot; como claves JSON, y los valores devueltos del captador como valores JSON.

La serialización directa de [!DNL Sling Models] les permite atender ambas solicitudes web normales con sus respuestas HTML creadas mediante el flujo de solicitud tradicional [!DNL Sling Model] (consulte más arriba), pero también exponer las representaciones JSON que pueden consumir los servicios web o las aplicaciones JavaScript.

![Flujo de solicitud HTTP del exportador del modelo de Sling](./assets/understand-sling-model-exporter/sling-model-exporter-request-flow.png)

*Este flujo describe el flujo utilizando el exportador Jackson proporcionado para producir la salida JSON. El uso de exportadores personalizados sigue el mismo flujo pero con su formato de salida.*

1. La solicitud HTTP GET se realiza para un recurso en AEM con el selector y la extensión registrados con el exportador de [!DNL Sling Model].

   Ejemplo: `HTTP GET /content/my-resource.model.json`

1. Sling resuelve el `sling:resourceType`, selector y extensión del recurso solicitado en un Servlet Sling Exporter generado dinámicamente, que está asignado al [!DNL Sling Model] con Exporter.
1. El Servlet del Exportador de Sling resuelto invoca el [!DNL Sling Model Exporter] con el objeto [!DNL Sling Model] adaptado de la solicitud o recurso (tal como determinan los adaptables de los Modelos Sling).
1. El exportador serializa el [!DNL Sling Model] en función de las anotaciones del modelo de Sling de opciones del exportador y específicas del exportador y devuelve el resultado al servlet del exportador de Sling.
1. El Servlet del Exportador de Sling devuelve la representación JSON del [!DNL Sling Model] en la respuesta HTTP.

>[!NOTE]
>
>Aunque el proyecto Apache Sling proporciona el exportador Jackson que serializa [!DNL Sling Models] a JSON, el marco de exportación también admite exportadores personalizados. Por ejemplo, un proyecto podría implementar un Exportador personalizado que serialice un [!DNL Sling Model] en XML.

>[!NOTE]
>
>No solo [!DNL Sling Model Exporter] *serializa* [!DNL Sling Models], sino que también puede exportarlas como objetos Java. La exportación a otros objetos Java no tiene una función en el flujo de solicitudes HTTP y, por lo tanto, no aparece en el diagrama anterior.

## Materiales de apoyo

* [ [!DNL Sling Model Exporter] Documentación de ApacheFramework](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
