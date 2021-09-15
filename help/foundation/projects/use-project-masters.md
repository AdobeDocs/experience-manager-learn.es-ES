---
title: Cómo usar Project Masters en AEM
description: Project Masters simplifica enormemente la administración de usuarios y equipos con AEM Proyectos.
version: 6.4, 6.5, Cloud Service
topic: Content Management, Collaboration
feature: Projects
level: Intermediate
role: User
kt: 256
thumbnail: 17740.jpg
exl-id: 78ff62ad-1017-4a02-80e9-81228f9e01eb
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '368'
ht-degree: 0%

---

# Usar Project Masters

Project Masters simplifica enormemente la administración de usuarios y equipos con [!DNL AEM Projects].

>[!VIDEO](https://video.tv.adobe.com/v/17740/?quality=12&learn=on)

Los administradores ahora pueden crear un **[!DNL Master Project]** y asignar usuarios a roles/permisos como parte de un equipo de proyecto. Los proyectos se pueden crear a partir de un proyecto maestro y heredar automáticamente la pertenencia al equipo. Esto ofrece varias ventajas:

* Reutilizar equipos existentes en varios proyectos
* Acelera la creación de proyectos, ya que no es necesario volver a crearlos manualmente
* Administrar la pertenencia al equipo desde una ubicación central y los proyectos heredan automáticamente cualquier actualización de los equipos
* evita la creación de ACL duplicados que pueden causar problemas de rendimiento

[!DNL Master Projects] se puede crear en la carpeta   maestra en  [!UICONTROL AEM proyectos]. Una vez creado un proyecto maestro, se muestra como una opción junto con las plantillas disponibles en el asistente cuando se crean nuevos proyectos.

[!DNL Project Masters] URL (instancia local de AEM Author):  [http://localhost:4502/projects.html/content/projects/masters](http://localhost:4502/projects.html/content/projects/masters)

## Eliminar [!DNL Project Masters]

La eliminación de un proyecto maestro da como resultado proyectos derivados que no se pueden utilizar.

Antes de eliminar un proyecto maestro, asegúrese de que todos los proyectos derivados hayan finalizado y se eliminen de AEM. Asegúrese de guardar los datos de proyecto necesarios antes de eliminar los proyectos derivados. Una vez que todos los proyectos derivados se eliminan de AEM, el proyecto maestro se puede eliminar de forma segura.

## Marcar [!DNL Project Masters] como Inactivo

Al cambiar el estado del proyecto maestro a inactivo en las propiedades del proyecto, los proyectos maestros inactivos desaparecen de la lista de proyectos maestros.

Para mostrar proyectos maestros inactivos, active el botón de filtro &quot;mostrar activo&quot; en la barra superior (junto al botón de visualización de la lista). Para volver a activar el proyecto inactivo, simplemente seleccione el proyecto maestro inactivo, edite las propiedades del proyecto y configúrelo una vez más para que esté activo.

## Comprender [!DNL Project Masters]

![Vista técnica de Project Master](assets/use-project-masters/project-masters-architecture.png)

[!DNL Project Masters] trabajar definiendo un conjunto de grupos de usuarios AEM (propietarios, editor y observador) y permitiendo que los proyectos derivados hagan referencia a esos grupos de usuarios definidos de forma centralizada y los reutilicen.

Esto reduce el número total de grupos de usuarios requeridos en AEM. Antes de [!DNL Project Masters], cada proyecto creaba 3 grupos de usuarios con los ACE correspondientes para hacer cumplir el permiso, de modo que 100 proyectos producían 300 grupos de usuarios. Project Masters permite que cualquier número de proyectos reutilice los mismos tres grupos, suponiendo que la pertenencia compartida se ajuste a los requisitos de Business en todo el proyecto.
