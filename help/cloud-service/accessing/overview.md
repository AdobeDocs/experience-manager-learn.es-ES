---
title: Configuración del acceso a AEM as a Cloud Service
description: 'AEM as a Cloud Service es la forma nativa de la nube de aprovechar las aplicaciones de AEM y, como tal, aprovecha Adobe IMS (Identity Management System) para facilitar el inicio de sesión de los usuarios, tanto administradores como usuarios habituales, en el servicio de AEM Author. Descubra cómo se utilizan los usuarios de IMS de Adobe, los grupos de usuarios y los perfiles de producto junto con los grupos y permisos de AEM para proporcionar acceso específico a AEM Author.  '
feature: 'Usuarios y grupos '
topics: authentication
version: cloud-service
activity: setup
audience: administrator
doc-type: article
kt: 5882
thumbnail: KT-5882.jpg
topic: Administración, seguridad
role: Administrador
level: Principiante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '602'
ht-degree: 1%

---


# Configuración del acceso a AEM as a Cloud Service

>[!CONTEXTUALHELP]
>id="aemcloud_adobeims_overview"
>title="Introducción a Adobe IMS"
>abstract="AEM as a Cloud Service aprovecha Adobe IMS (Identity Management System) para facilitar el inicio de sesión de sus usuarios, tanto administradores como usuarios normales, en el servicio de AEM Author. Descubra cómo se utilizan los usuarios, grupos y perfiles de producto de Adobe IMS de forma conjunta con los grupos y permisos de AEM para proporcionar un acceso detallado al servicio de AEM Author."

AEM as a Cloud Service es la forma nativa de la nube de aprovechar las aplicaciones de AEM y, como tal, aprovecha Adobe IMS (Identity Management System) para facilitar el inicio de sesión de sus usuarios, tanto administradores como usuarios habituales, en el servicio de AEM Author. Descubra cómo se utilizan los usuarios, grupos y perfiles de producto de Adobe IMS de forma conjunta con los grupos y permisos de AEM para proporcionar un acceso detallado al servicio de AEM Author.

## Usuarios de IMS de Adobe

Los usuarios que requieren acceso al servicio AEM Author se administran como [usuarios IMS de Adobe](https://helpx.adobe.com/es/enterprise/using/set-up-identity.html) en [Admin Console](https://adminconsole.adobe.com) de Adobe. Obtenga información sobre qué son los usuarios de IMS de Adobe y cómo se accede a ellos y cómo se administran en Admin Console.

[Más información sobre los usuarios de IMS de Adobe](./adobe-ims-users.md)

## Grupos de usuarios de IMS de Adobe

Los usuarios que accedan al servicio AEM Author deben organizarse en grupos lógicos utilizando [Grupos de usuarios IMS de Adobe](https://helpx.adobe.com/enterprise/using/user-groups.html) en [Admin Console](https://adminconsole.adobe.com) de Adobe. Los grupos de usuarios de IMS de Adobe no proporcionan permisos directos ni acceso a AEM (este es el trabajo de [Adobe IMS product profiles](#adobe-ims-product-profiles)); sin embargo, son una excelente manera de definir agrupaciones lógicas de usuarios que, a su vez, pueden traducirse a niveles de acceso específicos en el servicio de AEM Author, mediante grupos y permisos de AEM.

[Más información sobre los grupos de usuarios de IMS de Adobe](./adobe-ims-user-groups.md)

## Perfiles de producto de IMS de Adobe

[Los perfiles](https://helpx.adobe.com/enterprise/using/manage-permissions-and-roles.html) de producto de Adobe IMS, administrados en Admin Console [ de ](https://adminconsole.adobe.com)Adobe, son el mecanismo que proporciona a los  [usuarios de ](#adobe-ims-users) Adobe IMS acceso para iniciar sesión en el servicio de AEM Author con un nivel de acceso base.

+ El perfil de producto __Usuarios de AEM__ proporciona a los usuarios acceso de solo lectura a AEM mediante la pertenencia al grupo Colaboradores de AEM.
+ El perfil de producto __Administradores de AEM__ ofrece a los usuarios acceso completo y administrativo a AEM.

[Obtenga información sobre los perfiles de producto de Adobe IMS](./adobe-ims-product-profiles.md)

## Grupos y permisos de usuarios de AEM

Adobe Experience Manager se basa en los usuarios de IMS de Adobe, los grupos de usuarios y los perfiles de producto para proporcionar a los usuarios acceso personalizable a AEM. Obtenga información sobre cómo construir grupos y permisos de AEM y cómo funcionan de forma conjunta con las abstracciones de Adobe IMS para proporcionar acceso fácil y personalizable a AEM.

[Obtenga información sobre usuarios, grupos y permisos de AEM](./aem-users-groups-and-permissions.md)

## Introducción a los permisos y el acceso

Un tutorial abreviado que describe la configuración de usuarios de IMS de Adobe, grupos de usuarios y perfiles de producto en Adobe Admin Console, y cómo aprovechar estas abstracciones de Adobe IMS en AEM Author para definir y administrar permisos específicos basados en grupos.

[Introducción a los permisos y el acceso a AEM](./walk-through.md)

## Recursos adicionales de Adobe Admin Console

La siguiente documentación cubre los detalles y preocupaciones específicos de [Adobe Admin Console](https://adminconsole.adobe.com) que pueden ayudar a comprender mejor Adobe Admin Console y utilizarla para administrar usuarios y acceder a través de los productos de Experience Cloud.

+ [Información general sobre la identidad de Adobe Admin Console](https://helpx.adobe.com/enterprise/using/identity.html)
+ [Funciones de administrador de Adobe Admin Console](https://helpx.adobe.com/enterprise/using/admin-roles.html)
+ [Funciones de desarrollador de Adobe Admin Console](https://helpx.adobe.com/enterprise/using/manage-developers.html)