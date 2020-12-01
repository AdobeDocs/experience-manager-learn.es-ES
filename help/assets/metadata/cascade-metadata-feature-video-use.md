---
title: Uso de metadatos en cascada en AEM Assets
seo-title: Uso de metadatos en cascada en AEM Assets
description: La administración avanzada de metadatos permite a los usuarios crear reglas de campo en cascada para formar relaciones contextuales entre metadatos en AEM Assets. El siguiente vídeo muestra nuevas reglas dinámicas para requisitos de campo, visibilidad y opciones contextuales. El vídeo también detalla los pasos necesarios para que un administrador aplique estas reglas a un esquema de metadatos personalizado.
seo-description: La administración avanzada de metadatos permite a los usuarios crear reglas de campo en cascada para formar relaciones contextuales entre metadatos en AEM Assets. El siguiente vídeo muestra nuevas reglas dinámicas para requisitos de campo, visibilidad y opciones contextuales. El vídeo también detalla los pasos necesarios para que un administrador aplique estas reglas a un esquema de metadatos personalizado.
uuid: 470c1b1a-f888-4c90-87d7-acfa9a5fa6b1
discoiquuid: ccd1acb1-bb7f-48c2-91e0-cccbeedad831
topics: metadata
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '318'
ht-degree: 0%

---


# Uso de metadatos en cascada en AEM Assets{#using-cascading-metadata-in-aem-assets}

La administración avanzada de metadatos permite a los usuarios crear reglas de campo en cascada para formar relaciones contextuales entre metadatos en AEM Assets. El siguiente vídeo muestra nuevas reglas dinámicas para requisitos de campo, visibilidad y opciones contextuales. El vídeo también detalla los pasos necesarios para que un administrador aplique estas reglas a un esquema de metadatos personalizado.

>[!VIDEO](https://video.tv.adobe.com/v/20702/?quality=9&learn=on)

Existen tres conjuntos de reglas dinámicas que se pueden habilitar para un campo de metadatos determinado:

1. **Requisito** : un campo se puede marcar dinámicamente como necesario para que se base en el valor de otro campo desplegable.

2. **Visibilidad** : los campos siempre pueden ser visibles o solo visibles según el valor de otro campo desplegable.

3. **Opciones** : (sólo aplicable a los campos desplegables) filtre las opciones que se muestran al usuario en función del valor seleccionado actualmente en otro campo desplegable.

>[!NOTE]
>
>Las reglas en cascada SOLO se pueden crear en función de los valores de un campo desplegable. Es posible aplicar los tres conjuntos de reglas al mismo campo de metadatos, pero se recomienda que cada conjunto de reglas dependa de la misma lista desplegable de metadatos.

Descargar [paquete de metadatos personalizados](assets/cascade-metadata-values-001.zip)

## Recursos adicionales{#additional-resources}

Esquema de metadatos personalizados creado en: `/conf/global/settings/dam/adminui-extension/metadataschema/custom`. El siguiente paquete AEM aplicará esquema personalizado a la carpeta: `/content/dam/we-retail/en/activities`:

