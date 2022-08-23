---
title: Grupos de usuarios cerrados en AEM Assets
description: Grupos de usuarios cerrados (CUG) es una función que se utiliza para restringir el acceso al contenido a un grupo selecto de usuarios en un sitio publicado. Este vídeo muestra cómo se pueden utilizar los grupos de usuarios cerrados con Adobe Experience Manager Assets para restringir el acceso a una carpeta específica de recursos.
version: 6.4, 6.5, Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Intermediate
kt: 649
thumbnail: 22155.jpg
exl-id: a2bf8a82-15ee-478c-b7c3-de8a991dfeb8
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '377'
ht-degree: 0%

---

# Grupos de usuarios cerrados{#using-closed-user-groups-with-aem-assets}

Grupos de usuarios cerrados (CUG) es una función que se utiliza para restringir el acceso al contenido a un grupo selecto de usuarios en un sitio publicado. Este vídeo muestra cómo se pueden utilizar los grupos de usuarios cerrados con Adobe Experience Manager Assets para restringir el acceso a una carpeta específica de recursos. La compatibilidad con los grupos de usuarios cerrados con AEM Assets se introdujo por primera vez en la AEM 6.4.

>[!VIDEO](https://video.tv.adobe.com/v/22155?quality=12&learn=on)

## Grupo de usuarios cerrado (CUG) con AEM Assets

* Diseñado para restringir el acceso a los recursos en una instancia de AEM Publish.
* Otorga acceso de lectura a un conjunto de usuarios/grupos.
* CUG solo se puede configurar a nivel de carpeta. CUG no se puede establecer en recursos individuales.
* Las directivas CUG se heredan automáticamente por cualquier subcarpeta y recurso aplicado.
* Las subcarpetas pueden sobrescribir las políticas de CUG configurando una nueva directiva de CUG. Esto debe utilizarse con moderación y no se considera una práctica recomendada.

## Grupos de usuarios cerrados frente a listas de control de acceso {#closed-user-groups-vs-access-control-lists}

Los grupos de usuarios cerrados (CUG) y las listas de control de acceso (ACL) se utilizan para controlar el acceso al contenido en AEM y en función de los usuarios y grupos de seguridad de AEM. Sin embargo, la aplicación e implementación de estas funciones es muy diferente. La siguiente tabla resume las distinciones entre las dos características.

|  | ACL | CUG |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| Uso previsto | Configure y aplique permisos para contenido en la variable **current** AEM instancia. | Configurar directivas de CUG para contenido en AEM **author** instancia. Aplicar directivas CUG para contenido en AEM **publicar** instancias. |
| Niveles de permisos | Define los permisos concedidos/denegados para usuarios/grupos en todos los niveles: Leer, Modificar, Crear, Eliminar, Leer ACL, Editar ACL, Replicar. | Otorga acceso de lectura a un conjunto de usuarios/grupos. Deniega el acceso de lectura a *todos los demás* usuarios/grupos. |
| Publicación | Las ACL son *not* publicado con contenido. | Políticas de CUG *are* publicado con contenido. |

## Compatibilidad con vínculos {#supporting-links}

* [Administración de recursos y grupos de usuarios cerrados](https://experienceleague.adobe.com/docs/experience-manager-65/assets/managing/manage-assets.html?lang=en#closed-user-group)
* [Creación de un grupo de usuarios cerrado](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/cug.html)
* [Documentación del grupo de usuarios cerrado de Oak](https://jackrabbit.apache.org/oak/docs/security/authorization/cug.html)
