---
title: Añadir navegación y enrutamiento | Introducción al Editor de SPA de AEM y a Angular
description: Obtenga información sobre cómo se admiten varias vistas en el SPA mediante AEM Páginas y el SDK del Editor de SPA. La navegación dinámica se implementa mediante rutas angulares y se agrega a un componente Encabezado existente.
sub-product: sitios
feature: SPA Editor
topics: development
doc-type: tutorial
version: cloud-service
activity: develop
audience: developer
kt: 5312
thumbnail: 5312-spa-angular.jpg
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '2720'
ht-degree: 1%

---


# Añadir navegación y enrutamiento {#navigation-routing}

Obtenga información sobre cómo se admiten varias vistas en el SPA mediante AEM Páginas y el SDK del Editor de SPA. La navegación dinámica se implementa mediante rutas angulares y se agrega a un componente Encabezado existente.

## Objetivo

1. Comprender las opciones de enrutamiento del modelo de SPA disponibles al utilizar el Editor de SPA.
2. Aprenda a utilizar [enrutamiento angular](https://angular.io/guide/router) para navegar entre diferentes vistas del SPA.
3. Implemente una navegación dinámica impulsada por la jerarquía de páginas de AEM.

## Qué va a generar

Este capítulo agrega un menú de navegación a un componente `Header` existente. El menú de navegación está impulsado por la jerarquía de páginas AEM y utiliza el modelo JSON proporcionado por el [Componente principal de navegación](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html).

![Navegación implementada](assets/navigation-routing/final-navigation-implemented.gif)

## Requisitos previos

Revise las herramientas y las instrucciones necesarias para configurar un [entorno de desarrollo local](overview.md#local-dev-environment).

### Obtener el código

1. Descargue el punto de partida de este tutorial a través de Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/navigation-routing-start
   ```

2. Implemente el código base en una instancia de AEM local mediante Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Si utiliza [AEM 6.x](overview.md#compatibility) agregue el perfil `classic`:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. Instale el paquete terminado para el [sitio de referencia WKND tradicional](https://github.com/adobe/aem-guides-wknd/releases/latest). Las imágenes proporcionadas por [sitio de referencia WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) se reutilizarán en el SPA WKND. El paquete se puede instalar mediante [Administrador de paquetes de AEM](http://localhost:4502/crx/packmgr/index.jsp).

   ![Package Manager install wknd.all](./assets/map-components/package-manager-wknd-all.png)

Siempre puede realizar la vista del código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/navigation-routing-solution) o extraer el código localmente cambiando a la rama `Angular/navigation-routing-solution`.

## Actualizaciones de HeaderComponent de Inspect {#inspect-header}

En capítulos anteriores, el componente `HeaderComponent` se agregó como un componente angular puro incluido mediante `app.component.html`. En este capítulo, el componente `HeaderComponent` se elimina de la aplicación y se agregará mediante el [Editor de plantillas](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html). Esto permite a los usuarios configurar el menú de navegación de `HeaderComponent` desde dentro de AEM.

>[!NOTE]
>
> Ya se han realizado varias actualizaciones de CSS y JavaScript en la base de código para inicio de este capítulo. Para centrarse en los conceptos principales, no **todos** de los cambios de código se analizan. Puede vista de los cambios completos [aquí](https://github.com/adobe/aem-guides-wknd-spa/compare/Angular/map-components-solution...Angular/navigation-routing-start).

1. En el IDE de su elección, abra el proyecto de inicio de SPA para este capítulo.
2. Debajo del módulo `ui.frontend` inspeccione el archivo `header.component.ts` en: `ui.frontend/src/app/components/header/header.component.ts`.

   Se han realizado varias actualizaciones, incluida la adición de un `HeaderEditConfig` y un `MapTo` para permitir que el componente se asigne a un componente de AEM `wknd-spa-angular/components/header`.

   ```js
   /* header.component.ts */
   ...
   const HeaderEditConfig = {
       ...
   };
   
   @Component({
   selector: 'app-header',
   templateUrl: './header.component.html',
   styleUrls: ['./header.component.scss']
   })
   export class HeaderComponent implements OnInit {
   @Input() items: object[];
       ...
   }
   ...
   MapTo('wknd-spa-angular/components/header')(withRouter(Header), HeaderEditConfig);
   ```

   Observe la anotación `@Input()` para `items`. `items` contendrá una matriz de objetos de navegación pasados desde AEM.

3. En el módulo `ui.apps` inspeccione la definición de componente del componente AEM `Header`: `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/header/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Header"
       sling:resourceSuperType="wknd-spa-angular/components/navigation"
       componentGroup="WKND SPA Angular - Structure"/>
   ```

   El componente AEM `Header` heredará toda la funcionalidad del [Componente principal de navegación](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html) mediante la propiedad `sling:resourceSuperType`.

## Añadir HeaderComponent a la plantilla de SPA {#add-header-template}

1. Abra un explorador e inicie sesión en AEM, [http://localhost:4502/](http://localhost:4502/). La base de código de inicio ya debe implementarse.
2. Vaya a la **[!UICONTROL Plantilla de página SPA]**: [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html).
3. Seleccione el **[!UICONTROL Contenedor de diseño raíz]** más externo y haga clic en su icono **[!UICONTROL Política]**. Tenga cuidado **no** para seleccionar el **[!UICONTROL Contenedor de diseño]** desbloqueado para la creación.

   ![Seleccione el icono de directiva de contenedor de diseño raíz](assets/navigation-routing/root-layout-container-policy.png)

4. Copie la directiva actual y cree una nueva directiva denominada **[!UICONTROL Estructura SPA]**:

   ![Política de estructura SPA](assets/map-components/spa-policy-update.png)

   En **[!UICONTROL Componentes permitidos]** > **[!UICONTROL General]** > seleccione el componente **[!UICONTROL Contenedor de diseño]**.

   En **[!UICONTROL Componentes permitidos]** > **[!UICONTROL WKND SPA ANGULAR - ESTRUCTURA]** > seleccione el componente **[!UICONTROL Encabezado]**:

   ![Seleccionar componente de encabezado](assets/map-components/select-header-component.png)

   En **[!UICONTROL Componentes permitidos]** > **[!UICONTROL WKND SPA ANGULAR - Contenido]** > seleccione los componentes **[!UICONTROL Imagen]** y **[!UICONTROL Texto]**. Debe tener 4 componentes en total seleccionados.

   Haga clic en **[!UICONTROL Listo]** para guardar los cambios.

5. **Actualice la página.** Añada el componente **[!UICONTROL Header]** por encima del **[!UICONTROL Contenedor de diseño]** sin bloquear:

   ![agregar componente Encabezado a plantilla](./assets/navigation-routing/add-header-component.gif)

6. Seleccione el componente **[!UICONTROL Header]** y haga clic en su icono **Policy** para editar la directiva.

   ![Haga clic en Directiva de encabezado](assets/navigation-routing/header-policy-icon.png)

7. Cree una nueva directiva con un **[!UICONTROL Título de directiva]** de **&quot;Encabezado de SPA WKND&quot;**.

   En **[!UICONTROL Propiedades]**:

   * Establezca la **[!UICONTROL Raíz de navegación]** en `/content/wknd-spa-angular/us/en`.
   * Establezca **[!UICONTROL Excluir niveles raíz]** en **1**.
   * Desmarque **[!UICONTROL Recopilar todas las páginas secundarias]**.
   * Establezca la **[!UICONTROL Profundidad de la estructura de navegación]** en **3**.

   ![Configurar directiva de encabezado](assets/navigation-routing/header-policy.png)

   Esto recopilará los dos niveles de navegación más profundos debajo de `/content/wknd-spa-angular/us/en`.

8. Después de guardar los cambios, debe ver el `Header` relleno como parte de la plantilla:

   ![Componente de encabezado rellenado](assets/navigation-routing/populated-header.png)

## Crear páginas secundarias

A continuación, cree páginas adicionales en AEM que sirvan como las distintas vistas del SPA. También inspeccionaremos la estructura jerárquica del modelo JSON proporcionado por AEM.

1. Vaya a la consola **Sites**: [http://localhost:4502/sites.html/content/wknd-spa-angular/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-angular/us/en/home). Seleccione la **Página de inicio angular de WKND SPA** y haga clic en **[!UICONTROL Crear]** > **[!UICONTROL Página]**:

   ![Crear nueva página](assets/navigation-routing/create-new-page.png)

2. En **[!UICONTROL Plantilla]** seleccione **[!UICONTROL Página SPA]**. En **[!UICONTROL Propiedades]** escriba **&quot;Página 1&quot;** para **[!UICONTROL Título]** y **&quot;página-1&quot;** como nombre.

   ![Introduzca las propiedades iniciales de la página](assets/navigation-routing/initial-page-properties.png)

   Haga clic en **[!UICONTROL Crear]** y, en la ventana emergente de cuadro de diálogo, haga clic en **[!UICONTROL Abrir]** para abrir la página en el Editor de SPA de AEM.

3. Añada un nuevo componente **[!UICONTROL Texto]** en el **[!UICONTROL Contenedor de diseño principal]**. Edite el componente e introduzca el texto: **&quot;Página 1&quot;** con el elemento RTE y el elemento **H1** (tendrá que entrar al modo de pantalla completa para cambiar los elementos de párrafo)

   ![Página de contenido de muestra 1](assets/navigation-routing/page-1-sample-content.png)

   No dude en añadir contenido adicional, como una imagen.

4. Vuelva a la consola de AEM Sites y repita los pasos anteriores, creando una segunda página con el nombre **&quot;Página 2&quot;** como elemento secundario de **Página 1**. Añada el contenido a **Página 2** para que se pueda identificar fácilmente.
5. Por último, cree una tercera página, **&quot;Página 3&quot;** pero como **secundario** de **Página 2**. Una vez completada la jerarquía del sitio, debe tener el siguiente aspecto:

   ![Jerarquía del sitio de muestra](assets/navigation-routing/wknd-spa-sample-site-hierarchy.png)

6. En una nueva ficha, abra la API de modelo JSON proporcionada por AEM: [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json). Este contenido JSON se solicita cuando se carga por primera vez el SPA. La estructura exterior es similar a la siguiente:

   ```json
   {
   "language": "en",
   "title": "en",
   "templateName": "spa-app-template",
   "designPath": "/libs/settings/wcm/designs/default",
   "cssClassNames": "spa page basicpage",
   ":type": "wknd-spa-angular/components/spa",
   ":items": {},
   ":itemsOrder": [],
   ":hierarchyType": "page",
   ":path": "/content/wknd-spa-angular/us/en",
   ":children": {
       "/content/wknd-spa-angular/us/en/home": {},
       "/content/wknd-spa-angular/us/en/home/page-1": {},
       "/content/wknd-spa-angular/us/en/home/page-2": {},
       "/content/wknd-spa-angular/us/en/home/page-2/page-3": {}
       }
   }
   ```

   Debajo de `:children` debe ver una entrada para cada una de las páginas creadas. El contenido de todas las páginas se encuentra en esta solicitud JSON inicial. Una vez implementado el enrutamiento de navegación, las vistas posteriores del SPA se cargarán rápidamente, ya que el contenido ya está disponible en el cliente.

   No es aconsejable cargar **ALL** del contenido de un SPA en la solicitud JSON inicial, ya que esto ralentizaría la carga inicial de la página. A continuación, veamos cómo se recopila la profundidad de jerarquía de las páginas.

7. Vaya a la plantilla **SPA raíz** en: [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-app-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-app-template/structure.html).

   Haga clic en el **[!UICONTROL menú de propiedades de página]** > **[!UICONTROL Política de página]**:

   ![Abra la directiva de página para SPA raíz](assets/navigation-routing/open-page-policy.png)

8. La plantilla **SPA raíz** tiene una ficha **[!UICONTROL Estructura jerárquica]** adicional para controlar el contenido JSON recopilado. La **[!UICONTROL Profundidad de estructura]** determina la profundidad en la jerarquía del sitio para recopilar páginas secundarias debajo de la **raíz**. También puede utilizar el campo **[!UICONTROL Patrones de estructura]** para filtrar páginas adicionales basadas en una expresión normal.

   Actualice la **[!UICONTROL Profundidad de estructura]** a **&quot;2&quot;**:

   ![Actualizar profundidad de estructura](assets/navigation-routing/update-structure-depth.png)

   Haga clic en **[!UICONTROL Listo]** para guardar los cambios en la directiva.

9. Vuelva a abrir el modelo JSON [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json).

   ```json
   {
   "language": "en",
   "title": "en",
   "templateName": "spa-app-template",
   "designPath": "/libs/settings/wcm/designs/default",
   "cssClassNames": "spa page basicpage",
   ":type": "wknd-spa-angular/components/spa",
   ":items": {},
   ":itemsOrder": [],
   ":hierarchyType": "page",
   ":path": "/content/wknd-spa-angular/us/en",
   ":children": {
       "/content/wknd-spa-angular/us/en/home": {},
       "/content/wknd-spa-angular/us/en/home/page-1": {},
       "/content/wknd-spa-angular/us/en/home/page-2": {}
       }
   }
   ```

   Observe que la ruta **Página 3** se ha eliminado: `/content/wknd-spa-angular/us/en/home/page-2/page-3` del modelo JSON inicial.

   Posteriormente, observaremos cómo el SDK del Editor SPA de AEM puede cargar dinámicamente contenido adicional.

## Implementar la navegación

A continuación, implemente el menú de navegación con un nuevo `NavigationComponent`. Podríamos agregar el código directamente en `header.component.html` pero una mejor práctica es evitar los componentes grandes. En su lugar, implemente un `NavigationComponent` que podría reutilizarse más adelante.

1. Revise el JSON expuesto por el componente AEM `Header` en [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json):

   ```json
   ...
   "header": {
       "items": [
       {
       "level": 0,
       "active": true,
       "path": "/content/wknd-spa-angular/us/en/home",
       "description": null,
       "url": "/content/wknd-spa-angular/us/en/home.html",
       "lastModified": 1589062597083,
       "title": "WKND SPA Angular Home Page",
       "children": [
               {
               "children": [],
               "level": 1,
               "active": false,
               "path": "/content/wknd-spa-angular/us/en/home/page-1",
               "description": null,
               "url": "/content/wknd-spa-angular/us/en/home/page-1.html",
               "lastModified": 1589429385100,
               "title": "Page 1"
               },
               {
               "level": 1,
               "active": true,
               "path": "/content/wknd-spa-angular/us/en/home/page-2",
               "description": null,
               "url": "/content/wknd-spa-angular/us/en/home/page-2.html",
               "lastModified": 1589429603507,
               "title": "Page 2",
               "children": [
                   {
                   "children": [],
                   "level": 2,
                   "active": false,
                   "path": "/content/wknd-spa-angular/us/en/home/page-2/page-3",
                   "description": null,
                   "url": "/content/wknd-spa-angular/us/en/home/page-2/page-3.html",
                   "lastModified": 1589430413831,
                   "title": "Page 3"
                   }
               ],
               }
           ]
           }
       ],
   ":type": "wknd-spa-angular/components/header"
   ```

   La naturaleza jerárquica de las páginas AEM se modela en el JSON y se puede utilizar para rellenar un menú de navegación. Recuerde que el componente `Header` hereda toda la funcionalidad del [Componente principal de navegación](https://www.aemcomponents.dev/content/core-components-examples/library/templating/navigation.html) y que el contenido expuesto a través del JSON se asignará automáticamente a la anotación angular `@Input`.

2. Abra una nueva ventana de terminal y vaya a la carpeta `ui.frontend` del proyecto de SPA. Cree un `NavigationComponent` nuevo con la herramienta CLI angular:

   ```shell
   $ cd ui.frontend
   $ ng generate component components/navigation
   CREATE src/app/components/navigation/navigation.component.scss (0 bytes)
   CREATE src/app/components/navigation/navigation.component.html (25 bytes)
   CREATE src/app/components/navigation/navigation.component.spec.ts (656 bytes)
   CREATE src/app/components/navigation/navigation.component.ts (286 bytes)
   UPDATE src/app/app.module.ts (2032 bytes)
   ```

3. A continuación, cree una clase con el nombre `NavigationLink` mediante la CLI angular en el directorio `components/navigation` recién creado:

   ```shell
   $ cd src/app/components/navigation/
   $ ng generate class NavigationLink
   CREATE src/app/components/navigation/navigation-link.spec.ts (187 bytes)
   CREATE src/app/components/navigation/navigation-link.ts (32 bytes)
   ```

4. Vuelva al IDE de su elección y abra el archivo en `navigation-link.ts` en `/src/app/components/navigation/navigation-link.ts`.

   ![Abrir archivo navigation-link.ts](assets/navigation-routing/ide-navigation-link-file.png)

5. Rellene `navigation-link.ts` con lo siguiente:

   ```js
   export class NavigationLink {
   
       title: string;
       path: string;
       url: string;
       level: number;
       children: NavigationLink[];
       active: boolean;
   
       constructor(data) {
           this.path = data.path;
           this.title = data.title;
           this.url = data.url;
           this.level = data.level;
           this.active = data.active;
           this.children = data.children.map( item => {
               return new NavigationLink(item);
           });
       }
   }
   ```

   Es una clase sencilla que representa un vínculo de navegación individual. En el constructor de clase, esperamos que `data` sea el objeto JSON transferido desde AEM. Esta clase se utilizará dentro de `NavigationComponent` y `HeaderComponent` para rellenar fácilmente la estructura de navegación.

   No se realiza ninguna transformación de datos; esta clase se crea principalmente para escribir con fuerza el modelo JSON. Observe que `this.children` se escribe como `NavigationLink[]` y que el constructor crea de forma recursiva nuevos objetos `NavigationLink` para cada uno de los elementos de la matriz `children`. Recuerde que el modelo JSON para `Header` es jerárquico.

6. Abra el archivo `navigation-link.spec.ts`. Este es el archivo de prueba de la clase `NavigationLink`. Actualícelo con lo siguiente:

   ```js
   import { NavigationLink } from './navigation-link';
   
   describe('NavigationLink', () => {
       it('should create an instance', () => {
           const data = {
               children: [],
               level: 1,
               active: false,
               path: '/content/wknd-spa-angular/us/en/home/page-1',
               description: null,
               url: '/content/wknd-spa-angular/us/en/home/page-1.html',
               lastModified: 1589429385100,
               title: 'Page 1'
           };
           expect(new NavigationLink(data)).toBeTruthy();
       });
   });
   ```

   Observe que `const data` sigue el mismo modelo JSON inspeccionado anteriormente para un único enlace. Esto está lejos de ser una prueba unitaria sólida, pero debería bastar para probar el constructor de `NavigationLink`.

7. Abra el archivo `navigation.component.ts`. Actualícelo con lo siguiente:

   ```js
   import { Component, OnInit, Input } from '@angular/core';
   import { NavigationLink } from './navigation-link';
   
   @Component({
   selector: 'app-navigation',
   templateUrl: './navigation.component.html',
   styleUrls: ['./navigation.component.scss']
   })
   export class NavigationComponent implements OnInit {
   
       @Input() items: object[];
   
       constructor() { }
   
       get navigationLinks(): NavigationLink[] {
   
           if (this.items && this.items.length > 0) {
               return this.items.map(item => {
                   return new NavigationLink(item);
               });
           }
   
           return null;
       }
   
       ngOnInit() {}
   
   }
   ```

   `NavigationComponent` espera un  `object[]` nombre  `items` que sea el modelo JSON de AEM. Esta clase expone un único método `get navigationLinks()` que devuelve una matriz de objetos `NavigationLink`.

8. Abra el archivo `navigation.component.html` y actualícelo con lo siguiente:

   ```html
   <ul *ngIf="navigationLinks && navigationLinks.length > 0" class="navigation__group">
       <ng-container *ngTemplateOutlet="recursiveListTmpl; context:{ links: navigationLinks }"></ng-container>
   </ul>
   ```

   Esto genera un `<ul>` inicial y llama al método `get navigationLinks()` desde `navigation.component.ts`. Se utiliza un `<ng-container>` para realizar una llamada a una plantilla denominada `recursiveListTmpl` y pasa el `navigationLinks` como una variable denominada `links`.

   Añada el `recursiveListTmpl` siguiente:

   ```html
   <ng-template #recursiveListTmpl let-links="links">
       <li *ngFor="let link of links" class="{{'navigation__item navigation__item--' + link.level}}">
           <a [routerLink]="link.url" class="navigation__item-link" [title]="link.title" [attr.aria-current]="link.active">
               {{link.title}}
           </a>
           <ul *ngIf="link.children && link.children.length > 0">
               <ng-container *ngTemplateOutlet="recursiveListTmpl; context:{ links: link.children }"></ng-container>
           </ul>
       </li>
   </ng-template>
   ```

   Aquí se implementa el resto del procesamiento del vínculo de navegación. Tenga en cuenta que la variable `link` es de tipo `NavigationLink` y que todos los métodos/propiedades creados por esa clase están disponibles. [`[routerLink]`](https://angular.io/api/router/RouterLink) en lugar del  `href` atributo normal. Esto nos permite establecer vínculos a rutas específicas de la aplicación, sin necesidad de actualizar la página completa.

   La parte recursiva de la navegación también se implementa creando otro `<ul>` si el `link` actual tiene un arreglo de discos `children` no vacío.

9. Actualice `navigation.component.spec.ts` para agregar soporte para `RouterTestingModule`:

   ```diff
    ...
   + import { RouterTestingModule } from '@angular/router/testing';
    ...
    beforeEach(async(() => {
       TestBed.configureTestingModule({
   +   imports: [ RouterTestingModule ],
       declarations: [ NavigationComponent ]
       })
       .compileComponents();
    }));
    ...
   ```

   Se requiere añadir `RouterTestingModule` porque el componente utiliza `[routerLink]`.

10. Actualice `navigation.component.scss` para agregar algunos estilos básicos a `NavigationComponent`:

   ```scss
   @import "~src/styles/variables";
   
   $link-color: $black;
   $link-hover-color: $white;
   $link-background: $black;
   
   :host-context {
       display: block;
       width: 100%;
   }
   
   .navigation__item {
       list-style: none;
   }
   
   .navigation__item-link {
       color: $link-color;
       font-size: $font-size-large;
       text-transform: uppercase;
       padding: $gutter-padding;
       display: flex;
       border-bottom: 1px solid $gray;
   
       &:hover {
           background: $link-background;
           color: $link-hover-color;
       }
   
   }
   ```

## Actualización del componente de encabezado

Ahora que el `NavigationComponent` se ha implementado, el `HeaderComponent` debe actualizarse para hacer referencia a él.

1. Abra un terminal y vaya a la carpeta `ui.frontend` dentro del proyecto de SPA. Inicio del **servidor de desarrollo de webpack**:

   ```shell
   $ npm start
   ```

2. Abra una ficha del explorador y vaya a [http://localhost:4200/](http://localhost:4200/).

   El **servidor de desarrollo de webpack** debe configurarse para proxy el modelo JSON desde una instancia local de AEM (`ui.frontend/proxy.conf.json`). Esto nos permitirá codificar directamente con el contenido creado en AEM a partir de la versión anterior del tutorial.

   ![alternancia de menús](./assets/navigation-routing/nav-toggle-static.gif)

   El `HeaderComponent` tiene ya implementada la funcionalidad de alternancia de menú. A continuación, agregue el componente de navegación.

3. Vuelva al IDE de su elección y abra el archivo `header.component.ts` en `ui.frontend/src/app/components/header/header.component.ts`.
4. Actualice el método `setHomePage()` para eliminar la cadena codificada y utilizar las props dinámicas transferidas por el componente AEM:

   ```js
   /* header.component.ts */
   import { NavigationLink } from '../navigation/navigation-link';
   ...
    setHomePage() {
       if (this.hasNavigation) {
           const rootNavigationLink: NavigationLink = new NavigationLink(this.items[0]);
           this.isHome = rootNavigationLink.path === this.route.snapshot.data.path;
           this.homePageUrl = rootNavigationLink.url;
       }
   }
   ...
   ```

   Se crea una nueva instancia de `NavigationLink` basada en `items[0]`, la raíz del modelo JSON de navegación que se transfiere desde AEM. `this.route.snapshot.data.path` devuelve la ruta de la ruta angular actual. Este valor se utiliza para determinar si la ruta actual es la **Página de inicio**. `this.homePageUrl` se utiliza para rellenar el vínculo de anclaje en el  **logotipo**.

5. Abra `header.component.html` y reemplace el marcador de posición estático para la navegación con una referencia al `NavigationComponent` recién creado:

   ```diff
       <div class="header-navigation">
           <div class="navigation">
   -            Navigation Placeholder
   +           <app-navigation [items]="items"></app-navigation>
           </div>
       </div>
   ```

   `[items]=items` pasa el atributo  `@Input() items` desde el  `HeaderComponent` a  `NavigationComponent` donde generará la navegación.

6. Abra `header.component.spec.ts` y agregue una declaración para `NavigationComponent`:

   ```diff
       /* header.component.spect.ts */
   +   import { NavigationComponent } from '../navigation/navigation.component';
   
       describe('HeaderComponent', () => {
       let component: HeaderComponent;
       let fixture: ComponentFixture<HeaderComponent>;
   
       beforeEach(async(() => {
           TestBed.configureTestingModule({
           imports: [ RouterTestingModule ],
   +       declarations: [ HeaderComponent, NavigationComponent ]
           })
           .compileComponents();
       }));
   ```

   Dado que el `NavigationComponent` ahora se utiliza como parte del `HeaderComponent`, debe declararse como parte del banco de pruebas.

7. Guarde los cambios en los archivos abiertos y vuelva al **servidor de desarrollo de webpack**: [http://localhost:4200/](http://localhost:4200/)

   ![Navegación de encabezado completada](assets/navigation-routing/completed-header.png)

   Para abrir la navegación, haga clic en el botón de alternancia del menú y verá los vínculos de navegación rellenados. Debería poder navegar a diferentes vistas del SPA.

## Comprender el enrutamiento SPA

Ahora que la navegación se ha implementado, inspeccione el enrutamiento en AEM.

1. En el IDE, abra el archivo `app-routing.module.ts` en `ui.frontend/src/app`.

   ```js
   /* app-routing.module.ts */
   import { AemPageDataResolver, AemPageRouteReuseStrategy } from '@adobe/cq-angular-editable-components';
   import { NgModule } from '@angular/core';
   import { RouteReuseStrategy, RouterModule, Routes, UrlMatchResult, UrlSegment } from '@angular/router';
   import { PageComponent } from './components/page/page.component';
   
   export function AemPageMatcher(url: UrlSegment[]): UrlMatchResult {
       if (url.length) {
           return {
               consumed: url,
               posParams: {
                   path: url[url.length - 1]
               }
           };
       }
   }
   
   const routes: Routes = [
       {
           matcher: AemPageMatcher,
           component: PageComponent,
           resolve: {
               path: AemPageDataResolver
           }
       }
   ];
   @NgModule({
       imports: [RouterModule.forRoot(routes)],
       exports: [RouterModule],
       providers: [
           AemPageDataResolver,
           {
           provide: RouteReuseStrategy,
           useClass: AemPageRouteReuseStrategy
           }
       ]
   })
   export class AppRoutingModule {}
   ```

   La matriz `routes: Routes = [];` define las rutas o rutas de navegación a las asignaciones de componentes angulares.

   `AemPageMatcher` es un  [UrlMatcher](https://angular.io/api/router/UrlMatcher) de enrutador angular personalizado que coincide con cualquier elemento que &quot;parezca&quot; una página de AEM que forme parte de esta aplicación angular.

   `PageComponent` es el componente angular que representa una página en AEM y se invocarán las rutas coincidentes. El `PageComponent` será inspeccionado más a fondo.

   `AemPageDataResolver`, proporcionado por el SDK de JS Editor de SPA de AEM, es una  [resolución de enrutador ](https://angular.io/api/router/Resolve) angular personalizada que se utiliza para transformar la dirección URL de ruta, que es la ruta en AEM que incluye la extensión .html, en la ruta del recurso en AEM, que es la ruta de la página menos la extensión.

   Por ejemplo: `AemPageDataResolver` transforma la dirección URL de una ruta de `content/wknd-spa-angular/us/en/home.html` en una ruta de `/content/wknd-spa-angular/us/en/home`. Se utiliza para resolver el contenido de la página en función de la ruta en la API del modelo JSON.

   `AemPageRouteReuseStrategy`, proporcionado por el SDK de JS AEM Editor SPA, es una  [](https://angular.io/api/router/RouteReuseStrategy) RouteReuseStrategy personalizada que evita la reutilización de las rutas  `PageComponent` cruzadas. De lo contrario, el contenido de la página &quot;A&quot; podría aparecer al desplazarse a la página &quot;B&quot;.

2. Abra el archivo `page.component.ts` en `ui.frontend/src/app/components/page/`.

   ```js
   ...
   export class PageComponent {
       items;
       itemsOrder;
       path;
   
       constructor(
           private route: ActivatedRoute,
           private modelManagerService: ModelManagerService
       ) {
           this.modelManagerService
           .getData({ path: this.route.snapshot.data.path })
           .then(data => {
               this.path = data[Constants.PATH_PROP];
               this.items = data[Constants.ITEMS_PROP];
               this.itemsOrder = data[Constants.ITEMS_ORDER_PROP];
           });
       }
   }
   ```

   El `PageComponent` es necesario para procesar el JSON recuperado de AEM y se utiliza como componente angular para procesar las rutas.

   `ActivatedRoute`, que proporciona el módulo Enrutador angular, contiene el estado que indica qué contenido JSON de AEM página debe cargarse en esta instancia de componente Página angular.

   `ModelManagerService`, obtiene los datos JSON en función de la ruta y los asigna a variables de clase  `path`,  `items`,  `itemsOrder`. A continuación, se pasarán a [AEMPageComponent](https://www.npmjs.com/package/@adobe/cq-angular-editable-components#aempagecomponent.md)

3. Abra el archivo `page.component.html` en `ui.frontend/src/app/components/page/`

   ```html
   <aem-page 
       class="structure-page" 
       [attr.data-cq-page-path]="path" 
       [cqPath]="path" 
       [cqItems]="items" 
       [cqItemsOrder]="itemsOrder">
   </aem-page>
   ```

   `aem-page` incluye  [AEMPageComponent](https://www.npmjs.com/package/@adobe/cq-angular-editable-components#aempagecomponent.md). Las variables `path`, `items` y `itemsOrder` se pasan a `AEMPageComponent`. El `AemPageComponent`, que se proporciona a través de los SDK de JavaScript del Editor de SPA, repetirá estos datos y creará instancias dinámicas de componentes angulares basados en los datos JSON, como se ve en el [tutorial de componentes de mapa](./map-components.md).

   El `PageComponent` es simplemente un proxy para el `AEMPageComponent` y es el `AEMPageComponent` el que hace que la mayoría del trabajo pesado asigne correctamente el modelo JSON a los componentes angulares.

## Inspect el enrutamiento SPA en AEM

1. Abra un terminal y detenga el **servidor de desarrollo de webpack** si se inicia. Vaya a la raíz del proyecto e implemente el proyecto para AEM con sus habilidades Maven:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!CAUTION]
   >
   > El proyecto Angular tiene habilitadas algunas reglas de iluminación muy estrictas. Si la compilación de Maven falla, compruebe el error y busque los **errores de Lint encontrados en los archivos enumerados.**. Corrija cualquier problema que encuentre el filtro y vuelva a ejecutar el comando Maven.

2. Vaya a la página principal de SPA en AEM: [http://localhost:4502/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/content/wknd-spa-angular/us/en/home.html) y abra las herramientas de desarrollador del explorador. Las capturas de pantalla siguientes se obtienen del navegador Google Chrome.

   Actualice la página y debe ver una solicitud XHR a `/content/wknd-spa-angular/us/en.model.json`, que es la raíz de SPA. Tenga en cuenta que solo se incluyen tres páginas secundarias en función de la configuración de profundidad de jerarquía de la plantilla raíz de SPA realizada anteriormente en el tutorial. Esto no incluye **Página 3**.

   ![Solicitud JSON inicial: raíz SPA](assets/navigation-routing/initial-json-request.png)

3. Con las herramientas de desarrollo abiertas, navegue a **Página 3**:

   ![Navegación por la página 3](assets/navigation-routing/page-three-navigation.png)

   Observe que se realiza una nueva solicitud XHR a: `/content/wknd-spa-angular/us/en/home/page-2/page-3.model.json`

   ![Página tres Solicitud XHR](assets/navigation-routing/page-3-xhr-request.png)

   El Administrador de modelos de AEM entiende que el contenido **Página 3** JSON no está disponible y activa automáticamente la solicitud XHR adicional.

4. Continúe navegando por la SPA mediante los distintos vínculos de navegación. Observe que no se realizan solicitudes XHR adicionales y que no se actualiza la página completa. Esto hace que el SPA sea más rápido para el usuario final y reduce las solicitudes innecesarias a AEM.

   ![Navegación implementada](assets/navigation-routing/final-navigation-implemented.gif)

5. Experimente con vínculos profundos navegando directamente a: [http://localhost:4502/content/wknd-spa-angular/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-angular/us/en/home/page-2.html). Observe que el botón Atrás del explorador sigue funcionando.

## Felicitaciones! {#congratulations}

Enhorabuena, ha aprendido cómo se pueden admitir varias vistas en el SPA asignando a AEM páginas con el SDK del Editor de SPA. La navegación dinámica se ha implementado mediante el enrutamiento angular y se ha agregado al componente `Header`.

Siempre puede realizar la vista del código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/navigation-routing-solution) o extraer el código localmente cambiando a la rama `Angular/navigation-routing-solution`.

### Próximos pasos {#next-steps}

[Crear un componente](custom-component.md)  personalizado: aprenda a crear un componente personalizado para utilizarlo con el Editor de SPA de AEM. Obtenga información sobre cómo desarrollar diálogos de autor y modelos Sling para ampliar el modelo JSON y rellenar un componente personalizado.
