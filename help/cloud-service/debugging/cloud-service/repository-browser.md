---
title: AEM Depuración de la con el Explorador de repositorios
description: AEM El Explorador de repositorios es una potente herramienta que proporciona visibilidad sobre el almacén de datos subyacente de los que se dispone en el repositorio de datos, lo que permite depurar fácilmente el entorno de AEM as a Cloud Service.
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
jira: KT-10004
thumbnail: 341464.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 88af40fc-deff-4b92-84b1-88df2dbdd90b
duration: 305
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '253'
ht-degree: 0%

---

# Depuración de AEM as a Cloud Service con el Explorador de repositorios

AEM El Explorador de repositorios es una potente herramienta que proporciona visibilidad sobre el almacén de datos subyacente de los que se dispone en el repositorio de datos, lo que permite depurar fácilmente el entorno de AEM as a Cloud Service. AEM El Explorador de repositorios admite una vista de sólo lectura de los recursos y las propiedades de los recursos en producción, fase y desarrollo, así como de los servicios de autor, Publish y vista previa.

>[!VIDEO](https://video.tv.adobe.com/v/341464?quality=12&learn=on)

El Explorador de repositorios está __SOLAMENTE__ disponible en entornos AEM as a Cloud Service (use el [CRXDE Lite AEM](../aem-sdk-local-quickstart/other-tools.md#crxde-lite) para depurar el SDK local de la).

## Acceder al Explorador de repositorios

Para acceder al Explorador de repositorios en AEM as a Cloud Service:

1. Asegúrese de que el usuario tenga [el acceso necesario](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html#access-prerequisites)
1. Iniciar sesión en [Cloud Manager](https://my.cloudmanager.adobe.com)
1. Seleccione el programa que contiene el entorno de AEM as a Cloud Service que desea depurar
1. Abra [Developer Console](./developer-console.md) correspondiente al entorno de AEM as a Cloud Service para depurar
1. Seleccione la ficha __Explorador del repositorio__
1. AEM Seleccione el nivel de servicio de la que desea examinar
   + Todos los autores
   + Todos los editores
   + Todas las previsualizaciones
1. Seleccione __Abrir el explorador de repositorios__

El Explorador de repositorios se abre para el nivel de servicio seleccionado (Autor, Publish o Vista previa) en modo de solo lectura, mostrando los recursos y las propiedades a los que el usuario tiene acceso.

## Publish y acceso de previsualización

De forma predeterminada, el acceso a Publish o a la vista previa es limitado, lo que reduce los recursos disponibles en el Explorador de repositorios. [Para ver todos los recursos en Publish (o Vista previa), agregue usuarios a un rol de Administradores de Publish (o Vista previa).](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html#navigate-the-hierarchy)
