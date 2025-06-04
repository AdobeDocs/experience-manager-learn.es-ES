---
title: Configuración del acceso a AEM as a Cloud Service
description: AEM as a Cloud Service es la forma nativa en la nube de aprovechar las aplicaciones de AEM y, como tal, aprovecha Adobe IMS (Sistema de administración de identidades, Identity Management System) para facilitar el inicio de sesión de los usuarios, tanto administradores como usuarios habituales, en el servicio AEM Author. Descubra cómo se utilizan los usuarios, grupos de usuarios y perfiles de producto de Adobe IMS junto con los grupos y permisos de AEM para proporcionar un acceso detallado al servicio AEM Author.
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
workflow-type: ht
source-wordcount: '598'
ht-degree: 100%

---

# Configuración del acceso a AEM as a Cloud Service {#configuring-access-to-aem-as-a-cloud-service}

>[!CONTEXTUALHELP]
>id="aemcloud_adobeims_overview"
>title="Introducción a Adobe IMS"
>abstract="AEM as a Cloud Service aprovecha Adobe IMS (Identity Management System) para facilitar el inicio de sesión de sus usuarios, tanto administradores como usuarios comunes, en el servicio de AEM Author. Descubra cómo se utilizan los usuarios, grupos y perfiles de producto de Adobe IMS junto con los grupos y permisos de AEM para proporcionar un acceso detallado al servicio AEM Author."

AEM as a Cloud Service es la forma nativa en la nube de aprovechar las aplicaciones de AEM y, como tal, aprovecha Adobe IMS (sistema de administración de identidades, Identity Management System) para facilitar el inicio de sesión de sus usuarios, tanto administradores como usuarios habituales, en el servicio AEM Author. 

![Adobe Admin Console](./assets/hero.png)

Descubra cómo se utilizan los usuarios, grupos y perfiles de producto de Adobe IMS junto con los grupos y permisos de AEM para proporcionar un acceso detallado al servicio AEM Author.

## Usuarios de Adobe IMS

Los usuarios que requieren acceso al servicio AEM Author se administran como [usuarios de Adobe IMS](https://helpx.adobe.com/es/enterprise/using/set-up-identity.html) en [Adobe Admin Console](https://adminconsole.adobe.com). Obtenga información sobre qué son los usuarios de Adobe IMS y cómo se accede a ellos y se administran en Admin Console.

>[!NOTE]
>
>Cuando se elimina un usuario IMS de Admin Console, no se elimina automáticamente de AEM, pero una vez que la sesión (token) de AEM ha caducado, NO puede iniciar sesión en AEM.


[Más información sobre los usuarios de Adobe IMS](./adobe-ims-users.md)

## Grupos de usuarios de IMS de Adobe

Los usuarios que accedan al servicio AEM Author deben estar organizados en grupos lógicos usando [grupos de usuarios de Adobe IMS](https://helpx.adobe.com/es/enterprise/using/user-groups.html) en [Adobe Admin Console](https://adminconsole.adobe.com). Los grupos de usuarios de Adobe IMS no proporcionan permisos directos ni acceso a AEM (este es el trabajo de los [perfiles de productos de Adobe IMS](#adobe-ims-product-profiles)); sin embargo, son una buena manera de definir agrupaciones lógicas de usuarios que, a su vez, se pueden traducir a niveles específicos de acceso en el servicio AEM Author mediante grupos y permisos de AEM.

[Más información sobre los grupos de usuarios de Adobe IMS](./adobe-ims-user-groups.md)

## Perfiles del producto IMS de Adobe

[Los perfiles de producto Adobe IMS](https://helpx.adobe.com/enterprise/using/manage-permissions-and-roles.html?lang=es), administrados en [Adobe Admin Console](https://adminconsole.adobe.com), son el mecanismo que proporciona acceso a los [usuarios de Adobe IMS](#adobe-ims-users) para iniciar sesión en el servicio AEM Author con un nivel básico de acceso.

+ El perfil de producto de __usuarios de AEM__ proporciona a los usuarios acceso de solo lectura a AEM mediante el abono al grupo Colaboradores de AEM.
+ El perfil de producto de __Administradores de AEM__ proporciona a los usuarios acceso administrativo completo a AEM.

[Más información sobre los perfiles de producto Adobe IMS.](./adobe-ims-product-profiles.md)

## Grupos de usuarios y permisos de AEM

Adobe Experience Manager se basa en los usuarios, los grupos de usuarios y los perfiles de productos de Adobe IMS para proporcionar a los usuarios un acceso personalizable a AEM. Obtenga información sobre cómo crear grupos y permisos de AEM y cómo funcionan de forma combinada con las abstracciones de Adobe IMS para proporcionar un acceso fácil y personalizable a AEM.

[Más información sobre los usuarios, grupos y permisos de AEM](./aem-users-groups-and-permissions.md)

## Guía sobre acceso y permisos

Una explicación abreviada de cómo configurar los usuarios, los grupos de usuarios y los perfiles de producto Adobe IMS en Adobe Admin Console, y cómo aprovechar estas abstracciones de Adobe IMS en AEM Author para definir y administrar permisos específicos basados en grupos.

[Guía sobre acceso y permisos de AEM](./walk-through.md)

## Recursos adicionales de Adobe Admin Console

La siguiente documentación incluye detalles y cuestiones específicas de [Adobe Admin Console](https://adminconsole.adobe.com) que pueden resultar útiles para comprender mejor Adobe Admin Console y utilizarlo para administrar usuarios y accesos en los productos de Experience Cloud.

+ [Información general sobre la identidad de Adobe Admin Console](https://helpx.adobe.com/es/enterprise/using/identity.html)
+ [Funciones de administrador de Adobe Admin Console](https://helpx.adobe.com/enterprise/using/admin-roles.html?lang=es)
+ [Funciones de desarrollador de Adobe Admin Console](https://helpx.adobe.com/es/enterprise/using/manage-developers.html)
