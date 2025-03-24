---
title: Configurar el acceso a AEM as a Cloud Service
description: AEM as a Cloud Service es la forma nativa de la nube de aprovechar las aplicaciones de AEM y, como tal, aprovecha Adobe IMS (Identity Management System) para facilitar el inicio de sesión de los usuarios, tanto administradores como usuarios habituales, en el servicio de AEM Author. Descubra cómo se utilizan los usuarios de IMS de Adobe, los grupos de usuarios y los perfiles de producto junto con los grupos y permisos de AEM para proporcionar acceso específico al Autor de AEM.
version: Experience Manager as a Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Beginner
jira: KT-5882
thumbnail: KT-5882.jpg
last-substantial-update: 2022-10-06T00:00:00Z
exl-id: 4846a394-cf8e-4d52-8f8b-9e874f2f457b
duration: 113
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '598'
ht-degree: 26%

---

# Configurar el acceso a AEM as a Cloud Service {#configuring-access-to-aem-as-a-cloud-service}

>[!CONTEXTUALHELP]
>id="aemcloud_adobeims_overview"
>title="Introducción a Adobe IMS"
>abstract="AEM as a Cloud Service aprovecha Adobe IMS (Identity Management System) para facilitar el inicio de sesión de sus usuarios, tanto administradores como usuarios comunes, en el servicio de AEM Author. Descubra cómo se utilizan los usuarios, grupos y perfiles de producto de Adobe IMS junto con los grupos y permisos de AEM para proporcionar un acceso detallado al servicio de AEM Author."

AEM as a Cloud Service es la forma nativa de la nube de aprovechar las aplicaciones de AEM y, como tal, aprovecha Adobe IMS (Identity Management System) para facilitar el inicio de sesión de sus usuarios, tanto administradores como usuarios habituales, en el servicio de AEM Author.

![Adobe Admin Console](./assets/hero.png)

Descubra cómo se utilizan los usuarios, grupos y perfiles de producto de Adobe IMS junto con los grupos y permisos de AEM para proporcionar un acceso detallado al servicio de AEM Author.

## Usuarios de Adobe IMS

Los usuarios que requieren acceso al servicio de AEM Author se administran como [usuarios de Adobe IMS](https://helpx.adobe.com/es/enterprise/using/set-up-identity.html) en [Admin Console de Adobe](https://adminconsole.adobe.com). Obtenga información sobre qué son los usuarios de Adobe IMS y cómo se accede a ellos y se administran en Admin Console.

>[!NOTE]
>
>Cuando se elimina un usuario de IMS de Admin Console, no se elimina automáticamente de AEM, pero una vez que la sesión (token) de AEM ha caducado, NO puede iniciar sesión en AEM.


[Obtenga información acerca de los usuarios de Adobe IMS](./adobe-ims-users.md)

## Grupos de usuarios de IMS de Adobe

Los usuarios que accedan al servicio de AEM Author deben estar organizados en grupos lógicos usando [grupos de usuarios de Adobe IMS](https://helpx.adobe.com/es/enterprise/using/user-groups.html) en [Admin Console de Adobe](https://adminconsole.adobe.com). Los grupos de usuarios de IMS de Adobe no proporcionan permisos directos ni acceso a AEM (este es el trabajo de [perfiles de productos de IMS de Adobe](#adobe-ims-product-profiles)); sin embargo, son una buena manera de definir agrupaciones lógicas de usuarios que, a su vez, se pueden traducir a niveles específicos de acceso en el servicio de creación de AEM mediante grupos y permisos de AEM.

[Obtenga información acerca de los grupos de usuarios de Adobe IMS](./adobe-ims-user-groups.md)

## Perfiles del producto IMS de Adobe

[Los perfiles de producto de Adobe IMS](https://helpx.adobe.com/enterprise/using/manage-permissions-and-roles.html), administrados en [Admin Console de Adobe](https://adminconsole.adobe.com), son el mecanismo que proporciona a [usuarios de IMS de Adobe](#adobe-ims-users) acceso para iniciar sesión en el servicio de AEM Author con un nivel base de acceso.

+ El perfil de producto __Usuarios de AEM__ proporciona a los usuarios acceso de solo lectura a AEM mediante la pertenencia al grupo Colaboradores de AEM.
+ El perfil de producto __Administradores de AEM__ proporciona a los usuarios acceso administrativo completo a AEM.

[Obtenga información acerca de los perfiles de producto de Adobe IMS](./adobe-ims-product-profiles.md)

## Usuarios, grupos y permisos de AEM

Adobe Experience Manager se basa en los usuarios, los grupos de usuarios y los perfiles de productos de Adobe IMS para proporcionar a los usuarios un acceso personalizable a AEM. Aprenda a construir grupos y permisos de AEM y cómo funcionan de forma conjunta con las abstracciones de IMS de Adobe para proporcionar un acceso fácil y personalizable a AEM.

[Obtenga información acerca de usuarios, grupos y permisos de AEM](./aem-users-groups-and-permissions.md)

## Guía de acceso y permisos

Un tutorial abreviado sobre la configuración de los usuarios de IMS de Adobe, los grupos de usuarios y los perfiles de producto en Adobe Admin Console, y cómo aprovechar estas abstracciones de IMS de Adobe en AEM Author para definir y administrar permisos específicos basados en grupos.

[Guía de acceso y permisos de AEM](./walk-through.md)

## Recursos adicionales de Adobe Admin Console

La siguiente documentación cubre los detalles y preocupaciones específicos de [Adobe Admin Console](https://adminconsole.adobe.com) que pueden ayudar a comprender mejor Adobe Admin Console y utilizarlo para administrar usuarios y acceder a través de productos de Experience Cloud.

+ [Resumen de identidad de Adobe Admin Console](https://helpx.adobe.com/es/enterprise/using/identity.html)
+ [Funciones de administrador de Adobe Admin Console](https://helpx.adobe.com/enterprise/using/admin-roles.html)
+ [Funciones de desarrollador de Adobe Admin Console](https://helpx.adobe.com/es/enterprise/using/support-for-experience-cloud.html)
