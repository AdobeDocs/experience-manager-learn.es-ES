---
title: AEM Forms con esquema JSON y datos[parte 1]
seo-title: AEM Forms with JSON Schema and Data[Part1]
description: Tutorial de varias partes para guiarle por los pasos necesarios para crear un formulario adaptable con esquema JSON y consultar los datos enviados.
seo-description: Multi-Part tutorial to walk you through the steps involved in creating Adaptive Form with JSON schema and querying the submitted data.
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: c588bdca-b8a8-4de2-97e0-ba08b195699f
source-git-commit: 25a1a40f42d37443db9edc0e09b1691b1c19e848
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 0%

---

# Creación de un formulario adaptable basado en el esquema JSON


La capacidad de crear Forms adaptable basado en el esquema JSON se introdujo en la versión 6.3 de AEM Forms. Los detalles sobre la creación de Forms adaptable con esquema JSON se explican en detalle en esta [article](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-advanced-authoring/adaptive-form-json-schema-form-model.html).

Una vez que cree un formulario adaptable basado en el esquema JSON, el siguiente paso es almacenar los datos enviados en la base de datos. Para este fin, utilizaremos el nuevo tipo de datos JSON introducido por varios proveedores de bases de datos. A los efectos de este artículo, utilizaremos la base de datos MySql 8 para almacenar los datos enviados.

La base de datos MySql 8 se utilizó para este artículo. MySQL introdujo un nuevo tipo de datos llamado [JSON](https://dev.mysql.com/doc/refman/8.0/en/json.html). Esto facilita el almacenamiento y la consulta de objetos JSON. Almacenaremos los datos enviados en una columna de tipo JSON en nuestra base de datos.

La siguiente captura de pantalla muestra los datos del formulario enviados almacenados en el tipo de datos JSON. La columna &quot;formdata&quot; es de tipo JSON. También se ha almacenado el nombre del formulario asociado con los datos en la columna nombre del formulario

>[!NOTE]
>
>Asegúrese de que el nombre del archivo de esquema json sea el adecuado. Por ejemplo, debe tener el siguiente formato &lt;name>schema.json. Por lo tanto, el archivo de esquema puede ser hipoage.schema.json o credit.schema.json.


![datastored](assets/datastored.gif)


[Esquemas JSON de muestra que se pueden usar para crear Forms adaptable.](assets/samplejsonschemas.zip). Descargue y descomprima el archivo zip para obtener los esquemas JSON
