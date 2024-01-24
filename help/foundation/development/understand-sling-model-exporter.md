---
title: AEM Explicación del exportador de modelos Sling en el sector de la
description: Apache Sling Models 1.3.0 presenta el Exportador de modelos Sling, una forma elegante de exportar o serializar objetos del modelo Sling en abstracciones personalizadas. Este artículo yuxtapone el caso de uso tradicional de utilizar modelos Sling para rellenar scripts HTL, con el uso del marco del exportador de modelos Sling para serializar un modelo Sling en JSON.
version: 6.4, 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
doc-type: Article
topic: Development
role: Developer
level: Beginner
exl-id: 03cdf5d1-3253-44c9-ae1f-ec5d3c562427
duration: 150
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '568'
ht-degree: 0%

---

# Comprender [!DNL Sling Model Exporter]

Apache [!DNL Sling Models] 1.3.0 presenta [!DNL Sling Model Exporter], una forma elegante de exportar o serializar [!DNL Sling Model] en abstracciones personalizadas. Este artículo yuxtapone el caso de uso tradicional del uso de [!DNL Sling Models] para rellenar scripts HTL, con el uso de [!DNL Sling Model Exporter] marco de trabajo para serializar un [!DNL Sling Model] en JSON.

## Flujo de solicitud HTTP del modelo Sling tradicional

El caso de uso tradicional de [!DNL Sling Models] es proporcionar una abstracción empresarial para un recurso o una solicitud, que proporciona scripts HTL (o, anteriormente JSP) una interfaz para acceder a funciones empresariales.

Se están desarrollando patrones comunes [!DNL Sling Models] AEM que representan componentes o páginas de la aplicación, y que utilizan la variable [!DNL Sling Model] objetos para alimentar los scripts HTL con datos, con un resultado final de HTML que se muestra en el explorador.

### Flujo de solicitud HTTP del modelo Sling

![Flujo de solicitud del modelo Sling](./assets/understand-sling-model-exporter/sling-model-request-flow.png)

1. [!DNL HTTP GET] AEM Se realiza una solicitud de un recurso en la.

   Ejemplo: `HTTP GET /content/my-resource.html`

1. Basado en el de la solicitud `sling:resourceType`, se resuelve la secuencia de comandos adecuada.

1. El script adapta la solicitud o el recurso al deseado [!DNL Sling Model].

1. La secuencia de comandos utiliza [!DNL Sling Model] para generar la representación del HTML.

1. El HTML generado por el script se devuelve en la respuesta HTTP.

Este patrón tradicional funciona bien en el contexto de la generación de HTML como el [!DNL Sling Model] se puede aprovechar fácilmente mediante HTL. Crear datos más estructurados, como JSON o XML, es un esfuerzo mucho más tedioso, ya que HTL no se presta naturalmente a la definición de estos formatos.

## [!DNL Sling Model Exporter] Flujo de solicitud HTTP

Apache [!DNL Sling Model Exporter] viene con un exportador Jackson proporcionado por Sling que serializa automáticamente un &quot;ordinario&quot; [!DNL Sling Model] en JSON. El exportador Jackson, aunque bastante configurable, inspecciona en su núcleo el [!DNL Sling Model] y genera JSON utilizando cualquier método &quot;getter&quot; como claves JSON, y el getter devuelve valores como valores JSON.

La serialización directa de [!DNL Sling Models] les permite atender ambas solicitudes web normales con sus respuestas de HTML creadas con la versión tradicional [!DNL Sling Model] flujo de solicitud (consulte más arriba), pero también exponer las representaciones JSON que pueden consumir los servicios web o las aplicaciones JavaScript.

![Flujo de solicitud HTTP del exportador del modelo Sling](./assets/understand-sling-model-exporter/sling-model-exporter-request-flow.png)

*Este flujo describe el flujo utilizando el exportador Jackson proporcionado para producir la salida JSON. El uso de exportadores personalizados sigue el mismo flujo pero con su formato de salida.*

1. La solicitud de GET AEM HTTP se realiza para un recurso en el que se encuentra el selector y la extensión registrados con el recurso en el que se ha realizado la solicitud. [!DNL Sling Model]El exportador de.

   Ejemplo: `HTTP GET /content/my-resource.model.json`

1. Sling resuelve el problema del recurso solicitado `sling:resourceType`, selector y extensión a un servlet de exportador de Sling generado dinámicamente, que se asigna al [!DNL Sling Model] con el exportador.
1. El servlet del exportador de Sling resuelto invoca el [!DNL Sling Model Exporter] contra el [!DNL Sling Model] objeto adaptado de la solicitud o el recurso (tal como determinan los adaptables de los modelos Sling).
1. El exportador serializa el [!DNL Sling Model] se basa en las anotaciones Opciones del exportador y Modelo Sling específico del exportador, y devuelve el resultado al servlet Exportador de Sling.
1. El servlet del exportador Sling devuelve la representación JSON del [!DNL Sling Model] en la respuesta HTTP.

>[!NOTE]
>
>Mientras que el proyecto Apache Sling proporciona el Exportador Jackson que serializa [!DNL Sling Models] En JSON, el marco Exportador también es compatible con los exportadores personalizados. Por ejemplo, un proyecto podría implementar un exportador personalizado que serialice un [!DNL Sling Model] en XML.

>[!NOTE]
>
>No solo lo hace [!DNL Sling Model Exporter] *serializar* [!DNL Sling Models], también puede exportarlos como objetos Java. La exportación a otros objetos Java no desempeña un papel en el flujo de solicitud HTTP y, por lo tanto, no aparece en el diagrama anterior.

## Materiales de apoyo

* [Apache [!DNL Sling Model Exporter] Documentación de marco](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
