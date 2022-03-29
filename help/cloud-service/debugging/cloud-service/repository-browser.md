---
title: Depuración AEM con el explorador del repositorio
description: El Explorador de repositorios es una potente herramienta que proporciona visibilidad en AEM almacén de datos subyacente, lo que permite depurar fácilmente AEM entorno as a Cloud Service.
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 10004
thumbnail: 341464.jpg
topic: Development
role: Developer
level: Beginner
source-git-commit: 467b0c343a28eb573498a013b5490877e4497fe0
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 0%

---


# Depuración AEM as a Cloud Service con el explorador del repositorio

El Explorador de repositorios es una potente herramienta que proporciona visibilidad en AEM almacén de datos subyacente, lo que permite depurar fácilmente AEM entorno as a Cloud Service. El explorador de repositorios admite una vista de solo lectura de los recursos y las propiedades de AEM en producción, fase y desarrollo, así como los servicios de autor, publicación y vista previa.

>[!VIDEO](https://video.tv.adobe.com/v/341464/?quality=12&learn=on)

El explorador del repositorio es __SOLO__ disponible en entornos as a Cloud Service AEM (utilice [CRXDE Lite](../aem-sdk-local-quickstart/other-tools.md#crxde-lite) para depurar el SDK de AEM local).

## Acceso al explorador del repositorio

Para acceder al Explorador de repositorios en AEM as a Cloud Service:

1. Asegúrese de que el usuario tenga [el acceso requerido](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html#access-prerequisites)
1. Iniciar sesión en [Cloud Manager](https://my.cloudmanager.adobe.com)
1. Seleccione el Programa que contiene el entorno as a Cloud Service AEM que desea depurar
1. Abra el [Developer Console](./developer-console.md) correspondiente al entorno as a Cloud Service AEM que se va a depurar
1. Seleccione el __Explorador del repositorio__ ficha
1. Seleccione el nivel de servicio de AEM para examinar
   + Todos los autores
   + Todos los editores
   + Todas las vistas previas
1. Select __Abrir explorador de repositorios__

El navegador de repositorios se abre para el nivel de servicio seleccionado (Autor, Publicación o Vista previa) en modo de solo lectura, mostrando los recursos y las propiedades a las que el usuario tiene acceso.

## Acceso a la publicación y a la vista previa

De forma predeterminada, el acceso a Publicar o Vista previa es limitado, lo que reduce los recursos disponibles en el Explorador de repositorios. [Para ver todos los recursos en Publicar (o Vista previa), agregue usuarios a una función de Administrador de publicación (o vista previa).](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html#navigate-the-hierarchy)

