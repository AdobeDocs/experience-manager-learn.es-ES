---
title: Almacenar y recuperar datos de formulario de la base de datos MySQL Introducción
description: Tutorial de varias partes para guiarle por los pasos necesarios para almacenar y recuperar datos de formulario
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
exl-id: 95795102-4278-4556-8e0f-1b8a359ab093
last-substantial-update: 2019-07-07T00:00:00Z
duration: 236
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '165'
ht-degree: 0%

---

# Almacenar y recuperar datos de formulario adaptable de la base de datos MySQL

Este tutorial le guiará por los pasos necesarios para guardar y recuperar datos de formulario adaptables de la base de datos. Este tutorial utilizó la base de datos MySQL para almacenar datos de formulario adaptable. La base de datos que elija se puede utilizar para almacenar los datos siempre que haya implementado los controladores específicos de la base de datos en AEM. En un nivel superior, se necesitan los siguientes pasos para lograr el caso de uso:

* Utilice la API de GuideBridge para obtener acceso a los datos del formulario adaptable

* Realizar una llamada de POST a un servlet. Este servlet almacena los datos en la base de datos. Los datos almacenados están asociados a un GUID

* Si desea rellenar el formulario adaptable con los datos almacenados, recupere los datos asociados con el GUID y rellene el formulario adaptable con el método **request.setAttribute**.

## Muestra del caso de uso

>[!VIDEO](https://video.tv.adobe.com/v/27829?quality=12&learn=on)


