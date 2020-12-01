---
title: AEM Forms con Esquema y datos JSON[Parte 1]
seo-title: AEM Forms con Esquema y datos JSON[Part1]
description: Tutorial de varias partes para guiarle por los pasos necesarios para crear un formulario adaptable con esquema JSON y consultar los datos enviados.
seo-description: Tutorial de varias partes para guiarle por los pasos necesarios para crear un formulario adaptable con esquema JSON y consultar los datos enviados.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '294'
ht-degree: 0%

---


# Crear un formulario adaptable basado en el Esquema JSON


La capacidad de crear un Forms adaptable basado en el esquema JSON se introdujo con la versión AEM Forms 6.3. Los detalles sobre la creación de Forms adaptable con esquema JSON se explican en detalle en este [artículo](https://helpx.adobe.com/experience-manager/6-3/forms/using/adaptive-form-json-schema-form-model.html).

Una vez creado el formulario adaptable basado en el esquema JSON, el siguiente paso es almacenar los datos enviados en la base de datos. Con este fin, utilizaremos el nuevo tipo de datos JSON introducido por varios proveedores de bases de datos. A los efectos de este artículo, utilizaremos la base de datos MySql 8 para almacenar los datos enviados.

Se ha utilizado la base de datos MySql 8 para este artículo. MySQL introdujo un nuevo tipo de datos llamado [JSON](https://dev.mysql.com/doc/refman/8.0/en/json.html). Esto facilita el almacenamiento y la consulta de objetos JSON. Almacenaremos los datos enviados en una columna de tipo JSON en nuestra base de datos.

La siguiente captura de pantalla muestra los datos de formulario enviados almacenados en el tipo de datos JSON. La columna &quot;formdata&quot; es de tipo JSON. También se almacenó el nombre del formulario asociado con los datos en el nombre del formulario de columna

>[!NOTE]
>
>Asegúrese de que el nombre del archivo esquema json sea el adecuado. Por ejemplo, debe tener el siguiente formato: &lt;nombre>esquema.json. Así que el archivo esquema puede ser hipoage.esquema.json o credit.esquema.json.


![datastered](assets/datastored.gif)


[Esquemas JSON de muestra que se pueden usar para crear Forms adaptable.](assets/samplejsonschemas.zip). Descargue y descomprima el archivo zip para obtener los esquemas JSON

