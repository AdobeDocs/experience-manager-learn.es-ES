---
title: Implementar los recursos de ejemplo en el servidor
description: Probar la funcionalidad de guardar como borrador para Interactive Communications
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: Development
role: Developer
level: Intermediate
kt: 10208
exl-id: 9053ee29-436a-439a-b592-c3fef9852ea4
source-git-commit: db99787c48e49a9861de893e6cb7fbb7b31807b8
workflow-type: tm+mt
source-wordcount: '165'
ht-degree: 2%

---

# Implementar los recursos de ejemplo en el servidor

Siga las siguientes instrucciones para que esta funcionalidad funcione en su servidor AEM

* [Crear el esquema de la base de datos](assets/icdrafts.sql)
* [Importar la biblioteca del cliente](assets/icdrafts.zip)
* [Importar el formulario adaptable](assets/SavedDraftsAdaptiveForm.zip)
* Cree una fuente de datos llamada _SaveAndContinue_

![Crear fuente de datos](assets/data-source.png)

| Nombre de propiedad | Valor de propiedad |
|---|---|
| Nombre del origen de datos | SaveAndContinue |
| Clase de controlador JDBC | com.mysql.cj.jdbc.Driver |
| URL de conexión JDBC | jdbc:mysql://localhost:3306/aemformstutorial?autoReconnect=true&amp;useSSL=false&amp;characterEncoding=utf8&amp;useUnicode=true |

* [Implementar el paquete icrecluts](assets/icdrafts.icdrafts.core-1.0-SNAPSHOT.jar)
* Asegúrese de que _Habilitar Guardar usando CCRDocumentInstanceService_ en la configuración OSGI como se muestra a continuación
   ![Habilitar borradores](assets/enable-drafts.png)
* Abra cualquier comunicación interactiva. Haga clic en Guardar como borrador para guardar
* [Ver borradores guardados](http://localhost:4502/content/dam/formsanddocuments/saveddrafts/jcr:content?wcmmode=disabled)

>[!NOTE]
>Los archivos xml se almacenan en la carpeta raíz de la instalación del servidor AEM. El proyecto de eclipse >se proporciona para personalizar la solución según sus necesidades.

El proyecto eclipse con implementación de muestra puede ser [descargado desde aquí](assets/icdrafts-eclipse-project.zip)
