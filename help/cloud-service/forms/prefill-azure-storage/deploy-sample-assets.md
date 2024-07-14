---
title: Implementar los recursos de muestra
description: Implemente los recursos de muestra en el sistema local preparado para la nube.
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-13717
exl-id: ae8104fa-7af2-49c2-9e6b-704152d49149
duration: 38
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '188'
ht-degree: 0%

---

# Implementar los recursos de muestra

Para que este caso de uso funcione en su sistema, implemente los siguientes recursos en su sistema local preparado para la nube.

* Asegúrese de haber creado todas las configuraciones o cuentas necesarias que se enumeran en el[documento de introducción](./introduction.md)

* [Instalar la plantilla de formulario adaptable personalizada y el componente de página asociado](./assets/azure-portal-template-page-component.zip)

* [Instalar el modelo de datos de formulario SendGrid](./assets/send-grid-form-data-model.zip). Deberá proporcionar la clave de API y la dirección remitente verificada de SendGrid para que este modelo de datos de formulario funcione. Probar el modelo de datos de formulario en el editor del modelo de datos de formulario

* [Instale el modelo de datos de formulario respaldado por Azure](./assets/azure-storage-fdm.zip). Debe proporcionar las credenciales de la cuenta de almacenamiento de Azure para que funcione el modelo de datos de formulario. Pruebe el modelo de datos de formulario en el editor del modelo de datos de formulario.

* [Importar el formulario adaptable de ejemplo](./assets/credit-applications-af.zip)
* [Importar la biblioteca de cliente](./assets/client-lib.zip)
* [Vista previa del formulario](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/creditapplications/jcr:content?wcmmode=disabled). Introduzca un correo electrónico válido y haga clic en el botón Guardar. Los datos del formulario deben almacenarse en Azure Storage y se enviará un correo electrónico con un vínculo al formulario guardado a la dirección de correo electrónico especificada.
