---
title: CRXDE Lite
description: CRXDE Lite AEM es una herramienta clásica, pero potente, para depurar entornos de desarrollador as a Cloud Service de la. CRXDE Lite proporciona un conjunto de funcionalidades que ayuda a la depuración a partir de la inspección de todos los recursos y propiedades, la manipulación de las partes mutables del JCR y la investigación de permisos.
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
kt: KT-5481
thumbnail: kt-5481.jpg
topic: Development
role: Developer
level: Beginner
exl-id: f3f2c89f-6ec1-49d3-91c7-10a42b897780
duration: 140
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 0%

---

# AEM Depuración as a Cloud Service con el CRXDE Lite de

El CRXDE Lite es __SOLO__ AEM disponible en entornos de desarrollo as a Cloud Service AEM de la (así como en el SDK de la aplicación local de la aplicación).

## Acceder al CRXDE Lite AEM en el autor de

El CRXDE Lite es __solamente__ AEM accesible en entornos de desarrollo as a Cloud Service de la, y __no__ disponible en entornos de ensayo o producción.

Para acceder al CRXDE Lite AEM en el autor de la:

1. AEM Inicie sesión en el servicio de autor de as a Cloud Service AEM de.
1. Vaya a Herramientas > General > CRXDE Lite

Se abrirá el CRXDE Lite AEM con las credenciales y los permisos utilizados para iniciar sesión en el autor de la.

## Depuración de contenido

El CRXDE Lite de proporciona acceso directo al JCR. El contenido visible mediante CRXDE Lite está limitado por los permisos otorgados al usuario, lo que significa que es posible que no pueda ver o modificar todo en el JCR según su acceso.

Tenga en cuenta que `/apps`, `/libs` y `/oak:index` son inmutables, lo que significa que ningún usuario puede cambiarlas durante la ejecución. Estas ubicaciones en el JCR solo se pueden modificar mediante implementaciones de código.

+ La estructura JCR se navega y manipula mediante el panel de navegación izquierdo
+ Al seleccionar un nodo en el panel de navegación izquierdo, se exponen las propiedades del nodo en el panel inferior.
   + Las propiedades se pueden agregar, eliminar o cambiar en el panel
+ Al hacer doble clic en un nodo de archivo en el panel de navegación izquierdo, se abre el contenido del archivo en el panel superior derecho
+ Pulse el botón Guardar todo en la parte superior izquierda para mantener los cambios, o la flecha hacia abajo junto a Guardar todo para revertir los cambios no guardados.

![CRXDE Lite - Depuración de contenido](./assets/crxde-lite/debugging-content.png)

AEM La realización de cambios en el contenido mutable durante la ejecución en entornos de desarrollo as a Cloud Service mediante el CRXDE Lite debe realizarse con cuidado.
AEM Cualquier cambio realizado directamente en el recurso a través de un CRXDE Lite puede ser difícil de rastrear y controlar. Si corresponde, asegúrese de que los cambios realizados mediante CRXDE Lite AEM regresen a los paquetes de contenido mutable del proyecto de la (`ui.content`) y se compromete a Git, para garantizar que el problema se resuelva. AEM AEM Lo ideal es que todos los cambios en el contenido de la aplicación se originen en la base de código y se transfieran a través de implementaciones, en lugar de realizar cambios directamente en la aplicación a través de un CRXDE Lite.

### Depuración de controles de acceso

CRXDE Lite proporciona una forma de probar y evaluar el control de acceso en un nodo específico para un usuario o grupo específico (también conocido como principal).

Para acceder a la consola de Control de acceso de prueba en CRXDE Lite, vaya a:

+ CRXDE Lite > Herramientas > Probar control de acceso ...

![CRXDE Lite - Probar control de acceso](./assets/crxde-lite/permissions__test-access-control.png)

1. En el campo Ruta, seleccione una Ruta JCR para evaluar
1. En el campo Principal, seleccione el usuario o el grupo con el que desea evaluar la ruta
1. Pulse el botón Probar

Los resultados se muestran a continuación:

+ __Ruta__ reitera la ruta evaluada
+ __Principal__ reitera el usuario o grupo para el que se evaluó la ruta
+ __Entidades__ enumera todas las entidades de seguridad de las que forma parte la entidad de seguridad seleccionada.
   + Esto resulta útil para comprender las pertenencias a grupos transitivas que pueden proporcionar permisos a través de la herencia
+ __Privilegios en la ruta__ enumera todos los permisos JCR que tiene la entidad de seguridad seleccionada en la ruta evaluada

### Actividades de depuración no admitidas

Las siguientes son actividades de depuración que pueden __no__ se realizará en el CRXDE Lite.

### Depuración de configuraciones de OSGi

Las configuraciones de OSGi implementadas no se pueden revisar mediante el CRXDE Lite. AEM Las configuraciones de OSGi se mantienen en las del proyecto de `ui.apps` paquete de código en `/apps/example/config.xxx`AEM Sin embargo, tras la implementación en entornos as a Cloud Service de la, los recursos de configuraciones de OSGi no persisten en el JCR, por lo que no son visibles a través del CRXDE Lite.

En su lugar, utilice el [Consola de desarrollador > Configuraciones](./developer-console.md#configurations) para revisar las configuraciones de OSGi implementadas.
