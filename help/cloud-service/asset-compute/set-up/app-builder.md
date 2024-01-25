---
title: Configuración del Generador de aplicaciones para la extensibilidad de Assets computes
description: Los proyectos de asset compute son proyectos de App Builder especialmente definidos y, como tales, requieren acceso a App Builder en la consola de Adobe Developer para configurarlos e implementarlos.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6268
thumbnail: 40183.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 2b1d8786-592e-41f2-80cc-bc0b1c7e1b49
duration: 230
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '538'
ht-degree: 0%

---

# Configuración del Generador de aplicaciones

Los proyectos de asset compute son proyectos de App Builder especialmente definidos y, como tales, requieren acceso a App Builder en la consola de Adobe Developer para configurarlos e implementarlos.

## Creación y configuración del Generador de aplicaciones en la consola de Adobe Developer{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183?quality=12&learn=on)

_Pulsación al configurar el Generador de aplicaciones (sin audio)_

1. Iniciar sesión en [Consola de Adobe Developer](https://console.adobe.io) uso del Adobe ID asociado al aprovisionado [cuentas y servicios](./accounts-and-services.md). Asegúrese de que es un __Administrador del sistema__ o en el __Función Desarrollador__ para la organización de Adobe correcta.
1. Para crear un proyecto del Generador de aplicaciones, pulse __Cree un nuevo proyecto > Proyecto desde plantilla > Generador de aplicaciones__

   _Si:__ Crear nuevo proyecto __o el botón__ Generador de aplicaciones __El tipo no está disponible, lo que significa que la organización de Adobe no [aprovisionado con App Builder](#request-adobe-project-app-builder)._

   + __Título del proyecto__: `WKND AEM Asset Compute`
   + __Nombre de aplicación__: `wkndAemAssetCompute<YourName>`
      + El __Nombre de aplicación__ debe ser único en todos los proyectos de FApp Builderirefly y no se puede modificar más adelante. Prefijar el nombre de su empresa u organización y postfijar con un sufijo significativo es un buen enfoque, como por ejemplo: `wkndAemAssetCompute`.
      + Para la autohabilitación, a menudo es mejor postfijar su nombre en la variable __Nombre de aplicación__, como `wkndAemAssetComputeJaneDoe` para evitar conflictos con otros proyectos de App Builder.
   + En __Workspaces__ añada un nuevo entorno llamado `Development`
   + En __Adobe I/O Runtime__ asegurar __Incluir Runtime con cada espacio de trabajo__ está seleccionado
   + Tocar __Guardar__ para guardar el proyecto
1. En el proyecto del Creador de aplicaciones, seleccione `Development` desde el selector de workspace
1. Tocar __+ Agregar servicio > API__ para abrir __Añadir una API__ , utilice este método para añadir las siguientes API:

   + __EXPERIENCE CLOUD > ASSET COMPUTE__
      + Seleccionar __Generación de un par de claves__ y pulse el botón __Generar par de claves__ y guarde el archivo descargado `config.zip` a una ubicación segura para [uso posterior](#private-key)
      + Tocar __Siguiente__
      + Seleccione el perfil de producto __Integraciones: Cloud Service__ y pulse __Guardar API configurada__
   + __Servicios de Adobe > Eventos de I/O__ y pulse __Guardar API configurada__
   + __Servicios de Adobe > API de administración de E/S__ y pulse __Guardar API configurada__

## Acceda a la clave privada{#private-key}

Al configurar el [Integración de API de Asset compute](#set-up) se generó un nuevo par de claves y se `config.zip` se descargó automáticamente. Esta `config.zip` contiene el certificado público generado y la coincidencia `private.key` archivo.

1. Descomprimir `config.zip` a un lugar seguro en su sistema de archivos como `private.key` es [se usa más tarde](../develop/environment-variables.md)
   + Los secretos y las claves privadas nunca deben agregarse a Git por motivos de seguridad.

## Revise las credenciales de la cuenta de servicio (JWT)

La entidad local utiliza las credenciales de este proyecto de Adobe I/O [Herramienta de desarrollo de asset compute](../develop/development-tool.md) para interactuar con Adobe I/O Runtime, y deberá incorporarse al proyecto de Asset compute. Familiarícese con las credenciales de la cuenta de servicio (JWT).

![Credenciales de cuenta de servicio de Adobe Developer](./assets/app-builder/service-account.png)

1. En el proyecto de Adobe I/O del Generador de aplicaciones del proyecto, asegúrese de que `Development` workspace está seleccionado
1. Tocar en __Cuenta de servicio (JWT)__ bajo __Credenciales__
1. Revisar las credenciales de Adobe I/O mostradas
   + El __clave pública__ listado en la parte inferior tiene su __private.key__ contraparte en la `config.zip` descargado cuando el __API de asset compute__ se ha agregado a este proyecto.
      + Si la clave privada se pierde o se pone en peligro, se puede eliminar la clave pública coincidente y se puede generar un nuevo par de claves en el Adobe I/O o cargar en él mediante esta interfaz.
