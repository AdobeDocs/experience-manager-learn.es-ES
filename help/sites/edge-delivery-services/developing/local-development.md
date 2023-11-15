---
title: Configuración de un entorno de desarrollo local para Edge Delivery Services
description: Cómo configurar un entorno de desarrollo local para Edge Delivery Services.
version: 6.5, Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Technical Video
last-substantial-update: 2023-11-15T00:00:00Z
jira: KT-14483
thumbnail: 3425717.jpeg
source-git-commit: d17544c4f8dda03e5147a1f48dbbdae005ee9438
workflow-type: tm+mt
source-wordcount: '91'
ht-degree: 6%

---


# Configure un entorno de desarrollo de local

Cómo configurar un entorno de desarrollo local para el desarrollo de Edge Delivery Services.

>[!VIDEO](https://video.tv.adobe.com/v/3425717/?learn=on)


## Pasos descritos en el vídeo

1. AEM Instalación de la CLI de

   ```
   $ sudo npm install -g @adobe/aem-cli
   ```

1. Cambie al directorio de su proyecto que sea un repositorio de Git creado a partir de [AEM placa de caldera de](https://github.com/adobe/aem-boilerplate) plantilla.

   ```
   $ git clone git@github.com:my-org/my-project.git
   $ cd my-project
   ```

1. AEM AEM Ejecute la CLI de la para iniciar la instancia local de la aplicación de la configuración de seguridad.

   ```
   $ pwd
     /Users/my-user/my-project
   
   $ aem up
       ___    ________  ___                          __      __ 
      /   |  / ____/  |/  /  _____(_)___ ___  __  __/ /___ _/ /_____  _____
     / /| | / __/ / /|_/ /  / ___/ / __ `__ \/ / / / / __ `/ __/ __ \/ ___/
    / ___ |/ /___/ /  / /  (__  ) / / / / / / /_/ / / /_/ / /_/ /_/ / /
   /_/  |_/_____/_/  /_/  /____/_/_/ /_/ /_/\__,_/_/\__,_/\__/\____/_/
   
   info: Starting AEM dev server vx.x.x
   info: Local AEM dev server up and running: http://localhost:3000/
   info: Enabled reverse proxy to https://main--my-project--my-org.hlx.page
   opening default browser: http://localhost:3000/
   ```

1. AEM Abra http://localhost:3000/ su navegador web para ver su sitio web de la.

