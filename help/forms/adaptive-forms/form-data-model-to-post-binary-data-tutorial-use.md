---
title: Uso Del Modelo De Datos De Formulario Para Almacenar Datos Binarios De Post
description: AEM Publicar datos binarios en DAM de la mediante el modelo de datos de formulario
feature: Workflow
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 9c62a7d6-8846-424c-97b8-2e6e3c1501ec
last-substantial-update: 2021-01-09T00:00:00Z
duration: 95
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '454'
ht-degree: 0%

---

# Uso Del Modelo De Datos De Formulario Para Almacenar Datos Binarios De Post{#using-form-data-model-to-post-binary-data}

A partir de AEM Forms AEM 6.4, ahora podemos invocar el servicio de modelo de datos de formulario como paso en el flujo de trabajo de la. Este artículo le guiará por un ejemplo de caso de uso para publicar un documento de registro mediante el servicio de modelo de datos de formulario.

El caso de uso es el siguiente:

1. Un usuario rellena y envía formularios adaptables.
1. El formulario adaptable está configurado para generar el documento de registro.
1. AEM Al enviar estos formularios adaptables, se activa el flujo de trabajo de la, que utilizará el servicio de invocación del modelo de datos de formulario para almacenar en POST AEM el documento de registro en DAM.

![hoy](assets/posttodamshot1.png)

Pestaña Modelo de datos de formulario: propiedades

En la pestaña Entrada de servicio asignamos lo siguiente

* file(El objeto binario que debe almacenarse) con la propiedad DOR.pdf relativa a la carga útil. Esto significa que, cuando se envía el formulario adaptable, el documento de registro generado se almacena en un archivo llamado DOR.pdf en relación con la carga útil del flujo de trabajo.**Asegúrese de que este documento DOR.pdf sea el mismo que proporcionó al configurar la propiedad de envío del formulario adaptable.**

* fileName: es el nombre con el que se almacena el objeto binario en DAM. Por lo tanto, desea que esta propiedad se genere dinámicamente, de modo que cada fileName sea único por envío. Para ello, hemos utilizado el paso del proceso en el flujo de trabajo para crear una propiedad de metadatos llamada nombre de archivo y establecer su valor en la combinación del nombre de miembro y el número de cuenta de la persona que envía el formulario. Por ejemplo, si el nombre de miembro de la persona es John Jacobs y su número de cuenta es 9846, el nombre de archivo sería John Jacobs_9846.pdf

![fdmserviceinput](assets/fdminputservice.png)

Entrada de servicio

>[!NOTE]
>
>Consejos para solucionar problemas: si por alguna razón el documento DOR.pdf no se ha creado en DAM, restablezca la configuración de autenticación de la fuente de datos haciendo clic [aquí](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2Fglobal%2Fsettings%2Fcloudconfigs%2Ffdm%2Fpostdortodam). AEM Esta es la configuración de autenticación de la, que de forma predeterminada es admin/admin.

Para probar esta capacidad en su servidor, siga los pasos que se mencionan a continuación:

1.[Implementar el paquete Develingwithserviceuser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

1. [Descargue e implemente el paquete setvalue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Este paquete OSGI personalizado se usa para crear la propiedad de metadatos y establecer su valor a partir de los datos del formulario enviado.

1. AEM [Importe los recursos](assets/postdortodam.zip) asociados con este artículo a los recursos mediante el administrador de paquetes. Recibirá lo siguiente

   1. Modelo de flujo de trabajo
   1. AEM Formulario adaptable configurado para enviarse al flujo de trabajo de
   1. Fuente de datos configurada para utilizar el archivo PostToDam.JSON
   1. Modelo de datos de formulario que utiliza el Source de datos

1. Dirija su [explorador para abrir el formulario adaptable](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
1. Rellene el formulario y envíelo.
1. Compruebe la aplicación de Assets si se ha creado y almacenado el documento de registro.


[El archivo Swagger](http://localhost:4502/conf/global/settings/cloudconfigs/fdm/postdortodam/jcr:content/swaggerFile) utilizado para crear el origen de datos está disponible para su referencia
