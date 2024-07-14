---
title: Crear tablas de base de datos
description: Crear una base de datos para utilizarla en el modelo de datos de formulario
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-5811
thumbnail: kt-5811.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 1136244a-c3e6-45f6-8af8-eb3c100f838e
duration: 21
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '105'
ht-degree: 1%

---

# Crear tablas de base de datos

SOAP El modelo de datos de formulario se puede basar en fuentes RDBMS, RESTfull, o OData. El objetivo de este curso es precumplimentar el formulario adaptable mediante el modelo de datos de formulario respaldado por la fuente de datos RDBMS. Para el propósito de este tutorial se utilizó la base de datos MYSQL. Hemos creado las dos tablas siguientes para mostrar el caso de uso

* Tabla **newhire**: esta tabla almacena la información nueva

  ![nuevo](assets/newhire-table.png)


* Tabla **beneficiarios**: esto almacena nuevos beneficiarios

  ![beneficiarios](assets/beneficiaries-table.png)

Puede importar el [archivo sql](assets/db-schema.sql) mediante MySQL Workbench para crear tablas con algunos datos de ejemplo.

## Siguientes pasos

[Configurar modelo de datos de formulario](./configuring-form-data-model.md)
