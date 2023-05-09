---
title: Creación del formulario inicial para crear el Déclencheur del proceso
description: Cree un formulario inicial para almacenar en déclencheur la notificación por correo electrónico a fin de iniciar el proceso de firma.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: User
level: Intermediate
kt: 6892
thumbnail: 6892.jpg
exl-id: d7c55dc8-d886-4629-bb50-d927308d12e3
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '177'
ht-degree: 8%

---

# Crear formulario inicial

El formulario inicial (formulario de refinanciación) se utiliza para firmar varios formularios activando la variable **Firmar varios Forms** AEM flujo de trabajo. Puede introducir valores de su elección, pero asegúrese de que los siguientes campos se añaden al formulario.

| Tipo de campo | Nombre | Función | Oculto | Valor predeterminado |
| ------------------------|---------------------------------------|--------------------|--------|----------------- |
| TextField | signed | Para indicar el estado de firma | Y | N |
| TextField | guid | Para identificar formularios de forma única | Y | 3889 |
| TextField | customerName | Para capturar el nombre de los clientes | N |
| TextField | customerEmail | Correo electrónico del cliente para enviar una notificación | N |
| Casilla de verificación | formsToSign | Los elementos identifican los formularios del paquete | N |

El formulario inicial debe configurarse para almacenar en déclencheur un flujo de trabajo AEM llamado **signmultipleforms**
Asegúrese de que la ruta del archivo de datos esté configurada en **Data.xml**. Esto es muy importante, ya que el código de ejemplo busca un archivo llamado Data.xml en la carga útil para procesar el envío del formulario.

## Assets

El formulario inicial (formulario de refinanciación) puede ser [descargado desde aquí](assets/refinance-form.zip)

## Pasos siguientes

[Creación de formularios para firmar](./create-forms-for-signing.md)
