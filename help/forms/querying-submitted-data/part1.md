---
title: AEM Forms con esquema y datos JSON [Parte 1]
seo-title: AEM Forms with JSON Schema and Data[Part1]
description: Tutorial de varias partes para guiarle por los pasos necesarios para crear un formulario adaptable con esquema JSON y consultar los datos enviados.
seo-description: Multi-Part tutorial to walk you through the steps involved in creating Adaptive Form with JSON schema and querying the submitted data.
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: c588bdca-b8a8-4de2-97e0-ba08b195699f
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 0%

---

# Crear formulario adaptable basado en el esquema JSON


La capacidad de crear Forms adaptable basado en el esquema JSON se introdujo en la versión 6.3 de AEM Forms. Los detalles sobre la creación de Forms adaptable con esquema JSON se explican en detalle en esta sección [artículo](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-advanced-authoring/adaptive-form-json-schema-form-model.html).

Una vez creado un formulario adaptable basado en el esquema JSON, el siguiente paso es almacenar los datos enviados en la base de datos. Para ello, utilizaremos el nuevo tipo de datos JSON introducido por varios proveedores de bases de datos. Para el propósito de este artículo utilizaremos la base de datos MySql 8 para almacenar los datos enviados.

Para este artículo se utilizó la base de datos MySql 8. MySQL introdujo un nuevo tipo de datos llamado [JSON](https://dev.mysql.com/doc/refman/8.0/en/json.html). Esto facilita el almacenamiento y la consulta de objetos JSON. Almacenamos los datos enviados en una columna de tipo JSON en nuestra base de datos.

La siguiente captura de pantalla muestra los datos del formulario enviado almacenados en el tipo de datos JSON. La columna &quot;formdata&quot; es del tipo JSON. También almacenamos el nombre del formulario asociado con los datos en la columna formname

>[!NOTE]
>
>Asegúrese de que el nombre del archivo de esquema json sea correcto. Por ejemplo, debe tener el siguiente formato &lt;name>schema.json. Por lo tanto, el archivo de esquema puede ser mortgage.schema.json o credit.schema.json.


![almacenadas](assets/datastored.gif)


[Esquemas JSON de muestra que se pueden utilizar para crear Forms adaptable.](assets/samplejsonschemas.zip). Descargue y descomprima el archivo zip para obtener los esquemas JSON
