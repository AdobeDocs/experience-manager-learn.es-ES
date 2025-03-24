---
title: Eliminación de muestras de un proyecto Maven de AEM
description: Obtenga información sobre cómo limpiar y quitar código de ejemplo de un proyecto de AEM generado por el tipo de archivo del proyecto de AEM.
version: Experience Manager as a Cloud Service
topic: Development
feature: AEM Project Archetype
role: Developer
level: Beginner
jira: KT-9092
thumbnail: 337263.jpeg
exl-id: 4e10c2b7-41b6-41a0-b8d4-9207a9d3f9c8
duration: 341
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '85'
ht-degree: 3%

---

# Eliminación de muestras de un proyecto Maven de AEM

Obtenga información sobre cómo limpiar y quitar el código de ejemplo generado de un proyecto de AEM generado por el tipo de archivo del proyecto de AEM.

>[!VIDEO](https://video.tv.adobe.com/v/337263?quality=12&learn=on)


## Recursos

+ [Arquetipo del proyecto AEM Maven](https://github.com/adobe/aem-project-archetype)

## Comandos

Se pueden ejecutar los siguientes comandos para eliminar los archivos de ejemplo generados del proyecto Maven de AEM:

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

Quitar `<div class="helloworld" ...></div>` de:

```
ui.frontend/src/main/webpack/static/index.html
```

Quitar la definición de instancia del componente `<helloworld>` de:

```
ui.content/src/main/content/jcr_root/content/wknd-examples/us/en/.content.xml
```
