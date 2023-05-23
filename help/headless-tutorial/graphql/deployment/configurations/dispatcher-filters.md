---
title: AEM Filtros de Dispatcher para la aplicación de GraphQL en
description: AEM Obtenga información sobre cómo configurar los filtros de Dispatcher de publicación de AEM para usarlos con GraphQL de.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10829
thumbnail: kt-10829.jpg
exl-id: b76b7c46-5cbd-4039-8fd6-9f0f10a4a84f
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 2%

---

# Filtros de Dispatcher

Adobe Experience Manager as a Cloud Service AEM AEM utiliza los filtros de Dispatcher de publicación de AEM para garantizar que solo las solicitudes que deben llegar a los destinatarios de la publicación de datos no lleguen a los destinatarios de la publicación de datos De forma predeterminada, todas las solicitudes se deniegan y se deben agregar explícitamente patrones para las direcciones URL permitidas.

| Tipo de cliente | [SPA Aplicación de una sola página ()](../spa.md) | [Componente web/JS](../web-component.md) | [Móvil](../mobile.md) | [De servidor a servidor](../server-to-server.md) |
|------------------------------------------:|:---------------------:|:----------------:|:---------:|:----------------:|
| Requiere la configuración de filtros de Dispatcher | ✔ | ✔ | ✔ | ✔ |

>[!TIP]
>
> Las siguientes configuraciones son ejemplos. Asegúrese de ajustarlos para que se ajusten a los requisitos del proyecto.

## Configuración del filtro de Dispatcher

AEM AEM La configuración del filtro de AEM Publish Dispatcher define los patrones de URL permitidos para alcanzar el punto de conexión y debe incluir el prefijo de URL para el punto de conexión de consulta persistente de la.

| El cliente se conecta a | AEM Author | AEM Publish | AEM Previsualización de |
|------------------------------------------:|:----------:|:-------------:|:-------------:|
| Requiere la configuración de filtros de Dispatcher | ✘ | ✔ | ✔ |

Añadir un `allow` regla con el patrón URL `/graphql/execute.json/*`y compruebe el ID de archivo (por ejemplo, `/0600`, es único en el archivo de granja de ejemplos).
Esto permite realizar una solicitud HTTP al extremo de la GET persistente, como `HTTP GET /graphql/execute.json/wknd-shared/adventures-all` hasta AEM Publish.

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

+ [Se puede encontrar un ejemplo del filtro de Dispatcher en el proyecto WKND.](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/filters/filters.any#L28)
