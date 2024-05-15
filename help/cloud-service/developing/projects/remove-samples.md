---
title: AEM Eliminación de muestras de un proyecto de Maven de
description: AEM AEM Obtenga información sobre cómo limpiar y quitar código de ejemplo de un proyecto de generado por el tipo de archivo del proyecto de.
version: Cloud Service
topic: Development
feature: AEM Project Archetype
role: Developer
level: Beginner
jira: KT-9092
thumbnail: 337263.jpeg
exl-id: 4e10c2b7-41b6-41a0-b8d4-9207a9d3f9c8
duration: 341
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '85'
ht-degree: 3%

---

# AEM Eliminación de muestras de un proyecto de Maven de

AEM AEM Obtenga información sobre cómo limpiar y quitar el código de ejemplo generado de un proyecto de generado por el tipo de archivo del proyecto de.

>[!VIDEO](https://video.tv.adobe.com/v/337263?quality=12&learn=on)


## Recursos

+ [AEM Arquetipo de proyecto de Maven](https://github.com/adobe/aem-project-archetype)

## Comandos

AEM Los siguientes comandos se pueden ejecutar para eliminar los archivos de ejemplo generados del proyecto de Maven de:

```
rm -rf core/src/main/java/com/adobe/aem/wknd/examples/core/filters \
rm -rf core/src/main/java/com/adobe/aem/wknd/examples/core/listeners \
rm -rf core/src/main/java/com/adobe/aem/wknd/examples/core/models \
rm -rf core/src/main/java/com/adobe/aem/wknd/examples/core/schedulers \
rm -rf core/src/main/java/com/adobe/aem/wknd/examples/core/servlets \
rm -rf core/src/test/java/com/adobe/aem/wknd/examples/core/* \
rm -rf ui.apps/src/main/content/jcr_root/apps/wknd-examples/components/helloworld \
rm -rf ui.frontend/src/main/webpack/components/_helloworld.js \
rm -rf ui.frontend/src/main/webpack/components/_helloworld.css
```

## Ediciones

Retire el `<div class="helloworld" ...></div>` de:

```
ui.frontend/src/main/webpack/static/index.html
```

Retire el `<helloworld>` definición de instancia de componente desde:

```
ui.content/src/main/content/jcr_root/content/wknd-examples/us/en/.content.xml
```
