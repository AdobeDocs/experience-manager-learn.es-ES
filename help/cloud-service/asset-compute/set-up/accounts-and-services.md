---
title: Configuración de cuentas y servicios para la extensibilidad de Asset Compute
description: El desarrollo de los trabajadores de Asset Compute requiere acceso a cuentas y servicios, incluido AEM as a Cloud Service, App Builder y almacenamiento en la nube, que proporciona Microsoft o Amazon.
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-6264
thumbnail: 40377.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 707657ad-221e-4dab-ac2a-46a4fcbc55bc
duration: 212
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '592'
ht-degree: 2%

---

# Configuración de cuentas y servicios

Este tutorial requiere que los siguientes servicios estén aprovisionados y sean accesibles a través del Adobe ID del alumno.

Todos los servicios de Adobe deben ser accesibles a través de la misma organización de Adobe con Adobe ID.

+ [AEM as a Cloud Service](#aem-as-a-cloud-service)
+ [App Builder](#app-builder)
   + El aprovisionamiento puede tardar entre 2 y 10 días
+ Almacenamiento en la nube
   + [Almacenamiento de blob de Azure](https://azure.microsoft.com/en-us/services/storage/blobs/)
   + o [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&trk=ft_card)

>[!WARNING]
>
>Asegúrese de tener acceso a todos los servicios mencionados antes de continuar con este tutorial.
> 
> Consulte las secciones siguientes sobre cómo configurar y proporcionar los servicios necesarios.

## AEM as a Cloud Service{#aem-as-a-cloud-service}

Se requiere acceso a un entorno de AEM as a Cloud Service para configurar los perfiles de procesamiento de los AEM Assets para invocar el trabajador de Asset Compute personalizado.

Lo ideal es que haya disponible un programa de zona protegida o un entorno de desarrollo que no sea de zona protegida para su uso.

Tenga en cuenta que un SDK local de AEM no es suficiente para completar este tutorial, ya que el SDK local de AEM no se puede comunicar con los microservicios de Asset Compute, sino que se requiere un entorno de AEM as a Cloud Service verdadero.

## App Builder{#app-builder}

El módulo [App Builder](https://developer.adobe.com/app-builder/) se usa para generar e implementar acciones personalizadas en Adobe I/O Runtime, la plataforma sin servidor de Adobe. Los proyectos de AEM Asset Compute son proyectos de App Builder especialmente creados que se integran con los AEM Assets a través de Perfiles de procesamiento y proporcionan la capacidad de acceder y procesar binarios de recursos.

Para obtener acceso a App Builder, regístrese en la vista previa.

1. [Regístrese para la versión de prueba de App Builder](https://developer.adobe.com/app-builder/trial/).
1. Espere aproximadamente de 2 a 10 días hasta que se le notifique por correo electrónico que se le ha aprovisionado antes de continuar con el tutorial.
   + Si no está seguro de haber sido aprovisionado, continúe con los siguientes pasos y, si no puede crear un proyecto de __App Builder__ en [Adobe Developer Console](https://developer.adobe.com/console/), aún no lo ha sido.

## Almacenamiento en la nube

Se requiere almacenamiento en la nube para el desarrollo local de proyectos de Asset Compute.

Cuando los trabajadores de Asset Compute se implementan en Adobe I/O Runtime para su uso directo por parte de AEM as a Cloud Service, este almacenamiento en la nube no es estrictamente necesario, ya que AEM proporciona el almacenamiento en la nube desde el que se lee el recurso y en el que se escribe la representación.

### Almacenamiento del Blob de Microsoft Azure{#azure-blob-storage}

Si todavía no tiene acceso al almacenamiento del blob de Microsoft Azure, regístrese para obtener una [cuenta gratuita de 12 meses](https://azure.microsoft.com/en-us/free/).

Este tutorial usará Azure Blob Storage, pero [Amazon S3](#amazon-s3) también se puede usar como una variación menor del tutorial.

>[!VIDEO](https://video.tv.adobe.com/v/40377?quality=12&learn=on)

_Clic en el aprovisionamiento del almacenamiento del blob de Azure (sin audio)_

1. Inicie sesión en su [cuenta de Microsoft Azure](https://azure.microsoft.com/en-us/account/).
1. Vaya a la sección __Cuentas de almacenamiento__ servicios de Azure
1. Pulse __+ Agregar__ para crear una nueva cuenta de almacenamiento de blob
1. Cree un nuevo __grupo de recursos__ según sea necesario, por ejemplo: `aem-as-a-cloud-service`
1. Proporcione un __nombre de cuenta de almacenamiento__, por ejemplo: `aemguideswkndassetcomput`
   + El __nombre de cuenta de almacenamiento__ usado para [configurar el almacenamiento en la nube](../develop/environment-variables.md) en la herramienta de desarrollo de Asset Compute local
   + Las __claves de acceso__ asociadas con la cuenta de almacenamiento también son necesarias al [configurar el almacenamiento en la nube](../develop/environment-variables.md).
1. Deje todo lo demás como predeterminado y pulse el botón __Revisar + crear__
   + Si lo desea, seleccione la __ubicación__ cercana a usted.
1. Revise la solicitud de aprovisionamiento para comprobar que es correcta y pulse el botón __Crear__ si está satisfecho

### Amazon S3{#amazon-s3}

Se recomienda usar [Microsoft Azure Blob Storage](#azure-blob-storage) para completar este tutorial, aunque también se puede usar [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&trk=ft_card).

Si usa el almacenamiento de Amazon S3, especifique las credenciales del almacenamiento en la nube de Amazon S3 al [configurar las variables de entorno del proyecto](../develop/environment-variables.md#amazon-s3).

Si necesita aprovisionar el almacenamiento en la nube especialmente para este tutorial, le recomendamos que use [Azure Blob Storage](#azure-blob-storage).
