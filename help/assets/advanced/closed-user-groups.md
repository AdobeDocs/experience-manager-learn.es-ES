---
title: Grupos de usuarios cerrados en AEM Assets
description: Grupos de usuarios cerrados (CUG) es una función que se utiliza para restringir el acceso al contenido a un grupo selecto de usuarios en un sitio publicado. Este vídeo muestra cómo se pueden utilizar los grupos de usuarios cerrados con Adobe Experience Manager Assets para restringir el acceso a una carpeta específica de recursos.
version: Experience Manager 6.4, Experience Manager 6.5, Experience Manager as a Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Intermediate
jira: KT-649
thumbnail: 22155.jpg
last-substantial-update: 2022-06-06T00:00:00Z
doc-type: Feature Video
exl-id: a2bf8a82-15ee-478c-b7c3-de8a991dfeb8
duration: 321
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '357'
ht-degree: 0%

---

# Grupos de usuarios cerrados{#using-closed-user-groups-with-aem-assets}

Grupos de usuarios cerrados (CUG) es una función que se utiliza para restringir el acceso al contenido a un grupo selecto de usuarios en un sitio publicado. Este vídeo muestra cómo se pueden utilizar los grupos de usuarios cerrados con Adobe Experience Manager Assets para restringir el acceso a una carpeta específica de recursos. La compatibilidad con Grupos de usuarios cerrados con AEM Assets se introdujo por primera vez en AEM 6.4.

>[!VIDEO](https://video.tv.adobe.com/v/22155?quality=12&learn=on)

## Grupo de usuarios cerrado (CUG) con AEM Assets

* Diseñado para restringir el acceso a los recursos de una instancia de publicación de AEM.
* Otorga acceso de lectura a un conjunto de usuarios/grupos.
* El CUG solo se puede configurar en el nivel de carpeta. No se puede establecer un CUG en recursos individuales.
* Las subcarpetas y los recursos aplicados heredan automáticamente las políticas de CUG.
* Las subcarpetas pueden anular las directivas CUG al establecer una nueva directiva CUG. Esta opción debe utilizarse con moderación y no se considera una práctica recomendada.

## Grupos de usuarios cerrados frente a listas de control de acceso {#closed-user-groups-vs-access-control-lists}

Tanto los Grupos de usuarios cerrados (CUG) como las Listas de control de acceso (ACL) se utilizan para controlar el acceso al contenido en AEM y se basan en los usuarios y grupos de seguridad de AEM. Sin embargo, la aplicación e implementación de estas funciones es muy diferente. En la tabla siguiente se resumen las distinciones entre las dos funciones.

|                   | ACL | CUG |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| Uso previsto | Configure y aplique permisos para el contenido en la instancia de AEM **current**. | Configure las políticas de CUG para el contenido en la instancia de AEM **author**. Aplicar directivas CUG para el contenido en las instancias de AEM **publish**. |
| Niveles de permisos | Define permisos concedidos/denegados para usuarios/grupos para todos los niveles: Leer, Modificar, Crear, Eliminar, Leer ACL, Editar ACL, Replicar. | Otorga acceso de lectura a un conjunto de usuarios/grupos. Deniega el acceso de lectura a *todos los demás* usuarios/grupos. |
| Publicación | Las ACL *no* se han publicado con contenido. | Las directivas de CUG *se han* publicado con contenido. |

## Vínculos de soporte {#supporting-links}

* [Administración de Assets y grupos de usuarios cerrados](https://experienceleague.adobe.com/docs/experience-manager-65/assets/managing/manage-assets.html?lang=es#closed-user-group)
* [Creando un grupo de usuarios cerrado](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/cug.html?lang=es)
* [Documentación de grupo de usuarios cerrado de Oak](https://jackrabbit.apache.org/oak/docs/security/authorization/cug.html)
