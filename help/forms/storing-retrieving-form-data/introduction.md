---
title: Almacenamiento y recuperación de datos de formulario de la base de datos MySQL
description: Tutorial de varias partes para guiarle por los pasos necesarios para almacenar y recuperar datos de formulario
feature: Formularios adaptables
type: Tutorial
version: 6.3,6.4,6.5
topic: Desarrollo
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '167'
ht-degree: 1%

---


# Almacenamiento y recuperación de datos de formulario adaptable de la base de datos MySQL

Este tutorial le guiará por los pasos necesarios para guardar y recuperar los datos del formulario adaptable de la base de datos. Este tutorial utilizó la base de datos MySQL para almacenar datos de formulario adaptable. La base de datos que elija se puede utilizar para almacenar los datos siempre y cuando haya implementado los controladores específicos de la base de datos en AEM. En un nivel superior, se necesitan los siguientes pasos para lograr el caso de uso:

* Utilice la API de GuideBridge para obtener acceso a los datos del formulario adaptable

* Realice una llamada del POST a un servlet. Este servlet almacena los datos en la base de datos. Los datos almacenados están asociados a un GUID

* Si desea rellenar el formulario adaptable con los datos almacenados, recupere los datos asociados con el GUID y rellene el formulario adaptable con el método **request.setAttribute** .

## Demostración del caso de uso

>[!VIDEO](https://video.tv.adobe.com/v/27829?quality=9&learn=on)
