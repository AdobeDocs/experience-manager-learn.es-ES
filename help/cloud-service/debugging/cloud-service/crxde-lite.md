---
title: CRXDE Lite
description: CRXDE Lite es una herramienta clásica, pero potente, para depurar entornos de AEM as a Cloud Service Developer. CRXDE Lite proporciona un conjunto de funcionalidades que ayuda a la depuración a partir de la inspección de todos los recursos y propiedades, la manipulación de las partes mutables del JCR y la investigación de permisos.
feature: Developer Tools
version: Experience Manager as a Cloud Service
doc-type: Tutorial
kt: KT-5481
thumbnail: kt-5481.jpg
topic: Development
role: Developer
level: Beginner
exl-id: f3f2c89f-6ec1-49d3-91c7-10a42b897780
duration: 125
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 0%

---

# Depuración de AEM as a Cloud Service con CRXDE Lite

CRXDE Lite está __SOLAMENTE__ disponible en entornos de desarrollo de AEM as a Cloud Service (así como en el SDK local de AEM).

## Acceder a CRXDE Lite en AEM Author

Se puede acceder a CRXDE Lite __solamente__ en entornos de desarrollo de AEM as a Cloud Service, y __no__ está disponible en entornos de ensayo o producción.

Para acceder a CRXDE Lite en AEM Author:

1. Inicie sesión en el servicio de AEM as a Cloud Service AEM Author.
1. Vaya a Herramientas > General > CRXDE Lite

Se abrirá CRXDE Lite con las credenciales y los permisos utilizados para iniciar sesión en AEM Author.

## Depuración de contenido

CRXDE Lite proporciona acceso directo al JCR. El contenido visible a través de CRXDE Lite está limitado por los permisos otorgados al usuario, lo que significa que es posible que no pueda ver o modificar todo en el JCR según su acceso.

Tenga en cuenta que `/apps`, `/libs` y `/oak:index` son inmutables, lo que significa que ningún usuario puede cambiarlos durante la ejecución. Estas ubicaciones en el JCR solo se pueden modificar mediante implementaciones de código.

+ La estructura JCR se navega y manipula mediante el panel de navegación izquierdo
+ Al seleccionar un nodo en el panel de navegación izquierdo, se exponen las propiedades del nodo en el panel inferior.
   + Las propiedades se pueden agregar, eliminar o cambiar en el panel
+ Al hacer doble clic en un nodo de archivo en el panel de navegación izquierdo, se abre el contenido del archivo en el panel superior derecho
+ Pulse el botón Guardar todo en la parte superior izquierda para mantener los cambios, o la flecha hacia abajo junto a Guardar todo para revertir los cambios no guardados.

![CRXDE Lite - Depurando contenido](./assets/crxde-lite/debugging-content.png)

Realizar cambios en el contenido mutable durante la ejecución en entornos de desarrollo de AEM as a Cloud Service a través de CRXDE Lite debe hacerse con cuidado.
Cualquier cambio realizado directamente en AEM a través de CRXDE Lite puede ser difícil de rastrear y controlar. Si corresponde, asegúrese de que los cambios realizados mediante CRXDE Lite regresen a los paquetes de contenido mutable del proyecto AEM (`ui.content`) y se comprometan con Git para garantizar que se resuelva el problema. Lo ideal es que todos los cambios en el contenido de la aplicación se originen en la base de código y se transfieran a AEM a través de implementaciones, en lugar de realizar cambios directamente en AEM a través de CRXDE Lite.

### Depuración de controles de acceso

CRXDE Lite proporciona una forma de probar y evaluar el control de acceso en un nodo específico para un usuario o grupo específico (también conocido como principal).

Para acceder a la consola de Control de acceso de prueba en CRXDE Lite, vaya a:

+ CRXDE Lite > Herramientas > Probar control de acceso...

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

Las siguientes son actividades de depuración que __no__ se pueden realizar en CRXDE Lite.

### Depuración de configuraciones de OSGi

Las configuraciones de OSGi implementadas no se pueden revisar a través de CRXDE Lite. Las configuraciones de OSGi se mantienen en el paquete de código `ui.apps` del proyecto AEM en `/apps/example/config.xxx`, pero tras la implementación en entornos AEM as a Cloud Service, los recursos de configuraciones de OSGi no se mantienen en el JCR, por lo tanto no son visibles a través de CRXDE Lite.

En su lugar, use [Developer Console > Configuraciones](./developer-console.md#configurations) para revisar las configuraciones de OSGi implementadas.
