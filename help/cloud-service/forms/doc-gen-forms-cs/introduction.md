---
title: Microservicios de generación de documentos en AEM Forms CS
description: Utilice los microservicios de generación de documentos de una aplicación externa.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
feature: Servicios de documento
topic: Desarrollo
kt: 7432
thumbnail: 332439.jpg
source-git-commit: 33cb3d18b744d9a8e54a87152079b42ed09212f2
workflow-type: tm+mt
source-wordcount: '94'
ht-degree: 4%

---

# Introducción

En este curso, utilizaremos los microservicios de generación de documentos para generar un pdf fusionando datos con una plantilla XDP. Para utilizar estos microservicios desde una aplicación externa, se requieren los siguientes pasos:

1. Generar credenciales de servicio para una cuenta técnica AEM
1. Cree un token web JSON (JWT) a partir de las credenciales del servicio e intercambie el mismo por un token de acceso
1. Configuración del acceso para la cuenta técnica en AEM
1. Realización de llamadas HTTP mediante el token de acceso