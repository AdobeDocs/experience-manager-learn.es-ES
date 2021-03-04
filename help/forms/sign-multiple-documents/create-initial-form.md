---
title: Creación del formulario inicial para activar el proceso
description: Cree un formulario inicial para activar la notificación por correo electrónico para iniciar el proceso de firma.
feature: formularios adaptables
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6892
thumbnail: 6892.jpg
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '169'
ht-degree: 5%

---


# Crear formulario inicial

El formulario inicial (formulario de refinanciación) se utiliza para firmar varios formularios activando el flujo de trabajo de AEM **Sign Multiple Forms**. Puede introducir valores de su elección, pero asegúrese de que los siguientes campos se añaden al formulario.



| Tipo de campo | Nombre | Función | Oculto | Valor predeterminado |
------------------------|---------------------------------------|--------------------|--------|-----------------
| CampoTexto | signed | Para indicar el estado de firma | Y | N |
| CampoTexto | guid | Para identificar formularios de forma única | Y | 3889 |
| CampoTexto | customerName | Para capturar el nombre de los clientes | N |
| CampoTexto | customerEmail | Correo electrónico del cliente para enviar una notificación | N |
| Casilla de verificación | formsToSign | Los elementos identifican los formularios del paquete | N |



El formulario inicial debe configurarse para activar un flujo de trabajo de AEM denominado **signmultipleforms**
Asegúrese de que la ruta del archivo de datos esté configurada en **Data.xml**. Esto es muy importante, ya que el código de ejemplo busca un archivo llamado Data.xml en la carga útil para procesar el envío del formulario.

## Assets

El formulario inicial (formulario de refinanciación) se puede [descargar desde aquí](assets/refinance-form.zip)





