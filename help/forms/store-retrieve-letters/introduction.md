---
title: Guardar y reanudar cartas
seo-title: Save and resume letters
description: Obtenga información sobre cómo guardar y recuperar borradores de cartas
seo-description: Learn how to save and retrieve draft letters
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: Development
role: Developer
level: Intermediate
jira: KT-10208
exl-id: e032070b-7332-4c2f-97ee-7e887a61aa7a
last-substantial-update: 2022-01-07T00:00:00Z
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '132'
ht-degree: 0%

---

# Introducción

Las comunicaciones interactivas permiten a los agentes preparar correspondencias ad hoc para guardar correspondencias parcialmente completadas y recuperar las mismas para seguir trabajando. AEM Forms le proporciona el [Interfaz de proveedor de servicios](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/ccm/ccr/ccrDocumentInstance/api/services/CCRDocumentInstanceService.html). Se espera que el cliente implemente esta interfaz para obtener la funcionalidad Guardar y reanudar.

Este artículo utiliza la base de datos MySQL para almacenar los metadatos de la instancia de carta. Los datos de la carta se almacenan en el sistema de archivos.

El siguiente vídeo muestra el caso de uso:

>[!VIDEO](https://video.tv.adobe.com/v/342129?quality=12&learn=on)

## Requisitos previos

Necesitará lo siguiente para implementar la solución y así satisfacer sus necesidades

* Experiencia de trabajo con AEM Forms
* AEM Servidor de 6.5 con Forms Add on
* Debe estar familiarizado con la creación de paquetes OSGI
