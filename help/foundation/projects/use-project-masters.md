---
title: Cómo utilizar Project Masters en AEM
description: Project Masters simplifica considerablemente la administración de usuarios y equipos con AEM proyectos.
version: 6.4, 6.5, cloud-service
topic: Administración de contenido
feature: Proyectos
level: Intermedio
role: Profesional del negocio
kt: 256
thumbnail: 17740.jpg
translation-type: tm+mt
source-git-commit: 2d38baad8e8351d9debbef758050b9b63d860fe9
workflow-type: tm+mt
source-wordcount: '374'
ht-degree: 1%

---


# Usar maestros de proyecto

Project Masters simplifica considerablemente la administración de usuarios y equipos con [!DNL AEM Projects].

>[!VIDEO](https://video.tv.adobe.com/v/17740/?quality=12&learn=on)

Los administradores ahora pueden crear un **[!DNL Master Project]** y asignar usuarios a funciones/permisos como parte de un equipo de proyecto. Los proyectos se pueden crear a partir de un proyecto maestro y heredar automáticamente la pertenencia al equipo. Esto oferta varias ventajas:

* Reutilización de equipos existentes en varios proyectos
* Acelera la creación de proyectos, ya que no es necesario volver a crear los equipos manualmente
* Administrar la pertenencia a equipos desde una ubicación central y los proyectos heredan automáticamente cualquier actualización de los equipos
* evita la creación de ACL de duplicado que pueden causar problemas de rendimiento

[!DNL Master Projects] puede crearse en la carpeta   Mastersen  [!UICONTROL AEM proyectos]. Una vez creado un proyecto maestro, se muestra como una opción junto con las plantillas disponibles en el asistente cuando se crean nuevos proyectos.

[!DNL Project Masters] URL (instancia local de AEM Author):  [http://localhost:4502/projects.html/content/projects/masters](http://localhost:4502/projects.html/content/projects/masters)

## Eliminar [!DNL Project Masters]

Al eliminar un proyecto maestro, los proyectos derivados no se pueden utilizar.

Antes de eliminar un proyecto maestro, asegúrese de que todos los proyectos derivados se han terminado y eliminado de AEM. Asegúrese de guardar los datos del proyecto necesarios antes de eliminar los proyectos derivados. Una vez que se eliminan todos los proyectos derivados de AEM, el proyecto maestro puede eliminarse con seguridad.

## Marcar [!DNL Project Masters] como Inactivo

Al cambiar el estado del proyecto maestro a inactivo en las propiedades del proyecto, los proyectos maestros inactivos desaparecen de la lista de proyectos maestros.

Para mostrar los proyectos principales inactivos, active el botón de filtro &quot;mostrar activo&quot; en la barra superior (al lado del conmutador de visualización de listas). Para volver a activar el proyecto inactivo, simplemente seleccione el proyecto maestro inactivo, edite las propiedades del proyecto y configúrelo nuevamente para que esté activo.

## Comprender [!DNL Project Masters]

![Vista técnica de los jefes de proyecto](assets/use-project-masters/project-masters-architecture.png)

[!DNL Project Masters] trabajar definiendo un conjunto de grupos de usuarios AEM (propietarios, editor y observador) y permitiendo que los proyectos derivados hagan referencia y reutilicen esos grupos de usuarios definidos centralmente.

Esto reduce el número total de grupos de usuarios necesarios en AEM. Antes de [!DNL Project Masters], cada proyecto creaba 3 grupos de usuarios con las ACE correspondientes para aplicar permisos, de modo que 100 proyectos producían 300 grupos de usuarios. Project Masters permite que cualquier número de proyectos reutilice los mismos tres grupos, suponiendo que la pertenencia compartida se ajusta a los requisitos comerciales de todo el proyecto.
