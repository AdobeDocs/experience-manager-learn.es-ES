---
title: Solución de problemas de firmar varios documentos
description: Prueba y solución de problemas
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-6960
thumbnail: 6960.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: 99cba29e-4ae3-4160-a4c7-a5b6579618c0
duration: 98
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '382'
ht-degree: 0%

---

# Prueba y solución de problemas


## Previsualización del formulario de refinanciación

El caso de uso se activa cuando el agente de servicio de atención al cliente rellena y envía [formulario de refinanciación](http://localhost:4502/content/dam/formsanddocuments/formsandsigndemo/refinanceform/jcr:content?wcmmode=disabled).

El flujo de trabajo Firmar varios Forms obtiene déclencheur con este envío de formulario y el cliente recibe una notificación por correo electrónico con un vínculo para iniciar el proceso de rellenado y firma del formulario.

## Rellenar formularios en el paquete

El cliente se presenta para rellenar y firmar el primer formulario del paquete. Una vez firmado correctamente el formulario, el cliente puede desplazarse al siguiente formulario del paquete. Una vez rellenados y firmados todos los formularios, al cliente se le presenta el &quot;**AllDone**&quot;.

## Solución de problemas

### No se genera la notificación por correo electrónico

Las notificaciones por correo electrónico se envían mediante el componente Enviar correo electrónico en el flujo de trabajo Firmar varios formularios. Si alguno de los pasos de este flujo de trabajo falla, se envía la notificación por correo electrónico. Asegúrese de que el paso de proceso personalizado del flujo de trabajo esté creando filas en la base de datos MySQL. Si se están creando las filas, compruebe los ajustes de configuración del servicio de correo de CQ Day

### El vínculo de la notificación por correo electrónico no funciona

Los vínculos de las notificaciones por correo electrónico se generan dinámicamente. AEM Si el servidor de la no se está ejecutando en localhost:4502, proporcione el nombre de servidor y el puerto correctos en los argumentos del paso Almacenar Forms para firmar del flujo de trabajo Firmar varios Forms

### No se puede firmar el formulario

Esto puede ocurrir si el formulario no se rellenó correctamente al recuperar los datos del origen de datos. Compruebe los registros stdout del servidor. fetchformdata.jsp escribe algunos mensajes útiles en el stdout.

### No se puede navegar al siguiente formulario del paquete

Al firmar correctamente un formulario del paquete, se activa el flujo de trabajo Actualizar estado de firma. El primer paso del flujo de trabajo actualiza el estado de firma del formulario en la base de datos. Compruebe si el estado del formulario se actualiza de 0 a 1.

### No ver el formulario AllDone

Cuando no hay más formularios para firmar en el paquete, se presenta el formulario AllDone al usuario. Si no ve el formulario AllDone, compruebe la URL utilizada en la línea 33 del archivo GetNextFormToSign.js, que forma parte del **getnextform** biblioteca del cliente.
