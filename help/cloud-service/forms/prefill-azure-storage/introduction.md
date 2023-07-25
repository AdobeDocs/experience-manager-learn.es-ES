---
title: Implementar la funcionalidad de guardar y reanudar para un formulario adaptable
description: Obtenga información sobre cómo almacenar y recuperar datos de formulario adaptables de la cuenta de almacenamiento de Azure.
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
kt: 13717
source-git-commit: 52c8d96a03b4d6e4f2a0a3c92f4307203e236417
workflow-type: tm+mt
source-wordcount: '163'
ht-degree: 3%

---

# Introducción

En este tutorial implementaremos un caso de uso sencillo que permite a los rellenadores de formularios guardar y reanudar el proceso de rellenado de formularios. El flujo del caso de uso es el siguiente

* El usuario comienza a rellenar un formulario adaptable.
* El usuario guarda el formulario y desea continuar rellenándolo en una fecha posterior.
* El usuario recibe una notificación por correo electrónico con un vínculo al formulario guardado.

## Requisitos previos

* Experiencia con AEM Forms CS, especialmente creando modelos de datos de formulario.
* Experiencia en la implementación de código mediante Cloud Manager.
* Acceso a la instancia de AEM Forms CS lista para la nube.

Para implementar el caso de uso anterior en AEM Forms CS, necesitará lo siguiente

* [Instancia de AEM Forms CS lista para la nube](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/intellij-and-aem-sync.html?lang=en#set-up-aem-author-instance)
* [cuenta del portal de Azure](https://portal.azure.com/)
* [Cuenta de SendGrid](https://sendgrid.com/)

### Pasos siguientes

[Crear componente de página](./page-component.md)


