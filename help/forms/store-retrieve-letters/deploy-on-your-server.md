---
title: Implementar los recursos de ejemplo en el servidor
description: Prueba de la funcionalidad Guardar como borrador para comunicaciones interactivas
feature: Interactive Communication
doc-type: article
version: Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
jira: KT-10208
exl-id: 9053ee29-436a-439a-b592-c3fef9852ea4
duration: 28
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '143'
ht-degree: 4%

---

# Implementar los recursos de ejemplo en el servidor

Siga las instrucciones siguientes para que esta funcionalidad funcione en su servidor de AEM

* [Creación del esquema de base de datos](assets/icdrafts.sql)
* [Importar la biblioteca de cliente](assets/icdrafts.zip)
* [Importar el formulario adaptable](assets/SavedDraftsAdaptiveForm.zip)
* Crear origen de datos llamado _SaveAndContinue_

![Crear Source de datos](assets/data-source.png)

| Nombre de la propiedad | Valor de propiedad |
|---|---|
| Nombre de Datasource | `SaveAndContinue` |
| Clase de controlador JDBC | `com.mysql.cj.jdbc.Driver` |
| URL de conexión JDBC | `jdbc:mysql://localhost:3306/aemformstutorial?autoReconnect=true&useSSL=false&characterEncoding=utf8&useUnicode=true` |

* [Implementar el paquete icdrafts](assets/icdrafts.icdrafts.core-1.0-SNAPSHOT.jar)
* Asegúrese de _Habilitar Guardar usando CCRDocumentInstanceService_ en la configuración de OSGI como se muestra a continuación
  ![Habilitar borradores](assets/enable-drafts.png)
* Abra cualquier comunicación interactiva. Haga clic en Guardar como borrador para guardar
* [Ver borradores guardados](http://localhost:4502/content/dam/formsanddocuments/saveddrafts/jcr:content?wcmmode=disabled)

>[!NOTE]
>Los archivos xml se almacenan en la carpeta raíz de la instalación del servidor de AEM. El proyecto Eclipse > se proporciona para personalizar la solución según sus necesidades.

El proyecto Eclipse con implementación de ejemplo se puede [descargar desde aquí](assets/icdrafts-eclipse-project.zip)
