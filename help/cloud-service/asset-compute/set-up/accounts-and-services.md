---
title: Configurar cuentas y servicios para la extensibilidad del Asset compute
description: Para desarrollar los Asset compute AEM, es necesario tener acceso a cuentas y servicios, incluido el almacenamiento as a Cloud Service, el App Builder y el almacenamiento en la nube que proporciona Microsoft o Amazon.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6264
thumbnail: 40377.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 707657ad-221e-4dab-ac2a-46a4fcbc55bc
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '621'
ht-degree: 2%

---

# Configuración de cuentas y servicios

Este tutorial requiere que los siguientes servicios estén aprovisionados y sean accesibles a través del Adobe ID del alumno.

Todos los servicios de Adobe deben ser accesibles a través de la misma organización de Adobe con Adobe ID.

+ [AEM as a Cloud Service](#aem-as-a-cloud-service)
+ [Generador de aplicaciones](#app-builder)
   + El aprovisionamiento puede tardar entre 2 y 10 días
+ Almacenamiento en la nube
   + [Almacenamiento de Azure Blob](https://azure.microsoft.com/en-us/services/storage/blobs/)
   + o [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card)

>[!WARNING]
>
>Asegúrese de tener acceso a todos los servicios mencionados antes de continuar con este tutorial.
> 
> Consulte las secciones siguientes sobre cómo configurar y proporcionar los servicios necesarios.

## AEM as a Cloud Service{#aem-as-a-cloud-service}

AEM Se requiere acceso a un entorno as a Cloud Service de para configurar los perfiles de procesamiento de AEM Assets para invocar el trabajador de Asset compute personalizado.

Lo ideal es que haya disponible un programa de zona protegida o un entorno de desarrollo que no sea de zona protegida para su uso.

AEM AEM Tenga en cuenta que un SDK de la local no es suficiente para completar este tutorial, ya que el SDK de la local no puede comunicarse con los microservicios de Asset compute AEM, sino que se requiere un entorno de verdadero as a Cloud Service.

## Generador de aplicaciones{#app-builder}

El [Generador de aplicaciones](https://developer.adobe.com/app-builder/) Este módulo se utiliza para crear e implementar acciones personalizadas en Adobe I/O Runtime, la plataforma sin servidor de Adobe. AEM Los proyectos de Asset compute de recursos son proyectos de App Builder especialmente creados que se integran con AEM Assets a través de Perfiles de procesamiento y proporcionan la capacidad de acceder y procesar binarios de recursos.

Para obtener acceso al Generador de aplicaciones, regístrese en la vista previa.

1. [Regístrese para la versión de prueba del App Builder](https://developer.adobe.com/app-builder/trial/).
1. Espere aproximadamente de 2 a 10 días hasta que se le notifique por correo electrónico que se le ha aprovisionado antes de continuar con el tutorial.
   + Si no está seguro de haber sido aprovisionado, continúe con los siguientes pasos y si no puede crear un __Generador de aplicaciones__ proyecto en [Consola de Adobe Developer](https://developer.adobe.com/console/) aún no se ha aprovisionado.

## Almacenamiento en la nube

Se requiere almacenamiento en la nube para el desarrollo local de proyectos de Asset compute.

Cuando los trabajadores de Asset compute se implementan en Adobe I/O Runtime AEM as a Cloud Service AEM para su uso directo por parte de los, este almacenamiento en la nube no es estrictamente necesario, ya que proporciona el almacenamiento en la nube desde el que se lee el recurso y en el que se escribe la representación.

### Almacenamiento del Blob de Microsoft Azure{#azure-blob-storage}

Si aún no tiene acceso al almacenamiento del blob de Microsoft Azure, regístrese en [cuenta gratuita de 12 meses](https://azure.microsoft.com/en-us/free/).

Sin embargo, este tutorial utilizará Azure Blob Storage [Amazon S3](#amazon-s3) también se puede utilizar solo para variaciones menores del tutorial.

>[!VIDEO](https://video.tv.adobe.com/v/40377?quality=12&learn=on)

_Pulsación del aprovisionamiento de Azure Blob Storage (sin audio)_

1. Inicie sesión en su [cuenta de Microsoft Azure](https://azure.microsoft.com/en-us/account/).
1. Vaya a __Cuentas de almacenamiento__ sección de servicios de Azure
1. Tocar __+ Agregar__ para crear una nueva cuenta de almacenamiento de blobs
1. Crear un nuevo __Grupo de recursos__ según sea necesario, por ejemplo: `aem-as-a-cloud-service`
1. Proporcione un __Nombre de cuenta de almacenamiento__, por ejemplo: `aemguideswkndassetcomput`
   + El __Nombre de cuenta de almacenamiento__  se usa para [configurar el almacenamiento en la nube](../develop/environment-variables.md) en la herramienta de desarrollo de Assets computes local
   + El __Claves de acceso__ asociadas a la cuenta de almacenamiento también son necesarias cuando [configurar el almacenamiento en la nube](../develop/environment-variables.md).
1. Deje todo lo demás como predeterminado y pulse el botón __Revisar + crear__ botón
   + Si lo desea, seleccione __ubicación__ cerca de ti.
1. Revise la solicitud de aprovisionamiento para comprobar si es correcta y pulse __Crear__ botón si está satisfecho

### Amazon S3{#amazon-s3}

Uso de [Almacenamiento del blob de Microsoft Azure](#azure-blob-storage) Sin embargo, se recomienda completar este tutorial [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card) también se puede utilizar.

Si utiliza el almacenamiento de Amazon S3, especifique las credenciales de almacenamiento en la nube de Amazon S3 cuando [configuración de las variables de entorno del proyecto](../develop/environment-variables.md#amazon-s3).

Si necesita aprovisionar el almacenamiento en la nube especialmente para este tutorial, recomendamos utilizar [Almacenamiento de Azure Blob](#azure-blob-storage).
