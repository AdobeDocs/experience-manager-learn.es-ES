---
title: Solución de problemas de firma de soluciones de varios Documentos
description: Prueba y resolución de problemas de la solución
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6960
thumbnail: 6960.jpg
translation-type: tm+mt
source-git-commit: 049574ab2536b784d6b303f474dba0412007e18c
workflow-type: tm+mt
source-wordcount: '389'
ht-degree: 1%

---


# Comprobación y solución de problemas


## Previsualización del formulario de refinanciación

El caso de uso se activa cuando el agente de servicio al cliente rellena y envía [formulario de refinanciación](http://localhost:4502/content/dam/formsanddocuments/formsandsigndemo/refinanceform/jcr:content?wcmmode=disabled).

El flujo de trabajo Firmar varios Forms obtiene déclencheur en este envío de formulario y el cliente recibe una notificación por correo electrónico con un vínculo para el inicio del proceso de cumplimentación y firma del formulario.

## Rellenar formularios en el paquete

El cliente se presenta para rellenar y firmar el primer formulario del paquete. Al firmar correctamente el formulario, el cliente puede desplazarse al siguiente formulario del paquete. Una vez que se han rellenado y firmado todos los formularios, el cliente recibe el formulario &quot;**AllDone**&quot;.

## Solución de problemas

### No se genera la notificación por correo electrónico

La notificación por correo electrónico se envía mediante el componente Enviar correo electrónico en el flujo de trabajo Firmar varios formularios. Si se produce un error en alguno de los pasos de este flujo de trabajo, se enviará la notificación por correo electrónico. Asegúrese de que el paso de proceso personalizado en el flujo de trabajo esté creando filas en la base de datos de MySQL. Si se están creando las filas, compruebe la configuración del servicio de correo de CQ Day

### El vínculo de la notificación por correo electrónico no funciona

Los vínculos de las notificaciones por correo electrónico se generan de forma dinámica. Si su servidor AEM no se está ejecutando en localhost:4502, proporcione el nombre y el puerto del servidor correctos en los argumentos del paso Store Forms To Sign del flujo de trabajo Sign Multiple Forms

### No se puede firmar el formulario

Esto puede suceder si el formulario no se rellenó correctamente mediante la captura de los datos del origen de datos. Compruebe los registros de stdout del servidor. fetchformdata.jsp escribe algunos mensajes útiles en el stdout.

### No se puede navegar al siguiente formulario del paquete

Al firmar correctamente un formulario en el paquete, se activa el flujo de trabajo Actualizar estado de firma. El primer paso del flujo de trabajo actualiza el estado de firma del formulario en la base de datos. Compruebe si el estado del formulario se ha actualizado de 0 a 1.

### No ver el formulario AllDone

Cuando no hay más formularios para firmar en el paquete, el formulario AllDone se presenta al usuario.Si no ve el formulario AllDone, compruebe la dirección URL utilizada en la línea 33 del archivo GetNextFormToSign.js que forma parte de la biblioteca de cliente **getnextform**.











