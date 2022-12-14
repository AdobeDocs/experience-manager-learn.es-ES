---
title: Espacios de nombres personalizados
description: Obtenga información sobre cómo definir e implementar espacios de nombres personalizados en AEM as a Cloud Service.
version: Cloud Service
topic: Development, Content Management
feature: Metadata
role: Developer
level: Intermediate
kt: 11618
thumbnail: 3412319.jpg
last-substantial-update: 2022-12-14T00:00:00Z
source-git-commit: 79804ac1ec18f8ac4815bd5ee6f309ef85110cb9
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 4%

---

# Espacios de nombres personalizados

Obtenga información sobre cómo definir e implementar [áreas de nombres](https://developer.adobe.com/experience-manager/reference-materials/spec/jcr/1.0/4.5_Namespaces.html) a AEM as a Cloud Service.

Las áreas de nombres personalizadas son la parte opcional de una propiedad JCR que precede a una `:`. AEM utiliza varios espacios de nombres como:

+ `jcr` para propiedades del sistema JCR
+ `cq` para propiedades de AEM (anteriormente conocido como Adobe CQ)
+ `dam` para AEM propiedades específicas de los recursos DAM
+ `dc` para las propiedades principales de Dublín

... y muchos otros.

Los espacios de nombres se pueden utilizar para denotar el ámbito y la intención de una propiedad. La creación de un área de nombres personalizada, a menudo el nombre de su empresa, ayuda a identificar claramente los nodos o las propiedades específicas de su implementación de AEM y contiene datos específicos de su empresa.

Las áreas de nombres personalizadas se administran en [Inicialización del repositorio de Sling (informe)](https://sling.apache.org/documentation/bundles/repository-initialization.html) , y se implementa para AEM as a Cloud Service como configuraciones de OSGi, y se agrega al [AEM proyecto](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=es) `ui.config` proyecto.

>[!VIDEO](https://video.tv.adobe.com/v/3412319/?quality=12&learn=on)

## Medios

+ [Documentación sobre la inicialización del repositorio de Sling (repositorio)](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios)

## Código

El siguiente código se utiliza para configurar un `wknd` espacio de nombres.

### Configuración de RepositoryInitializer OSGi

`/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config/org.apache.sling.jcr.repoinit.RepositoryInitializer~wknd-examples-namespaces.cfg.json`

```json
{

    "scripts": [
        "register namespace (wknd) https://site.wknd/1.0"
    ]
}
```

Esto permite que las propiedades personalizadas que usan la variable `wknd` espacio de nombres, como se indica como el primer parámetro después de `register namespace` para usar en AEM. Para definiciones de secuencias de comandos más avanzadas, revise los ejemplos de la sección [Documentación sobre la inicialización del repositorio de Sling (repositorio)](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios).
