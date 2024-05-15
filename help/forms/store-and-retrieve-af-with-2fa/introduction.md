---
title: Almacenar y recuperar datos de formulario con archivos adjuntos de la base de datos MySQL
description: Tutorial de varias partes para guiarle por los pasos necesarios para almacenar y recuperar datos de formulario con archivos adjuntos
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
jira: KT-6593
thumbnail: 327122.jpg
topic: Development
role: Developer
level: Experienced
exl-id: b278652f-6c09-4abc-b92e-20bfaf2e791a
last-substantial-update: 2020-11-07T00:00:00Z
duration: 148
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '212'
ht-degree: 3%

---

# Almacenar y recuperar datos de formularios adaptables con 2FA

Este tutorial le guiará por los pasos necesarios para guardar y recuperar datos de formulario adaptables con archivos adjuntos mediante 2FA. Este tutorial utilizó la base de datos MySQL para almacenar datos de formulario adaptable. AEM La base de datos que elija se puede utilizar para almacenar los datos siempre y cuando haya implementado los controladores específicos de la base de datos en la base de datos de la que haya hecho clic en el botón de la barra de herramientas de la base de datos de la base de datos de la base de datos de la base de datos de la base de datos de la base de datos de la base de datos de la base de datos de la base de datos de la base de datos de la base de datos de la base de datos de la base de datos de su elección. En un nivel superior, se necesitan los siguientes pasos para lograr el caso de uso:

* Utilice la API de GuideBridge para obtener acceso a los datos del formulario adaptable

* Realizar una llamada del POST a un servlet. Este servlet almacena los datos en la base de datos y los archivos adjuntos del formulario en el repositorio CRX. Los datos almacenados en la base de datos están asociados a un GUID.

* Si desea rellenar el formulario adaptable con los datos almacenados, recupere los datos asociados con el GUID y rellene el formulario adaptable con el **request.setAttribute** método.

## Muestra del caso de uso

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=12&learn=on)

## Requisitos previos

Se espera que la audiencia de este contenido tenga alguna experiencia en las siguientes áreas:

* Formulario adaptable
* Modelo de datos de formulario
* Componentes/servicios OSGi
* AEM Bibliotecas de cliente de


## Siguientes pasos

[Configurar la fuente de datos](./configure-data-source.md)