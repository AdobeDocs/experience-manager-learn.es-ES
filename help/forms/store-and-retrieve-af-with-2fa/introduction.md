---
title: Almacenamiento y recuperación de datos de formulario con datos adjuntos de la base de datos MySQL
description: Tutorial de varias partes para guiarle por los pasos necesarios para almacenar y recuperar datos de formulario con datos adjuntos
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6593
thumbnail: 327122.jpg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 2%

---


# Almacenamiento y recuperación de datos de formularios adaptables con 2FA

Este tutorial le guiará por los pasos necesarios para guardar y recuperar datos de formulario adaptables con datos adjuntos mediante 2FA. Este tutorial utilizó la base de datos MySQL para almacenar datos de formulario adaptable. La base de datos que elija se puede utilizar para almacenar los datos siempre que haya implementado los controladores específicos de la base de datos en AEM. En un nivel superior, se necesitan los siguientes pasos para lograr el caso de uso:

* Uso de la API de GuideBridge para obtener acceso a los datos del formulario adaptable

* Realice una llamada del POST a un servlet. Este servlet almacena los datos en la base de datos y los datos adjuntos del formulario en el repositorio de CRX. Los datos almacenados en la base de datos están asociados a un GUID.

* Si desea rellenar el formulario adaptable con los datos almacenados, recupere los datos asociados con el GUID y rellene el formulario adaptable con el método **request.setAttribute**.

## Demostración del caso de uso

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=9&learn=on)

## Requisitos previos

Se espera que la audiencia de este contenido tenga cierta experiencia en las siguientes áreas:

* Formulario adaptable
* Modelo de datos de formulario
* Servicios y componentes OSGi
* Bibliotecas del cliente de AEM
