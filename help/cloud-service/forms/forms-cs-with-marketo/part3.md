---
title: Integrar AEM Forms as a Cloud Service y Marketo (parte 3)
description: Aprenda a integrar AEM Forms y Marketo mediante el modelo de datos de formulario de AEM Forms.
feature: Form Data Model,Integration
version: Cloud Service
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="Integración" type="positive"
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
last-substantial-update: 2024-07-24T00:00:00Z
jira: KT-15876
exl-id: 43737765-b1ea-4594-853a-d78f41136b5e
source-git-commit: b4df652fcda0af5d01077b97aa7fa17cfe2abf4b
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 2%

---

# Crear modelo de datos de formulario

Después de configurar la fuente de datos, el siguiente paso es crear un modelo de datos de formulario basado en la fuente de datos configurada en el paso anterior. Para crear un modelo de datos de formulario, siga estos pasos:

Dirija su explorador a la página [integraciones de datos.AEM ](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm) Esto enumera todas las integraciones de datos creadas en su instancia de.

1. Haga clic en Crear | Modelo de datos de formulario
1. Proporcione un título significativo como FormsAndMarketo y haga clic en Siguiente
1. Seleccione el origen de datos configurado en el paso anterior y haga clic en crear y editar para abrir el modelo de datos de formulario en el modo de edición
1. Expanda el nodo FormsAndMarketo. Expanda el nodo Servicios
1. Seleccione la primera operación &quot;Get&quot;
1. Haga clic en Agregar selección
1. Haga clic en &quot;Seleccionar todo&quot; en el cuadro de diálogo &quot;Agregar objetos de modelo asociados&quot; y luego haga clic en Agregar
1. Para guardar el modelo de datos de formulario, haga clic en el botón Guardar
1. Vaya a la pestaña Servicios
1. Seleccione el único servicio de la lista y haga clic en Probar servicio
1. Proporcione un leadId válido y haga clic en Probar. Si todo va bien, debe obtener los detalles del posible cliente, como se muestra en la captura de pantalla siguiente
   ![testresults](assets/testresults.png)

Ahora puede crear un formulario adaptable basado en este modelo de datos de formulario para insertar y recuperar objetos de Marketo.
