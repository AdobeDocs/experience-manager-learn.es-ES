---
title: Envío de archivos adjuntos de formularios adaptables
description: Añada archivos adjuntos de formularios adaptables y envíelos mediante el componente de envío de correo electrónico
sub-product: formularios
feature: Flujo de trabajo
topics: integrations
audience: developer
doc-type: technical video
activity: setup
version: 6.4,6.5
topic: Desarrollo
role: Developer
level: Beginner
source-git-commit: 22437e93cbf8f36d723dc573fa327562cb51b562
workflow-type: tm+mt
source-wordcount: '83'
ht-degree: 3%

---


# Introducción


Un caso de uso común es comprimir los archivos adjuntos de los formularios adaptables y enviarlos mediante el componente de envío de correo electrónico en un flujo de trabajo AEM. Para lograr el caso de uso, se escribió un paso personalizado en el proceso del flujo de trabajo. En este paso de proceso personalizado, se crea un archivo zip y se añaden los archivos adjuntos al archivo zip. El archivo zip creado se almacena en la variable de flujo de trabajo denominada zippedfile.

