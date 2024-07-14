---
title: Almacenar y recuperar datos de formulario de la base de datos MySQL Introducción
description: Tutorial de varias partes para guiarle por los pasos necesarios para almacenar y recuperar datos de formulario
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 95795102-4278-4556-8e0f-1b8a359ab093
last-substantial-update: 2019-07-07T00:00:00Z
duration: 236
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '165'
ht-degree: 0%

---

# Almacenar y recuperar datos de formulario adaptable de la base de datos MySQL

Este tutorial le guiará por los pasos necesarios para guardar y recuperar datos de formulario adaptables de la base de datos. Este tutorial utilizó la base de datos MySQL para almacenar datos de formulario adaptable. AEM La base de datos que elija se puede utilizar para almacenar los datos siempre y cuando haya implementado los controladores específicos de la base de datos en la base de datos de la que haya hecho clic en el botón de la barra de herramientas de la base de datos de la base de datos de la base de datos de la base de datos de la base de datos de la base de datos de la base de datos de la base de datos de la base de datos de la base de datos de la base de datos de la base de datos de la base de datos de su elección. En un nivel superior, se necesitan los siguientes pasos para lograr el caso de uso:

* Utilice la API de GuideBridge para obtener acceso a los datos del formulario adaptable

* Realizar una llamada del POST a un servlet. Este servlet almacena los datos en la base de datos. Los datos almacenados están asociados a un GUID

* Si desea rellenar el formulario adaptable con los datos almacenados, recupere los datos asociados con el GUID y rellene el formulario adaptable con el método **request.setAttribute**.

## Muestra del caso de uso

>[!VIDEO](https://video.tv.adobe.com/v/27829?quality=12&learn=on)


