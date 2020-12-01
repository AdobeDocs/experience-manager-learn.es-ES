---
title: Crear tablas de base de datos
description: Crear base de datos para que la utilice el modelo de datos de formulario
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5811
thumbnail: kt-5811.jpg
translation-type: tm+mt
source-git-commit: b085a2c75f8e0b4860d503774ea01a108773ad09
workflow-type: tm+mt
source-wordcount: '99'
ht-degree: 0%

---


# Crear tablas de base de datos

El modelo de datos de formulario puede basarse en orígenes RDBMS, RESTfull, SOAP o OData. Este curso se centra en la precreación de formularios adaptables mediante un modelo de datos de formulario respaldado por un origen de datos RDBMS. Para este tutorial se ha utilizado la base de datos MYSQL. Hemos creado las dos tablas siguientes para demostrar el caso de uso

* **** newhiretable - Esta tabla almacena toda la información

   ![newhire](assets/newhire-table.png)


* **** beneficioso - Esto almacena a nuevos beneficiarios

   ![beneficiarios](assets/beneficiaries-table.png)

Puede importar el [archivo sql](assets/db-schema.sql) utilizando el área de trabajo de MySQL para crear tablas con algunos datos de muestra.