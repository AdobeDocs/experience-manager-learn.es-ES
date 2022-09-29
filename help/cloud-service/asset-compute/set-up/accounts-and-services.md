---
title: Configuración de cuentas y servicios para la extensibilidad del Asset compute
description: El desarrollo de los trabajadores de Asset compute requiere acceso a cuentas y servicios, incluidos AEM as a Cloud Service, App Builder y almacenamiento en la nube, que proporciona Microsoft o Amazon.
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
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '621'
ht-degree: 1%

---

# Configuración de cuentas y servicios

Este tutorial requiere que los siguientes servicios sean de aprovisionamiento y accesibles a través del Adobe ID del alumno.

Se debe acceder a todos los servicios de Adobe a través de la misma organización de Adobe y mediante Adobe ID.

+ [AEM as a Cloud Service](#aem-as-a-cloud-service)
+ [Creador de aplicaciones](#app-builder)
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

Se requiere acceso a un entorno as a Cloud Service AEM para configurar los perfiles de procesamiento de AEM Assets para que invoquen al programa de trabajo de Asset compute personalizado.

Lo ideal es que haya un programa de simulación de pruebas o un entorno de desarrollo que no sea de simulación de pruebas disponibles para su uso.

Tenga en cuenta que un SDK de AEM local no es suficiente para completar este tutorial, ya que el SDK de AEM local no puede comunicarse con los microservicios de Asset compute, por lo que se requiere un entorno as a Cloud Service AEM verdadero.

## Creador de aplicaciones{#app-builder}

La variable [Creador de aplicaciones](https://developer.adobe.com/app-builder/) framework se utiliza para crear e implementar acciones personalizadas en Adobe I/O Runtime, la plataforma sin servidor de Adobe. AEM proyectos de Asset compute son proyectos de App Builder creados especialmente que se integran con AEM Assets mediante Perfiles de procesamiento y permiten acceder y procesar archivos binarios de recursos.

Para obtener acceso a App Builder, regístrese en la vista previa.

1. [Regístrese para obtener una versión de prueba de App Builder](https://developer.adobe.com/app-builder/trial/).
1. Espere aproximadamente 2 a 10 días hasta que se le notifique por correo electrónico que está aprovisionado antes de continuar con el tutorial.
   + Si no está seguro de si ha sido aprovisionado, continúe con los siguientes pasos y si no puede crear un __Creador de aplicaciones__ proyecto en [Consola de Adobe Developer](https://developer.adobe.com/console/) todavía no se ha aprovisionado.

## Almacenamiento en la nube

Se requiere almacenamiento en la nube para el desarrollo local de proyectos de Asset compute.

Cuando los Assets computes se implementan en Adobe I/O Runtime para su uso directo por parte de AEM as a Cloud Service, este almacenamiento en la nube no es estrictamente necesario, ya que AEM proporciona el almacenamiento en la nube desde el que se lee el recurso y se escribe su representación.

### Almacenamiento de Microsoft Azure Blob{#azure-blob-storage}

Si todavía no tiene acceso al almacenamiento de blob de Microsoft Azure, regístrese para obtener un [cuenta gratuita de 12 meses](https://azure.microsoft.com/en-us/free/).

Sin embargo, este tutorial utilizará Azure Blob Storage. [Amazon S3](#amazon-s3) también se puede utilizar solo una variación menor al tutorial.

>[!VIDEO](https://video.tv.adobe.com/v/40377/?quality=12&learn=on)

_Pulsación del aprovisionamiento del almacenamiento del blob de Azure (sin audio)_

1. Inicie sesión en su [Cuenta de Microsoft Azure](https://azure.microsoft.com/en-us/account/).
1. Vaya a la __Cuentas de almacenamiento__ Sección de servicios de Azure
1. Toque __+ Agregar__ para crear una nueva cuenta de almacenamiento de blob
1. Cree una nueva __Grupo de recursos__ según sea necesario, por ejemplo: `aem-as-a-cloud-service`
1. Proporcione un __Nombre de la cuenta de almacenamiento__, por ejemplo: `aemguideswkndassetcomput`
   + La variable __Nombre de la cuenta de almacenamiento__  usado para [configuración del almacenamiento en la nube](../develop/environment-variables.md) en la herramienta de desarrollo del Asset compute local
   + La variable __Teclas de acceso__ asociados a la cuenta de almacenamiento también son necesarios cuando [configuración del almacenamiento en la nube](../develop/environment-variables.md).
1. Deje todo lo demás como predeterminado y pulse el botón __Revisar + crear__ botón
   + De forma opcional, seleccione la __ubicación__ cerca de usted.
1. Revise la solicitud de aprovisionamiento para comprobar si es correcta y pulse __Crear__ botón si está satisfecho

### Amazon S3{#amazon-s3}

Uso [Almacenamiento de Microsoft Azure Blob](#azure-blob-storage) no obstante, se recomienda completar este tutorial. [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card) también se puede utilizar.

Si utiliza el almacenamiento de Amazon S3, especifique las credenciales de almacenamiento en la nube de Amazon S3 cuando [configuración de las variables de entorno del proyecto](../develop/environment-variables.md#amazon-s3).

Si necesita aprovisionar almacenamiento en la nube especialmente para este tutorial, recomendamos utilizar [Almacenamiento de Azure Blob](#azure-blob-storage).
