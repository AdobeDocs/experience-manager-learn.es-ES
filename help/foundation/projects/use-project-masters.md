---
title: Cómo utilizar Project Masters en AEM
description: Project Masters simplifica considerablemente la administración de usuarios y equipos con AEM Projects.
version: Experience Manager 6.4, Experience Manager 6.5, Experience Manager as a Cloud Service
topic: Content Management, Collaboration
feature: Projects
level: Intermediate
role: User
jira: KT-256
thumbnail: 17740.jpg
doc-type: Feature Video
exl-id: 78ff62ad-1017-4a02-80e9-81228f9e01eb
duration: 260
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '361'
ht-degree: 0%

---

# Utilizar proyectos principales

Project Masters simplifica considerablemente la administración de usuarios y equipos con [!DNL AEM Projects].

>[!VIDEO](https://video.tv.adobe.com/v/17740?quality=12&learn=on)

Los administradores ahora pueden crear un(a) **[!DNL Master Project]** y asignar usuarios a roles/permisos como parte de un equipo del proyecto. Los proyectos se pueden crear a partir de un proyecto maestro y heredar automáticamente la pertenencia al equipo. Esto ofrece varias ventajas:

* Reutilizar equipos existentes en varios proyectos
* Acelera la creación de proyectos, ya que no es necesario volver a crear los equipos manualmente
* Administrar la pertenencia al equipo desde una ubicación central y cualquier actualización a los equipos la heredan automáticamente los proyectos
* evita la creación de ACL duplicados que pueden causar problemas de rendimiento

[!DNL Master Projects] se puede crear en la carpeta [!UICONTROL Masters] en [!UICONTROL Proyectos AEM]. Una vez creado un proyecto maestro, se muestra como una opción junto con las plantillas disponibles en el asistente cuando se crean nuevos proyectos.

URL [!DNL Project Masters] (instancia local de autor de AEM): [http://localhost:4502/projects.html/content/projects/masters](http://localhost:4502/projects.html/content/projects/masters)

## Eliminar [!DNL Project Masters]

Al eliminar un proyecto principal, se producen proyectos derivados que no se pueden utilizar.

Antes de eliminar un proyecto principal, asegúrese de que todos los proyectos derivados han finalizado y se han eliminado de AEM. Asegúrese de guardar los datos de proyecto necesarios antes de quitar los proyectos derivados. Una vez que todos los proyectos derivados se quitan de AEM, el proyecto principal se puede eliminar de forma segura.

## Marcar [!DNL Project Masters] como inactivo

Al cambiar el estado del proyecto principal a inactivo en las propiedades del proyecto, los proyectos principales inactivos desaparecen de la lista de proyectos principales.

Para mostrar proyectos principales inactivos, active el botón de filtro &quot;mostrar activo&quot; en la barra superior (junto al botón de alternancia de visualización de lista). Para que el proyecto inactivo vuelva a estar activo, simplemente seleccione el proyecto principal inactivo, edite las propiedades del proyecto y vuelva a establecerlo como activo.

## Comprender [!DNL Project Masters]

![Vista técnica de maestros de proyectos](assets/use-project-masters/project-masters-architecture.png)

[!DNL Project Masters] funciona definiendo un conjunto de grupos de usuarios de AEM (propietarios, editor y observador) y permitiendo que los proyectos derivados hagan referencia a esos grupos de usuarios definidos centralmente y los reutilicen.

Esto reduce el número total de grupos de usuarios necesarios en AEM. Antes de [!DNL Project Masters], cada proyecto creaba 3 grupos de usuarios con las ACE correspondientes para aplicar permisos, de modo que 100 proyectos arrojaban 300 grupos de usuarios. Project Masters permite que cualquier número de proyectos reutilice los mismos tres grupos, suponiendo que la pertenencia compartida se ajuste a los requisitos comerciales en todo el proyecto.
