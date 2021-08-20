---
title: Almacenamiento y recuperación de datos de formulario con datos adjuntos de la base de datos MySQL
description: Tutorial de varias partes para guiarle por los pasos necesarios para almacenar y recuperar datos de formulario con archivos adjuntos
feature: Formularios adaptables
type: Tutorial
version: 6.4,6.5
kt: 6593
thumbnail: 327122.jpg
topic: Desarrollo
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 4%

---


# Almacenamiento y recuperación de datos de formulario adaptables con 2FA

Este tutorial le guiará por los pasos necesarios para guardar y recuperar los datos del formulario adaptable con archivos adjuntos que utilizan 2FA. Este tutorial utilizó la base de datos MySQL para almacenar datos de formulario adaptable. La base de datos que elija se puede utilizar para almacenar los datos siempre y cuando haya implementado los controladores específicos de la base de datos en AEM. En un nivel superior, se necesitan los siguientes pasos para lograr el caso de uso:

* Utilice la API de GuideBridge para obtener acceso a los datos del formulario adaptable

* Realice una llamada del POST a un servlet. Este servlet almacena los datos de la base de datos y los archivos adjuntos de los formularios en el repositorio CRX. Los datos almacenados de la base de datos están asociados a un GUID.

* Si desea rellenar el formulario adaptable con los datos almacenados, recupere los datos asociados con el GUID y rellene el formulario adaptable con el método **request.setAttribute** .

## Demostración del caso de uso

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=9&learn=on)

## Requisitos previos

Se espera que la audiencia de este contenido tenga alguna experiencia en las siguientes áreas:

* Formulario adaptable
* Modelo de datos de formulario
* Servicios y componentes de OSGi
* Bibliotecas de cliente de AEM
