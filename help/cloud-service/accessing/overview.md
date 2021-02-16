---
title: Configuración del acceso a AEM como Cloud Service
description: 'AEM como Cloud Service es la forma nativa de la nube de aprovechar las aplicaciones AEM y, como tal, aprovecha Adobe IMS (Identity Management System) para facilitar el inicio de sesión de los usuarios, tanto administradores como usuarios habituales, en el servicio AEM Author. Descubra cómo se utilizan los usuarios de Adobe IMS, los grupos de usuarios y los perfiles de productos junto con los grupos de AEM y los permisos para proporcionar acceso específico a AEM Author.  '
feature: users-and-groups
topics: authentication
version: cloud-service
activity: setup
audience: administrator
doc-type: article
kt: 5882
thumbnail: KT-5882.jpg
translation-type: tm+mt
source-git-commit: f30d15f0578b7e529e4acefb8e1d2e29157ab359
workflow-type: tm+mt
source-wordcount: '595'
ht-degree: 0%

---


# Configuración del acceso a AEM como Cloud Service

>[!CONTEXTUALHELP]
>id="aemcloud_adobeims_overview"
>title="Introducción al Adobe IMS"
>abstract="AEM como Cloud Service aprovecha el sistema IMS de Adobe (Identity Management System) para facilitar el inicio de sesión de sus usuarios, tanto administradores como usuarios habituales, en el servicio AEM Author. Descubra cómo se utilizan los usuarios, grupos y perfiles de productos de Adobe IMS junto con los grupos y permisos de AEM para proporcionar un acceso preciso al servicio de AEM Author."

AEM como Cloud Service es la forma nativa de la nube de aprovechar las aplicaciones AEM y, como tal, aprovecha Adobe IMS (Identity Management System) para facilitar el inicio de sesión de sus usuarios, tanto administradores como usuarios habituales, en el servicio AEM Author. Descubra cómo se utilizan los usuarios, grupos y perfiles de productos de Adobe IMS junto con los grupos y permisos de AEM para proporcionar un acceso preciso al servicio de AEM Author.

## Usuarios de IMS de Adobe

Los usuarios que requieren acceso al servicio AEM Author se administran como [usuarios de IMS de Adobe](https://helpx.adobe.com/es/enterprise/using/set-up-identity.html) en [AdminConsole de Adobe](https://adminconsole.adobe.com). Obtenga información sobre qué son los usuarios de IMS de Adobe y cómo se accede a ellos y cómo se administran en Admin Console.

[Información sobre los usuarios de IMS de Adobe](./adobe-ims-users.md)

## Grupos de usuarios de IMS de Adobe

Los usuarios que accedan al servicio AEM Author deben organizarse en grupos lógicos mediante [grupos de usuarios de IMS de Adobe](https://helpx.adobe.com/enterprise/using/user-groups.html) en [AdminConsole de Adobe](https://adminconsole.adobe.com). Los grupos de usuarios de IMS de Adobe no proporcionan permisos directos ni acceso a AEM (este es el trabajo de [perfiles de productos de IMS de Adobe](#adobe-ims-product-profiles)); sin embargo, son una buena manera de definir agrupaciones lógicas de usuarios que, a su vez, pueden traducirse a niveles específicos de acceso en el servicio de AEM Author, mediante grupos de AEM y permisos.

[Información sobre los grupos de usuarios de IMS de Adobe](./adobe-ims-user-groups.md)

## Perfiles del producto IMS de Adobe

[Los perfiles](https://helpx.adobe.com/enterprise/using/manage-permissions-and-roles.html) del producto Adobe IMS, administrados en AdminConsole [ de ](https://adminconsole.adobe.com)Adobe, son el mecanismo que proporciona a los usuarios de IMS de  [Adobe ](#adobe-ims-users) acceso para iniciar sesión en el servicio AEM Author con un nivel base de acceso.

+ El perfil del producto __Usuarios de AEM__ proporciona a los usuarios acceso de solo lectura a AEM mediante la pertenencia al grupo Colaboradores de AEM.
+ El perfil del producto __Administradores de AEM__ permite a los usuarios acceso administrativo y completo a AEM.

[Obtenga información sobre los perfiles del producto IMS de Adobe](./adobe-ims-product-profiles.md)

## AEM grupos de usuarios y permisos

Adobe Experience Manager se basa en los usuarios de IMS de Adobe, los grupos de usuarios y los perfiles de productos para proporcionar a los usuarios acceso personalizable a AEM. Conozca cómo construir grupos y permisos de AEM y cómo funcionan en conjunto con las abstracciones de IMS de Adobe para proporcionar un acceso fácil y personalizable a AEM.

[Obtenga información sobre AEM usuario, grupos y permisos](./aem-users-groups-and-permissions.md)

## Acceso y obtención de permisos

Un tutorial abreviado que configura usuarios de IMS de Adobe, grupos de usuarios y perfiles de productos en Admin Console de Adobe, y cómo aprovechar estas abstracciones de IMS de Adobe en AEM Author para definir y administrar permisos específicos basados en grupos.

[Acceso a AEM y acceso a permisos](./walk-through.md)

## Recursos adicionales de Adobe Admin Console

La siguiente documentación abarca detalles y preocupaciones específicos de [Adobe Admin Console](https://adminconsole.adobe.com) que pueden ayudar a comprender mejor el Adobe Admin Console y usarlo para administrar usuarios y acceder a los productos de Experience Cloud.

+ [Información general sobre Adobe Admin Console Identity](https://helpx.adobe.com/enterprise/using/identity.html)
+ [Funciones de administrador de Adobe Admin Console](https://helpx.adobe.com/enterprise/using/admin-roles.html)
+ [Funciones de desarrollador de Adobe Admin Console](https://helpx.adobe.com/enterprise/using/manage-developers.html)