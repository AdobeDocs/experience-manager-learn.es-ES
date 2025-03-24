---
title: Manipulación de PDF en Forms CS mediante la operación invocar DDX
description: Monte los archivos PDF utilizando invocar DDX.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Output Service
topic: Development
jira: KT-9980
exl-id: 713c4e9e-95ac-48e1-a7fc-2b3ec0b145e5
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
duration: 18
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '102'
ht-degree: 0%

---

# Introducción

En este curso, utilizaremos la manipulación de PDF y el archivado de documentos de PDF mediante Forms CS. El uso de estos microservicios desde una aplicación externa implica los siguientes pasos:

1. Generar credenciales de servicio para una cuenta técnica de AEM
1. Cree un token web JSON (JWT) a partir de las credenciales del servicio e intercámbielo por un token de acceso
1. Configuración del acceso para la cuenta técnica en AEM
1. Realizar llamadas HTTP mediante el token de acceso para manipular archivos PDF/generar y validar archivos PDF/A
