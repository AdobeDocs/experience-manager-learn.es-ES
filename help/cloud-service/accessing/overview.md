---
title: Configuración del acceso a AEM as a Cloud Service
description: AEM as a Cloud Service es la forma nativa de la nube de aprovechar las aplicaciones de AEM y, como tal, aprovecha Adobe IMS (Identity Management System) para facilitar el inicio de sesión de los usuarios, tanto administradores como usuarios regulares, en el servicio de AEM Author. Descubra cómo se utilizan los usuarios de IMS de Adobe, los grupos de usuarios y los perfiles de producto junto con los grupos de AEM y los permisos para proporcionar acceso específico a AEM Author.
version: Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Beginner
kt: 5882
thumbnail: KT-5882.jpg
exl-id: 4846a394-cf8e-4d52-8f8b-9e874f2f457b
source-git-commit: bca51ece7a9b249727b8746cc9654503059116fb
workflow-type: tm+mt
source-wordcount: '598'
ht-degree: 3%

---

# Configuración del acceso a AEM as a Cloud Service {#configuring-access-to-aem-as-a-cloud-service}

>[!CONTEXTUALHELP]
>id="aemcloud_adobeims_overview"
>title="Introducción a Adobe IMS"
>abstract="AEM as a Cloud Service aprovecha Adobe IMS (Identity Management System) para facilitar el inicio de sesión de sus usuarios, tanto administradores como usuarios regulares, en el servicio de AEM Author. Descubra cómo se utilizan los usuarios, grupos y perfiles de producto de IMS de Adobe junto con los grupos y permisos de AEM para proporcionar un acceso detallado al servicio de AEM Author."

AEM as a Cloud Service es la forma nativa de la nube de aprovechar las aplicaciones de AEM y, como tal, aprovecha Adobe IMS (Identity Management System) para facilitar el inicio de sesión de sus usuarios, tanto administradores como usuarios regulares, en el servicio de AEM Author.

![Adobe Admin Console](./assets/hero.png)

Descubra cómo se utilizan los usuarios, grupos y perfiles de producto de IMS de Adobe junto con los grupos y permisos de AEM para proporcionar un acceso detallado al servicio de AEM Author.

## Usuarios de IMS de Adobe

Los usuarios que requieren acceso al servicio de AEM Author se administran como [Usuarios de IMS de Adobe](https://helpx.adobe.com/es/enterprise/using/set-up-identity.html) en [Admin Console de Adobe](https://adminconsole.adobe.com). Obtenga información sobre qué son los usuarios de IMS de Adobe y cómo se accede a ellos y cómo se administran en Admin Console.

[Obtenga información sobre los usuarios de IMS de Adobe](./adobe-ims-users.md)

## Grupos de usuarios de IMS de Adobe

Los usuarios que accedan al servicio de AEM Author deben organizarse en grupos lógicos mediante [Grupos de usuarios de IMS de Adobe](https://helpx.adobe.com/es/enterprise/using/user-groups.html) en [Admin Console de Adobe](https://adminconsole.adobe.com). Los grupos de usuarios de IMS de Adobe no proporcionan permisos directos ni acceso a AEM (este es el trabajo de [Perfiles de producto de IMS de Adobe](#adobe-ims-product-profiles)), sin embargo, son una buena forma de definir agrupaciones lógicas de usuarios que, a su vez, pueden traducirse a niveles específicos de acceso en el servicio de AEM Author, mediante grupos de AEM y permisos.

[Más información sobre los grupos de usuarios de IMS de Adobe](./adobe-ims-user-groups.md)

## Perfiles de producto de IMS de Adobe

[Perfiles de producto de IMS de Adobe](https://helpx.adobe.com/enterprise/using/manage-permissions-and-roles.html), gestionado en [Admin Console de Adobe](https://adminconsole.adobe.com), son el mecanismo que proporciona [Usuarios de IMS de Adobe](#adobe-ims-users) acceso para iniciar sesión en el servicio AEM Author con un nivel de acceso base.

+ La variable __AEM usuarios__ perfil de producto ofrece a los usuarios acceso de solo lectura a AEM mediante la pertenencia a AEM grupo Colaboradores .
+ La variable __Administradores de AEM__ perfil de producto ofrece a los usuarios acceso administrativo y completo a AEM.

[Obtenga información sobre los perfiles de producto de Adobe IMS](./adobe-ims-product-profiles.md)

## AEM grupos de usuarios y permisos

Adobe Experience Manager se basa en los usuarios de IMS de Adobe, los grupos de usuarios y los perfiles de producto de para proporcionar a los usuarios acceso personalizable a AEM. Obtenga información sobre cómo construir grupos y permisos de AEM y cómo funcionan de forma conjunta con las abstracciones de Adobe IMS para proporcionar un acceso fácil y personalizable a los AEM.

[Obtenga información sobre AEM usuario, grupos y permisos](./aem-users-groups-and-permissions.md)

## Introducción a los permisos y el acceso

Un tutorial abreviado que describe la configuración de usuarios de IMS de Adobe, grupos de usuarios y perfiles de producto en Admin Console de Adobe, y cómo aprovechar estas abstracciones de Adobe IMS en AEM Author para definir y administrar permisos específicos basados en grupos.

[Introducción a AEM y permisos](./walk-through.md)

## Recursos adicionales de Adobe Admin Console

La siguiente documentación cubre [Adobe Admin Console](https://adminconsole.adobe.com)Detalles y preocupaciones específicos que pueden ayudar a comprender mejor Adobe Admin Console y utilizarla para administrar usuarios y acceder a través de los productos de Experience Cloud.

+ [Información general sobre la identidad de Adobe Admin Console](https://helpx.adobe.com/enterprise/using/identity.html)
+ [Funciones de administrador de Adobe Admin Console](https://helpx.adobe.com/enterprise/using/admin-roles.html)
+ [Funciones de desarrollador de Adobe Admin Console](https://helpx.adobe.com/enterprise/using/manage-developers.html)
