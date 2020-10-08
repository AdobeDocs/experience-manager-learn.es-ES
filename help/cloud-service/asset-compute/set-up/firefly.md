---
title: Configuración de la migración de proyectos de Adobe para la extensibilidad de cómputo de recursos
description: Los proyectos de Asset Compute son proyectos de Adobe Project Firefly especialmente definidos y, como tales, requieren acceso a Adobe Project Firefly en la consola de desarrolladores de Adobe para configurarlos e implementarlos.
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

Los proyectos de Asset Compute son proyectos de Adobe Project Firefly especialmente definidos y, como tales, requieren acceso a Adobe Project Firefly en la consola de desarrolladores de Adobe para configurarlos e implementarlos.

## Creación y configuración de la luciérnaga de proyectos de Adobe en Adobe Developer Console{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183/?quality=12&learn=on)

_Pulsaciones para configurar la luciérnaga del proyecto de Adobe (sin audio)_

1. Inicie sesión en [Adobe Developer Console](https://console.adobe.io) con el Adobe ID asociado a las [cuentas y servicios](./accounts-and-services.md)aprovisionados. Asegúrese de que es administrador ____ del sistema o de que está en la función __de__ desarrollador de la organización de Adobe correcta.
1. Crear un proyecto de Firefly tocando __Crear nuevo proyecto > Proyecto a partir de plantilla > Proyecto de Firefly__

   _Si el botón__ Crear nuevo proyecto __o el tipo__ Proyecto de búsqueda __no están disponibles, significa que la organización de Adobe no está[aprovisionada con Project Firefly](#request-adobe-project-firefly)._

   + __Título__ del proyecto: `WKND AEM Asset Compute`
   + __Nombre__ de la aplicación: `wkndAemAssetCompute<YourName>`
      + El nombre __de la__ aplicación debe ser único en todos los proyectos de Firefly y no se puede modificar posteriormente. Prefijar el nombre de la compañía u organización y postfijar con un sufijo significativo es un buen método, como por ejemplo: `wkndAemAssetCompute`.
      + Para la auto-activación, a menudo es mejor posponer el nombre en el nombre __de la__ aplicación, como `wkndAemAssetComputeJaneDoe` para evitar conflictos con otros proyectos de Project Firefly.
   + En __Workspaces__ , agregue un nuevo entorno con el nombre `Development`
   + En __Adobe I/O Runtime__ , asegúrese de que la opción __Incluir tiempo de ejecución con cada espacio de trabajo__ está seleccionada
   + Toque __Guardar__ para guardar el proyecto
1. En el proyecto Adobe Firefly, seleccione `Development` en el selector de espacio de trabajo
1. Toque __+ Añadir servicio > API__ para abrir el asistente para __Añadir una API__ , utilice este método para agregar las siguientes API:

   + __Experience Cloud > Cálculo de recursos__
      + Seleccione __Generar un par__ de claves y toque el botón __Generar par__ de claves, y guarde la descarga `config.zip` en una ubicación segura para utilizarla [posteriormente](#private-key)
      + Puntee __Siguiente__
      + Seleccione las __integraciones de perfil de productos: Cloud Service__ y toque __Guardar la API configurada__
   + __Servicios de Adobe > Eventos__ de E/S y toque __Guardar API configurada__
   + __Servicios de Adobe > API__ de administración de E/S y toque __Guardar API configurada__

## Acceso a private.key{#private-key}

Al configurar la integración [de API de cálculo de](#set-up) recursos, se generó un nuevo par de claves y se descargó automáticamente un `config.zip` archivo. Esto `config.zip` contiene el certificado público generado y el `private.key` archivo coincidente.

1. Descomprima `config.zip` en un lugar seguro del sistema de archivos, ya que el `private.key` se [utiliza más tarde](../develop/environment-variables.md)
   + Los secretos y las claves privadas nunca deben añadirse a Git como cuestión de seguridad.

## Revisar las credenciales de la cuenta de servicio (JWT)

Las credenciales de este proyecto de E/S de Adobe son utilizadas por la herramienta [local de desarrollo de cómputo de](../develop/development-tool.md) recursos para interactuar con Adobe I/O Runtime y deberán incorporarse al proyecto de cómputo de recursos. Familiarícese con las credenciales de la cuenta de servicio (JWT).

![Credenciales de cuenta de Adobe Developer Service](./assets/firefly/service-account.png)

1. En el proyecto de búsqueda de proyectos de E/S de Adobe, asegúrese de que el espacio de trabajo está seleccionado `Development`
1. Puntee en Cuenta __de servicio (JWT)__ en __Credenciales__
1. Revisar las credenciales de E/S de Adobe mostradas
   + La clave ____ pública que aparece en la parte inferior tiene su contraparte __private.key__ en la `config.zip` descarga cuando se agregó a este proyecto la API __de cómputo de__ recursos.
      + Si la clave privada se pierde o se ve comprometida, se puede eliminar la clave pública coincidente y se puede generar un nuevo par de claves en la E/S de Adobe o cargarla en ella mediante esta interfaz.
