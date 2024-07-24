---
title: AEM Forms con Marketo (parte 3)
description: Tutorial para integrar AEM Forms con Marketo mediante el modelo de datos de formulario de AEM Forms.
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="Integración" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: 7096340b-8ccf-4f5e-b264-9157232e96ba
duration: 78
source-git-commit: 7e0d7e87d72aa1e4450649afa6a962099ceb2db4
workflow-type: tm+mt
source-wordcount: '217'
ht-degree: 3%

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

## Siguientes pasos

[Poniéndolo todo junto para la prueba](./part4.md)
