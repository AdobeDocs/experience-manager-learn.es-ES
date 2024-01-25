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
duration: 97
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '379'
ht-degree: 10%

---

# Configurar fuente de datos

La integración de datos de AEM Forms le permite configurar y conectarse a fuentes de datos diferentes. Los siguientes tipos son compatibles de forma predeterminada. Sin embargo, con un poco de personalización, también puede integrarse con otras fuentes de datos.

1. Bases de datos relacionales: MySQL, Microsoft SQL Server, IBM DB2 y RDBMS de Oracle
1. Perfil de usuario de AEM
1. Servicios web RESTful
1. Servicios web basados en SOAP
1. Servicios OData

Para integrar AEM Forms con Marketo, utilizamos los servicios web RESTful. El primer paso para integrar es configurar una [fuente de datos.](https://helpx.adobe.com/experience-manager/6-4/forms/using/configure-data-sources.html#ConfigureRESTfulwebservices) Utilice el archivo swagger proporcionado como parte de este tutorial. La siguiente captura de pantalla muestra las propiedades importantes que deben especificarse al configurar la fuente de datos.
![datasource](assets/datasource.jfif)

El &quot;marketo.json&quot; es el archivo swagger y se le proporciona como parte de los recursos de este tutorial.
El host de propiedades es específico de la instancia de Marketo.
El tipo de autenticación es personalizada y la implementación de autenticación debe coincidir con &quot;AemForms con Marketo&quot;. (A menos que haya cambiado esto en su código).

## Crear modelo de datos de formulario

Después de configurar la fuente de datos, el siguiente paso es crear un modelo de datos de formulario basado en la fuente de datos configurada en el paso anterior. Para crear un modelo de datos de formulario, siga estos pasos:

Dirija el explorador a [página de integraciones de datos.](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm) AEM Esta lista enumera todas las integraciones de datos creadas en la instancia de.

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
   ![resultados de las pruebas](assets/testresults.jfif)

## Pasos siguientes

[Poniéndolo todo junto para la prueba](./part4.md)
