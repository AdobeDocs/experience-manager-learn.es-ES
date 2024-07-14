---
title: Configuración de App Builder para la extensibilidad de la Asset compute
description: Los proyectos de asset compute son proyectos de App Builder especialmente definidos y, como tales, requieren acceso a App Builder en Adobe Developer Console para configurarlos e implementarlos.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6268
thumbnail: 40183.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 2b1d8786-592e-41f2-80cc-bc0b1c7e1b49
duration: 197
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '538'
ht-degree: 0%

---

# Configuración de App Builder

Los proyectos de asset compute son proyectos de App Builder especialmente definidos y, como tales, requieren acceso a App Builder en Adobe Developer Console para configurarlos e implementarlos.

## Creación y configuración de App Builder en Adobe Developer Console{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183?quality=12&learn=on)

_Pulsación al configurar el Generador de aplicaciones (sin audio)_

1. Inicie sesión en [Adobe Developer Console](https://console.adobe.io) con el Adobe ID asociado con las [cuentas y servicios](./accounts-and-services.md) proporcionados. Asegúrese de que es __Administrador del sistema__ o que se encuentra en la __Función de desarrollador__ para la organización de Adobe correcta.
1. Para crear un proyecto de App Builder, pulse __Crear nuevo proyecto > Proyecto a partir de la plantilla > App Builder__

   _Si el botón__ Crear nuevo proyecto __o el tipo__ App Builder __no están disponibles, significa que su organización de Adobe no está [aprovisionada con App Builder](#request-adobe-project-app-builder)._

   + __Título del proyecto__: `WKND AEM Asset Compute`
   + __Nombre de aplicación__: `wkndAemAssetCompute<YourName>`
      + __App name__ debe ser único en todos los proyectos de FApp Builderirefly y no se puede modificar posteriormente. Prefijar el nombre de su empresa u organización y postfijar con un sufijo significativo es un buen enfoque, como: `wkndAemAssetCompute`.
      + Para la autohabilitación, a menudo es mejor postfijar su nombre en __Nombre de aplicación__, como `wkndAemAssetComputeJaneDoe` para evitar conflictos con otros proyectos de App Builder.
   + En __Espacios de trabajo__, agregue un nuevo entorno denominado `Development`
   + En __Adobe I/O Runtime__, asegúrese de que está seleccionado __Incluir tiempo de ejecución con cada espacio de trabajo__
   + Pulse __Guardar__ para guardar el proyecto
1. En el proyecto de App Builder, seleccione `Development` del selector del área de trabajo
1. Pulse __+ Agregar servicio > API__ para abrir el asistente __Agregar una API__; use este método para agregar las siguientes API:

   + __Experience Cloud > Asset compute__
      + Seleccione __Generar un par de claves__ y pulse el botón __Generar par de claves__, y guarde el(la) `config.zip` descargado(a) en una ubicación segura para [utilizarlo(a) más adelante](#private-key)
      + Pulse __Siguiente__
      + Seleccione el perfil de producto __Integraciones - Cloud Service__ y pulse __Guardar API configurada__
   + __Servicios de Adobe > Eventos de E/S__ y pulsa __Guardar la API configurada__
   + __Servicios de Adobe > API de administración de E/S__ y pulsa __Guardar la API configurada__

## Acceda a la clave privada{#private-key}

Al configurar la [integración de API de Asset compute](#set-up), se generó un nuevo par de claves y se descargó automáticamente un archivo de `config.zip`. Este(a) `config.zip` contiene el certificado público generado y el archivo `private.key` correspondiente.

1. Descomprima `config.zip` en un lugar seguro de su sistema de archivos, ya que `private.key` se [utilizará más adelante](../develop/environment-variables.md)
   + Los secretos y las claves privadas nunca deben agregarse a Git por motivos de seguridad.

## Revise las credenciales de la cuenta de servicio (JWT)

La [Herramienta de desarrollo de Assets computes](../develop/development-tool.md) local usa las credenciales de este proyecto de Adobe I/O para interactuar con Adobe I/O Runtime y tendrá que incorporarse al proyecto de Asset compute. Familiarícese con las credenciales de la cuenta de servicio (JWT).

![Credenciales de cuenta de servicio de Adobe Developer](./assets/app-builder/service-account.png)

1. En el proyecto App Builder del proyecto de Adobe I/O, asegúrese de que el área de trabajo `Development` esté seleccionado
1. Toque en __Cuenta de servicio (JWT)__ en __Credenciales__
1. Revisar las credenciales de Adobe I/O mostradas
   + La __clave pública__ que aparece en la parte inferior tiene su equivalente de __clave privada__ en la `config.zip` descargada cuando se agregó la __API de Asset compute__ a este proyecto.
      + Si la clave privada se pierde o se pone en peligro, se puede eliminar la clave pública coincidente y se puede generar un nuevo par de claves en el Adobe I/O o cargar en él mediante esta interfaz.
