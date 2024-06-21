---
title: Creando componente de dirección
description: Creación de un nuevo componente principal de dirección en AEM Forms Cloud Service
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-15752
source-git-commit: a8fc8fa19ae19e27b07fa81fc931eca51cb982a1
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 0%

---

# Implemente su proyecto

Antes de empezar a implementar el proyecto en el Cloud Service de AEM Forms, se recomienda implementar el proyecto en la instancia local de AEM Forms lista para la nube.

## AEM Sincronización de cambios con el proyecto de

Inicie IntelliJ y navegue hasta la carpeta de formulario adaptable en ``ui.apps`` como se muestra a continuación
![intellij](assets/intellij.png)

Clic derecho en ``adaptiveForm`` y seleccione Nuevo | Paquete Asegúrese de añadir el nombre **bloque de direcciones** al paquete

Haga clic con el botón derecho en el paquete recién creado ``addressblock`` y seleccione ``repo | Get Command`` como se muestra a continuación
![repo-sync](assets/sync-repo.png)

Esto debe sincronizar el proyecto con la instancia de AEM Forms local lista para la nube. Puede comprobar el archivo .content.xml para confirmar las propiedades
![después de sincronizar](assets/after-sync.png)

## Implementar el proyecto en la instancia local

Inicie una nueva ventana del símbolo del sistema, vaya a la carpeta raíz del proyecto y genere el proyecto con el comando que se muestra a continuación
![implementar](assets/build-project.png)

Una vez que el proyecto se haya implementado correctamente, el componente Dirección ahora se puede utilizar en un formulario adaptable

## Implemente el proyecto en el entorno de la nube

Si todo parece correcto en su entorno de desarrollo local, el siguiente paso es implementar en el [instancia de nube con cloud manager.](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/push-project-to-cloud-manager-git)



