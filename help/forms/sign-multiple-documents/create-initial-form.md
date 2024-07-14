---
title: Crear el formulario inicial para almacenar en Déclencheur el proceso
description: Cree el formulario inicial para almacenar en déclencheur la notificación por correo electrónico e iniciar el proceso de firma.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: User
level: Intermediate
jira: KT-6892
thumbnail: 6892.jpg
exl-id: d7c55dc8-d886-4629-bb50-d927308d12e3
duration: 35
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '177'
ht-degree: 8%

---

# Crear formulario inicial

El formulario inicial (formulario de refinanciación) se utiliza para firmar varios formularios activando el flujo de trabajo de **Firmar varios Forms AEM** de la. Puede introducir los valores que desee, pero asegúrese de que se añaden los campos siguientes al formulario.

| Tipo de campo | Nombre | Función | Oculto | Valor predeterminado |
| ------------------------|---------------------------------------|--------------------|--------|----------------- |
| TextField | firmado | Para indicar el estado de la firma | Y | N |
| TextField | guid | Para identificar el formulario de forma exclusiva | Y | 3889 |
| TextField | customerName | Para capturar el nombre de los clientes | N |
| TextField | customerEmail | Correo electrónico del cliente para enviar la notificación | N |
| CheckBox | formsToSign | Los elementos identifican los formularios del paquete | N |

El formulario inicial debe configurarse para almacenar en déclencheur AEM un flujo de trabajo de denominado **signmultipleforms**
Asegúrese de que la ruta del archivo de datos esté establecida en **Data.xml**. Esto es muy importante, ya que el código de ejemplo busca un archivo llamado Data.xml en la carga útil del proceso de envío del formulario.

## Recursos

El formulario inicial (formulario de refinanciación) se puede [descargar desde aquí](assets/refinance-form.zip)

## Siguientes pasos

[Crear formularios para utilizarlos para firmar](./create-forms-for-signing.md)
