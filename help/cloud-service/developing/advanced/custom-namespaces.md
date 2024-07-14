---
title: Áreas de nombres personalizadas
description: Obtenga información sobre cómo definir e implementar áreas de nombres personalizadas en AEM as a Cloud Service.
version: Cloud Service
topic: Development, Content Management
feature: Metadata
role: Developer
level: Intermediate
jira: KT-11618
thumbnail: 3412319.jpg
last-substantial-update: 2022-12-14T00:00:00Z
exl-id: e86ddc9d-ce44-407a-a20c-fb3297bb0eb2
duration: 496
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 2%

---

# Áreas de nombres personalizadas

Obtenga información sobre cómo definir e implementar [áreas de nombres](https://developer.adobe.com/experience-manager/reference-materials/spec/jcr/1.0/4.5_Namespaces.html?lang=es) personalizadas en AEM as a Cloud Service.

Las áreas de nombres personalizadas son la parte opcional de una propiedad JCR que precede a `:`. AEM Utiliza varias áreas de nombres como:

+ `jcr` para propiedades del sistema JCR
+ AEM `cq` para propiedades (anteriormente conocidas como propiedades de Adobe CQ) de la
+ AEM `dam` para propiedades de específicas de los recursos DAM
+ `dc` para propiedades principales de Dublín

... y muchos otros.

Las áreas de nombres se pueden utilizar para denotar el ámbito y la intención de una propiedad. AEM La creación de un área de nombres personalizada, a menudo el nombre de su empresa, ayuda a identificar claramente los nodos o las propiedades específicas de su implementación de y contiene datos específicos de su empresa.

Las áreas de nombres personalizadas se administran en [scripts de inicialización de repositorios de Sling (repoinit)](https://sling.apache.org/documentation/bundles/repository-initialization.html) y se implementan en AEM as a Cloud Service AEM como configuraciones OSGi. Además, se agregan a su proyecto [](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=es) `ui.config` del proyecto.

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

AEM Esto permite utilizar propiedades personalizadas con el espacio de nombres `wknd`, como se indica como el primer parámetro después de la instrucción `register namespace`, en el espacio de nombres de la. Para obtener definiciones de scripts más avanzadas, consulte los ejemplos de la [documentación de inicialización del repositorio de Sling (repoinit)](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios).
