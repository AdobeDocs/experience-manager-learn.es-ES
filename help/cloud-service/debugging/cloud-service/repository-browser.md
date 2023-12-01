---
title: AEM Depuración de la con el Explorador de repositorios
description: AEM AEM El Explorador de repositorios es una potente herramienta que proporciona visibilidad sobre el almacén de datos subyacente, lo que permite depurar fácilmente el entorno as a Cloud Service de la.
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
jira: KT-10004
thumbnail: 341464.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 88af40fc-deff-4b92-84b1-88df2dbdd90b
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 0%

---

# AEM Depuración as a Cloud Service con el Explorador de repositorios

AEM AEM El Explorador de repositorios es una potente herramienta que proporciona visibilidad sobre el almacén de datos subyacente, lo que permite depurar fácilmente el entorno as a Cloud Service de la. AEM El Explorador de repositorios admite una vista de sólo lectura de los recursos y las propiedades de los recursos en producción, fase y desarrollo, así como los servicios de creación, publicación y vista previa.

>[!VIDEO](https://video.tv.adobe.com/v/341464?quality=12&learn=on)

El Explorador del repositorio es __SOLO__ AEM disponible en entornos as a Cloud Service de la (use) [CRXDE Lite](../aem-sdk-local-quickstart/other-tools.md#crxde-lite) AEM para depurar el SDK local de la.

## Acceder al Explorador de repositorios

AEM Para acceder al Explorador de repositorios en las as a Cloud Service:

1. Asegúrese de que el usuario tenga [el acceso requerido](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html#access-prerequisites)
1. Iniciar sesión en [Cloud Manager](https://my.cloudmanager.adobe.com)
1. AEM Seleccione el programa que contiene el entorno as a Cloud Service de la que depurar
1. Abra el [Developer Console](./developer-console.md) AEM correspondiente al entorno as a Cloud Service de la que se va a depurar
1. Seleccione el __Explorador del repositorio__ pestaña
1. AEM Seleccione el nivel de servicio de la que desea examinar
   + Todos los autores
   + Todos los editores
   + Todas las previsualizaciones
1. Seleccionar __Abrir el Explorador del repositorio__

El Explorador de repositorios se abre para el nivel de servicio seleccionado (autor, publicación o vista previa) en modo de solo lectura, mostrando los recursos y las propiedades a los que el usuario tiene acceso.

## Acceso de publicación y previsualización

De forma predeterminada, el acceso a Publicación o Vista previa es limitado, lo que reduce los recursos disponibles en el Explorador de repositorios. [Para ver todos los recursos en Publicar (o Vista previa), agregue usuarios a una función de Administradores de publicación (o Vista previa).](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html#navigate-the-hierarchy)
