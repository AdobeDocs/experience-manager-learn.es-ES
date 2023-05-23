---
title: Crear tablas de base de datos
description: Crear una base de datos para utilizarla en el modelo de datos de formulario
feature: Adaptive Forms
version: 6.4,6.5
kt: 5811
thumbnail: kt-5811.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 1136244a-c3e6-45f6-8af8-eb3c100f838e
source-git-commit: bd41cd9d64253413e793479b5ba900c8e01c0eab
workflow-type: tm+mt
source-wordcount: '105'
ht-degree: 1%

---

# Crear tablas de base de datos

El modelo de datos de formulario se puede basar en fuentes RDBMS, RESTfull, SOAP o OData. El objetivo de este curso es precumplimentar el formulario adaptable mediante el modelo de datos de formulario respaldado por la fuente de datos RDBMS. Para el propósito de este tutorial se utilizó la base de datos MYSQL. Hemos creado las dos tablas siguientes para mostrar el caso de uso

* **nuevo hilo** tabla: esta tabla almacena la nueva información

   ![nuevo hilo](assets/newhire-table.png)


* **beneficiarios** tabla: esto almacena nuevos beneficiarios de hardware

   ![beneficiarios](assets/beneficiaries-table.png)

Puede importar el [archivo sql](assets/db-schema.sql) uso de MySQL Workbench para crear dos tablas con algunos datos de ejemplo.

## Pasos siguientes

[Configurar modelo de datos de formulario](./configuring-form-data-model.md)
