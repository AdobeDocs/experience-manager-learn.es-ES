---
title: AEM Forms con esquema JSON y datos[Part4]
seo-title: AEM Forms con esquema JSON y datos[Part4]
description: Tutorial de varias partes para guiarle por los pasos necesarios para crear un formulario adaptable con esquema JSON y consultar los datos enviados.
seo-description: Tutorial de varias partes para guiarle por los pasos necesarios para crear un formulario adaptable con esquema JSON y consultar los datos enviados.
feature: formularios adaptables
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '476'
ht-degree: 0%

---


# Consulta de datos enviados


El siguiente paso es consultar los datos enviados y mostrar los resultados en forma de tabla. Para lograrlo, utilizaremos el siguiente software

[QueryBuilder](https://querybuilder.js.org/) : componente de interfaz de usuario para crear consultas

[Tablas de datos](https://datatables.net/): para mostrar los resultados de la consulta de forma tabular.

La siguiente interfaz de usuario se creó para permitir la consulta de los datos enviados. Solo los elementos marcados como requeridos en el esquema JSON están disponibles para realizar consultas con. En la captura de pantalla de abajo, estamos consultando todos los envíos donde delivery pref es SMS.

La interfaz de usuario de ejemplo para consultar los datos enviados no utiliza todas las capacidades avanzadas disponibles en QueryBuilder. Se le recomienda que los pruebe por su cuenta.

![querybuilder](assets/querybuilderui.gif)

>[!NOTE]
>
>La versión actual de este tutorial no admite la consulta de varias columnas.

Cuando selecciona un formulario para realizar la consulta, se realiza una llamada GET a **/bin/getdatakeysfromschema**. Esta llamada GET devuelve los campos obligatorios asociados al esquema de los formularios. A continuación, los campos obligatorios se rellenan en la lista desplegable de QueryBuilder para que usted pueda crear la consulta.

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

Cuando se hace clic en el botón GetResult, se realiza una llamada Get a **&quot;/bin/querydata&quot;**. Pasamos la consulta generada por la interfaz de usuario de QueryBuilder al servlet a través del parámetro de consulta . A continuación, el servlet analiza esta consulta en consulta SQL que se puede utilizar para consultar la base de datos. Por ejemplo, si está buscando recuperar todos los productos llamados &quot;Mouse&quot;, la cadena de consulta de Query Builder será $.productname = &quot;Mouse&quot;. Esta consulta se convierte a continuación

SELECCIONE * en aemformswithjson .  envíos de formularios en los que JSON_EXTRACT( para envíos .formdata,&quot;$.productName &quot;)= &#39;Mouse&#39;

A continuación, se devuelve el resultado de esta consulta para rellenar la tabla en la interfaz de usuario de .

Para que este ejemplo se ejecute en su sistema local, realice los siguientes pasos

1. [Asegúrese de haber seguido todos los pasos mencionados aquí](part2.md)
1. [Importe Dashboardv2.zip mediante el Administrador de paquetes AEM.](assets/dashboardv2.zip) Este paquete contiene todos los paquetes, configuraciones, envíos personalizados y páginas de muestra necesarios para consultar los datos.
1. Creación de un formulario adaptable utilizando el esquema json de ejemplo
1. Configurar el formulario adaptable para enviar a la acción de envío personalizada &quot;customsubmithelpx&quot;
1. Rellene el formulario y envíe
1. Apunte el navegador a [dashboard.html](http://localhost:4502/content/AemForms/dashboard.html)
1. Seleccione el formulario y realice una consulta sencilla

