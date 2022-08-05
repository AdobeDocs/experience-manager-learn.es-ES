---
title: Filtros de Dispatcher para AEM GraphQL
description: Aprenda a configurar los filtros de Dispatcher de publicación de AEM para utilizarlos con AEM GraphQL.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10829
thumbnail: kt-10829.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 2%

---


# Filtros de Dispatcher

Adobe Experience Manager as a Cloud Service utiliza los filtros de Dispatcher de publicación de AEM para garantizar que solo las solicitudes que deben llegar AEM llegan a AEM. De forma predeterminada, todas las solicitudes están denegadas y los patrones para las direcciones URL permitidas deben agregarse explícitamente.

| Tipo de cliente | [Aplicación de una sola página (SPA)](../spa.md) | [Componente web/JS](../web-component.md) | [Móvil](../mobile.md) | [Servidor a servidor](../server-to-server.md) |
|------------------------------------------:|:---------------------:|:----------------:|:---------:|:----------------:|
| Requiere la configuración de filtros de Dispatcher | š | š | š | š |

>[!TIP]
>
> Las siguientes configuraciones son ejemplos. Asegúrese de ajustarlos para adaptarlos a los requisitos del proyecto.

## Configuración del filtro de Dispatcher

La configuración del filtro de AEM Publish Dispatcher define los patrones de URL a los que se permite llegar a AEM y debe incluir el prefijo de URL para el extremo de consulta persistente de AEM.

| El cliente se conecta a | AEM Author | AEM Publish | Vista previa de AEM |
|------------------------------------------:|:----------:|:-------------:|:-------------:|
| Requiere la configuración de filtros de Dispatcher | ü | š | š |

Agregue un `allow` regla con el patrón de URL `/graphql/execute.json/*`y asegúrese de que el ID de archivo (por ejemplo `/0600`, es único en el archivo de granja de ejemplos).
Esto permite una solicitud de GET HTTP al extremo de consulta persistente, como `HTTP GET /graphql/execute.json/wknd-shared/adventures-all` hasta AEM Publish.

+ `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...
/0600 { /type "allow" /url "/graphql/execute.json/*" }
...
```

### Ejemplo de configuración de filtros

+ [Se puede encontrar un ejemplo del filtro de Dispatcher en el proyecto WKND.](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/filters/filters.any#L28)