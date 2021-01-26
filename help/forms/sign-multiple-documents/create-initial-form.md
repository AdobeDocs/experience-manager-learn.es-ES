---
title: Crear el formulario inicial para Déclencheur del proceso
description: Cree un formulario inicial para el déclencheur de la notificación por correo electrónico con el fin de inicio del proceso de firma.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6892
thumbnail: 6892.jpg
translation-type: tm+mt
source-git-commit: 049574ab2536b784d6b303f474dba0412007e18c
workflow-type: tm+mt
source-wordcount: '168'
ht-degree: 5%

---


# Crear formulario inicial

El formulario inicial (formulario de refinanciación) se utiliza para firmar varios formularios activando el flujo de trabajo de AEM **Firmar varios Forms**. Puede introducir los valores que desee pero asegurarse de que se agregan al formulario los campos siguientes.



| Tipo de campo | Nombre | Función | Oculto | Valor predeterminado |
------------------------|---------------------------------------|--------------------|--------|-----------------
| TextField | firmado | Para indicar el estado de la firma | Y | N |
| TextField | guid | Identificar el formulario de forma única | Y | 3889 |
| TextField | customerName | Para capturar el nombre de los clientes | N |
| TextField | customerEmail | Correo electrónico del cliente para enviar la notificación | N |
| Casilla de verificación | formsToSign | Los elementos identifican los formularios del paquete | N |



El formulario inicial debe configurarse para que se déclencheur un flujo de trabajo AEM denominado **signmultipleforms**
Asegúrese de que la ruta del archivo de datos esté configurada en **Data.xml**. Esto es muy importante, ya que el código de muestra busca un archivo llamado Data.xml en la carga útil del proceso de envío del formulario.

## Assets

El formulario inicial (formulario de refinanciación) se puede [descargar desde aquí](assets/refinance-form.zip)





