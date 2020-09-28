---
title: Configuración de cuentas y servicios para la extensibilidad de Asset Compute
description: Para desarrollar los trabajadores de Asset Compute, es necesario tener acceso a cuentas y servicios, incluido AEM como Cloud Service, Adobe Project Firefly y almacenamiento en la nube, que proporciona Microsoft o Amazon.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6264
thumbnail: 40377.jpg
translation-type: tm+mt
source-git-commit: 9cf01dbf9461df4cc96d5bd0a96c0d4d900af089
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

Se requiere acceso a un AEM como entorno de Cloud Service para configurar los Perfiles de procesamiento de AEM Assets a fin de que invoquen la aplicación de cálculo de recursos personalizada.

Idealmente, hay un programa de simulación de pruebas o un entorno de desarrollo sin simulación de pruebas disponibles para su uso.

Tenga en cuenta que un SDK de AEM local no es suficiente para completar este tutorial, ya que el SDK de AEM local no puede comunicarse con los microservicios de Asset Compute, sino que se requiere un AEM verdadero como entorno de Cloud Service.

## Luciérnagas del proyecto Adobe{#adobe-project-firefly}

El módulo [Adobe Project Firefly](https://www.adobe.io/apis/experienceplatform/project-firefly.html) se utiliza para crear e implementar aplicaciones personalizadas en Adobe I/O Runtime, la plataforma sin servidor de Adobe. Las aplicaciones de AEM Asset Compute son aplicaciones de Firefly especialmente creadas que se integran con AEM Assets a través de Perfiles de procesamiento y proporcionan la capacidad de acceder y procesar binarios de recursos.

Para obtener acceso a Project Firefly, regístrese en la previsualización.

1. [Regístrese para la previsualización](https://adobeio.typeform.com/to/obqgRm)de Project Firefly.
1. Espere entre 2 y 10 días hasta que se le notifique por correo electrónico que está aprovisionado antes de continuar con el tutorial.
   + Si no está seguro de si ha sido aprovisionado, continúe con los pasos siguientes y si no puede crear un proyecto __de Repetición__ de proyecto en la consola [de desarrollador de](https://console.adobe.io) Adobe, aún no se le ha aprovisionado.

## Almacenamiento de nube

El almacenamiento de nube es necesario para el desarrollo local de aplicaciones de Asset Compute.

Cuando se implementan aplicaciones de Asset Compute en Adobe I/O Runtime para uso directo de AEM como Cloud Service, este almacenamiento de nube no es estrictamente necesario, ya que AEM proporciona el almacenamiento de nube desde el cual se lee el recurso y se escribe la representación.

### Almacenamiento de blob de Microsoft Azure{#azure-blob-storage}

Si todavía no tiene acceso al Almacenamiento de blob de Microsoft Azure, regístrese para obtener una cuenta [de 12 meses](https://azure.microsoft.com/en-us/free/)gratuita.

Este tutorial usará el Almacenamiento de blob de Azure, pero [Amazon S3](#amazon-s3) solo se puede usar una variación menor del tutorial.

>[!VIDEO](https://video.tv.adobe.com/v/40377/?quality=12&learn=on)
_Pulsación del aprovisionamiento del Almacenamiento de blob de Azure (sin audio)_


1. Inicie sesión en su cuenta [de](https://azure.microsoft.com/en-us/account/)Microsoft Azure.
1. Vaya a la sección Servicios de Azure de __Almacenamiento Accounts__
1. Toque __+ Añadir__ para crear una nueva cuenta de Almacenamiento de Blob
1. Cree un nuevo grupo ____ de recursos según sea necesario, por ejemplo: `aem-as-a-cloud-service`
1. Proporcione un nombre __de cuenta de__ Almacenamiento, por ejemplo: `aemguideswkndassetcomput`
   + El nombre __de cuenta de__ Almacenamiento se utilizará para [configurar el almacenamiento](../develop/environment-variables.md) de nube para la herramienta de desarrollo de cómputo de recursos local
   + Las claves __de__ acceso asociadas con la cuenta de almacenamiento también son necesarias al [configurar el almacenamiento](../develop/environment-variables.md)de nube.
1. Deje todo lo demás como predeterminado y toque el botón __Revisar + crear__ .
   + Si lo desea, seleccione la __ubicación__ que se encuentra cerca.
1. Revise la solicitud de aprovisionamiento para comprobar si es correcta y toque el botón __Crear__ si está satisfecho

### Amazon S3{#amazon-s3}

Se recomienda utilizar [Microsoft Azure Blob Almacenamiento](#azure-blob-storage) para completar este tutorial, pero también se puede utilizar [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card) .

Si utiliza el almacenamiento Amazon S3, especifique las credenciales del almacenamiento en la nube Amazon S3 al [configurar las variables](../develop/environment-variables.md#amazon-s3)de entorno del proyecto.

Si necesita aprovisionar el almacenamiento en la nube especialmente para este tutorial, le recomendamos que utilice [Azure Blob Almacenamiento](#azure-blob-storage).
