---
title: Configurar la búsqueda del proyecto de Adobe para la extensibilidad de la Asset compute
description: Los proyectos de asset compute son proyectos de Adobe definidos especialmente para proyectos de Firefly y, como tales, requieren acceso a Adobe Project Firefly en la consola de desarrolladores de Adobe para configurarlos e implementarlos.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6268
thumbnail: 40183.jpg
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '545'
ht-degree: 0%

---


# Configurar el proyecto de Adobe Firefly

Los proyectos de asset compute son proyectos de Adobe definidos especialmente para proyectos de Firefly y, como tales, requieren acceso a Adobe Project Firefly en la consola de desarrolladores de Adobe para configurarlos e implementarlos.

## Cree y configure Referencias de proyectos de Adobe en la Consola de programadores de Adobe{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183/?quality=12&learn=on)

_Pulsaciones para configurar la luciérnaga del proyecto de Adobe (sin audio)_

1. Inicie sesión en [Adobe Developer Console](https://console.adobe.io) con el Adobe ID asociado a las [cuentas y servicios](./accounts-and-services.md) aprovisionados. Asegúrese de que es __Administrador del sistema__ o está en la __Función de desarrollador__ para la organización de Adobe correcta.
1. Cree un proyecto de Firefly tocando __Crear nuevo proyecto > Proyecto a partir de plantilla > Proyecto de Firefly__

   _Si__ Crear nuevo __botón de proyecto o__ Proyecto __Fireflytype no está disponible, significa que la organización de Adobe no se  [aprovisiona con Project Firefly](#request-adobe-project-firefly)._

   + __Título__ del proyecto:  `WKND AEM Asset Compute`
   + __Nombre__ de la aplicación:  `wkndAemAssetCompute<YourName>`
      + El __nombre de la aplicación__ debe ser único en todos los proyectos de Firefly y no se puede modificar posteriormente. Prefijar el nombre de la compañía u organización y postfijar con un sufijo significativo es un buen método, como por ejemplo: `wkndAemAssetCompute`.
      + Para la auto-activación, a menudo es mejor postfijar su nombre en el __nombre de la aplicación__, como `wkndAemAssetComputeJaneDoe` para evitar conflictos con otros proyectos de Project Firefly.
   + En __Workspaces__ agregue un nuevo entorno denominado `Development`
   + En __Adobe I/O Runtime__ asegúrese de que __Incluir tiempo de ejecución con cada área de trabajo__ está seleccionada
   + Toque __Guardar__ para guardar el proyecto
1. En el proyecto Adobe Firefly, seleccione `Development` en el selector de espacio de trabajo
1. Toque __+ Añadir servicio > API__ para abrir el asistente __Añadir una API__, utilice este método para agregar las siguientes API:

   + __Experience Cloud > Asset compute__
      + Seleccione __Generar un par de claves__ y toque el botón __Generar par de claves__ y guarde el `config.zip` descargado en una ubicación segura para [utilizarlo posteriormente](#private-key)
      + Toque __Siguiente__
      + Seleccione el perfil de productos __Integraciones - Cloud Service__ y toque __Guardar la API configurada__
   + __Servicios de Adobe >__ Eventos de E/S y toque  __Guardar API configurada__
   + __Servicios de Adobe >__ API de administración de E/S y toque  __Guardar API configurada__

## Acceda a private.key{#private-key}

Al configurar la [integración de API de Asset compute](#set-up) se generó un nuevo par de claves y se descargó automáticamente un archivo `config.zip`. Este `config.zip` contiene el certificado público generado y el archivo `private.key` correspondiente.

1. Descomprima `config.zip` en un lugar seguro del sistema de archivos, ya que `private.key` se [usa más tarde](../develop/environment-variables.md)
   + Los secretos y las claves privadas nunca deben añadirse a Git como cuestión de seguridad.

## Revisar las credenciales de la cuenta de servicio (JWT)

Las credenciales de este proyecto de Adobe I/O son utilizadas por la [Herramienta de desarrollo de Assets computes](../develop/development-tool.md) local para interactuar con Adobe I/O Runtime y deberán incorporarse al proyecto de Asset compute. Familiarícese con las credenciales de la cuenta de servicio (JWT).

![Credenciales de cuenta de Adobe Developer Service](./assets/firefly/service-account.png)

1. En el proyecto de Adobe I/O Project Firefly, asegúrese de que el espacio de trabajo `Development` está seleccionado
1. Toque en __Cuenta de servicio (JWT)__ en __Credenciales__
1. Revisar las credenciales de Adobe I/O mostradas
   + La __clave pública__ que aparece en la parte inferior tiene su contrapartida __private.key__ en la `config.zip` descargada cuando se agregó la __API de Asset compute__ a este proyecto.
      + Si la clave privada se pierde o se ve comprometida, se puede eliminar la clave pública coincidente y se puede generar un nuevo par de claves en Adobe I/O o cargarlas a mediante esta interfaz.
