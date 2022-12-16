---
title: Prueba de la solución de kit de bienvenida
description: Implementación de los recursos de la solución para probar la solución
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-12-14T00:00:00Z
source-git-commit: 0e27907066c7d688549a980ccd17b3f17d74b60b
workflow-type: tm+mt
source-wordcount: '157'
ht-degree: 0%

---

# Implementar y probar los recursos de ejemplo

[Instale el paquete del kit de bienvenida](assets/welcomekit.zip). Este paquete contiene la plantilla de página, el componente personalizado para enumerar los recursos, el flujo de trabajo de muestra, la plantilla de correo electrónico y los documentos pdf de muestra que se incluirán en el kit de bienvenida.
[Instalación del paquete de bienvenida](assets/welcomekit.core-1.0.0-SNAPSHOT.jar). Este paquete contiene el código para crear la página y la clase java para devolver los recursos que se van a mostrar en la página web.
[Instalación del formulario adaptable de ejemplo](assets/account-openeing-form.zip)
Configure el servicio Day CQ Mail. El flujo de trabajo envía el vínculo de kit de bienvenida al remitente del formulario que necesita que SMTP esté configurado correctamente.
Configure el componente Enviar correo electrónico del flujo de trabajo según sus necesidades
[Vista previa del formulario](http://localhost:4502/content/dam/formsanddocuments/co-operators/accountopeningform/jcr:content?wcmmode=disabled)
Introduzca sus datos y seleccione uno o más fondos mutuos y envíe el formulario Debe recibir un correo electrónico con un enlace al kit de bienvenida


