---
title: Configuración de cuentas y servicios para la extensibilidad de Asset Compute
description: El desarrollo de los trabajadores de Asset Compute requiere acceso a cuentas y servicios, incluido AEM as a Cloud Service, Adobe Project Firefly y almacenamiento en la nube, que proporciona Microsoft o Amazon.
feature: Asset Compute Microservices
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6264
thumbnail: 40377.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '635'
ht-degree: 2%

---


# Configuración de cuentas y servicios

Este tutorial requiere que los siguientes servicios sean de aprovisionamiento y accesibles a través del Adobe ID del estudiante.

Se debe acceder a todos los servicios de Adobe a través de la misma organización de Adobe con su Adobe ID.

+ [AEM as a Cloud Service](#aem-as-a-cloud-service)
+ [Adobe Project FireFly](#adobe-project-firefly)
   + El aprovisionamiento puede tardar entre 2 y 10 días
+ Almacenamiento en la nube
   + [Almacenamiento de Azure Blob](https://azure.microsoft.com/en-us/services/storage/blobs/)
   + o [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card)

>[!WARNING]
>
>Asegúrese de tener acceso a todos los servicios mencionados anteriormente, antes de continuar con este tutorial.
> 
> Consulte las secciones siguientes sobre cómo configurar y aprovisionar los servicios necesarios.

## AEM as a Cloud Service{#aem-as-a-cloud-service}

Se requiere acceso a un entorno de AEM as a Cloud Service para configurar los Perfiles de procesamiento de AEM Assets para que invoquen al trabajador personalizado de Asset Compute.

Lo ideal es que haya un programa de simulación de pruebas o un entorno de desarrollo que no sea de simulación de pruebas disponibles para su uso.

Tenga en cuenta que un SDK de AEM local no es suficiente para completar este tutorial, ya que el SDK de AEM local no puede comunicarse con los microservicios de Asset Compute, en su lugar se requiere un entorno de AEM as a Cloud Service verdadero.

## Adobe Project Firefly{#adobe-project-firefly}

El marco [Adobe Project Firefly](https://www.adobe.io/apis/experienceplatform/project-firefly.html) se utiliza para crear e implementar acciones personalizadas en Adobe I/O Runtime, la plataforma sin servidor de Adobe. Los proyectos de AEM Asset Compute son proyectos de Firefly especialmente creados que se integran con AEM Assets a través de Perfiles de procesamiento y ofrecen la capacidad de acceder y procesar binarios de recursos.

Para obtener acceso a Project Firefly, regístrese en la vista previa.

1. [Regístrese para la vista previa](https://adobeio.typeform.com/to/obqgRm) de Project Firefly.
1. Espere aproximadamente 2 a 10 días hasta que se le notifique por correo electrónico que está aprovisionado antes de continuar con el tutorial.
   + Si no está seguro de si ha sido aprovisionado, continúe con los siguientes pasos y si no puede crear un proyecto __Project Firefly__ en [Adobe Developer Console](https://console.adobe.io) aún no está aprovisionado.

## Almacenamiento en la nube

Se requiere almacenamiento en la nube para el desarrollo local de proyectos de Asset Compute.

Cuando los trabajadores de Asset Compute se implementan en Adobe I/O Runtime para uso directo de AEM as a Cloud Service, este almacenamiento en la nube no es estrictamente necesario, ya que AEM proporciona el almacenamiento en la nube desde el que se lee el recurso y se escribe en su representación.

### Almacenamiento de blob de Microsoft Azure{#azure-blob-storage}

Si todavía no tiene acceso al almacenamiento de blob de Microsoft Azure, regístrese para obtener una [cuenta gratuita de 12 meses](https://azure.microsoft.com/en-us/free/).

Este tutorial utilizará Azure Blob Storage, aunque [Amazon S3](#amazon-s3) solo se puede utilizar una variación menor en el tutorial.

>[!VIDEO](https://video.tv.adobe.com/v/40377/?quality=12&learn=on)

_Pulsación del aprovisionamiento del almacenamiento del blob de Azure (sin audio)_


1. Inicie sesión en su [cuenta de Microsoft Azure](https://azure.microsoft.com/en-us/account/).
1. Vaya a la sección __Cuentas de almacenamiento__ Servicios de Azure
1. Toque __+ Agregar__ para crear una nueva cuenta de almacenamiento de blob
1. Cree un nuevo __grupo de recursos__ según sea necesario, por ejemplo: `aem-as-a-cloud-service`
1. Proporcione un __Nombre de cuenta de almacenamiento__, por ejemplo: `aemguideswkndassetcomput`
   + El __nombre de cuenta de almacenamiento__ se utilizará para [configurar el almacenamiento en la nube](../develop/environment-variables.md) para la herramienta de desarrollo de Asset Compute local
   + Las __Claves de acceso__ asociadas con la cuenta de almacenamiento también son necesarias al [configurar el almacenamiento en la nube](../develop/environment-variables.md).
1. Deje todo lo demás como predeterminado y pulse el botón __Revisar + crear__
   + De forma opcional, seleccione la __ubicación__ próxima a usted.
1. Revise la solicitud de aprovisionamiento para ver si es correcta y pulse el botón __Crear__ si está satisfecho

### Amazon S3{#amazon-s3}

Se recomienda utilizar [Microsoft Azure Blob Storage](#azure-blob-storage) para completar este tutorial, pero también se puede utilizar [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card).

Si utiliza el almacenamiento de Amazon S3, especifique las credenciales de almacenamiento en la nube de Amazon S3 al [configurar las variables de entorno del proyecto](../develop/environment-variables.md#amazon-s3).

Si necesita aprovisionar almacenamiento en la nube especialmente para este tutorial, recomendamos utilizar [Azure Blob Storage](#azure-blob-storage).
