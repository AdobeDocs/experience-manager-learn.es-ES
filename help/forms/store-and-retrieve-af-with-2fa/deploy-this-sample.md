---
title: Implementación del ejemplo
description: Obtener caso de uso en ejecución en la instancia local de AEM Forms
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6602
thumbnail: 6602.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: cdfae631-86d7-438f-9baf-afd621802723
source-git-commit: 631fef25620c84e04c012c8337c9b76613e3ad46
workflow-type: tm+mt
source-wordcount: '376'
ht-degree: 1%

---

# Implementación del ejemplo

Para que este caso de uso funcione en su sistema, siga las siguientes instrucciones:

>[!NOTE]
>Se da por hecho que está ejecutando AEM Forms en el puerto 4502.


## Crear base de datos

Este ejemplo utiliza la base de datos MySQL para almacenar los datos del formulario adaptable. Deberá crear la variable [esquema de base de datos importando el archivo de esquema](assets/data-base-schema.sql) en MySQL workbench.

## Crear fuente de datos

Debe crear una fuente de datos llamada **StoreAndRetrieveAfData**. El código del paquete OSGi utiliza este nombre de fuente de datos

## Crear modelo de datos de formulario

El modelo de datos de formulario debe crearse en función de esta fuente de datos llamada **StoreAndRetrieveAfData**. Este modelo de datos de formulario se utiliza para recuperar el número de teléfono móvil asociado con el id de la aplicación. El modelo de datos de formulario puede ser [descargado desde aquí.](assets/2-Factor-Authentication-DataSource-and-FDM.zip)

## Crear cuenta de desarrollador con el siguiente ejemplo

Cree una cuenta de desarrollador con [Nexmo](https://dashboard.nexmo.com/) para enviar y verificar códigos OTP. Anote la clave de API y la clave secreta de API. El origen de datos y el modelo de datos de formulario ya se han creado para usted con este servicio y se incluyen en los recursos mencionados en el paso anterior.

## Implementar los siguientes paquetes OSGi

Implemente el paquete que tiene la variable [código para almacenar y recuperar datos de la base de datos](assets/FetchPartiallyCompletedForm.PartiallyCompletedForm.core-1.0-SNAPSHOT.jar)
Descargue y descomprima el [desarrollarcon serviceuser.zip](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip).
Implemente el archivo DevelopingWithServiceUser.jar utilizando la consola web Felix.

## Implementar la biblioteca de cliente

El ejemplo utiliza 2 bibliotecas de cliente. Importe estos [bibliotecas de cliente](assets/client-libraries.zip) en AEM.

## Importación de la plantilla de formulario adaptable personalizada

Los formularios de ejemplo utilizados en esta demostración se basan en una plantilla personalizada. Importe el [plantilla personalizada en AEM](assets/custom-template-with-page-component.zip)

## Importación de formularios adaptables de ejemplo

Los dos formularios que componen esta muestra deben importarse en AEM. Los formularios de ejemplo se pueden [descargado desde aquí](assets/sample-forms.zip)

Abra el [MyAccountForm](http://localhost:4502/editor.html/content/forms/af/myaccountform.html) en modo de edición. Especifique los valores Clave de API y Secreto de API en los campos correspondientes del formulario adaptable.

## Probar la solución

Previsualice la [StoreAFWithAttachments](http://localhost:4502/content/dam/formsanddocuments/storeafwithattachments/jcr:content?wcmmode=disabled)
Introduzca su número de móvil, incluido el código de país, rellene los detalles del usuario y añada algunos archivos adjuntos. Haga clic en el botón Guardar y salir para guardar el formulario adaptable y sus archivos adjuntos


## Demostración del caso de uso

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=9&learn=on)
