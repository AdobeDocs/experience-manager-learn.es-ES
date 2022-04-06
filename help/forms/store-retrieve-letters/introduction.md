---
title: Guardar y reanudar letras
seo-title: Save and resume letters
description: Obtenga información sobre cómo guardar y recuperar letras de borrador
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
kt: 10208
source-git-commit: 0a52ea9f5a475814740bb0701a09f1a6735c6b72
workflow-type: tm+mt
source-wordcount: '132'
ht-degree: 0%

---

# Introducción

Interactive Communications permite que los agentes que preparan correspondencias ad hoc guarden correspondencias parcialmente completadas y recuperen las mismas para continuar trabajando. AEM Forms le proporciona el [Interfaz del proveedor de servicios](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/ccm/ccr/ccrDocumentInstance/api/services/CCRDocumentInstanceService.html). Se espera que el cliente implemente esta interfaz para obtener la funcionalidad Guardar y reanudar .

Este artículo utiliza la base de datos MySQL para almacenar los metadatos de la instancia de letra. Los datos de la carta se almacenan en el sistema de archivos.

El siguiente vídeo muestra el caso de uso:

>[!VIDEO](https://video.tv.adobe.com/v/342129/quality=9)

## Requisitos

Necesitará lo siguiente para implementar la solución y satisfacer sus necesidades

* Experiencia laboral con AEM Forms
* AEM Server 6.5 con Forms Add en
* Debe estar familiarizado con la creación de paquetes OSGI
