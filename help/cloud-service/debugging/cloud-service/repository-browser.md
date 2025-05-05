---
title: Depuración de AEM con el Explorador de repositorios
description: El Explorador de repositorios es una potente herramienta que proporciona visibilidad sobre el almacén de datos subyacente de AEM, lo que permite depurar fácilmente el entorno de AEM as a Cloud Service.
feature: Developer Tools
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-10004
thumbnail: 341464.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 88af40fc-deff-4b92-84b1-88df2dbdd90b
duration: 305
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '253'
ht-degree: 0%

---

# Depuración de AEM as a Cloud Service con el Explorador de repositorios

El Explorador de repositorios es una potente herramienta que proporciona visibilidad sobre el almacén de datos subyacente de AEM, lo que permite depurar fácilmente el entorno de AEM as a Cloud Service. El Explorador de repositorios admite una vista de solo lectura de los recursos y las propiedades de AEM en Producción, Ensayo y Desarrollo, así como los servicios de autor, publicación y vista previa.

>[!VIDEO](https://video.tv.adobe.com/v/3447057?quality=12&learn=on&captions=spa)

El Explorador de repositorios está __SOLAMENTE__ disponible en entornos AEM as a Cloud Service (use [CRXDE Lite](../aem-sdk-local-quickstart/other-tools.md#crxde-lite) para depurar el SDK local de AEM).

## Acceder al Explorador de repositorios

Para acceder al Explorador de repositorios en AEM as a Cloud Service:

1. Asegúrese de que el usuario tenga [el acceso necesario](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html?lang=es#access-prerequisites)
1. Iniciar sesión en [Cloud Manager](https://my.cloudmanager.adobe.com)
1. Seleccione el programa que contiene el entorno de AEM as a Cloud Service que desea depurar
1. Abra [Developer Console](./developer-console.md) correspondiente al entorno de AEM as a Cloud Service para depurar
1. Seleccione la ficha __Explorador del repositorio__
1. Seleccione el nivel de servicio de AEM que desee examinar
   + Todos los autores
   + Todos los editores
   + Todas las previsualizaciones
1. Seleccione __Abrir el explorador de repositorios__

El Explorador de repositorios se abre para el nivel de servicio seleccionado (autor, publicación o vista previa) en modo de solo lectura, mostrando los recursos y las propiedades a los que el usuario tiene acceso.

## Acceso de publicación y previsualización

De forma predeterminada, el acceso a Publicación o Vista previa es limitado, lo que reduce los recursos disponibles en el Explorador de repositorios. [Para ver todos los recursos en Publicar (o Vista previa), agregue usuarios a un rol de Administradores de Publicación (o Vista previa).](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html?lang=es#navigate-the-hierarchy)
