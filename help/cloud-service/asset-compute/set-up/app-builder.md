---
title: Configuración de App Builder para la extensibilidad de Asset compute
description: Los proyectos de asset compute son proyectos de App Builder especialmente definidos y, como tales, requieren acceso a App Builder en la consola de desarrollador de Adobe para configurarlos e implementarlos.
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
source-git-commit: eb6a7ef343a43000855f8d5cc69bde0fae81d3e6
workflow-type: tm+mt
source-wordcount: '539'
ht-degree: 0%

---

# Configuración de App Builder

Los proyectos de asset compute son proyectos de App Builder especialmente definidos y, como tales, requieren acceso a App Builder en la consola de desarrollador de Adobe para configurarlos e implementarlos.

## Creación y configuración de App Builder en Adobe Developer Console{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183/?quality=12&learn=on)

_Pulsación de la configuración de App Builder (sin audio)_

1. Iniciar sesión en [Adobe Developer Console](https://console.adobe.io) uso del Adobe ID asociado al aprovisionado [cuentas y servicios](./accounts-and-services.md). Asegúrese de que __Administrador del sistema__ o en el __Función de desarrollador__ para la organización de Adobe correcta.
1. Cree un proyecto de App Builder tocando __Crear nuevo proyecto > Proyecto desde plantilla > Creador de aplicaciones__

   _Si:__ Crear nuevo proyecto __o__ Creador de aplicaciones __no está disponible, esto significa que la organización de Adobe no [aprovisionado con App Builder](#request-adobe-project-app-builder)._

   + __Título del proyecto__: `WKND AEM Asset Compute`
   + __Nombre de la aplicación__: `wkndAemAssetCompute<YourName>`
      + La variable __Nombre de la aplicación__ debe ser único en todos los proyectos de Generador de aplicaciones y no se puede modificar posteriormente. Prefijar el nombre de su empresa u organización y posfijar con un sufijo significativo es un buen enfoque, como por ejemplo: `wkndAemAssetCompute`.
      + Para la auto-activación, a menudo es mejor posponer su nombre en la variable __Nombre de la aplicación__, como `wkndAemAssetComputeJaneDoe` para evitar conflictos con otros proyectos de App Builder.
   + En __Espacios de trabajo__ agregar un nuevo entorno denominado `Development`
   + En __Adobe I/O Runtime__ garantizar __Incluir tiempo de ejecución en cada espacio de trabajo__ está seleccionado
   + Toque __Guardar__ para guardar el proyecto
1. En el proyecto de App Builder, seleccione `Development` desde el selector de espacio de trabajo
1. Toque __+ Añadir servicio > API__ para abrir el __Añadir una API__ utilice este método para añadir las siguientes API:

   + __Experience Cloud > Asset compute__
      + Select __Generación de un par de claves__ y pulse __Generar par de teclas__ y guarde el `config.zip` a un lugar seguro para [uso posterior](#private-key)
      + Toque __Siguiente__
      + Seleccione el perfil de producto __Integraciones: Cloud Service__ y toque __Guardar la API configurada__
   + __Servicios de Adobe > Eventos de E/S__ y toque __Guardar la API configurada__
   + __Adobe Services > API de administración de E/S__ y toque __Guardar la API configurada__

## Acceda a private.key{#private-key}

Al configurar la variable [Integración de la API de asset compute](#set-up) se generó un nuevo par de claves y un `config.zip` se descargó automáticamente. Esta `config.zip` contiene el certificado público generado y la coincidencia `private.key` archivo.

1. Descartar `config.zip` a un lugar seguro en su sistema de archivos como `private.key` es [usado más tarde](../develop/environment-variables.md)
   + Los secretos y las claves privadas nunca deben añadirse a Git como cuestión de seguridad.

## Revisar las credenciales de la cuenta de servicio (JWT)

Las credenciales de este proyecto de Adobe I/O las usa el [Herramienta de desarrollo de assets computes](../develop/development-tool.md) para interactuar con Adobe I/O Runtime, y deberá incorporarse al proyecto de Asset compute. Familiarícese con las credenciales de la cuenta de servicio (JWT).

![Credenciales de cuenta de Adobe Developer Service](./assets/app-builder/service-account.png)

1. En el proyecto de Adobe I/O de Project App Builder, asegúrese de que la variable `Development` el espacio de trabajo está seleccionado
1. Toque en __Cuenta de servicio (JWT)__ under __Credenciales__
1. Revise las Credenciales de Adobe I/O mostradas
   + La variable __clave pública__ aparece en la parte inferior tiene es __private.key__ homólogo de la `config.zip` descargado cuando la variable __API de asset compute__ se ha añadido a este proyecto.
      + Si la clave privada se pierde o se ve comprometida, la clave pública coincidente se puede eliminar y se puede generar un nuevo par de claves en el Adobe I/O o cargarse en él mediante esta interfaz.
