---
title: Enviar correo electrónico mediante SendGrid
description: Almacenar en déclencheur un mensaje de correo electrónico con un vínculo al formulario guardado
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
kt: 13717
source-git-commit: 52c8d96a03b4d6e4f2a0a3c92f4307203e236417
workflow-type: tm+mt
source-wordcount: '197'
ht-degree: 12%

---

# Integración con SendGrid

La integración de datos de AEM Forms permite configurar y conectar distintas fuentes de datos con AEM Forms. Proporciona una interfaz de usuario intuitiva para crear un esquema de representación de datos unificado de entidades y servicios empresariales a través de fuentes de datos conectadas.

Hemos utilizado la API de SendGrid para enviar correos electrónicos mediante una plantilla dinámica. Se da por hecho que está familiarizado con la API de SendGrid para enviar correos electrónicos mediante plantillas dinámicas. Se le ha proporcionado un archivo swagger para describir a la API como parte de este tutorial.

## Creación de la integración

Siga los siguientes pasos para crear la integración entre AEM Forms y SendGrid

* Crear una fuente de datos RESTful usando [archivo swagger](./assets/SendGridWithDynamicTemplate.yaml). [Siga este vídeo para obtener instrucciones detalladas](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) sobre la creación de fuentes de datos en AEM Forms
* Cree un modelo de datos de formulario basado en la fuente de datos creada en el paso anterior.[Siga la documentación detallada](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/integrate/use-form-data-model/create-form-data-models.html) al crear el modelo de datos de formulario.

El modelo de datos de formulario creado para este tutorial se incluye como parte de los recursos del artículo.

### Pasos siguientes

[Crear integración de almacenamiento de Azure](./create-fdm.md)


