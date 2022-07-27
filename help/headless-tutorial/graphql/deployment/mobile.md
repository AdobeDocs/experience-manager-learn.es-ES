---
title: AEM implementaciones móviles sin encabezado
description: null
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: null
thumbnail: KT-.jpg
mini-toc-levels: 2
source-git-commit: 10032375a23ba99f674fb59a6f6e48bf93908421
workflow-type: tm+mt
source-wordcount: '83'
ht-degree: 4%

---


# AEM implementaciones móviles sin encabezado

AEM implementaciones móviles sin encabezado son aplicaciones móviles nativas para iOS, Android, etc. que consumen e interactúan con el contenido de AEM de una manera sin encabezado.

Las implementaciones móviles requieren una configuración mínima, ya que las conexiones HTTP a API sin encabezado AEM no se inician en el contexto de un explorador.

## Configuraciones de implementación

| La aplicación móvil se conecta a | AEM Author | AEM Publish | Vista previa de AEM |
|-----------------------:|:----------:|:-----------:|:-----------:|
| [Filtros de Dispatcher](./dispatcher-fitlers.md) | ü | š | š |
| [Configuración CORS](./cors.md) | ü | ü | ü |
| Nombre de host de URL de imagen | š | š | š |

## Ejemplos de aplicaciones móviles

Adobe proporciona ejemplos de aplicaciones móviles de iOS y Android.


