---
title: Áreas de nombres personalizadas
description: AEM Obtenga información sobre cómo definir e implementar áreas de nombres personalizadas para que se puedan usar en el as a Cloud Service de la.
version: Cloud Service
topic: Development, Content Management
feature: Metadata
role: Developer
level: Intermediate
kt: 11618
thumbnail: 3412319.jpg
last-substantial-update: 2022-12-14T00:00:00Z
exl-id: e86ddc9d-ce44-407a-a20c-fb3297bb0eb2
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 6%

---

# Áreas de nombres personalizadas

Obtenga información sobre cómo definir e implementar funciones personalizadas [áreas de nombres](https://developer.adobe.com/experience-manager/reference-materials/spec/jcr/1.0/4.5_Namespaces.html) AEM a los as a Cloud Service.

Las áreas de nombres personalizadas son la parte opcional de una propiedad JCR que precede a una `:`. AEM Utiliza varias áreas de nombres como:

+ `jcr` para propiedades del sistema JCR
+ `cq` AEM para propiedades (anteriormente conocidas como Adobe CQ) de la
+ `dam` AEM para propiedades de la específicas de los recursos DAM
+ `dc` para las propiedades principales de Dublín

... y muchos otros.

Las áreas de nombres se pueden utilizar para denotar el ámbito y la intención de una propiedad. AEM La creación de un área de nombres personalizada, a menudo el nombre de su empresa, ayuda a identificar claramente los nodos o las propiedades específicas de su implementación de y contiene datos específicos de su empresa.

Las áreas de nombres personalizadas se administran en [Inicialización del repositorio de Sling (repoinit)](https://sling.apache.org/documentation/bundles/repository-initialization.html) AEM scripts, e implementaciones en configuraciones as a Cloud Service como OSGi, y se añaden a las [AEM de proyecto de](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=es) `ui.config` proyecto.

>[!VIDEO](https://video.tv.adobe.com/v/3412319?quality=12&learn=on)

## Recursos

+ [Documentación de inicialización del repositorio de Sling (repoinit)](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios)

## Código 

El siguiente código se utiliza para configurar un `wknd` namespace.

### Configuración OSGi de RepositoryInitializer

`/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config/org.apache.sling.jcr.repoinit.RepositoryInitializer~wknd-examples-namespaces.cfg.json`

```json
{

    "scripts": [
        "register namespace (wknd) https://site.wknd/1.0"
    ]
}
```

Esto permite utilizar las propiedades personalizadas de `wknd` namespace, como se indica como el primer parámetro después de `register namespace` AEM instrucciones, para usar en el. Para ver definiciones de scripts más avanzadas, consulte los ejemplos en la [Documentación de inicialización del repositorio de Sling (repoinit)](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios).
