---
title: Modernización del repositorio
description: Obtenga información sobre la modernización del repositorio, el contenido mutable e inmutable, la estructura del paquete y la herramienta CLI del modernizador del repositorio.
version: Cloud Service
feature: Developer Tools
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8630
thumbnail: 336958.jpeg
exl-id: e9bd9035-1f2d-4a34-a581-9c1ec2c7bc04
source-git-commit: 53a314c5cd9eaad5a26a0992c750c159f8e3697f
workflow-type: tm+mt
source-wordcount: '161'
ht-degree: 1%

---

# Modernización del repositorio

Obtenga información sobre la modernización del repositorio, el contenido mutable e inmutable, la estructura del paquete y la herramienta CLI del modernizador del repositorio.

>[!VIDEO](https://video.tv.adobe.com/v/336958/?quality=12&learn=on)

## Herramienta Modernizador de repositorio

![Dispatcher Converter](./assets/repository-modernizer.png)

Como parte de la refactorización de la base de código, utilice la variable [Herramienta Modernizador de repositorio](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/repo-modernizer.html) para reestructurar una base de código 6.x a una estructura más moderna.

### Actividades clave

* Utilice la variable [Modernizador de repositorio de Adobe I/O](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationrepository-modernizer) herramienta para reestructurar un proyecto para que coincida con la estructura esperada de un proyecto as a Cloud Service AEM.
* Ajuste y corrija manualmente cualquier error de compilación en la base de código actualizada.
* Configure un [entorno de desarrollo local](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html) e implemente la base de código actualizada. Iterar hasta que el proyecto esté en un estado estable.
* Implemente la base de código actualizada en un entorno de desarrollo as a Cloud Service AEM y continúe validando.
