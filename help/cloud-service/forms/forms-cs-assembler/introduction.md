---
title: Manipulación del PDF en Forms CS mediante la operación invocar DDX
description: Montar archivos de PDF utilizando invocar DDX.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
jira: KT-9980
exl-id: 713c4e9e-95ac-48e1-a7fc-2b3ec0b145e5
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
duration: 18
source-git-commit: b4df652fcda0af5d01077b97aa7fa17cfe2abf4b
workflow-type: tm+mt
source-wordcount: '102'
ht-degree: 0%

---

# Introducción

En este curso, utilizaremos la manipulación y el archivado por parte del PDF de documentos de PDF mediante Forms CS. El uso de estos microservicios desde una aplicación externa implica los siguientes pasos:

1. AEM Generación de credenciales de servicio para una cuenta técnica de
1. Cree un token web JSON (JWT) a partir de las credenciales del servicio e intercámbielo por un token de acceso
1. AEM Configuración del acceso para la cuenta técnica en la
1. Realizar llamadas HTTP mediante el token de acceso para manipular archivos PDF/generar y validar archivos PDF/A
