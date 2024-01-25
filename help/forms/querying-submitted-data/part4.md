---
title: AEM Forms con esquema y datos JSON [Part4]
description: Tutorial de varias partes para guiarle por los pasos necesarios para crear un formulario adaptable con esquema JSON y consultar los datos enviados.
feature: Adaptive Forms
doc-type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: a8d8118d-f4a1-483f-83b4-77190f6a42a4
duration: 116
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '432'
ht-degree: 0%

---

# Consulta de datos enviados


El siguiente paso es consultar los datos enviados y mostrar los resultados en forma de tabla. Para ello utilizamos el siguiente software:

[QueryBuilder](https://querybuilder.js.org/) : componente de interfaz de usuario para crear consultas

[Tablas de datos](https://datatables.net/)- Para mostrar los resultados de la consulta en forma de tabla.

La siguiente interfaz de usuario se creó para permitir la consulta de los datos enviados. Solo los elementos marcados como necesarios en el esquema JSON están disponibles para realizar consultas. En la captura de pantalla siguiente, estamos consultando todos los envíos en los que el prefijo de entrega es SMS.

La IU de ejemplo para consultar los datos enviados no utiliza todas las funciones avanzadas disponibles en QueryBuilder. Se le anima a probarlos por su cuenta.

![querybuilder](assets/querybuilderui.gif)

>[!NOTE]
>
>La versión actual de este tutorial no admite la consulta de varias columnas.

Al seleccionar un formulario para realizar la consulta, se realiza una llamada de GET a **/bin/getdatakeysfromschema**. Esta llamada de GET devuelve los campos obligatorios asociados al esquema de los formularios. A continuación, los campos obligatorios se rellenan en la lista desplegable de QueryBuilder para que pueda generar la consulta.

El siguiente fragmento de código realiza una llamada al método getRequiredColumnsFromSchema del servicio JSONSchemaOperations. Pasamos las propiedades y los elementos requeridos del esquema a esta llamada al método. La matriz que devuelve esta llamada a la función se utiliza para rellenar la lista desplegable del generador de consultas

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

Cuando se hace clic en el botón GetResult, se realiza una llamada Get a **&quot;/bin/querydata&quot;**. Pasamos la consulta creada por la interfaz de usuario de QueryBuilder al servlet a través del parámetro query. A continuación, el servlet fusiona esta consulta en una consulta SQL que se puede utilizar para consultar la base de datos. Por ejemplo, si está buscando para recuperar todos los productos llamados &quot;Mouse&quot;, la cadena de consulta del Generador de consultas es `$.productname = 'Mouse'`. Esta consulta se convertirá en lo siguiente

SELECT &#42; de aemformswithjson .  formsubmissions donde JSON_EXTRACT( formsubmissions .formdata,&quot;$.productName &quot;)= &#39;Mouse&#39;

A continuación, se devuelve el resultado de esta consulta para rellenar la tabla en la interfaz de usuario.

Para ejecutar este ejemplo en el sistema local, realice los siguientes pasos

1. [Asegúrese de haber seguido todos los pasos mencionados aquí](part2.md)
1. [AEM Importe el Dashboard v2.zip mediante el Administrador de paquetes de.](assets/dashboardv2.zip) Este paquete contiene todos los paquetes, las opciones de configuración, el envío personalizado y la página de muestra necesarios para consultar los datos.
1. Crear un formulario adaptable mediante el esquema json de ejemplo
1. Configure el formulario adaptable para enviar a la acción de envío personalizada &quot;customsubmithelpx&quot;
1. Rellene el formulario y envíelo
1. Dirija el explorador a [dashboard.html](http://localhost:4502/content/AemForms/dashboard.html)
1. Seleccione el formulario y realice una consulta simple
