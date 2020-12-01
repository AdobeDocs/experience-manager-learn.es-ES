---
title: AEM Forms con marketing (parte 3)
seo-title: AEM Forms con marketing (parte 3)
description: Tutorial para integrar AEM Forms con Marketing mediante el Modelo de datos de formulario de AEM Forms.
seo-description: Tutorial para integrar AEM Forms con Marketing mediante el Modelo de datos de formulario de AEM Forms.
feature: adaptive-forms, form-data-model
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '397'
ht-degree: 1%

---


# Configurar fuente de datos

La integración de datos de AEM Forms le permite configurar y conectar fuentes de datos dispares. Los siguientes tipos son compatibles de forma predeterminada. Sin embargo, con un poco de personalización, también puede realizar la integración con otras fuentes de datos.

1. Bases de datos relacionales: MySQL, Microsoft SQL Server, IBM DB2 y Oracle RDBMS
1. perfil del usuario de AEM
1. Servicios web RESTful
1. Servicios Web basados en SOAP
1. Servicios OData

Para la integración de AEM Forms con Marketing, utilizaremos los servicios web RESTful. El primer paso en la integración es configurar un [origen de datos.](https://helpx.adobe.com/experience-manager/6-4/forms/using/configure-data-sources.html#ConfigureRESTfulwebservices) Utilice el archivo swagger que se proporciona como parte de este tutorial. La siguiente captura de pantalla muestra las propiedades importantes que deben especificarse al configurar el origen de datos.
![datasource](assets/datasource.jfif)

El archivo &quot;marketo.json&quot; es el archivo swagger y se le proporciona como parte de los recursos de este tutorial.
El host de propiedad es específico de la instancia de Marketing.
El tipo de autenticación es personalizado y la implementación de autenticación debe coincidir con &quot;AemForms With Marketing&quot;. (A menos que haya cambiado esto en el código).

## Crear modelo de datos de formulario

Después de configurar el origen de datos, el siguiente paso es crear un modelo de datos de formulario basado en el origen de datos configurado en el paso anterior. Para crear el modelo de datos de formulario, siga los pasos siguientes:

Diríjase el explorador a la página [integraciones de datos.](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm) Esto lista todas las integraciones de datos creadas en la instancia de AEM.

1. Haga clic en Crear | Modelo de datos de formulario
1. Proporcione un título significativo como FormsAndMarketing y haga clic en Siguiente
1. Seleccione el origen de datos configurado en el paso anterior y haga clic en crear y editar para abrir el modelo de datos de formulario en el modo de edición
1. Expanda el nodo &quot;FormsAndMarketing&quot;. Expandir el nodo Servicios
1. Seleccione la primera operación &quot;Get&quot;
1. Haga clic en Añadir selección
1. Haga clic en &quot;Seleccionar todo&quot; en el cuadro de diálogo &quot;Añadir objetos de modelo asociados&quot; y, a continuación, haga clic en Añadir
1. Guarde el modelo de datos de formulario haciendo clic en el botón Guardar
1. Ficha a la ficha Servicios
1. Seleccione el único servicio que aparece en la lista y haga clic en Servicio de prueba
1. Proporcione un leadId válido y haga clic en Prueba. Si todo va bien, debería recuperar los detalles del posible cliente como se muestra en la captura de pantalla de abajo
   ![testresults](assets/testresults.jfif)
