---
title: Modernización de repositorios
description: Obtenga información acerca de la modernización de repositorios, el contenido mutable e inmutable, la estructura de los paquetes y la herramienta CLI de modernización de repositorios.
version: Cloud Service
feature: Developer Tools
topic: Migration, Upgrade
role: Developer
level: Experienced
jira: KT-8630
thumbnail: 336958.jpeg
exl-id: e9bd9035-1f2d-4a34-a581-9c1ec2c7bc04
duration: 1305
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '145'
ht-degree: 1%

---

# Modernización de repositorios

Obtenga información acerca de la modernización de repositorios, el contenido mutable e inmutable, la estructura de los paquetes y la herramienta CLI de modernización de repositorios.

>[!VIDEO](https://video.tv.adobe.com/v/336958?quality=12&learn=on)

## Herramienta Modernizador de repositorio

![Modernizador de repositorio](./assets/repository-modernizer.png)

Como parte de la refactorización del código base, utilice el [Herramienta Modernizador de repositorio](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/repo-modernizer.html?lang=es) para reestructurar una base de código 6.x a una estructura más moderna.

## Actividades clave

* Utilice el [Modernizador de repositorio de Adobe I/O](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationrepository-modernizer) AEM herramienta para reestructurar un proyecto de modo que coincida con la estructura esperada de un proyecto as a Cloud Service de la.
* Ajuste y corrija manualmente cualquier error de compilación en la base de código actualizada.
* Configuración de un [entorno de desarrollo local](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=es) e implementar la base de código actualizada. Itere hasta que el proyecto esté en un estado estable.
* AEM Implemente la base de código actualizada en un entorno de desarrollo as a Cloud Service de y siga validando.
