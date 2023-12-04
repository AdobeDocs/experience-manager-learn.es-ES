---
title: Implementar los recursos de ejemplo en el servidor
description: Prueba de la funcionalidad Guardar como borrador para comunicaciones interactivas
feature: Interactive Communication
doc-type: article
version: 6.5
topic: Development
role: Developer
level: Intermediate
jira: KT-10208
exl-id: 9053ee29-436a-439a-b592-c3fef9852ea4
duration: 46
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '153'
ht-degree: 4%

---

# Implementar los recursos de ejemplo en el servidor

AEM Siga las instrucciones siguientes para que esta funcionalidad funcione en su servidor de

* [Creación del esquema de base de datos](assets/icdrafts.sql)
* [Importar la biblioteca de cliente](assets/icdrafts.zip)
* [Importar el formulario adaptable](assets/SavedDraftsAdaptiveForm.zip)
* Crear fuente de datos llamada _Guardar y continuar_

![Crear fuente de datos](assets/data-source.png)

| Nombre de la propiedad | Valor de propiedad |
|---|---|
| Nombre de Datasource | Guardar y continuar |
| Clase de controlador JDBC | com.mysql.cj.jdbc.Driver |
| URL de conexión JDBC | jdbc:mysql://localhost:3306/aemformstutorial?autoReconnect=true&amp;useSSL=false&amp;characterEncoding=utf8&amp;useUnicode=true |

* [Implementar el paquete icdrafts](assets/icdrafts.icdrafts.core-1.0-SNAPSHOT.jar)
* Asegúrese de que _Habilitar Guardar usando CCRDocumentInstanceService_ en la configuración OSGI como se muestra a continuación
  ![Activar borradores](assets/enable-drafts.png)
* Abra cualquier comunicación interactiva. Haga clic en Guardar como borrador para guardar
* [Ver borradores guardados](http://localhost:4502/content/dam/formsanddocuments/saveddrafts/jcr:content?wcmmode=disabled)

>[!NOTE]
>AEM Los archivos xml se almacenan en la carpeta raíz de la instalación del servidor de la instalación del servidor de la aplicación de. El proyecto Eclipse > se proporciona para personalizar la solución según sus necesidades.

El proyecto Eclipse con implementación de muestra puede ser [descargado desde aquí](assets/icdrafts-eclipse-project.zip)
