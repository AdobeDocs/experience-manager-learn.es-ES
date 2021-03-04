---
title: Solución de problemas para firmar varios documentos
description: Prueba y resolución de problemas de la solución
feature: Formularios adaptables
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6960
thumbnail: 6960.jpg
topic: Desarrollo
role: Desarrollador
level: Intermedio
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '394'
ht-degree: 2%

---


# Comprobación y solución de problemas


## Vista previa del formulario de refinanciación

El caso de uso se activa cuando el agente de servicio al cliente rellena y envía [formulario de refinanciación](http://localhost:4502/content/dam/formsanddocuments/formsandsigndemo/refinanceform/jcr:content?wcmmode=disabled).

El flujo de trabajo Firmar varios formularios recibe activadores en este envío de formulario y el cliente recibe una notificación por correo electrónico con un vínculo para iniciar el proceso de cumplimentación y firma del formulario.

## Rellenar formularios en el paquete

El cliente se presenta para rellenar y firmar el primer formulario del paquete. Al firmar correctamente el formulario, el cliente puede acceder al siguiente formulario del paquete. Una vez que se han rellenado y firmado todos los formularios, el cliente recibe el formulario &quot;**AllDone**&quot;.

## Solución de problemas

### La notificación por correo electrónico no se está generando

La notificación por correo electrónico se envía mediante el componente Enviar correo electrónico en el flujo de trabajo Firmar varios formularios . Si se produce un error en cualquiera de los pasos de este flujo de trabajo, se envía la notificación por correo electrónico. Asegúrese de que el paso de proceso personalizado del flujo de trabajo esté creando filas en la base de datos MySQL. Si las filas se están creando, compruebe los ajustes de configuración del Servicio de correo de CQ Day

### El vínculo de la notificación por correo electrónico no funciona

Los vínculos de las notificaciones por correo electrónico se generan de forma dinámica. Si su servidor AEM no se está ejecutando en localhost:4502, proporcione el nombre y puerto del servidor correcto en los argumentos del paso Store Forms To Sign del flujo de trabajo Sign Multiple Forms

### No se puede firmar el formulario

Esto puede suceder si el formulario no se rellena correctamente recuperando los datos del origen de datos. Compruebe los registros de stdout del servidor. fetchformdata.jsp escribe algunos mensajes útiles en el stdout.

### No se puede navegar al siguiente formulario en el paquete

Al firmar correctamente un formulario en el paquete, se activa el flujo de trabajo Actualizar estado de firma . El primer paso del flujo de trabajo actualiza el estado de firma del formulario en la base de datos. Compruebe si el estado del formulario se actualiza de 0 a 1.

### No ver el formulario AllDone

Cuando no hay más formularios para iniciar sesión en el paquete, el formulario AllDone se presenta al usuario. Si no ve el formulario AllDone, compruebe la URL utilizada en la línea 33 del archivo GetNextFormToSign.js que forma parte de la biblioteca de cliente **getnextform**.











