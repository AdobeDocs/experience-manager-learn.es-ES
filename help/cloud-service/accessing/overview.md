---
title: Configurar el acceso a AEM as a Cloud Service
description: AEM El as a Cloud Service AEM es la forma nativa de la nube de aprovechar las aplicaciones de la y, como tal, aprovecha Adobe IMS (Identity Management System) para facilitar el inicio de sesión de los usuarios, tanto los administradores como los usuarios habituales, en el servicio de AEM Author. AEM Descubra cómo se utilizan los usuarios de IMS de Adobe, los grupos de usuarios y los perfiles de producto junto con los grupos de usuarios y permisos para proporcionar acceso específico al Autor de AEM.
version: Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Beginner
kt: 5882
thumbnail: KT-5882.jpg
last-substantial-update: 2022-10-06T00:00:00Z
exl-id: 4846a394-cf8e-4d52-8f8b-9e874f2f457b
source-git-commit: 4c91ab68f6e31f0eb549689c7ecfd0ee009801d9
workflow-type: tm+mt
source-wordcount: '626'
ht-degree: 27%

---

# Configurar el acceso a AEM as a Cloud Service {#configuring-access-to-aem-as-a-cloud-service}

>[!CONTEXTUALHELP]
>id="aemcloud_adobeims_overview"
>title="Introducción a Adobe IMS"
>abstract="AEM as a Cloud Service aprovecha Adobe IMS (Identity Management System) para facilitar el inicio de sesión de sus usuarios, tanto administradores como usuarios comunes, en el servicio de AEM Author. Descubra cómo se utilizan los usuarios, grupos y perfiles de producto de Adobe IMS junto con los grupos y permisos de AEM para proporcionar un acceso detallado al servicio de AEM Author."

AEM El as a Cloud Service AEM es la forma nativa de la nube de aprovechar las aplicaciones de la y, como tal, aprovecha Adobe IMS (Identity Management System) para facilitar el inicio de sesión de sus usuarios, tanto administradores como usuarios habituales, en el servicio de AEM Author.

![Adobe Admin Console](./assets/hero.png)

Descubra cómo se utilizan los usuarios, grupos y perfiles de producto de Adobe IMS junto con los grupos y permisos de AEM para proporcionar un acceso detallado al servicio de AEM Author.

## Usuarios de IMS de Adobe

Los usuarios que requieren acceso al servicio de AEM Author se administran como [Usuarios de IMS de Adobe](https://helpx.adobe.com/es/enterprise/using/set-up-identity.html) in [Admin Console de Adobe](https://adminconsole.adobe.com). Obtenga información sobre qué son los usuarios de Adobe IMS y cómo se accede a ellos y se administran en Admin Console.

>[!NOTE]
>
>AEM AEM AEM Cuando se elimina un usuario de IMS de Admin Console, no se elimina automáticamente de la consola de administración, pero una vez que la sesión (token) ha caducado, NO se puede iniciar sesión en la consola de administración de IMS. El usuario no puede iniciar sesión en la consola de administración de.


[Obtenga información acerca de los usuarios de Adobe IMS](./adobe-ims-users.md)

## Grupos de usuarios de IMS de Adobe

Los usuarios que acceden al servicio de AEM Author deben organizarse en grupos lógicos utilizando [Grupos de usuarios de IMS de Adobe](https://helpx.adobe.com/es/enterprise/using/user-groups.html) in [Admin Console de Adobe](https://adminconsole.adobe.com). AEM Los grupos de usuarios de IMS de Adobe no proporcionan permisos directos ni acceso a los usuarios (este es el trabajo de [Perfiles de producto de Adobe IMS](#adobe-ims-product-profiles)), sin embargo, son una buena AEM manera de definir agrupaciones lógicas de usuarios que, a su vez, se pueden traducir a niveles específicos de acceso en el servicio de AEM Author, utilizando grupos y permisos de.

[Obtenga información acerca de los grupos de usuarios de Adobe IMS](./adobe-ims-user-groups.md)

## Perfiles de producto de Adobe IMS

[Perfiles de producto de Adobe IMS](https://helpx.adobe.com/enterprise/using/manage-permissions-and-roles.html), gestionado en [Admin Console de Adobe](https://adminconsole.adobe.com), son la mecánica que proporciona [Usuarios de IMS de Adobe](#adobe-ims-users) acceso para iniciar sesión en el servicio de AEM Author con un nivel base de acceso.

+ El __AEM Usuarios de__ AEM AEM El perfil de producto de permite a los usuarios acceder en solo lectura a los recursos a través de la pertenencia a un grupo de Colaboradores de.
+ El __AEM Administradores de__ AEM El perfil de producto de permite a los usuarios un acceso administrativo completo a los recursos de la administración de la.

[Obtenga información acerca de los perfiles de producto de Adobe IMS](./adobe-ims-product-profiles.md)

## AEM grupos de usuarios y permisos de

Adobe Experience Manager se basa en los usuarios, los grupos de usuarios y los perfiles de productos de Adobe IMS para proporcionar a los usuarios un acceso personalizable a AEM. AEM AEM Aprenda a construir grupos de y permisos, y cómo funcionan de forma conjunta con las abstracciones de Adobe IMS para proporcionar un acceso fácil y personalizable a los grupos de usuarios de la aplicación de correo electrónico de Adobe ().

[AEM Obtenga información acerca de usuarios, grupos y permisos de](./aem-users-groups-and-permissions.md)

## Guía de acceso y permisos

Un tutorial abreviado sobre la configuración de los usuarios de IMS de Adobe, los grupos de usuarios y los perfiles de producto en Admin Console de Adobe, y cómo aprovechar estas abstracciones de IMS de Adobe en Autor de AEM para definir y administrar permisos específicos basados en grupos.

[AEM Guía de acceso y permisos de la aplicación](./walk-through.md)

## Recursos adicionales de Adobe Admin Console

La siguiente documentación abarca [Adobe Admin Console](https://adminconsole.adobe.com)Detalles e inquietudes específicos de que pueden ayudar a comprender mejor Adobe Admin Console y utilizarlo para administrar usuarios y acceder a productos de Experience Cloud.

+ [Información general sobre la identidad de Adobe Admin Console](https://helpx.adobe.com/es/enterprise/using/identity.html)
+ [Funciones de administrador de Adobe Admin Console](https://helpx.adobe.com/enterprise/using/admin-roles.html)
+ [Funciones de desarrollador de Adobe Admin Console](https://helpx.adobe.com/es/enterprise/using/support-for-experience-cloud.html)
