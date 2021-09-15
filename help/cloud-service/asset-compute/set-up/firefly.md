---
title: Configuración del proyecto de Adobe Firefly para la extensibilidad del Asset compute
description: Los proyectos de asset compute son proyectos de Adobe especialmente definidos y como tales, requieren acceso a Proyecto de Adobe Firefly en la consola para desarrolladores de Adobe para configurarlos e implementarlos.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6268
thumbnail: 40183.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 2b1d8786-592e-41f2-80cc-bc0b1c7e1b49
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '545'
ht-degree: 0%

---

# Configuración del proyecto de Adobe Firefly

Los proyectos de asset compute son proyectos de Adobe especialmente definidos y como tales, requieren acceso a Proyecto de Adobe Firefly en la consola para desarrolladores de Adobe para configurarlos e implementarlos.

## Crear y configurar el proyecto de Adobe Firefly en Adobe Developer Console{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183/?quality=12&learn=on)

_Pulsación para configurar el proyecto de Adobe Firefly (sin audio)_

1. Inicie sesión en [Adobe Developer Console](https://console.adobe.io) utilizando el Adobe ID asociado con las [cuentas y servicios](./accounts-and-services.md) aprovisionados. Asegúrese de que es __administrador del sistema__ o está en __rol de desarrollador__ para la organización de Adobe correcta.
1. Para crear un proyecto de Firefly, pulse __Crear nuevo proyecto > Proyecto a partir de plantilla > Proyecto Firefly__

   _Si no está disponible__ Crear nuevo __botón de proyecto o__ Proyecto __Firefytype , significa que la organización de Adobe no está  [aprovisionada con Project Firefly](#request-adobe-project-firefly)._

   + __Título__ del proyecto:  `WKND AEM Asset Compute`
   + __Nombre__ de la aplicación:  `wkndAemAssetCompute<YourName>`
      + El __nombre de la aplicación__ debe ser único en todos los proyectos de Firefly y no se puede modificar posteriormente. Prefijar el nombre de su empresa u organización y posfijar con un sufijo significativo es un buen enfoque, como por ejemplo: `wkndAemAssetCompute`.
      + Para la auto-habilitación, a menudo es mejor posponer su nombre en el __Nombre de la aplicación__, como `wkndAemAssetComputeJaneDoe` para evitar conflictos con otros proyectos de Project Firefly.
   + En __Workspaces__ agregue un nuevo entorno denominado `Development`
   + En __Adobe I/O Runtime__ asegúrese de que __Include Runtime with each workspace__ está seleccionado
   + Toque __Guardar__ para guardar el proyecto
1. En el proyecto Adobe Firefly, seleccione `Development` en el selector de espacio de trabajo
1. Pulse __+ Añadir servicio > API__ para abrir el asistente __Añadir una API__, utilice este método para añadir las siguientes API:

   + __Experience Cloud > Asset compute__
      + Seleccione __Generate a key pair__ y pulse el botón __Generate keypair__ y guarde el `config.zip` descargado en una ubicación segura para [utilizarlo posteriormente](#private-key)
      + Toque __Siguiente__
      + Seleccione el perfil de producto __Integraciones: Cloud Service__ y pulse __Guardar API configurada__
   + __Servicios de Adobe > Eventos de__ E/S y pulse  __Guardar API configurada__
   + __Adobe Services > API de administración de__ E/S y pulse  __Guardar API configurada__

## Acceda a private.key{#private-key}

Al configurar la [integración de API de Asset compute](#set-up) se generó un nuevo par de claves y se descargó automáticamente un archivo `config.zip`. Este `config.zip` contiene el certificado público generado y el archivo `private.key` correspondiente.

1. Descomprima `config.zip` a un lugar seguro en su sistema de archivos, ya que `private.key` se [utiliza más adelante](../develop/environment-variables.md)
   + Los secretos y las claves privadas nunca deben añadirse a Git como cuestión de seguridad.

## Revisar las credenciales de la cuenta de servicio (JWT)

Las credenciales de este proyecto de Adobe I/O las utiliza la [Herramienta de desarrollo de Asset compute](../develop/development-tool.md) local para interactuar con Adobe I/O Runtime, y deberán incorporarse al proyecto de Asset compute. Familiarícese con las credenciales de la cuenta de servicio (JWT).

![Credenciales de cuenta de Adobe Developer Service](./assets/firefly/service-account.png)

1. En el proyecto Proyecto de Adobe I/O Firefly, asegúrese de que el espacio de trabajo `Development` esté seleccionado
1. Pulse en __Cuenta de servicio (JWT)__ en __Credenciales__
1. Revise las Credenciales de Adobe I/O mostradas
   + La __clave pública__ que aparece en la parte inferior tiene su __private.key__ contrapartida en la `config.zip` descargada cuando la __API de Asset compute__ se agregó a este proyecto.
      + Si la clave privada se pierde o se ve comprometida, la clave pública coincidente se puede eliminar y se puede generar un nuevo par de claves en el Adobe I/O o cargarse en él mediante esta interfaz.
