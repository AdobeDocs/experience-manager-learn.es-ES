---
title: Agregar navegación y enrutamiento | Introducción al Editor de SPA de AEM y Angular
description: Descubra cómo se admiten varias vistas en la SPA mediante páginas de AEM y el Editor de SPA de SDK. La navegación dinámica se implementa mediante rutas de Angular y se añade a un componente de encabezado existente.
feature: SPA Editor
version: Experience Manager as a Cloud Service
jira: KT-5312
thumbnail: 5312-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 197a0c1f-4d0a-4b99-ba89-cdff2e6ac4ec
duration: 669
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '2531'
ht-degree: 0%

---

# Agregar navegación y enrutamiento {#navigation-routing}

Descubra cómo se admiten varias vistas en la SPA mediante páginas de AEM y el Editor de SPA de SDK. La navegación dinámica se implementa mediante rutas de Angular y se añade a un componente de encabezado existente.

## Objetivo

1. Comprenda las opciones de enrutamiento del modelo de SPA disponibles al utilizar el Editor de SPA.
2. Aprenda a utilizar el [enrutamiento de Angular](https://angular.io/guide/router) para navegar entre diferentes vistas de la SPA.
3. Implemente una navegación dinámica controlada por la jerarquía de páginas de AEM.

## Qué va a generar

Este capítulo agrega un menú de navegación a un componente `Header` existente. El menú de navegación está gobernado por la jerarquía de páginas de AEM y usa el modelo JSON proporcionado por el [componente principal de navegación](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/navigation.html).

![Navegación implementada](assets/navigation-routing/final-navigation-implemented.gif)

## Requisitos previos

Revise las herramientas y las instrucciones necesarias para configurar un [entorno de desarrollo local](overview.md#local-dev-environment).

### Obtener el código

1. Descargue el punto de partida para este tutorial mediante Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/navigation-routing-start
   ```

2. Implemente el código base en una instancia local de AEM mediante Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Si usa [AEM 6.x](overview.md#compatibility), agregue el perfil `classic`:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. Instale el paquete terminado para el [sitio de referencia WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) tradicional. Las imágenes proporcionadas por [sitio de referencia WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) se reutilizarán en el SPA de WKND. El paquete se puede instalar usando [Administrador de paquetes de AEM](http://localhost:4502/crx/packmgr/index.jsp).

   ![Administrador de paquetes instala wknd.all](./assets/map-components/package-manager-wknd-all.png)

Siempre puede ver el código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/navigation-routing-solution) o desprotegerlo localmente cambiando a la rama `Angular/navigation-routing-solution`.

## Inspeccionar actualizaciones de componentes de encabezado {#inspect-header}

En capítulos anteriores, el componente `HeaderComponent` se agregó como un componente Angular puro incluido mediante `app.component.html`. En este capítulo, el componente `HeaderComponent` se quita de la aplicación y se agrega mediante el [Editor de plantillas](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html?lang=es). Esto permite a los usuarios configurar el menú de navegación de `HeaderComponent` desde AEM.

>[!NOTE]
>
> Ya se han realizado varias actualizaciones de CSS y JavaScript en la base de código para iniciar este capítulo. Para centrarse en los conceptos principales, no se discuten **todos** los cambios de código. Puede ver los cambios completos [aquí](https://github.com/adobe/aem-guides-wknd-spa/compare/Angular/map-components-solution...Angular/navigation-routing-start).

1. En el IDE de su elección, abra el proyecto de inicio de SPA para este capítulo.
2. Debajo del módulo `ui.frontend`, inspeccione el archivo `header.component.ts` en: `ui.frontend/src/app/components/header/header.component.ts`.

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

3. En el módulo `ui.apps`, inspeccione la definición del componente del componente `Header` de AEM: `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/header/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Header"
       sling:resourceSuperType="wknd-spa-angular/components/navigation"
       componentGroup="WKND SPA Angular - Structure"/>
   ```

   El componente `Header` de AEM heredará toda la funcionalidad del [componente principal de navegación](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/navigation.html) a través de la propiedad `sling:resourceSuperType`.

## Añadir el componente HeaderComponent a la plantilla de la SPA {#add-header-template}

1. Abra un explorador e inicie sesión en AEM, [http://localhost:4502/](http://localhost:4502/). La base del código de inicio ya debería implementarse.
2. Vaya a la **[!UICONTROL plantilla de página SPA]**: [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html).
3. Seleccione el **[!UICONTROL contenedor de diseño raíz más externo]** y haga clic en su icono **[!UICONTROL directiva]**. Tenga cuidado **no** al seleccionar el **[!UICONTROL contenedor de diseño]** desbloqueado para la creación.

   ![Seleccione el icono de directiva del contenedor de diseño raíz](assets/navigation-routing/root-layout-container-policy.png)

4. Copie la directiva actual y cree una directiva nueva con el nombre **[!UICONTROL Estructura de SPA]**:

   ![Política de estructura de SPA](assets/map-components/spa-policy-update.png)

   En **[!UICONTROL Componentes permitidos]** > **[!UICONTROL General]** > seleccione el componente **[!UICONTROL Contenedor de diseño]**.

   En **[!UICONTROL Componentes permitidos]** > **[!UICONTROL WKND SPA ANGULAR - ESTRUCTURA]** > seleccione el componente **[!UICONTROL Encabezado]**:

   ![Seleccionar componente de encabezado](assets/map-components/select-header-component.png)

   En **[!UICONTROL Componentes permitidos]** > **[!UICONTROL WKND SPA ANGULAR - Contenido]** > seleccione los componentes **[!UICONTROL Imagen]** y **[!UICONTROL Texto]**. Debe tener un total de 4 componentes seleccionados.

   Haga clic en **[!UICONTROL Listo]** para guardar los cambios.

5. **Actualizar** la página. Agregue el componente **[!UICONTROL Header]** sobre el **[!UICONTROL Contenedor de diseño]** desbloqueado:

   ![agregar componente de encabezado a la plantilla](./assets/navigation-routing/add-header-component.gif)

6. Seleccione el componente **[!UICONTROL Header]** y haga clic en su icono **Policy** para editar la directiva.

   ![Haga clic en la directiva de encabezado](assets/navigation-routing/header-policy-icon.png)

7. Cree una nueva directiva con un **[!UICONTROL Título de directiva]** de **&quot;Encabezado de SPA de WKND&quot;**.

   En **[!UICONTROL Propiedades]**:

   * Establezca **[!UICONTROL Raíz de navegación]** en `/content/wknd-spa-angular/us/en`.
   * Establecer **[!UICONTROL Excluir niveles de raíz]** en **1**.
   * Desmarque **[!UICONTROL Recopilar todas las páginas secundarias]**.
   * Establezca la **[!UICONTROL profundidad de la estructura de navegación]** en **3**.

   ![Configurar directiva de encabezado](assets/navigation-routing/header-policy.png)

   Se recopilará la navegación 2 niveles por debajo de `/content/wknd-spa-angular/us/en`.

8. Después de guardar los cambios, debería ver los `Header` rellenados como parte de la plantilla:

   ![Componente de encabezado rellenado](assets/navigation-routing/populated-header.png)

## Creación de páginas secundarias

A continuación, cree páginas adicionales en AEM que sirvan como vistas diferentes en la SPA. También analizaremos la estructura jerárquica del modelo JSON proporcionado por AEM.

1. Vaya a la consola de **Sites**: [http://localhost:4502/sites.html/content/wknd-spa-angular/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-angular/us/en/home). Seleccione la **página de inicio de Angular de la SPA de WKND** y haga clic en **[!UICONTROL Crear]** > **[!UICONTROL Página]**:

   ![Crear nueva página](assets/navigation-routing/create-new-page.png)

2. En **[!UICONTROL Plantilla]**, seleccione **[!UICONTROL Página de SPA]**. En **[!UICONTROL Propiedades]**, escriba **&quot;Página 1&quot;** para **[!UICONTROL Título]** y **&quot;Página-1&quot;** como nombre.

   ![Escriba las propiedades de la página inicial](assets/navigation-routing/initial-page-properties.png)

   Haga clic en **[!UICONTROL Crear]** y, en el cuadro de diálogo emergente, haga clic en **[!UICONTROL Abrir]** para abrir la página en el Editor de SPA de AEM.

3. Agregue un nuevo componente **[!UICONTROL Text]** al **[!UICONTROL contenedor de diseño principal]**. Edite el componente e introduzca el texto: **&quot;Página 1&quot;** con el RTE y el elemento **H1** (tendrá que ingresar al modo de pantalla completa para cambiar los elementos del párrafo)

   ![Página de contenido de muestra 1](assets/navigation-routing/page-1-sample-content.png)

   No dude en añadir contenido adicional, como una imagen.

4. Vuelva a la consola de AEM Sites y repita los pasos anteriores, creando una segunda página denominada **&quot;Página 2&quot;** como elemento secundario de **Página 1**. Agregue contenido a **Página 2** para que se pueda identificar fácilmente.
5. Por último, cree una tercera página, **&quot;Página 3&quot;**, pero como **elemento secundario** de **Página 2**. Una vez completada, la jerarquía del sitio debe tener el siguiente aspecto:

   ![Jerarquía de sitios de muestra](assets/navigation-routing/wknd-spa-sample-site-hierarchy.png)

6. En una pestaña nueva, abra la API del modelo JSON proporcionada por AEM: [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json). Este contenido JSON se solicita cuando se carga la SPA por primera vez. La estructura exterior tiene el siguiente aspecto:

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

   En `:children` debería ver una entrada para cada una de las páginas creadas. El contenido de todas las páginas se encuentra en esta solicitud JSON inicial. Una vez implementado el enrutamiento de navegación, las vistas posteriores de la SPA se cargan rápidamente, ya que el contenido ya está disponible en el lado del cliente.

   No es aconsejable cargar **ALL** del contenido de una SPA en la solicitud JSON inicial, ya que esto ralentizaría la carga inicial de la página. A continuación, veamos cómo se recopila la profundidad de jerarquía de las páginas.

7. Vaya a la plantilla **SPA Root** en: [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-app-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-app-template/structure.html).

   Haga clic en el **[!UICONTROL menú Propiedades de página]** > **[!UICONTROL Directiva de página]**:

   ![Abrir la directiva de página para la raíz de SPA](assets/navigation-routing/open-page-policy.png)

8. La plantilla **SPA Root** tiene una ficha **[!UICONTROL Estructura jerárquica]** adicional para controlar el contenido JSON recopilado. La **[!UICONTROL profundidad de la estructura]** determina la profundidad en la jerarquía del sitio para recopilar páginas secundarias debajo de la **raíz**. También puede usar el campo **[!UICONTROL Patrones de estructura]** para filtrar páginas adicionales en función de una expresión regular.

   Actualizar la **[!UICONTROL profundidad de la estructura]** a **&quot;2&quot;**:

   ![Profundidad de la estructura de actualización](assets/navigation-routing/update-structure-depth.png)

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

   Observe que la ruta de acceso **Página 3** se ha eliminado: `/content/wknd-spa-angular/us/en/home/page-2/page-3` del modelo JSON inicial.

   Más adelante, observaremos cómo AEM SPA Editor SDK puede cargar dinámicamente contenido adicional.

## Implementación de la navegación

A continuación, implemente el menú de navegación con un nuevo `NavigationComponent`. Podríamos agregar el código directamente en `header.component.html`, pero se recomienda evitar componentes grandes. En su lugar, implemente un `NavigationComponent` que podría reutilizarse más adelante.

1. Revise el JSON expuesto por el componente de AEM `Header` en [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json):

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

   La naturaleza jerárquica de las páginas de AEM se modela en el JSON, que se puede utilizar para rellenar un menú de navegación. Recuerde que el componente `Header` hereda toda la funcionalidad del [componente principal de navegación](https://www.aemcomponents.dev/content/core-components-examples/library/core-structure/navigation.html) y que el contenido expuesto a través del JSON se asigna automáticamente a la anotación `@Input` de Angular.

2. Abra una nueva ventana de terminal y vaya a la carpeta `ui.frontend` del proyecto de SPA. Cree un nuevo(a) `NavigationComponent` mediante la herramienta CLI de Angular:

   ```shell
   $ cd ui.frontend
   $ ng generate component components/navigation
   CREATE src/app/components/navigation/navigation.component.scss (0 bytes)
   CREATE src/app/components/navigation/navigation.component.html (25 bytes)
   CREATE src/app/components/navigation/navigation.component.spec.ts (656 bytes)
   CREATE src/app/components/navigation/navigation.component.ts (286 bytes)
   UPDATE src/app/app.module.ts (2032 bytes)
   ```

3. A continuación, cree una clase denominada `NavigationLink` utilizando la CLI de Angular en el directorio `components/navigation` recién creado:

   ```shell
   $ cd src/app/components/navigation/
   $ ng generate class NavigationLink
   CREATE src/app/components/navigation/navigation-link.spec.ts (187 bytes)
   CREATE src/app/components/navigation/navigation-link.ts (32 bytes)
   ```

4. Vuelva al IDE de su elección y abra el archivo en `navigation-link.ts` a las `/src/app/components/navigation/navigation-link.ts`.

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

   Se trata de una clase simple que representa un vínculo de navegación individual. En el constructor de clase esperamos que `data` sea el objeto JSON pasado desde AEM. Esta clase se usa tanto en `NavigationComponent` como en `HeaderComponent` para rellenar fácilmente la estructura de exploración.

   No se realiza ninguna transformación de datos, esta clase se crea principalmente para escribir de forma segura el modelo JSON. Observe que `this.children` tiene el tipo `NavigationLink[]` y que el constructor crea recursivamente nuevos objetos `NavigationLink` para cada uno de los elementos de la matriz `children`. Recuerde que el modelo JSON para `Header` es jerárquico.

6. Abra el archivo `navigation-link.spec.ts`. Este es el archivo de prueba para la clase `NavigationLink`. Actualícelo con lo siguiente:

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

   Observe que `const data` sigue el mismo modelo JSON inspeccionado anteriormente para un solo vínculo. No se trata de una prueba unitaria sólida; sin embargo, debería bastar con probar el constructor de `NavigationLink`.

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

   `NavigationComponent` espera un `object[]` denominado `items` que es el modelo JSON de AEM. Esta clase expone un único método `get navigationLinks()` que devuelve una matriz de `NavigationLink` objetos.

8. Abra el archivo `navigation.component.html` y actualícelo con lo siguiente:

   ```html
   <ul *ngIf="navigationLinks && navigationLinks.length > 0" class="navigation__group">
       <ng-container *ngTemplateOutlet="recursiveListTmpl; context:{ links: navigationLinks }"></ng-container>
   </ul>
   ```

   Esto genera un `<ul>` inicial y llama al método `get navigationLinks()` desde `navigation.component.ts`. Se utiliza un `<ng-container>` para realizar una llamada a una plantilla denominada `recursiveListTmpl` y se pasa el `navigationLinks` como una variable denominada `links`.

   Agregar `recursiveListTmpl` siguiente:

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

   Aquí se implementa el resto de la renderización del vínculo de navegación. Tenga en cuenta que la variable `link` es del tipo `NavigationLink` y que todos los métodos/propiedades creados por esa clase están disponibles. [`[routerLink]`](https://angular.io/api/router/RouterLink) se usa en lugar del atributo `href` normal. Esto nos permite vincular a rutas específicas en la aplicación, sin una actualización de página completa.

   La parte recursiva de la navegación también se implementa creando otro `<ul>` si el objeto `link` actual tiene una matriz `children` que no esté vacía.

9. Actualizar `navigation.component.spec.ts` para agregar compatibilidad con `RouterTestingModule`:

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

   Se requiere agregar `RouterTestingModule` porque el componente usa `[routerLink]`.

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

## Actualización del componente del encabezado

Ahora que `NavigationComponent` se ha implementado, `HeaderComponent` debe actualizarse para hacer referencia a él.

1. Abra un terminal y vaya a la carpeta `ui.frontend` dentro del proyecto SPA. Inicie el **servidor de desarrollo de Webpack**:

   ```shell
   $ npm start
   ```

2. Abra una ficha del explorador y vaya a [http://localhost:4200/](http://localhost:4200/).

   El **servidor de desarrollo de Webpack** debe configurarse para representar el modelo JSON desde una instancia local de AEM (`ui.frontend/proxy.conf.json`). Esto nos permite codificar directamente en el contenido creado en AEM anteriormente en el tutorial.

   ![la opción de menú está funcionando](./assets/navigation-routing/nav-toggle-static.gif)

   `HeaderComponent` tiene implementada actualmente la funcionalidad de alternancia de menús. A continuación, añada el componente de navegación.

3. Vuelva al IDE que desee y abra el archivo `header.component.ts` en `ui.frontend/src/app/components/header/header.component.ts`.
4. Actualice el método `setHomePage()` para quitar el objeto String codificado de forma rígida y utilice las props dinámicas pasadas por el componente AEM:

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

   Se crea una nueva instancia de `NavigationLink` basada en `items[0]`, la raíz del modelo de navegación JSON pasado desde AEM. `this.route.snapshot.data.path` devuelve la ruta de acceso de la ruta de acceso de Angular actual. Este valor se usa para determinar si la ruta actual es la **página principal**. `this.homePageUrl` se usa para rellenar el vínculo de anclaje en el **logotipo**.

5. Abra `header.component.html` y reemplace el marcador de posición estático para la navegación con una referencia al `NavigationComponent` recién creado:

   ```diff
       <div class="header-navigation">
           <div class="navigation">
   -            Navigation Placeholder
   +           <app-navigation [items]="items"></app-navigation>
           </div>
       </div>
   ```

   El atributo `[items]=items` pasa `@Input() items` de `HeaderComponent` a `NavigationComponent`, donde generará la navegación.

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

   Dado que `NavigationComponent` se utiliza ahora como parte de `HeaderComponent`, debe declararse como parte del banco de pruebas.

7. Guardar cambios en cualquier archivo abierto y volver al **servidor de desarrollo de Webpack**: [http://localhost:4200/](http://localhost:4200/)

   ![Navegación de encabezado completada](assets/navigation-routing/completed-header.png)

   Abra la navegación haciendo clic en el botón de opción de menú y verá los vínculos de navegación rellenados. Debe poder navegar a diferentes vistas de la SPA.

## Comprender el enrutamiento de SPA

Ahora que la navegación ha sido implementada, inspeccione el enrutamiento en AEM.

1. En el IDE, abra el archivo `app-routing.module.ts` a las `ui.frontend/src/app`.

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

   La matriz `routes: Routes = [];` define las rutas o rutas de navegación a las asignaciones de componentes de Angular.

   `AemPageMatcher` es un enrutador de Angular personalizado [UrlMatcher](https://angular.io/api/router/UrlMatcher), que coincide con cualquier elemento que &quot;se parezca&quot; a una página de AEM que forma parte de esta aplicación de Angular.

   `PageComponent` es el componente de Angular que representa una página en AEM y que se utiliza para representar las rutas coincidentes. `PageComponent` se revisa más adelante en el tutorial.

   `AemPageDataResolver`, proporcionado por AEM SPA Editor JS SDK, es un [Angular Router Resolver](https://angular.io/api/router/Resolve) personalizado que se usa para transformar la dirección URL de ruta, que es la ruta en AEM incluyendo la extensión .html, a la ruta de recurso en AEM, que es la ruta de página menos la extensión.

   Por ejemplo, `AemPageDataResolver` transforma la dirección URL de una ruta de `content/wknd-spa-angular/us/en/home.html` en una ruta de acceso de `/content/wknd-spa-angular/us/en/home`. Se utiliza para resolver el contenido de la página en función de la ruta en la API del modelo JSON.

   `AemPageRouteReuseStrategy`, proporcionado por AEM SPA Editor JS SDK, es una [RouteReuseStrategy](https://angular.io/api/router/RouteReuseStrategy) personalizada que evita la reutilización de `PageComponent` entre rutas. De lo contrario, el contenido de la página &quot;A&quot; podría mostrarse al navegar a la página &quot;B&quot;.

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

   Se requiere `PageComponent` para procesar el JSON recuperado de AEM y se usa como componente de Angular para procesar las rutas.

   `ActivatedRoute`, que proporciona el módulo Angular Router, contiene el estado que indica qué contenido JSON de la página de AEM debe cargarse en esta instancia de componente de página de Angular.

   `ModelManagerService`, obtiene los datos JSON en función de la ruta y asigna los datos a las variables de clase `path`, `items`, `itemsOrder`. Estos se pasarán a [AEMPageComponent](https://www.npmjs.com/package/@adobe/cq-angular-editable-components#aempagecomponent.md)

3. Abrir el archivo `page.component.html` en `ui.frontend/src/app/components/page/`

   ```html
   <aem-page 
       class="structure-page" 
       [attr.data-cq-page-path]="path" 
       [cqPath]="path" 
       [cqItems]="items" 
       [cqItemsOrder]="itemsOrder">
   </aem-page>
   ```

   `aem-page` incluye [AEMPageComponent](https://www.npmjs.com/package/@adobe/cq-angular-editable-components#aempagecomponent.md). Las variables `path`, `items` y `itemsOrder` se pasan a `AEMPageComponent`. El `AemPageComponent`, proporcionado mediante el Editor de SPA de JavaScript SDK, iterará estos datos e instanciará dinámicamente componentes de Angular basados en los datos JSON, tal como se ve en el [tutorial de componentes de mapa](./map-components.md).

   `PageComponent` es realmente solo un proxy para `AEMPageComponent` y es `AEMPageComponent` el que hace la mayor parte del trabajo pesado para asignar correctamente el modelo JSON a los componentes de Angular.

## Inspeccionar el enrutamiento de SPA en AEM

1. Abra un terminal y detenga **webpack dev server** si se inició. Vaya a la raíz del proyecto e implemente el proyecto en AEM con sus habilidades con Maven:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!CAUTION]
   >
   > El proyecto de Angular tiene habilitadas algunas reglas de vinculación muy estrictas. Si la generación de Maven falla, compruebe el error y busque **Errores de Lint encontrados en los archivos enumerados.**. Corrija los problemas que encuentre el filtro y vuelva a ejecutar el comando de Maven.

2. Vaya a la página de inicio de SPA en AEM: [http://localhost:4502/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/content/wknd-spa-angular/us/en/home.html) y abra las herramientas para desarrolladores del explorador. Las capturas de pantalla a continuación se capturan desde el navegador Google Chrome.

   Actualice la página y debería ver una solicitud XHR a `/content/wknd-spa-angular/us/en.model.json`, que es la raíz de la SPA. Tenga en cuenta que solo se incluyen tres páginas secundarias en función de la configuración de profundidad de jerarquía en la plantilla raíz de SPA realizada anteriormente en el tutorial. Esto no incluye **Página 3**.

   ![Solicitud JSON inicial - Raíz de SPA](assets/navigation-routing/initial-json-request.png)

3. Con las herramientas para desarrolladores abiertas, vaya a **Página 3**:

   ![Navegar por la página 3](assets/navigation-routing/page-three-navigation.png)

   Observe que se hace una nueva solicitud XHR a: `/content/wknd-spa-angular/us/en/home/page-2/page-3.model.json`

   ![Solicitud XHR de página tres](assets/navigation-routing/page-3-xhr-request.png)

   El administrador de modelos de AEM comprende que el contenido JSON de la **página 3** no está disponible y déclencheur automáticamente la solicitud XHR adicional.

4. Continúe navegando por la SPA utilizando los distintos vínculos de navegación. Observe que no se realizan solicitudes XHR adicionales y que no se actualiza la página completa. Esto hace que el SPA sea más rápido para el usuario final y reduce las solicitudes innecesarias a AEM.

   ![Navegación implementada](assets/navigation-routing/final-navigation-implemented.gif)

5. Experimente con vínculos profundos directamente a: [http://localhost:4502/content/wknd-spa-angular/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-angular/us/en/home/page-2.html). Observe que el botón Atrás del explorador sigue funcionando.

## Enhorabuena. {#congratulations}

Felicidades, ha aprendido cómo se pueden admitir varias vistas en la SPA asignando a páginas de AEM con el SDK del Editor de SPA. La navegación dinámica se ha implementado mediante el enrutamiento Angular y se ha agregado al componente `Header`.

Siempre puede ver el código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/navigation-routing-solution) o desprotegerlo localmente cambiando a la rama `Angular/navigation-routing-solution`.

### Siguientes pasos {#next-steps}

[Crear un componente personalizado](custom-component.md): aprenda a crear un componente personalizado para utilizarlo con el Editor de SPA de AEM. Aprenda a desarrollar cuadros de diálogo de autor y modelos Sling para ampliar el modelo JSON y rellenar un componente personalizado.
