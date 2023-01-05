---
title: Puntos de interrupción interactivos
description: Aprenda a configurar nuevos puntos de interrupción adaptables para AEM editor de páginas adaptable.
version: Cloud Service
feature: Page Editor
topic: Mobile, Development
role: Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-01-05T00:00:00Z
kt: 11664
thumbnail: kt-11664.jpeg
source-git-commit: c82965636ddeef7dc165e0bea079c99f1a16e0ca
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 0%

---


# Puntos de interrupción interactivos

Aprenda a configurar nuevos puntos de interrupción adaptables para AEM editor de páginas adaptable.

## Crear puntos de interrupción CSS

En primer lugar, cree puntos de interrupción multimedia en la cuadrícula AEM adaptable CSS a la que se adhiera el sitio de AEM adaptable.

En `/ui.apps/src/main/content/jcr_root/apps/[app name]/clientlibs/clientlib-grid/less/grid.less` cree los puntos de interrupción que quiera usar junto con el emulador móvil. Tenga en cuenta que `max-width` para cada punto de interrupción, ya que esto asigna los puntos de interrupción CSS a los puntos de interrupción AEM Editor de páginas interactivos.

![Crear nuevos puntos de interrupción interactivos](./assets/responsive-breakpoints/create-new-breakpoints.jpg)

## Personalización de los puntos de interrupción de la plantilla

Abra el `ui.content/src/main/content/jcr_root/conf/<app name>/settings/wcm/templates/page-content/structure/.content.xml` archivo y actualizar `cq:responsive/breakpoints` con las nuevas definiciones de nodo de punto de interrupción. Cada [Punto de interrupción CSS](#create-new-css-breakpoints) debe tener un nodo correspondiente debajo de `breakpoints` con su `width` propiedad establecida en el punto de interrupción CSS `max-width`.

![Personalización de los puntos de interrupción adaptables de la plantilla](./assets/responsive-breakpoints/customize-template-breakpoints.jpg)

## Creación de emuladores

Se deben definir emuladores de AEM que permitan a los autores seleccionar la vista interactiva que se editará en el Editor de páginas.

Cree nodos de emuladores en `/ui.apps/src/main/content/jcr_root/apps/<app name>/emulators`

Por ejemplo, `/ui.apps/src/main/content/jcr_root/apps/wknd-examples/emulators/phone-landscape`. Copiar un nodo emulador de referencia de `/libs/wcm/mobile/components/emulators` en CRXDE Lite a y actualice la copia para acelerar la definición del nodo.

![Crear nuevos emuladores](./assets/responsive-breakpoints/create-new-emulators.jpg)

## Crear grupo de dispositivos

Agrupar los emuladores en [haga que estén disponibles en AEM Editor de páginas](#update-the-templates-device-group).

Crear `/apps/settings/mobile/groups/<name of device group>` estructura de nodos bajo `/ui.apps/src/main/content/jcr_root`.

![Crear nuevo grupo de dispositivos](./assets/responsive-breakpoints/create-new-device-group.jpg)

Cree un `.content.xml` en `/apps/settings/mobile/groups/<device group name>` y defina los nuevos emuladores utilizando un código similar al que se muestra a continuación:

![Crear nuevo dispositivo](./assets/responsive-breakpoints/create-new-device.jpg)

## Actualizar el grupo de dispositivos de la plantilla

Por último, asigne de nuevo el grupo de dispositivos a la plantilla de página para que los emuladores estén disponibles en el Editor de páginas para las páginas creadas a partir de esta plantilla.

Abra el `ui.content/src/main/content/jcr_root/conf/[app name]/settings/wcm/templates/page-content/structure/.content.xml` y actualice el `cq:deviceGroups` propiedad para hacer referencia al nuevo grupo móvil (por ejemplo, `cq:deviceGroups="[mobile/groups/customdevices]"`)
