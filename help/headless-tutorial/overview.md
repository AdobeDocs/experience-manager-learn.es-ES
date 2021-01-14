---
title: Tutoriales AEM sin encabezado
description: Una colección de tutoriales sobre cómo usar Adobe Experience Manager como un CMS sin cabeza.
translation-type: tm+mt
source-git-commit: eb2b556c5947b15a31a74a86dadd525fb06bcf14
workflow-type: tm+mt
source-wordcount: '373'
ht-degree: 5%

---


# Tutoriales AEM sin encabezado

Adobe Experience Manager tiene varias opciones para definir extremos sin encabezado y entregar su contenido como JSON. Utilice tutoriales prácticos para explorar cómo utilizar las distintas opciones y elegir lo que es adecuado para usted.

## Tutorial de API de AEM GraphQL

>[!CAUTION]
>
> El Envío API de GraphQL de AEM para fragmentos de contenido está disponible bajo petición.
> Póngase en contacto con la asistencia de Adobe para habilitar la API para su AEM como programa de Cloud Service.

API de AEM GraphQL para fragmentos de contenido
admite escenarios CMS sin encabezado en los que las aplicaciones cliente externas representan experiencias mediante contenido gestionado en AEM.

Una moderna API de envío de contenido es clave para la eficacia y el rendimiento de las aplicaciones de front-end basadas en Javascript. Usar una API de REST presenta desafíos:

* Gran número de solicitudes para recuperar un objeto a la vez
* A menudo, el contenido &quot;sobreentrega&quot;, lo que significa que la aplicación recibe más de lo que necesita

Para superar estos desafíos, GraphQL proporciona una API basada en consultas que permite a los clientes realizar consultas de AEM solo para el contenido que necesitan y recibir mediante una sola llamada de API.

* Obtenga información sobre cómo utilizar las API de GraphQL de AEM en el tutorial [Introducción a las API de GraphQL de AEM](./graphql/overview.md)

## Tutorial de servicios de contenido AEM

AEM Content Services aprovecha las páginas AEM tradicionales para componer extremos de API de REST sin encabezado y AEM Componentes define o hace referencia al contenido que se va a exponer en estos extremos.

AEM Content Services permite que las mismas abstracciones de contenido utilizadas para crear páginas web en AEM Sites definan el contenido y los esquemas de estas API HTTP. El uso de AEM páginas y componentes de AEM permite a los especialistas en marketing componer y actualizar rápidamente API de JSON flexibles que pueden activar cualquier aplicación.

* Obtenga información sobre cómo utilizar AEM Content Services en el tutorial [Introducción a los servicios de contenido de AEM](./content-services/overview.md)

## AEM GraphQL vs. Servicios de contenido AEM

|  | API de AEM GraphQL | Servicios de contenido AEM |
|--------------------------------|:-----------------|:---------------------|
| Definición de esquema | Modelos de fragmento de contenido estructurado | Componentes AEM |
| Contenido | Fragmentos de contenido | Componentes AEM |
| Descubrimiento de contenido | Por consulta de GraphQL | Por página AEM |
| Formato envío | GraphQL JSON | AEM ComponentExporter JSON |

## Otros tutoriales útiles

Otros tutoriales AEM relacionados con conceptos sin encabezado incluyen:

* [Introducción al Editor de SPA y Angular de AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-angular-tutorial/overview.html)
* [Introducción al Editor de SPA de AEM y React](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html)