---
title: Tutoriales de AEM Headless
description: Una colección de tutoriales sobre cómo utilizar Adobe Experience Manager como un CMS sin encabezado.
feature: Fragmentos de contenido, API
topic: Sin objetivos, Administración de contenido
role: Desarrollador
level: Principiante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '468'
ht-degree: 4%

---


# Tutoriales de AEM Headless

Adobe Experience Manager tiene varias opciones para definir extremos sin encabezado y entregar su contenido como JSON. Utilice tutoriales prácticos para explorar cómo utilizar las distintas opciones y elegir lo que es adecuado para usted.

## Tutorial de las API de AEM GraphQL

>[!CAUTION]
>
> La API de AEM GraphQL para la entrega de fragmentos de contenido está disponible bajo petición.
> Póngase en contacto con el servicio de asistencia de Adobe para habilitar la API para su programa de AEM as a Cloud Service.

API de GraphQL de AEM para fragmentos de contenido
admite escenarios CMS sin encabezado en los que las aplicaciones cliente externas procesan experiencias mediante contenido administrado en AEM.

Una API de envío de contenido moderna es clave para la eficacia y el rendimiento de las aplicaciones de front-end basadas en JavaScript. El uso de una API de REST presenta desafíos:

* Gran número de solicitudes para recuperar un objeto a la vez
* A menudo, el contenido &quot;sobreentrega&quot;, lo que significa que la aplicación recibe más de lo que necesita

Para superar estos desafíos, GraphQL proporciona una API basada en consultas que permite a los clientes consultar AEM solo el contenido que necesita y recibir mediante una sola llamada de API.

* Aprenda a utilizar las API de GraphQL de AEM en el tutorial [Introducción a las API de AEM GraphQL](./graphql/overview.md)

## Tutorial de autenticación basada en tokens

AEM expone una variedad de extremos HTTP con los que se puede interactuar de forma directa, desde GraphQL, AEM Content Services a la API HTTP de Assets. A menudo, estos consumidores sin encabezado pueden tener que autenticarse en AEM para acceder a contenido o acciones protegidos. Para facilitar esto, AEM admite la autenticación basada en token de solicitudes HTTP de aplicaciones, servicios o sistemas externos.

* Obtenga información sobre cómo autenticarse en AEM a través de HTTP mediante tokens de acceso en [Autenticar con AEM as a Cloud Service desde un tutorial de aplicación externo](./authentication/overview.md)

## Tutorial de AEM Content Services

Los servicios de contenido de AEM aprovechan las páginas tradicionales de AEM para componer extremos de API de REST sin encabezado, y los componentes de AEM definen, o hacen referencia, el contenido que se va a exponer en estos puntos finales.

Los servicios de contenido de AEM permiten las mismas abstracciones de contenido utilizadas para crear páginas web en AEM Sites, para definir el contenido y los esquemas de estas API HTTP. El uso de AEM Pages y AEM Components permite a los especialistas en marketing componer y actualizar rápidamente API JSON flexibles que pueden activar cualquier aplicación.

* Aprenda a utilizar los servicios de contenido de AEM en el [tutorial Introducción a los servicios de contenido de AEM](./content-services/overview.md)

## AEM GraphQL y AEM Content Services

|  | API de AEM GraphQL | Servicios de contenido de AEM |
|--------------------------------|:-----------------|:---------------------|
| Definición del esquema | Modelos de fragmento de contenido estructurados | Componentes de AEM |
| Contenido | Fragmentos de contenido | Componentes de AEM |
| Descubrimiento de contenido | Por consulta de GraphQL | Por página de AEM |
| Formato de entrega | JSON de GraphQL | JSON de ComponentExporter de AEM |

## Otros tutoriales útiles

Otros tutoriales de AEM que pertenecen a conceptos sin encabezado son:

* [Introducción al Editor de SPA y Angular de AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-angular-tutorial/overview.html)
* [Introducción al Editor de SPA de AEM y React](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html)