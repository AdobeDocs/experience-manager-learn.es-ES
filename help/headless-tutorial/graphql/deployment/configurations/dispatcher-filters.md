---
title: Filtros de Dispatcher para AEM GraphQL
description: Obtenga información sobre cómo configurar los filtros de Dispatcher de publicación de AEM para utilizarlos con AEM GraphQL.
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10829
thumbnail: kt-10829.jpg
exl-id: b76b7c46-5cbd-4039-8fd6-9f0f10a4a84f
duration: 48
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 2%

---

# Dispatcher filters

Adobe Experience Manager as a Cloud Service utiliza los filtros de Dispatcher de publicación de AEM para garantizar que solo las solicitudes que deberían llegar a AEM lleguen a AEM. De forma predeterminada, todas las solicitudes se deniegan y se deben agregar explícitamente patrones para las direcciones URL permitidas.

| Tipo de cliente | [Aplicación de una sola página (SPA)](../spa.md) | [Componente web/JS](../web-component.md) | [Móvil](../mobile.md) | [Servidor a servidor](../server-to-server.md) |
|------------------------------------------:|:---------------------:|:----------------:|:---------:|:----------------:|
| Requiere la configuración de filtros Dispatcher | ✔ | ✔ | ✔ | ✔ |

>[!TIP]
>
> Las siguientes configuraciones son ejemplos. Asegúrese de ajustarlos para que se ajusten a los requisitos del proyecto.

## Configuración del filtro de Dispatcher

La configuración del filtro de AEM Publish Dispatcher define los patrones de URL permitidos para llegar a AEM y debe incluir el prefijo de URL del extremo de consulta persistente de AEM.

| El cliente se conecta a | AEM Author | Publicación de AEM | Previsualización de AEM |
|------------------------------------------:|:----------:|:-------------:|:-------------:|
| Requiere la configuración de filtros Dispatcher | ✘ | ✔ | ✔ |

Agregue una regla `allow` con el patrón de URL `/graphql/execute.json/*` y asegúrese de que el ID de archivo (por ejemplo, `/0600`, sea único en el archivo de granja de servidores de ejemplo).
Esto permite realizar una solicitud HTTP GET al extremo de la consulta persistente, como `HTTP GET /graphql/execute.json/wknd-shared/adventures-all` hasta AEM Publish.

Si utiliza fragmentos de experiencias en la experiencia sin encabezado de AEM, haga lo mismo para estas rutas.

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
