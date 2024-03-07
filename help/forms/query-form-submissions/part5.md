---
title: Implementar el ejemplo en el servidor local
description: Tutorial de varias partes para guiarle por los pasos necesarios para consultar los envíos de formularios almacenados en Azure Portal
feature: Adaptive Forms
doc-type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-14884
last-substantial-update: 2024-03-03T00:00:00Z
source-git-commit: ae2a2cbde1bf21314cc77863014cb0f013b6e0bb
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 0%

---

# Implementar el ejemplo en el servidor local

AEM Para que este caso de uso funcione en el servidor local, siga los pasos que se indican a continuación. Se supone que la instancia de la se está ejecutando en localhost, puerto 4502.

* [Instalación del paquete](assets/azuredemo.all-1.0.0-SNAPSHOT.zip) uso del administrador de paquetes.

* Proporcione las credenciales del portal de Azure mediante OSGi configMgr
  ![azure-portal](assets/azure-portal-config.png)
Asegúrese de que el URI de almacenamiento termina en una barra diagonal y que el token SAS comienza con un símbolo ?
* Vaya a [AzureDemo](http://localhost:4502/libs/fd/fdm/gui/components/admin/fdmcloudservice/fdm.html/conf/azuredemo)

* Edite la configuración de autenticación de los tres orígenes de datos siguientes para que coincida con su entorno
  ![data-sources](assets/fdm-data-sources.png)

* Previsualización y envío [Formulario de ContactUs](http://localhost:4502/content/dam/formsanddocuments/azureportal/contactus/jcr:content?wcmmode=disabled)

* [Consulte el envío del formulario](http://localhost:4502/content/dam/formsanddocuments/azureportal/queryformsubmissions/jcr:content?wcmmode=disabled)

