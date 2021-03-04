---
title: Implementación del ejemplo
description: Obtener caso de uso en ejecución en la instancia local de AEM Forms
feature: Formularios adaptables
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6602
thumbnail: 6602.jpg
topic: Desarrollo
role: Desarrollador
level: Intermedio
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '375'
ht-degree: 2%

---



# Implementación del ejemplo

Para que este caso de uso funcione en su sistema, siga las siguientes instrucciones:

>[!NOTE]
>Se da por hecho que se está ejecutando AEM Forms en el puerto 4502.


## Crear base de datos

Este ejemplo utiliza la base de datos MySQL para almacenar los datos del formulario adaptable. Deberá crear el esquema de la base de datos [importando el archivo de esquema](assets/data-base-schema.sql) en MySQL Workbench.

## Crear fuente de datos

Debe crear una fuente de datos denominada **StoreAndRetrieveAfData**. El código del paquete OSGi utiliza este nombre de fuente de datos

## Crear modelo de datos de formulario

El modelo de datos de formulario debe crearse en función de esta fuente de datos denominada **StoreAndRetrieveAfData**. Este modelo de datos de formulario se utiliza para recuperar el número de teléfono móvil asociado con el id de la aplicación. El modelo de datos de formulario se puede [descargar desde aquí.](assets/2-Factor-Authentication-DataSource-and-FDM.zip)

## Crear cuenta de desarrollador con el siguiente ejemplo

Cree una cuenta de desarrollador con [Nexmo](https://dashboard.nexmo.com/) para enviar y verificar códigos OTP. Anote la clave de API y la clave secreta de API. El origen de datos y el modelo de datos de formulario ya se han creado para usted con este servicio y se incluyen en los recursos mencionados en el paso anterior.

## Implementar los siguientes paquetes OSGi

Implemente el paquete que tiene el código [para almacenar y recuperar datos de la base de datos](assets/FetchPartiallyCompletedForm.PartiallyCompletedForm.core-1.0-SNAPSHOT.jar)
Implemente el [DevelopingWithServiceUser Bundle](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar).

## Implementar la biblioteca de cliente

El ejemplo utiliza 2 bibliotecas de cliente. Importe estas [bibliotecas de cliente](assets/client-libraries.zip) en AEM.

## Importación de la plantilla de formulario adaptable personalizada

Los formularios de ejemplo utilizados en esta demostración se basan en una plantilla personalizada. Importar la [plantilla personalizada en AEM](assets/custom-template-with-page-component.zip)

## Importación de formularios adaptables de ejemplo

Los 2 formularios que componen esta muestra deben importarse en AEM. Los formularios de ejemplo se pueden [descargar desde aquí](assets/sample-forms.zip)

Abra [MyAccountForm](http://localhost:4502/editor.html/content/forms/af/myaccountform.html) en modo de edición. Especifique los valores Clave de API y Secreto de API en los campos correspondientes del formulario adaptable.

## Probar la solución

Vista previa de [StoreAFWithAttachments](http://localhost:4502/content/dam/formsanddocuments/storeafwithattachments/jcr:content?wcmmode=disabled)
Introduzca su número de móvil, incluido el código de país, rellene los detalles del usuario y añada algunos archivos adjuntos. Haga clic en el botón Guardar y salir para guardar el formulario adaptable y sus archivos adjuntos


## Demostración del caso de uso

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=9&learn=on)
