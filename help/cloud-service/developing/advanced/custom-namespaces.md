---
title: Áreas de nombres personalizadas
description: Obtenga información sobre cómo definir e implementar áreas de nombres personalizadas en AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
topic: Development, Content Management
feature: Metadata
role: Developer
level: Intermediate
jira: KT-11618
thumbnail: 3412319.jpg
last-substantial-update: 2022-12-14T00:00:00Z
exl-id: e86ddc9d-ce44-407a-a20c-fb3297bb0eb2
duration: 496
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 2%

---

# Áreas de nombres personalizadas

Obtenga información sobre cómo definir e implementar [áreas de nombres](https://developer.adobe.com/experience-manager/reference-materials/spec/jcr/1.0/4.5_Namespaces.html?lang=es) personalizadas en AEM as a Cloud Service.

Las áreas de nombres personalizadas son la parte opcional de una propiedad JCR que precede a `:`. AEM utiliza varias áreas de nombres como:

+ `jcr` para propiedades del sistema JCR
+ `cq` para propiedades de AEM (anteriormente conocidas como Adobe CQ)
+ `dam` para propiedades de AEM específicas de recursos DAM
+ `dc` para propiedades principales de Dublín

... y muchos otros.

Las áreas de nombres se pueden utilizar para denotar el ámbito y la intención de una propiedad. La creación de un área de nombres personalizada, a menudo el nombre de su empresa, ayuda a identificar claramente los nodos o las propiedades específicas de su implementación de AEM y contienen datos específicos de su empresa.

Las áreas de nombres personalizadas se administran en [scripts de inicialización de repositorios de Sling (repoinit)](https://sling.apache.org/documentation/bundles/repository-initialization.html) y se implementan en AEM as a Cloud Service como configuraciones OSGi, y se agregan al proyecto [AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=es) `ui.config` del proyecto.

>[!VIDEO](https://video.tv.adobe.com/v/3412319?quality=12&learn=on)

## Recursos

+ [Documentación de inicialización del repositorio Sling (repoinit)](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios)

## Código

El siguiente código se usa para configurar un área de nombres `wknd`.

### Configuración OSGi de RepositoryInitializer

`/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config/org.apache.sling.jcr.repoinit.RepositoryInitializer~wknd-examples-namespaces.cfg.json`

```json
{

    "scripts": [
        "register namespace (wknd) https://site.wknd/1.0"
    ]
}
```

Esto permite que las propiedades personalizadas que utilizan el espacio de nombres `wknd`, tal como se indica como el primer parámetro después de la instrucción `register namespace`, se utilicen en AEM. Para obtener definiciones de scripts más avanzadas, consulte los ejemplos de la [documentación de inicialización del repositorio de Sling (repoinit)](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios).
