---
title: CRXDE Lite
description: 'CRXDE Lite es una herramienta clásica pero potente para depurar entornos de desarrollador de AEM as a Cloud Service. CRXDE Lite proporciona un conjunto de funciones que ayudan a depurar desde la inspección de todos los recursos y propiedades, la manipulación de las partes mutables del JCR y la investigación de permisos. '
feature: Herramientas para desarrolladores
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: KT-5481
thumbnail: kt-5481.jpg
topic: Desarrollo
role: Desarrollador
level: Principiante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '618'
ht-degree: 0%

---


# Depuración de AEM as a Cloud Service con CRXDE Lite

CRXDE Lite __SOLO__ está disponible en los entornos de desarrollo de AEM as a Cloud Service (así como en el SDK local de AEM).

## Acceso a CRXDE Lite en AEM Author

CRXDE Lite es __solo__ accesible en entornos de desarrollo de AEM as a Cloud Service y __no__ está disponible en entornos de fase o producción.

Para acceder a CRXDE Lite en AEM Author:

1. Inicie sesión en el servicio AEM as a Cloud Service Autor.
1. Vaya a Herramientas > General > CRXDE Lite

Esto abrirá CRXDE Lite con las credenciales y los permisos utilizados para iniciar sesión en AEM Author.

## Depuración de contenido

CRXDE Lite proporciona acceso directo al JCR. El contenido visible a través de CRXDE Lite está limitado por los permisos otorgados al usuario, lo que significa que es posible que no pueda ver o modificar todo en el JCR en función de su acceso.

Tenga en cuenta que `/apps`, `/libs` y `/oak:index` son inmutables, lo que significa que ningún usuario puede modificarlos durante la ejecución. Estas ubicaciones en el JCR solo se pueden modificar mediante implementaciones de código.

+ La estructura JCR se navega y manipula con el panel de navegación izquierdo
+ Al seleccionar un nodo en el panel de navegación izquierdo, se muestra la propiedad del nodo en el panel inferior.
   + Las propiedades se pueden agregar, quitar o cambiar desde el panel
+ Al hacer doble clic en un nodo de archivo en el panel de navegación izquierdo, se abre el contenido del archivo en el panel superior derecho
+ Pulse el botón Guardar todo en la parte superior izquierda para mantener el cambio o la flecha hacia abajo situada junto a Guardar todo para revertir los cambios sin guardar.

![CRXDE Lite: Depuración de contenido](./assets/crxde-lite/debugging-content.png)

Los cambios en el contenido mutable durante la ejecución en los entornos de desarrollo de AEM as a Cloud Service a través de CRXDE Lite deben realizarse con cuidado.
Cualquier cambio realizado directamente en AEM a través de CRXDE Lite puede ser difícil de rastrear y gobernar. Si procede, asegúrese de que los cambios realizados a través de CRXDE Lite regresan a los paquetes de contenido mutable (`ui.content`) del proyecto AEM y se comprometen con Git, para garantizar que el problema se resuelva. Lo ideal es que todos los cambios en el contenido de la aplicación se originen en la base de código y fluyan a AEM a través de implementaciones, en lugar de realizar cambios directamente en AEM a través de CRXDE Lite.

### Depuración de controles de acceso

CRXDE Lite proporciona una forma de probar y evaluar el control de acceso en un nodo específico para un usuario o grupo específico (también conocido como principal).

Para acceder a la consola Test Access Control en CRXDE Lite, vaya a:

+ CRXDE Lite > Herramientas > Probar control de acceso ...

![CRXDE Lite: control de acceso de prueba](./assets/crxde-lite/permissions__test-access-control.png)

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

Las configuraciones de OSGi implementadas no se pueden revisar a través de CRXDE Lite. Las configuraciones de OSGi se mantienen en el paquete de código `ui.apps` del proyecto AEM en `/apps/example/config.xxx`, sin embargo, tras la implementación en los entornos de AEM as a Cloud Service, los recursos de las configuraciones de OSGi no se mantienen en el JCR, por lo que no son visibles a través de CRXDE Lite.

En su lugar, utilice [Developer Console > Configurations](./developer-console.md#configurations) para revisar las configuraciones de OSGi implementadas.
