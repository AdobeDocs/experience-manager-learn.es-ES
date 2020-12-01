---
title: Uso Del Modelo De Datos De Formulario Para Publicar Datos Binario
seo-title: Uso Del Modelo De Datos De Formulario Para Publicar Datos Binario
description: Registro de datos binarios en AEM DAM mediante el modelo de datos de formulario
seo-description: Registro de datos binarios en AEM DAM mediante el modelo de datos de formulario
uuid: dd344ed8-69f7-4d63-888a-3c96993fe99d
feature: workflow
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
discoiquuid: 6e99df7d-c030-416b-83d2-24247f673b33
translation-type: tm+mt
source-git-commit: f07680e73316efb859a675f4b2212d8c3e03f6a0
workflow-type: tm+mt
source-wordcount: '509'
ht-degree: 0%

---


# Uso del modelo de datos de formulario para publicar datos binarios{#using-form-data-model-to-post-binary-data}

A partir de AEM Forms 6.4, ahora podemos invocar el servicio del modelo de datos de formulario como un paso en AEM flujo de trabajo. Este artículo le guiará por un caso de uso de muestra para registrar el Documento de registro mediante el servicio de modelo de datos de formulario.

El caso de uso es el siguiente:

1. Un usuario rellena y envía el formulario adaptable.
1. El formulario adaptable está configurado para generar Documento de registro.
1. Al enviar estos formularios adaptables, se activa AEM Flujo de trabajo que utilizará el servicio invocar Modelo de datos de formulario para el POST del Documento de registro a AEM DAM.

![posttodam](assets/posttodamshot1.png)

Ficha Modelo de datos de formulario - Propiedades

En la ficha Entrada de servicio, asignamos lo siguiente

* file(El objeto binario que debe almacenarse) con la propiedad DOR.pdf relativa a la carga útil. Esto significa que cuando se envía el formulario adaptable, el Documento de registro que se genera se almacenará en un archivo llamado DOR.pdf en relación con la carga útil del flujo de trabajo.**Asegúrese de que este DOR.pdf es el mismo que el que proporciona al configurar la propiedad de envío del formulario adaptable.**

* fileName: es el nombre por el que se almacenará el objeto binario en DAM. Por lo tanto, desea que esta propiedad se genere de forma dinámica, de modo que cada fileName sea único por envío. Con este fin, hemos utilizado el paso de proceso del flujo de trabajo para crear una propiedad de metadatos denominada filename y establecer su valor en una combinación de Nombre de miembro y Número de cuenta de la persona que envía el formulario. Por ejemplo, si el nombre del miembro de la persona es John Jacobs y su número de cuenta es 9846, el nombre del archivo sería John Jacobs_9846.pdf

![fdmserviceinput](assets/fdminputservice.png)

Entrada de servicio

>[!NOTE]
>
>Sugerencias para la grabación de problemas: si, por alguna razón, el archivo DOR.pdf no se ha creado en DAM, restablezca la configuración de autenticación de la fuente de datos haciendo clic [aquí](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2Fglobal%2Fsettings%2Fcloudconfigs%2Ffdm%2Fpostdortodam). Estas son las opciones de autenticación AEM, que de forma predeterminada son admin/admin.

Para probar esta capacidad en su servidor, siga los pasos que se mencionan a continuación:

1.[Implementar el paquete DevelopmentWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

1. [Descargue e implemente el paquete](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar) setvalue.Este paquete OSGI personalizado se utiliza para crear la propiedad metadata y establecer su valor a partir de los datos del formulario enviados.

1. [Importe los ](assets/postdortodam.zip) recursos asociados a este artículo en AEM mediante el administrador de paquetes.Obtendrá lo siguiente

   1. Modelo de flujo de trabajo
   1. Formulario adaptable configurado para enviarse al flujo de trabajo AEM
   1. Fuente de datos configurada para usar el archivo PostToDam.JSON
   1. Modelo de datos de formulario que utiliza la fuente de datos

1. Seleccione el explorador [para abrir el formulario adaptable](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
1. Rellene el formulario y envíelo.
1. Compruebe la aplicación Assets si se ha creado y almacenado el Documento de registro.


[Swagger ](http://localhost:4502/conf/global/settings/cloudconfigs/fdm/postdortodam/jcr:content/swaggerFile) File utilizado para crear la fuente de datos está disponible para su referencia
