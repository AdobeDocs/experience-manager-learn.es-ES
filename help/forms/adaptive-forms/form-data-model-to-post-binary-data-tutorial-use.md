---
title: Uso Del Modelo De Datos De Formulario Para Publicar Datos Binarios
description: Registro de datos binarios en AEM DAM mediante el Modelo de datos de formulario
feature: Workflow
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 9c62a7d6-8846-424c-97b8-2e6e3c1501ec
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '489'
ht-degree: 0%

---

# Uso Del Modelo De Datos De Formulario Para Publicar Datos Binarios{#using-form-data-model-to-post-binary-data}

A partir de AEM Forms 6.4, ahora se puede invocar el servicio del modelo de datos de formulario como un paso en AEM flujo de trabajo. Este artículo le guiará por un caso de uso de ejemplo para registrar el documento de registro mediante el servicio del modelo de datos de formulario.

El caso de uso es el siguiente:

1. Un usuario rellena y envía el formulario adaptable.
1. El formulario adaptable está configurado para generar el documento de registro.
1. Al enviar estos formularios adaptables, se activa AEM flujo de trabajo que utilizará el servicio de invocar modelo de datos de formulario para POST el documento de registro a AEM DAM.

![posttodam](assets/posttodamshot1.png)

Ficha Modelo de datos de formulario - Propiedades

En la pestaña Entrada de servicio , asignamos lo siguiente

* file (El objeto binario que debe almacenarse) con la propiedad DOR.pdf relativa a la carga útil. Esto significa que cuando se envía el formulario adaptable, el documento de registro que se genera se almacena en un archivo llamado DOR.pdf en relación con la carga útil del flujo de trabajo.**Asegúrese de que este DOR.pdf es el mismo que proporcionó al configurar la propiedad de envío del formulario adaptable.**

* fileName - Es el nombre por el cual se almacena el objeto binario en DAM. Por lo tanto, desea que esta propiedad se genere de forma dinámica, de modo que cada fileName sea único por envío. Para este fin, se ha utilizado el paso de proceso del flujo de trabajo para crear una propiedad de metadatos denominada filename y establecer su valor en una combinación de Nombre de miembro y Número de cuenta de la persona que envía el formulario. Por ejemplo, si el nombre del miembro de la persona es John Jacobs y su número de cuenta es 9846, el nombre del archivo sería John Jacobs_9846.pdf

![fdmserviceinput](assets/fdminputservice.png)

Entrada de servicio

>[!NOTE]
>
>Consejos para la resolución de problemas : Si, por alguna razón, el DOR.pdf no se crea en DAM, restablezca la configuración de autenticación de la fuente de datos haciendo clic en [here](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2Fglobal%2Fsettings%2Fcloudconfigs%2Ffdm%2Fpostdortodam). Estos son los ajustes de autenticación AEM, que de forma predeterminada son admin/admin.

Para probar esta capacidad en su servidor, siga los pasos que se mencionan a continuación:

1.[Implementar el paquete de usuario Desarrollo con servicio](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

1. [Descargar e implementar el paquete setvalue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar).Este paquete OSGI personalizado se utiliza para crear la propiedad metadata y establecer su valor a partir de los datos de formulario enviados.

1. [Importación de recursos](assets/postdortodam.zip) asociado con este artículo a AEM usando el administrador de paquetes.Obtendrá lo siguiente

   1. Modelo de flujo de trabajo
   1. Formulario adaptable configurado para enviarse al flujo de trabajo AEM
   1. Fuente de datos configurada para usar el archivo PostToDam.JSON
   1. Modelo de datos de formulario que utiliza la fuente de datos

1. Apunte a [explorador para abrir el formulario adaptable](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
1. Rellene el formulario y envíe.
1. Compruebe la aplicación Assets si se ha creado y almacenado el documento de registro.


[Archivo Swagger](http://localhost:4502/conf/global/settings/cloudconfigs/fdm/postdortodam/jcr:content/swaggerFile) se utiliza para crear la fuente de datos y está disponible para la referencia
