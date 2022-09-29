---
title: AEM Forms con Marketo (parte 3)
description: Tutorial para integrar AEM Forms con Marketo mediante el Modelo de datos de formulario de AEM Forms.
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 7096340b-8ccf-4f5e-b264-9157232e96ba
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '378'
ht-degree: 1%

---

# Configurar fuentes de datos

La integración de datos de AEM Forms le permite configurar y conectarse a fuentes de datos diferentes. Los siguientes tipos son compatibles de serie. Sin embargo, con un poco de personalización, también puede integrarse con otras fuentes de datos.

1. Bases de datos relacionales: MySQL, Microsoft SQL Server, IBM DB2 y RDBMS de Oracle
1. AEM perfil de usuario
1. Servicios web RESTful
1. Servicios web basados en SOAP
1. Servicios OData

Para la integración de AEM Forms con Marketo, utilizamos los servicios web de RESTful. El primer paso para la integración es configurar un [fuente de datos.](https://helpx.adobe.com/experience-manager/6-4/forms/using/configure-data-sources.html#ConfigureRESTfulwebservices) Utilice el archivo de intercambio proporcionado como parte de este tutorial. La siguiente captura de pantalla muestra las propiedades importantes que deben especificarse al configurar el origen de datos.
![datasource](assets/datasource.jfif)

El &quot;marketing.json&quot; es el archivo de intercambio y se proporciona como parte de los recursos de este tutorial.
El host de propiedades es específico de la instancia de Marketo.
El tipo de autenticación es personalizado y la implementación de autenticación debe coincidir con &quot;AemForms With Marketo&quot;. (A menos que haya cambiado esto en el código).

## Crear modelo de datos de formulario

Después, al configurar el origen de datos, el siguiente paso es crear un Modelo de datos de formulario basado en el origen de datos configurado en el paso anterior. Para crear el Modelo de datos de formulario, siga los siguientes pasos:

Apunte el navegador a [integraciones de datos .](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm) Esto enumera todas las integraciones de datos creadas en la instancia de AEM.

1. Haga clic en Crear | Modelo de datos de formulario
1. Proporcione un título significativo como FormsAndMarketo y haga clic en Siguiente
1. Seleccione el origen de datos configurado en el paso anterior y haga clic en crear y editar para abrir el Modelo de datos de formulario en el modo de edición
1. Expanda el nodo &quot;FormsAndMarketo&quot;. Expanda el nodo Servicios
1. Seleccione la primera operación &quot;Get&quot;
1. Haga clic en Agregar selección
1. Haga clic en &quot;Seleccionar todo&quot; en el cuadro de diálogo &quot;Agregar objetos de modelo asociados&quot; y, a continuación, haga clic en Agregar
1. Guarde el modelo de datos de formulario haciendo clic en el botón Guardar
1. Tabulación a la pestaña Servicios
1. Seleccione el único servicio que aparece en la lista y haga clic en Test Service
1. Proporcione un leadId válido y haga clic en Probar. Si todo va bien, debería recuperar los detalles del posible cliente como se muestra en la captura de pantalla siguiente
   ![testresults](assets/testresults.jfif)
