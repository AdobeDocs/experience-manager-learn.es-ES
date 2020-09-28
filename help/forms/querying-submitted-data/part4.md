---
title: AEM Forms con Esquema y datos JSON[Parte 4]
seo-title: AEM Forms con Esquema y datos JSON[Parte 4]
description: Tutorial de varias partes para guiarle por los pasos necesarios para crear un formulario adaptable con esquema JSON y consultar los datos enviados.
seo-description: Tutorial de varias partes para guiarle por los pasos necesarios para crear un formulario adaptable con esquema JSON y consultar los datos enviados.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '475'
ht-degree: 0%

---


# Consulta de datos enviados


El siguiente paso es la consulta de los datos enviados y la visualización de los resultados en forma de tabla. Para lograrlo, usaremos el siguiente software

[QueryBuilder](https://querybuilder.js.org/) : componente de interfaz de usuario para crear consultas

[Tablas](https://datatables.net/)de datos: para mostrar los resultados de la consulta en forma de tabla.

Se ha creado la siguiente interfaz de usuario para permitir la consulta de los datos enviados. Sólo los elementos marcados como obligatorios en el esquema JSON están disponibles para la consulta. En la captura de pantalla de abajo, estamos consultando todas las presentaciones donde el proveedor es SMS.

La interfaz de usuario de ejemplo para la consulta de los datos enviados no utiliza todas las funciones avanzadas disponibles en QueryBuilder. Se te anima a probarlos por tu cuenta.

![querybuilder](assets/querybuilderui.gif)

>[!NOTE]
>
>La versión actual de este tutorial no admite la consulta de varias columnas.

Al seleccionar un formulario para realizar la consulta, se realiza una llamada de GET a **/bin/getdatakeysfromschema**. Esta llamada de GET devuelve los campos requeridos asociados con el esquema de los formularios. A continuación, los campos obligatorios se rellenan en la lista desplegable de QueryBuilder para que pueda crear la consulta.

El siguiente fragmento de código realiza una llamada al método getRequiredColumnsFromSchema del servicio JSONSchemaOperations. Pasamos las propiedades y los elementos requeridos del esquema a esta llamada de método. La matriz devuelta por esta llamada de función se utiliza para rellenar la lista desplegable del generador de consultas

```java
public JSONArray getData(String formName) throws SQLException, IOException {

  org.json.JSONArray arrayOfDataKeys = new org.json.JSONArray();
  JSONObject jsonSchema = jsonSchemaOperations.getJSONSchemaFromDataBase(formName);
  Map<String, String> refKeys = new HashMap<String, String>();

  try {
   JSONObject properties = jsonSchema.getJSONObject("properties");
   JSONArray requiredFields = jsonSchema.has("required") ? jsonSchema.getJSONArray("required") : null;
   jsonSchemaOperations.getRequiredColumnsFromSchema(properties, arrayOfDataKeys, "", jsonSchema, refKeys,
     requiredFields);
  } catch (JSONException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }
  return arrayOfDataKeys;

 }
```

Cuando se hace clic en el botón GetResult, se realiza una llamada Get a **&quot;/bin/querydata&quot;**. Pasamos la consulta generada por la interfaz de usuario de QueryBuilder al servlet a través del parámetro de consulta. A continuación, el servlet analiza esta consulta en consulta SQL que se puede utilizar para la consulta de la base de datos. Por ejemplo, si busca recuperar todos los productos con el nombre &#39;Mouse&#39;, la cadena de consulta del Generador de Consultas será $.productname = &#39;Mouse&#39;. Esta consulta se convertirá a continuación

SELECT * de aemformswithjson .  formularios de envíos en los que JSON_EXTRACT( para envíos .formdata,&quot;$.productName &quot;)= &#39;Mouse&#39;

A continuación, se devuelve el resultado de esta consulta para rellenar la tabla en la interfaz de usuario.

Para que este ejemplo se ejecute en el sistema local, realice los siguientes pasos

1. [Asegúrese de haber seguido todos los pasos mencionados aquí](part2.md)
1. [Importe Dashboardv2.zip mediante AEM Administrador de paquetes.](assets/dashboardv2.zip) Este paquete contiene todos los paquetes necesarios, la configuración, el envío personalizado y la página de muestra para los datos de consulta.
1. Creación de un formulario adaptable mediante el esquema json de ejemplo
1. Configure el formulario adaptable para que se envíe a la acción de envío personalizado &quot;custom submithelpx&quot;
1. Rellene el formulario y envíe
1. Apunte el navegador a [panel.html](http://localhost:4502/content/AemForms/dashboard.html)
1. Seleccione el formulario y realice una consulta sencilla

