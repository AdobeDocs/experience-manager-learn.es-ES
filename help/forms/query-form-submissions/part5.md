---
title: Implementar el ejemplo en el servidor local
description: Tutorial de varias partes para guiarle por los pasos necesarios para consultar los envíos de formularios almacenados en Azure Portal
feature: Adaptive Forms
doc-type: Tutorial
version: Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-14884
last-substantial-update: 2024-03-03T00:00:00Z
exl-id: 44841a3c-85e0-447f-85e2-169a451d9c68
duration: 20
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 0%

---

# Implementar el ejemplo en el servidor local

Para que este caso de uso funcione en el servidor local, siga los pasos que se indican a continuación. Se supone que la instancia de AEM se está ejecutando en localhost, puerto 4502.

* [Instale el paquete](assets/azuredemo.all-1.0.0-SNAPSHOT.zip) mediante el administrador de paquetes.

* Proporcione las credenciales del portal de Azure mediante OSGi configMgr
  ![azure-portal](assets/azure-portal-config.png)
Asegúrese de que el URI de almacenamiento termina en una barra diagonal y que el token SAS comienza con un símbolo ?
* Vaya a [AzureDemo](http://localhost:4502/libs/fd/fdm/gui/components/admin/fdmcloudservice/fdm.html/conf/azuredemo)

* Edite la configuración de autenticación de los tres orígenes de datos siguientes para que coincida con su entorno
  ![fuentes de datos](assets/fdm-data-sources.png)

* Previsualizar y enviar [formulario de contacto](http://localhost:4502/content/dam/formsanddocuments/azureportal/contactus/jcr:content?wcmmode=disabled)

* [Consulte el envío del formulario](http://localhost:4502/content/dam/formsanddocuments/azureportal/queryformsubmissions/jcr:content?wcmmode=disabled)
