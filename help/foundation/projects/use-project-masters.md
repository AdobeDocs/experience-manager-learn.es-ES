---
title: AEM Cómo usar Project Masters en el área de la
description: AEM Project Masters simplifica enormemente la administración de usuarios y equipos con Proyectos de la.
version: 6.4, 6.5, Cloud Service
topic: Content Management, Collaboration
feature: Projects
level: Intermediate
role: User
jira: KT-256
thumbnail: 17740.jpg
doc-type: Feature Video
exl-id: 78ff62ad-1017-4a02-80e9-81228f9e01eb
duration: 279
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '361'
ht-degree: 0%

---

# Utilizar proyectos principales

Project Masters simplifica considerablemente la administración de usuarios y equipos con [!DNL AEM Projects].

>[!VIDEO](https://video.tv.adobe.com/v/17740?quality=12&learn=on)

Los administradores ahora pueden crear un **[!DNL Master Project]** y asignar usuarios a funciones/permisos como parte de un equipo del proyecto. Los proyectos se pueden crear a partir de un proyecto maestro y heredar automáticamente la pertenencia al equipo. Esto ofrece varias ventajas:

* Reutilizar equipos existentes en varios proyectos
* Acelera la creación de proyectos, ya que no es necesario volver a crear los equipos manualmente
* Administrar la pertenencia al equipo desde una ubicación central y cualquier actualización a los equipos la heredan automáticamente los proyectos
* evita la creación de ACL duplicados que pueden causar problemas de rendimiento

[!DNL Master Projects] se puede crear en [!UICONTROL Maestros] carpeta bajo [!UICONTROL AEM Proyectos de]. Una vez creado un proyecto maestro, se muestra como una opción junto con las plantillas disponibles en el asistente cuando se crean nuevos proyectos.

[!DNL Project Masters] AEM URL (instancia local de autor de la): [http://localhost:4502/projects.html/content/projects/masters](http://localhost:4502/projects.html/content/projects/masters)

## Eliminar [!DNL Project Masters]

Al eliminar un proyecto principal, se producen proyectos derivados que no se pueden utilizar.

AEM Antes de eliminar un proyecto principal, asegúrese de que todos los proyectos derivados han finalizado y se han eliminado de la lista de proyectos de la. Asegúrese de guardar los datos de proyecto necesarios antes de quitar los proyectos derivados. AEM Una vez que todos los proyectos derivados se han eliminado de la, el proyecto principal se puede eliminar de forma segura.

## Marcar [!DNL Project Masters] como inactivo

Al cambiar el estado del proyecto principal a inactivo en las propiedades del proyecto, los proyectos principales inactivos desaparecen de la lista de proyectos principales.

Para mostrar proyectos principales inactivos, active el botón de filtro &quot;mostrar activo&quot; en la barra superior (junto al botón de alternancia de visualización de lista). Para que el proyecto inactivo vuelva a estar activo, simplemente seleccione el proyecto principal inactivo, edite las propiedades del proyecto y vuelva a establecerlo como activo.

## Comprender [!DNL Project Masters]

![Vista técnica de maestros de proyectos](assets/use-project-masters/project-masters-architecture.png)

[!DNL Project Masters] AEM Trabaje definiendo un conjunto de grupos de usuarios (propietarios, editor y observador) y permitiendo que los proyectos derivados hagan referencia a esos grupos de usuarios definidos de forma centralizada y los reutilicen.

AEM Esto reduce el número total de grupos de usuarios necesarios en los grupos de usuarios que se encuentran en la zona de trabajo de la. Antes [!DNL Project Masters]Además, cada proyecto creó 3 grupos de usuarios con los ACE adjuntos para aplicar permisos, de modo que 100 proyectos dieron como resultado 300 grupos de usuarios. Project Masters permite que cualquier número de proyectos reutilice los mismos tres grupos, suponiendo que la pertenencia compartida se ajuste a los requisitos comerciales en todo el proyecto.
