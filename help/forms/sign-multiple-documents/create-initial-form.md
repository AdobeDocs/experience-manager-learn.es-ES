---
title: Creación del formulario inicial para crear el Déclencheur del proceso
description: Cree un formulario inicial para almacenar en déclencheur la notificación por correo electrónico a fin de iniciar el proceso de firma.
feature: Formularios adaptables
version: 6.4,6.5
topic: Desarrollo
role: Business Practitioner
level: Intermediate
kt: 6892
thumbnail: 6892.jpg
source-git-commit: 3569d8b2a38d1cce02f6f4ff8b0c583f4dc118b6
workflow-type: tm+mt
source-wordcount: '171'
ht-degree: 7%

---


# Crear formulario inicial

El formulario inicial (formulario de refinanciación) se utiliza para firmar varios formularios activando el flujo de trabajo de AEM **Sign Multiple Forms**. Puede introducir valores de su elección, pero asegúrese de que los siguientes campos se añaden al formulario.

| Tipo de campo | Nombre | Función | Oculto | Valor predeterminado |
| ------------------------|---------------------------------------|--------------------|--------|----------------- |
| CampoTexto | signed | Para indicar el estado de firma | Y | N |
| CampoTexto | guid | Para identificar formularios de forma única | Y | 3889 |
| CampoTexto | customerName | Para capturar el nombre de los clientes | N |
| CampoTexto | customerEmail | Correo electrónico del cliente para enviar una notificación | N |
| Casilla de verificación | formsToSign | Los elementos identifican los formularios del paquete | N |

El formulario inicial debe configurarse para almacenar en déclencheur un flujo de trabajo AEM denominado **signmultipleforms**
Asegúrese de que la ruta del archivo de datos esté configurada en **Data.xml**. Esto es muy importante, ya que el código de ejemplo busca un archivo llamado Data.xml en la carga útil para procesar el envío del formulario.

## Assets

El formulario inicial (formulario de refinanciación) se puede [descargar desde aquí](assets/refinance-form.zip)





