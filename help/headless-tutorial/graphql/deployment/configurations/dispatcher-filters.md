---
title: Filtros de Dispatcher AEM para GraphQL
description: AEM Obtenga información sobre cómo configurar filtros de Dispatcher AEM de Publish para su uso con GraphQL de la.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10829
thumbnail: kt-10829.jpg
exl-id: b76b7c46-5cbd-4039-8fd6-9f0f10a4a84f
duration: 48
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 2%

---

# Dispatcher filters

Adobe Experience Manager as a Cloud Service AEM utiliza filtros de Dispatcher AEM AEM de Publish para garantizar que solo las solicitudes que deben llegar a los lleguen a los usuarios De forma predeterminada, todas las solicitudes se deniegan y se deben agregar explícitamente patrones para las direcciones URL permitidas.

| Tipo de cliente | SPA [Aplicación de una sola página ()](../spa.md) | [Componente web/JS](../web-component.md) | [Móvil](../mobile.md) | [Servidor a servidor](../server-to-server.md) |
|------------------------------------------:|:---------------------:|:----------------:|:---------:|:----------------:|
| Requiere la configuración de filtros Dispatcher | ✔ | ✔ | ✔ | ✔ |

>[!TIP]
>
> Las siguientes configuraciones son ejemplos. Asegúrese de ajustarlos para que se ajusten a los requisitos del proyecto.

## Configuración del filtro de Dispatcher

AEM La configuración del filtro de Publish Dispatcher AEM AEM define los patrones de URL que permiten alcanzar el punto de conexión, y debe incluir el prefijo de URL para el punto de conexión de la consulta persistente del.

| El cliente se conecta a | AEM Author | Publicación de AEM | AEM Previsualización de |
|------------------------------------------:|:----------:|:-------------:|:-------------:|
| Requiere la configuración de filtros Dispatcher | ✘ | ✔ | ✔ |

Agregue una regla `allow` con el patrón de URL `/graphql/execute.json/*` y asegúrese de que el ID de archivo (por ejemplo, `/0600`, sea único en el archivo de granja de servidores de ejemplo).
Esto permite realizar una solicitud HTTP al extremo de la GET AEM persistente, como `HTTP GET /graphql/execute.json/wknd-shared/adventures-all` a través de la Publish.

AEM Si utiliza fragmentos de experiencias en la experiencia sin encabezado de la, haga lo mismo para estas rutas.

+ `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...
# Allow headless requests for Persisted Query endpoints
/0600 { /type "allow" /method '(POST|OPTIONS)' /url "/graphql/execute.json/*" }
# Allow headless requests for Experience Fragments
/0601 { /type "allow" /method '(GET|OPTIONS)' /url "/content/experience-fragments/*" }
...
```

### Ejemplo de configuración de filtros

+ [Se puede encontrar un ejemplo del filtro Dispatcher en el proyecto WKND.](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/filters/filters.any#L28)
