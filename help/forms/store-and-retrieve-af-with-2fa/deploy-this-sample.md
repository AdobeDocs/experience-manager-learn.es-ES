---
title: Implementar el ejemplo
description: Ejecute el caso de uso en su instancia local de AEM Forms.
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6602
thumbnail: 6602.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: cdfae631-86d7-438f-9baf-afd621802723
duration: 186
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '373'
ht-degree: 1%

---

# Implementar el ejemplo

Para que este caso de uso funcione en su sistema, siga las siguientes instrucciones:

>[!NOTE]
>Se supone que está ejecutando AEM Forms en el puerto 4502.


## Crear base de datos

Este ejemplo utiliza la base de datos MySQL para almacenar los datos del formulario adaptable. Deberá crear el esquema de la base de datos [importando el archivo de esquema](assets/data-base-schema.sql) en MySQL Workbench.

## Crear fuente de datos

Debe crear una fuente de datos obtenida de una conexión Apache Sling llamada **StoreAndRetrieveAfData** que apunte al esquema de la base de datos creado en el paso anterior. El código del paquete OSGi utiliza este nombre de fuente de datos.

## Crear modelo de datos de formulario

Es necesario crear un modelo de datos de formulario basado en esta fuente de datos llamada **StoreAndRetrieveAfData**. Este modelo de datos de formulario se utiliza para recuperar el número de teléfono móvil asociado al ID de la aplicación. El modelo de datos de formulario se puede [descargar desde aquí.](assets/2-Factor-Authentication-DataSource-and-FDM.zip)

## Crear cuenta de desarrollador con nexmo

Cree una cuenta de desarrollador con [Nexmo](https://dashboard.nexmo.com/) para enviar y verificar códigos OTP. Anote la clave de API y la clave secreta de API. La fuente de datos y el modelo de datos de formulario ya se han creado para usted con este servicio y se incluyen con los recursos mencionados en el paso anterior.

## Implemente los siguientes paquetes OSGi

Implemente el paquete que tiene el [código para almacenar y recuperar datos de la base de datos](assets/SaveAndResume.core-1.0.0-SNAPSHOT.jar)
Descargue y descomprima [developerswithserviceuser.zip](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip?lang=es).
Implemente el archivo DevelopersWithServiceUser.jar mediante la consola web de Felix.

## Implementar la biblioteca de cliente

El ejemplo utiliza 2 bibliotecas de cliente. Importe estas [bibliotecas de cliente](assets/store-af-with-attachments-client-lib.zip) en AEM.

## Importar la plantilla de formulario adaptable personalizada

Los formularios de ejemplo utilizados en esta demostración se basan en una plantilla personalizada. Importar la plantilla personalizada [en AEM](assets/custom-template-with-page-component.zip)

## Importar los formularios adaptables de ejemplo

Los dos formularios que componen este ejemplo deben importarse en AEM. Los formularios de ejemplo se pueden [descargar desde aquí](assets/sample-forms.zip)

Abra [MyAccountForm](http://localhost:4502/editor.html/content/forms/af/myaccountform.html) en modo de edición. Especifique los valores Clave de API de Vonage y Secreto de API en los campos correspondientes del formulario adaptable.

## Prueba de la solución

Vista previa de [StoreAFWithAttachments](http://localhost:4502/content/dam/formsanddocuments/storeafwithattachments/jcr:content?wcmmode=disabled)
Introduzca su número de móvil, incluido el código de país, rellene sus datos de usuario y añada algunos archivos adjuntos. Haga clic en el botón &quot;Guardar y salir&quot; para guardar el formulario adaptable y sus archivos adjuntos


## Muestra del caso de uso

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=12&learn=on)
