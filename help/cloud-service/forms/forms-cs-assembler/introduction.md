---
title: Manipulación del PDF en Forms CS mediante la operación invocar DDX
description: Ensamble archivos PDF utilizando invocar DDX.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
kt: 9980
source-git-commit: b7ff98dccc1381abe057a80b96268742d0a0629b
workflow-type: tm+mt
source-wordcount: '96'
ht-degree: 1%

---

# Introducción

En este curso, utilizaremos la manipulación y el archivado de PDF de documentos PDF mediante Forms CS. Para utilizar estos microservicios desde una aplicación externa, se requieren los siguientes pasos:

1. Generar credenciales de servicio para una cuenta técnica AEM
1. Cree un token web JSON (JWT) a partir de las credenciales del servicio e intercambie el mismo por un token de acceso
1. Configuración del acceso para la cuenta técnica en AEM
1. Realizar llamadas HTTP utilizando el token de acceso para manipular archivos PDF/generar y validar archivos PDF/A
