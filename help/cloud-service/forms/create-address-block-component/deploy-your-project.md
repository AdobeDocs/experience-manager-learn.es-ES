---
title: Creando componente de dirección
description: Creación de un nuevo componente principal de dirección en AEM Forms as a Cloud Service
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-15752
exl-id: be25be52-2914-4820-9356-678a326f8edc
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '200'
ht-degree: 0%

---

# Implemente su proyecto

Antes de empezar a implementar el proyecto en AEM Forms as a Cloud Service, se recomienda implementar el proyecto en la instancia local de AEM Forms lista para la nube.

## Sincronizar cambios con el proyecto de AEM

Inicie IntelliJ y navegue hasta la carpeta adaptiveForm bajo la carpeta ``ui.apps``, como se muestra a continuación
![intellij](assets/intellij.png)

Haga clic con el botón derecho en el nodo ``adaptiveForm`` y seleccione Nuevo | Paquete
Asegúrese de agregar el nombre **bloque de direcciones** al paquete

Haga clic con el botón derecho en el paquete ``addressblock`` recién creado y seleccione ``repo | Get Command`` como se muestra a continuación
![repo-sync](assets/sync-repo.png)

Esto debe sincronizar el proyecto con la instancia de AEM Forms local lista para la nube. Puede comprobar el archivo .content.xml para confirmar las propiedades
![después de la sincronización](assets/after-sync.png)

## Implementar el proyecto en la instancia local

Inicie una nueva ventana del símbolo del sistema, vaya a la carpeta raíz del proyecto y genere el proyecto con el comando que se muestra a continuación
![implementar](assets/build-project.png)

Una vez que el proyecto se haya implementado correctamente, el
Ahora, el componente Dirección se puede utilizar en un formulario adaptable

## Implemente el proyecto en el entorno de la nube

Si todo parece correcto en su entorno de desarrollo local, el siguiente paso es implementar en la [instancia de nube mediante Cloud Manager.](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/push-project-to-cloud-manager-git)
