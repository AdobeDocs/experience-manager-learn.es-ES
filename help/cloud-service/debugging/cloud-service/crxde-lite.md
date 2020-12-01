---
title: CRXDE Lite
description: 'CRXDE Lite es una herramienta clásica pero poderosa para depurar AEM como entornos de Cloud Service Developer. CRXDE Lite proporciona un conjunto de funciones que ayudan a depurar todos los recursos y propiedades, a manipular las partes mutables del JCR y a investigar los permisos. '
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: KT-5481
thumbnail: kt-5481.jpg
translation-type: tm+mt
source-git-commit: 10d0970c1b0debfec7cf94cc106bcae414fb55f6
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 0%

---


# Depuración de AEM como Cloud Service con CRXDE Lite

CRXDE Lite está __SOLO__ disponible en AEM como entornos de desarrollo de Cloud Service (así como el SDK de AEM local).

## Acceso a CRXDE Lite en AEM Author

El CRXDE Lite es __sólo__ accesible en AEM como entornos de desarrollo de Cloud Service y __no__ está disponible en los entornos de etapa o producción.

Para acceder a CRXDE Lite en AEM Author:

1. Inicie sesión en el AEM como un servicio Cloud Service de AEM Author.
1. Vaya a Herramientas > General > CRXDE Lite

Se abrirá CRXDE Lite con las credenciales y los permisos utilizados para iniciar sesión en AEM Author.

## Depuración de contenido

CRXDE Lite proporciona acceso directo al JCR. El contenido visible mediante CRXDE Lite está limitado por los permisos concedidos a su usuario, lo que significa que es posible que no pueda ver o modificar todo en el JCR en función de su acceso.

Tenga en cuenta que `/apps`, `/libs` y `/oak:index` son inmutables, lo que significa que ningún usuario puede cambiarlas en tiempo de ejecución. Estas ubicaciones en el JCR solo se pueden modificar mediante implementaciones de código.

+ La estructura JCR se navega y manipula con el panel de navegación izquierdo
+ Al seleccionar un nodo en el panel de navegación izquierdo, se muestra el nodo de la propiedad del nodo en el panel inferior.
   + Las propiedades se pueden agregar, eliminar o cambiar desde el panel
+ Al hacer clic con el doble en un nodo de archivo en el panel de navegación izquierdo, se abre el contenido del archivo en el panel superior derecho
+ Toque el botón Guardar todo en la parte superior izquierda para mantener el cambio o la flecha abajo situada junto a Guardar todo para revertir los cambios no guardados.

![CRXDE Lite: Depuración de contenido](./assets/crxde-lite/debugging-content.png)

Realizar cambios en el contenido mutable durante la ejecución en AEM como entornos de desarrollo Cloud Service mediante CRXDE Lite debe realizarse con cuidado.
Cualquier cambio que se realice directamente a AEM mediante CRXDE Lite puede ser difícil de rastrear y gobernar. Si procede, asegúrese de que los cambios realizados a través del CRXDE Lite regresen a los paquetes de contenido mutable del proyecto de AEM (`ui.content`) y se comprometan con Git, para asegurarse de que el problema se resuelva. Idealmente, todos los cambios en el contenido de la aplicación se originan en la base de código y se dirigen a AEM mediante implementaciones, en lugar de realizar cambios directamente en el AEM mediante CRXDE Lite.

### Controles de acceso de depuración

CRXDE Lite proporciona una manera de probar y evaluar el control de acceso en un nodo específico para un usuario o grupo específico (también conocido como principal).

Para acceder a la consola de Test Control de acceso en CRXDE Lite, vaya a:

+ CRXDE Lite > Herramientas > Probar Control de acceso...

![CRXDE Lite - Control de acceso de prueba](./assets/crxde-lite/permissions__test-access-control.png)

1. Con el campo Ruta, seleccione una ruta JCR para evaluar
1. Con el campo Principal, seleccione el usuario o grupo con el que desea valorar la ruta
1. Puntee en el botón Prueba

Los resultados se muestran a continuación:

+ ____ Path reitera la ruta evaluada
+ ____ Principalreitera al usuario o grupo para el que se evaluó la ruta
+ ____ Principaldesliza todos los principales de los que forma parte el principal seleccionado.
   + Esto resulta útil para comprender las pertenencias transitivas a grupos que pueden proporcionar permisos mediante herencia
+ __Privilegios en__ rutas enumera todos los permisos JCR que tiene el principal seleccionado en la ruta evaluada

### Actividades de depuración no admitidas

Las siguientes son actividades de depuración que pueden __no__ realizarse en CRXDE Lite.

### Depuración de las configuraciones de OSGi

Las configuraciones de OSGi implementadas no se pueden revisar mediante CRXDE Lite. Las configuraciones de OSGi se mantienen en el paquete de código `ui.apps` del proyecto de AEM en `/apps/example/config.xxx`, sin embargo, al implementarse en AEM como entornos de Cloud Service, los recursos de configuración de OSGi no se mantienen en el JCR, por lo tanto no son visibles a través del CRXDE Lite.

En su lugar, utilice [Developer Console > Configuraciones](./developer-console.md#configurations) para revisar las configuraciones de OSGi implementadas.
