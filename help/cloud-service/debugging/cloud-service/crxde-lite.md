---
title: CRXDE Lite
description: CRXDE Lite es una herramienta clásica pero poderosa para depurar AEM como entornos de desarrollador Cloud Service. CRXDE Lite proporciona un conjunto de funciones que ayuda a depurar para que no inspeccione todos los recursos y propiedades, manipule las partes mutables del JCR e investigue los permisos.
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: KT-5481
thumbnail: kt-5481.jpg
topic: Development
role: Developer
level: Beginner
exl-id: f3f2c89f-6ec1-49d3-91c7-10a42b897780
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 0%

---

# Depuración AEM como Cloud Service con CRXDE Lite

El CRXDE Lite __SOLO__ está disponible en AEM como entornos de desarrollo de Cloud Service (así como el SDK de AEM local).

## Acceso al CRXDE Lite en AEM Author

El CRXDE Lite es __solo__ accesible en AEM entornos de desarrollo de Cloud Service y __no__ está disponible en entornos de fase o producción.

Para acceder al CRXDE Lite en AEM Author:

1. Inicie sesión en el servicio de AEM Author de AEM as a Cloud Service.
1. Vaya a Herramientas > General > CRXDE Lite

Se abrirá el CRXDE Lite con las credenciales y los permisos utilizados para iniciar sesión en AEM Author.

## Depuración de contenido

CRXDE Lite proporciona acceso directo al JCR. El contenido visible mediante CRXDE Lite está limitado por los permisos otorgados al usuario, lo que significa que es posible que no pueda ver o modificar todo en el JCR en función de su acceso.

Tenga en cuenta que `/apps`, `/libs` y `/oak:index` son inmutables, lo que significa que ningún usuario puede modificarlos durante la ejecución. Estas ubicaciones en el JCR solo se pueden modificar mediante implementaciones de código.

+ La estructura JCR se navega y manipula con el panel de navegación izquierdo
+ Al seleccionar un nodo en el panel de navegación izquierdo, se muestra la propiedad del nodo en el panel inferior.
   + Las propiedades se pueden agregar, quitar o cambiar desde el panel
+ Al hacer doble clic en un nodo de archivo en el panel de navegación izquierdo, se abre el contenido del archivo en el panel superior derecho
+ Pulse el botón Guardar todo en la parte superior izquierda para mantener el cambio o la flecha hacia abajo situada junto a Guardar todo para revertir los cambios sin guardar.

![CRXDE Lite: Depuración de contenido](./assets/crxde-lite/debugging-content.png)

La realización de cambios en el contenido mutable durante la ejecución en entornos de desarrollo de AEM as a Cloud Service a través de CRXDE Lite debe realizarse con cuidado.
Cualquier cambio realizado directamente en AEM mediante el CRXDE Lite puede ser difícil de rastrear y gobernar. Si procede, asegúrese de que los cambios realizados mediante CRXDE Lite regresan a los paquetes de contenido mutable (`ui.content`) del proyecto de AEM y se comprometen con Git para garantizar que el problema se resuelva. Lo ideal es que todos los cambios en el contenido de la aplicación se originen en la base de código y se transfieran a AEM a través de implementaciones, en lugar de realizar cambios directamente en la AEM a través del CRXDE Lite.

### Depuración de controles de acceso

CRXDE Lite proporciona una forma de probar y evaluar el control de acceso en un nodo específico para un usuario o grupo específico (también conocido como principal).

Para acceder a la consola Control de acceso de prueba en CRXDE Lite, vaya a:

+ CRXDE Lite > Herramientas > Probar control de acceso...

![CRXDE Lite - Control de acceso de prueba](./assets/crxde-lite/permissions__test-access-control.png)

1. Con el campo Ruta , seleccione una ruta JCR para evaluar
1. Con el campo Principal, seleccione el usuario o grupo con el que valorar la ruta
1. Puntee en el botón Probar

Los resultados se muestran a continuación:

+ ____ Path reitera la ruta evaluada
+ ____ Principalreitera al usuario o grupo para el que se evaluó la ruta
+ ____ Principalincluye todas las entidades principales de las que forma parte el principal seleccionado.
   + Esto resulta útil para comprender las pertenencias transitivas a grupos que pueden proporcionar permisos mediante herencia
+ __Privilegios en__ rutas enumera todos los permisos JCR que la entidad de seguridad seleccionada tiene en la ruta evaluada

### Actividades de depuración no admitidas

Las siguientes son actividades de depuración que pueden __no__ realizarse en CRXDE Lite.

### Depuración de configuraciones de OSGi

Las configuraciones de OSGi implementadas no se pueden revisar mediante CRXDE Lite. Las configuraciones de OSGi se mantienen en el paquete de código `ui.apps` del proyecto de AEM en `/apps/example/config.xxx`, sin embargo, tras la implementación a AEM como entornos de Cloud Service, los recursos de las configuraciones de OSGi no se mantienen en el JCR, por lo que no son visibles a través del CRXDE Lite.

En su lugar, utilice [Developer Console > Configurations](./developer-console.md#configurations) para revisar las configuraciones de OSGi implementadas.
