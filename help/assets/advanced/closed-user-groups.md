---
title: Grupos de usuarios cerrados en AEM Assets
description: Grupos cerrados de usuarios (CUG) es una función utilizada para restringir el acceso al contenido a un grupo seleccionado de usuarios en un sitio publicado. Este vídeo muestra cómo se pueden utilizar los grupos de usuarios cerrados con Adobe Experience Manager Assets para restringir el acceso a una carpeta específica de recursos.
version: 6.3, 6.4, 6.5, cloud-service
topic: Administración, seguridad
feature: Usuarios y grupos
role: Administrador
level: Intermedio
kt: 649
thumbnail: 22155.jpg
translation-type: tm+mt
source-git-commit: 407840a0e0c90c4f004390a052d036f9b69fa8df
workflow-type: tm+mt
source-wordcount: '384'
ht-degree: 1%

---


# Grupos de usuarios cerrados{#using-closed-user-groups-with-aem-assets}

Grupos cerrados de usuarios (CUG) es una función utilizada para restringir el acceso al contenido a un grupo seleccionado de usuarios en un sitio publicado. Este vídeo muestra cómo se pueden utilizar los grupos de usuarios cerrados con Adobe Experience Manager Assets para restringir el acceso a una carpeta específica de recursos. La compatibilidad con los grupos de usuarios cerrados con AEM Assets se introdujo por primera vez en AEM 6.4.

>[!VIDEO](https://video.tv.adobe.com/v/22155?quality=12&learn=on)

## Grupo de usuarios cerrado (CUG) con AEM Assets

* Diseñado para restringir el acceso a los recursos en una instancia de AEM Publish.
* Concede acceso de lectura a un conjunto de usuarios/grupos.
* El CUG solo se puede configurar a nivel de carpeta. El CUG no se puede establecer en recursos individuales.
* Las directivas de CUG se heredan automáticamente por cualquier subcarpeta o recurso aplicado.
* Las subcarpetas pueden anular las directivas de CUG estableciendo una nueva directiva de CUG. Esto debe utilizarse con moderación y no se considera una práctica recomendada.

## Grupos de usuarios cerrados vs. Listas de Controles de acceso {#closed-user-groups-vs-access-control-lists}

Tanto los grupos de usuarios cerrados (CUG) como las Listas de Control de acceso (ACL) se utilizan para controlar el acceso al contenido en AEM y en función de los usuarios y grupos de seguridad AEM. Sin embargo, la aplicación e implementación de estas características es muy diferente. La siguiente tabla resume las distinciones entre las dos características.

|  | ACL | CUG |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| Uso previsto | Configure y aplique permisos para el contenido en la instancia de AEM **actual**. | Configure las directivas de CUG para el contenido en AEM instancia **de autor**. Aplique directivas de CUG para el contenido en AEM **instancia de publicación**. |
| Niveles de permisos | Define permisos concedidos o denegados para usuarios o grupos en todos los niveles: Leer, Modificar, Crear, Eliminar, Leer ACL, Editar ACL, Replicar. | Concede acceso de lectura a un conjunto de usuarios/grupos. Niega el acceso de lectura a *todos los demás* usuarios/grupos. |
| Publicación | Las ACL *no* se publican con contenido. | Las directivas de CUG *se* publican con contenido. |

## Vínculos de soporte {#supporting-links}

* [Administración de recursos y grupos de usuarios cerrados](https://experienceleague.adobe.com/docs/experience-manager-65/assets/managing/manage-assets.html?lang=en#closed-user-group)
* [Creación de un grupo de usuarios cerrado](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/cug.html)
* [Documentación Del Grupo De Usuarios Cerrado Oak](https://jackrabbit.apache.org/oak/docs/security/authorization/cug.html)
