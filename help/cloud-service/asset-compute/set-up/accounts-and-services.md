---
title: Configurar cuentas y servicios para la extensibilidad de la Asset compute
description: El desarrollo de los trabajadores de Asset compute requiere acceso a cuentas y servicios, incluyendo AEM como Cloud Service, Adobe Project Firefly y almacenamiento en la nube proporcionado por Microsoft o Amazon.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6264
thumbnail: 40377.jpg
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '627'
ht-degree: 1%

---


# Configurar cuentas y servicios

Este tutorial requiere que los siguientes servicios sean de aprovisionamiento y accesibles a través del Adobe ID del alumno.

Todos los servicios de Adobe deben ser accesibles a través de la misma organización de Adobe, utilizando su Adobe ID.

+ [AEM as a Cloud Service](#aem-as-a-cloud-service)
+ [FireFly del proyecto de Adobe](#adobe-project-firefly)
   + El aprovisionamiento puede tardar entre 2 y 10 días
+ Almacenamiento de nube
   + [Almacenamiento de blob de Azure](https://azure.microsoft.com/en-us/services/storage/blobs/)
   + o [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card)

>[!WARNING]
>
>Antes de continuar con este tutorial, asegúrese de tener acceso a todos los servicios mencionados anteriormente.
> 
> Consulte las secciones siguientes sobre cómo configurar y suministrar los servicios necesarios.

## AEM as a Cloud Service{#aem-as-a-cloud-service}

Se requiere acceso a un AEM como entorno de Cloud Service para configurar los Perfiles de procesamiento de AEM Assets a fin de que invoquen al trabajador de Asset compute personalizado.

Idealmente, hay un programa de simulación de pruebas o un entorno de desarrollo sin simulación de pruebas disponibles para su uso.

Tenga en cuenta que un SDK de AEM local no es suficiente para completar este tutorial, ya que el SDK de AEM local no puede comunicarse con los microservicios de Asset compute, sino que se requiere un AEM verdadero como entorno de Cloud Service.

## Repetición del proyecto de Adobe{#adobe-project-firefly}

La estructura [Adobe Project Firefly](https://www.adobe.io/apis/experienceplatform/project-firefly.html) se utiliza para generar e implementar acciones personalizadas en Adobe I/O Runtime, la plataforma sin servidor de Adobe. AEM proyectos de Asset compute son proyectos especialmente creados de Firefly que se integran con AEM Assets a través de Perfiles de procesamiento, y proporcionan la capacidad de acceder y procesar binarios de recursos.

Para obtener acceso a Project Firefly, regístrese en la previsualización.

1. [Regístrese para la previsualización](https://adobeio.typeform.com/to/obqgRm) de Project Firefly.
1. Espere entre 2 y 10 días hasta que se le notifique por correo electrónico que está aprovisionado antes de continuar con el tutorial.
   + Si no está seguro de si ha sido aprovisionado, continúe con los pasos siguientes y si no puede crear un proyecto __de Refiebres de proyecto__ en [Consola de programadores de Adobe](https://console.adobe.io) aún no se le ha suministrado.

## Almacenamiento de nube

El almacenamiento de nube es necesario para el desarrollo local de proyectos de Asset compute.

Cuando los trabajadores de la Asset compute se implementan en Adobe I/O Runtime para uso directo de AEM como Cloud Service, este almacenamiento de nube no es estrictamente necesario, ya que AEM proporciona el almacenamiento de nube desde el cual se lee el recurso y se escribe la representación.

### Almacenamiento blob de Microsoft Azure{#azure-blob-storage}

Si todavía no tiene acceso al Almacenamiento de blob de Microsoft Azure, regístrese para obtener una cuenta [gratuita de 12 meses](https://azure.microsoft.com/en-us/free/).

Este tutorial usará el Almacenamiento de blob de Azure, pero [Amazon S3](#amazon-s3) también se puede usar con variaciones menores en el tutorial.

>[!VIDEO](https://video.tv.adobe.com/v/40377/?quality=12&learn=on)

_Pulsación del aprovisionamiento del Almacenamiento de blob de Azure (sin audio)_


1. Inicie sesión en su [cuenta de Microsoft Azure](https://azure.microsoft.com/en-us/account/).
1. Vaya a la sección __Cuentas de Almacenamiento__ Servicios de Azure
1. Toque __+ Añadir__ para crear una nueva cuenta de Almacenamiento Blob
1. Cree un nuevo __grupo de recursos__ según sea necesario, por ejemplo: `aem-as-a-cloud-service`
1. Proporcione un __nombre de cuenta de Almacenamiento__, por ejemplo: `aemguideswkndassetcomput`
   + El __nombre de cuenta de Almacenamiento__ se utilizará para [configurar el almacenamiento de nube](../develop/environment-variables.md) para la herramienta de desarrollo de Assets computes local
   + Las __claves de acceso__ asociadas con la cuenta de almacenamiento también son necesarias cuando [configura el almacenamiento de nube](../develop/environment-variables.md).
1. Deje todo lo demás como predeterminado y toque el botón __Revisar + crear__
   + Si lo desea, seleccione la __ubicación__ cercana.
1. Revise la solicitud de aprovisionamiento para comprobar si es correcta y toque el botón __Crear__ si está satisfecho

### Amazon S3{#amazon-s3}

Se recomienda utilizar [Almacenamiento de blob de Microsoft Azure](#azure-blob-storage) para completar este tutorial, pero también se puede utilizar [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card).

Si utiliza el almacenamiento Amazon S3, especifique las credenciales del almacenamiento en la nube Amazon S3 cuando [configure las variables de entorno del proyecto](../develop/environment-variables.md#amazon-s3).

Si necesita aprovisionar almacenamiento en la nube especialmente para este tutorial, le recomendamos utilizar [Almacenamiento de blob de Azure](#azure-blob-storage).
