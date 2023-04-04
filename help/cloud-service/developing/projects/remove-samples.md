---
title: Eliminación de muestras de un proyecto AEM Maven
description: Obtenga información sobre cómo limpiar y eliminar código de muestra de un proyecto de AEM generado por el tipo de archivo del proyecto de AEM.
version: Cloud Service
topic: Development
feature: AEM Project Archetype
role: Developer
level: Beginner
kt: 9092
thumbnail: 337263.jpeg
exl-id: 4e10c2b7-41b6-41a0-b8d4-9207a9d3f9c8
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '88'
ht-degree: 6%

---

# Eliminación de muestras de un proyecto de Maven AEM

Obtenga información sobre cómo limpiar y eliminar el código de muestra generado de un proyecto de AEM generado por el tipo de archivo del proyecto de AEM.

>[!VIDEO](https://video.tv.adobe.com/v/337263?quality=12&learn=on)


## Recursos

+ [Tipo de archivo del proyecto AEM Maven](https://github.com/adobe/aem-project-archetype)

## Comandos

Se pueden ejecutar los siguientes comandos para eliminar los archivos de ejemplo generados del proyecto AEM Maven:

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

Elimine el `<div class="helloworld" ...></div>` de:

```
ui.frontend/src/main/webpack/static/index.html
```

Elimine el `<helloworld>` definición de instancia de componente desde:

```
ui.content/src/main/content/jcr_root/content/wknd-examples/us/en/.content.xml
```
