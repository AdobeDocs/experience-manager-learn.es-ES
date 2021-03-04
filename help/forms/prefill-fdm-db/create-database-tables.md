---
title: Crear tablas de base de datos
description: Crear base de datos para que la utilice el modelo de datos de formulario
feature: formularios adaptables
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5811
thumbnail: kt-5811.jpg
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '100'
ht-degree: 0%

---


# Crear tablas de base de datos

El modelo de datos de formulario se puede basar en orígenes RDBMS, RESTfull, SOAP o OData. El objetivo de este curso es prearchivar el formulario adaptable utilizando el modelo de datos de formulario respaldado por el origen de datos RDBMS. Para este tutorial se ha utilizado la base de datos MYSQL. Hemos creado las dos tablas siguientes para demostrar el caso de uso

* **** newhiretable: esta tabla almacena la información nueva

   ![newhire](assets/newhire-table.png)


* **** beneficiariable - Esto almacena nuevos beneficiarios

   ![beneficiarios](assets/beneficiaries-table.png)

Puede importar el [archivo sql](assets/db-schema.sql) utilizando el área de trabajo MySQL para crear tablas con algunos datos de ejemplo.