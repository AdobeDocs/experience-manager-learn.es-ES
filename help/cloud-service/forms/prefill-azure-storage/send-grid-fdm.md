---
title: Enviar correo electrónico mediante SendGrid
description: Almacenar en déclencheur un mensaje de correo electrónico con un vínculo al formulario guardado
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-13717
exl-id: 4b2d1e50-9fa1-4934-820b-7dae984cee00
duration: 37
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '183'
ht-degree: 1%

---

# Integración con SendGrid

La integración de datos de AEM Forms permite configurar y conectar distintas fuentes de datos con AEM Forms. Proporciona una interfaz de usuario intuitiva para crear un esquema de representación de datos unificado de entidades y servicios empresariales a través de fuentes de datos conectadas.

Hemos utilizado la API de SendGrid para enviar correos electrónicos mediante una plantilla dinámica. Se da por hecho que está familiarizado con la API de SendGrid para enviar correos electrónicos mediante plantillas dinámicas. Se le ha proporcionado un archivo swagger para describir a la API como parte de este tutorial.

## Creación de la integración

Siga los siguientes pasos para crear la integración entre AEM Forms y SendGrid

* Cree un origen de datos RESTful usando el [archivo swagger](./assets/SendGridWithDynamicTemplate.yaml). [Siga este vídeo para obtener instrucciones detalladas](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) sobre la creación de fuentes de datos en AEM Forms
* Cree un modelo de datos de formulario basado en la fuente de datos creada en el paso anterior.[Siga la documentación detallada](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/integrate/use-form-data-model/create-form-data-models.html) sobre la creación del modelo de datos de formulario.

El modelo de datos de formulario creado para este tutorial se incluye como parte de los recursos del artículo.

### Siguientes pasos

[Crear integración de almacenamiento de Azure](./create-fdm.md)
