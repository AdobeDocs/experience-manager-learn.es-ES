---
title: CRXDE Lite
description: CRXDE Lite es una herramienta clásica, pero potente, para depurar entornos de AEM as a Cloud Service Developer. CRXDE Lite proporciona un conjunto de funcionalidades que ayuda a la depuración a partir de la inspección de todos los recursos y propiedades, la manipulación de las partes mutables del JCR y la investigación de permisos.
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
kt: KT-5481
thumbnail: kt-5481.jpg
topic: Development
role: Developer
level: Beginner
exl-id: f3f2c89f-6ec1-49d3-91c7-10a42b897780
duration: 125
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 0%

---

# Depuración de AEM as a Cloud Service con CRXDE Lite

El CRXDE Lite __SOLAMENTE__ está disponible en los entornos de desarrollo de AEM as a Cloud Service AEM (así como en el SDK local de la).

## Acceder al CRXDE Lite AEM en el autor de

Se puede acceder al CRXDE Lite __solamente__ en entornos de desarrollo de AEM as a Cloud Service, y __no__ está disponible en entornos de ensayo o producción.

Para acceder al CRXDE Lite AEM en el autor de la:

1. Inicie sesión en el servicio de autor de AEM as a Cloud Service AEM.
1. Vaya a Herramientas > General > CRXDE Lite

Se abrirá el CRXDE Lite AEM con las credenciales y los permisos utilizados para iniciar sesión en el autor de la.

## Depuración de contenido

El CRXDE Lite de proporciona acceso directo al JCR. El contenido visible mediante CRXDE Lite está limitado por los permisos otorgados al usuario, lo que significa que es posible que no pueda ver o modificar todo en el JCR según su acceso.

Tenga en cuenta que `/apps`, `/libs` y `/oak:index` son inmutables, lo que significa que ningún usuario puede cambiarlos durante la ejecución. Estas ubicaciones en el JCR solo se pueden modificar mediante implementaciones de código.

+ La estructura JCR se navega y manipula mediante el panel de navegación izquierdo
+ Al seleccionar un nodo en el panel de navegación izquierdo, se exponen las propiedades del nodo en el panel inferior.
   + Las propiedades se pueden agregar, eliminar o cambiar en el panel
+ Al hacer doble clic en un nodo de archivo en el panel de navegación izquierdo, se abre el contenido del archivo en el panel superior derecho
+ Pulse el botón Guardar todo en la parte superior izquierda para mantener los cambios, o la flecha hacia abajo junto a Guardar todo para revertir los cambios no guardados.

![CRXDE Lite - Depurando contenido](./assets/crxde-lite/debugging-content.png)

Realizar cambios en el contenido mutable durante la ejecución en entornos de desarrollo de AEM as a Cloud Service mediante CRXDE Lite debe hacerse con cuidado.
AEM Cualquier cambio realizado directamente en el recurso a través de un CRXDE Lite puede ser difícil de rastrear y controlar. Si corresponde, asegúrese de que los cambios realizados mediante el CRXDE Lite AEM regresen a los paquetes de contenido mutable (`ui.content`) del proyecto de la y se comprometan con Git para garantizar que se resuelva el problema. AEM AEM Lo ideal es que todos los cambios en el contenido de la aplicación se originen en la base de código y se transfieran a través de implementaciones, en lugar de realizar cambios directamente en la aplicación a través de un CRXDE Lite.

### Depuración de controles de acceso

CRXDE Lite proporciona una forma de probar y evaluar el control de acceso en un nodo específico para un usuario o grupo específico (también conocido como principal).

Para acceder a la consola de Control de acceso de prueba en CRXDE Lite, vaya a:

+ CRXDE Lite > Herramientas > Probar control de acceso ...

![CRXDE Lite - Probar control de acceso](./assets/crxde-lite/permissions__test-access-control.png)

1. En el campo Ruta, seleccione una Ruta JCR para evaluar
1. En el campo Principal, seleccione el usuario o el grupo con el que desea evaluar la ruta
1. Pulse el botón Probar

Los resultados se muestran a continuación:

+ __Ruta__ reitera la ruta que se evaluó
+ __Principal__ reitera al usuario o grupo para el que se evaluó la ruta
+ __Principales__ enumera todas las principales de las que forma parte el principal seleccionado.
   + Esto resulta útil para comprender las pertenencias a grupos transitivas que pueden proporcionar permisos a través de la herencia
+ __Privilegios en la ruta__ enumera todos los permisos JCR que la entidad de seguridad seleccionada tiene en la ruta evaluada

### Actividades de depuración no admitidas

Las siguientes son actividades de depuración que __no__ se pueden realizar en el CRXDE Lite.

### Depuración de configuraciones de OSGi

Las configuraciones de OSGi implementadas no se pueden revisar mediante el CRXDE Lite. AEM Las configuraciones de OSGi se mantienen en el paquete de código `ui.apps` del proyecto de en `/apps/example/config.xxx`, sin embargo, tras la implementación en entornos de AEM as a Cloud Service, los recursos de configuraciones de OSGi no se mantienen en el JCR, por lo que no son visibles a través del CRXDE Lite.

En su lugar, use [Developer Console > Configuraciones](./developer-console.md#configurations) para revisar las configuraciones de OSGi implementadas.
