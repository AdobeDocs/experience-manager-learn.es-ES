---
title: AEM Forms con esquema y datos JSON [Parte 1]
description: Tutorial de varias partes para guiarle por los pasos necesarios para crear un formulario adaptable con esquema JSON y consultar los datos enviados.
feature: Adaptive Forms
doc-type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: c588bdca-b8a8-4de2-97e0-ba08b195699f
duration: 70
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '258'
ht-degree: 0%

---

# Crear formulario adaptable basado en el esquema JSON


La capacidad de crear Forms adaptable basado en el esquema JSON se introdujo en la versión 6.3 de AEM Forms. Los detalles sobre la creación de Forms adaptable con esquema JSON se explican en detalle en esta sección [artículo](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-advanced-authoring/adaptive-form-json-schema-form-model.htmll?lang=es).

Una vez creado un formulario adaptable basado en el esquema JSON, el siguiente paso es almacenar los datos enviados en la base de datos. Para ello, utilizaremos el nuevo tipo de datos JSON introducido por varios proveedores de bases de datos. Para el propósito de este artículo utilizaremos la base de datos MySql 8 para almacenar los datos enviados.

Para este artículo se utilizó la base de datos MySql 8. MySQL introdujo un nuevo tipo de datos llamado [JSON](https://dev.mysql.com/doc/refman/8.0/en/json.html). Esto facilita el almacenamiento y la consulta de objetos JSON. Almacenamos los datos enviados en una columna de tipo JSON en nuestra base de datos.

La siguiente captura de pantalla muestra los datos del formulario enviado almacenados en el tipo de datos JSON. La columna &quot;formdata&quot; es del tipo JSON. También almacenamos el nombre del formulario asociado con los datos en la columna formname

>[!NOTE]
>
>Asegúrese de que el nombre del archivo de esquema json sea correcto. Por ejemplo, debe tener el siguiente formato &lt;name>schema.json. Por lo tanto, el archivo de esquema puede ser mortgage.schema.json o credit.schema.json.


![almacenadas](assets/datastored.gif)


[Esquemas JSON de muestra que se pueden utilizar para crear Forms adaptable.](assets/samplejsonschemas.zip). Descargue y descomprima el archivo zip para obtener los esquemas JSON
