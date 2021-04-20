---
title: Asignación de componentes de SPA a componentes de AEM | Introducción a AEM SPA Editor y Angular
description: Obtenga información sobre cómo asignar componentes angulares a componentes de Adobe Experience Manager (AEM) con el SDK de JS del Editor de SPA de AEM. La asignación de componentes permite a los usuarios realizar actualizaciones dinámicas de los componentes de SPA dentro del Editor de SPA de AEM, de forma similar a la creación tradicional de AEM.
sub-product: sitios
feature: SPA Editor
topics: development
doc-type: tutorial
version: cloud-service
activity: develop
audience: developer
kt: 5311
thumbnail: 5311-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2392'
ht-degree: 1%

---


# Asignación de componentes de SPA a componentes de AEM {#map-components}

Obtenga información sobre cómo asignar componentes angulares a componentes de Adobe Experience Manager (AEM) con el SDK de JS del Editor de SPA de AEM. La asignación de componentes permite a los usuarios realizar actualizaciones dinámicas de los componentes de SPA dentro del Editor de SPA de AEM, de forma similar a la creación tradicional de AEM.

Este capítulo profundiza en la API del modelo JSON de AEM y en cómo el contenido JSON expuesto por un componente AEM se puede insertar automáticamente en un componente angular como props.

## Objetivo

1. Obtenga información sobre cómo asignar componentes de AEM a componentes de SPA.
2. Comprenda la diferencia entre los componentes **Container** y los componentes **Content**.
3. Cree un nuevo componente angular que se asigne a un componente de AEM existente.

## Qué va a generar

Este capítulo inspeccionará cómo se asigna el componente SPA `Text` proporcionado al componente `Text`de AEM. Se creará un nuevo componente `Image` SPA que se puede usar en la SPA y crear en AEM. Las funciones predeterminadas de las políticas **Contenedor de diseño** y **Editor de plantillas** también se utilizarán para crear una vista con un aspecto un poco más variado.

![Creación final de un ejemplo de capítulo](./assets/map-components/final-page.png)

## Requisitos previos

Revise las herramientas e instrucciones necesarias para configurar un [entorno de desarrollo local](overview.md#local-dev-environment).

### Obtención del código

1. Descargue el punto de partida para este tutorial mediante Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/map-components-start
   ```

2. Implemente el código base en una instancia local de AEM mediante Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Si utiliza [AEM 6.x](overview.md#compatibility), añada el perfil `classic`:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Siempre puede ver el código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/map-components-solution) o extraer el código localmente cambiando a la rama `Angular/map-components-solution`.

## Enfoque de asignación

El concepto básico es asignar un componente SPA a un componente AEM. Componentes de AEM, ejecute contenido de exportación del lado del servidor como parte de la API del modelo JSON. El contenido JSON lo consume la SPA, que ejecuta el lado del cliente en el explorador. Se crea una asignación 1:1 entre los componentes de SPA y un componente de AEM.

![Descripción general de alto nivel de la asignación de un componente AEM a un componente angular](./assets/map-components/high-level-approach.png)

*Descripción general de alto nivel de la asignación de un componente AEM a un componente angular*

## Inspeccione el componente de texto

El [Tipo de archivo del proyecto AEM](https://github.com/adobe/aem-project-archetype) proporciona un componente `Text` asignado al componente [Texto](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/text.html) de AEM. Este es un ejemplo de un componente **content**, en el que procesa *content* desde AEM.

Veamos cómo funciona el componente.

### Inspeccione el modelo JSON

1. Antes de pasar al código SPA, es importante comprender el modelo JSON que proporciona AEM. Vaya a la [Biblioteca de componentes principales](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/text.html) y vea la página del componente Texto. La biblioteca de componentes principales proporciona ejemplos de todos los componentes principales de AEM.
2. Seleccione la pestaña **JSON** para uno de los ejemplos:

   ![Modelo JSON de texto](./assets/map-components/text-json.png)

   Debería ver tres propiedades: `text`, `richText` y `:type`.

   `:type` es una propiedad reservada que enumera la  `sling:resourceType` (o ruta) del componente AEM. El valor de `:type` es lo que se utiliza para asignar el componente AEM al componente SPA.

   `text` y  `richText` son propiedades adicionales que se expondrán al componente SPA.

### Inspeccione el componente Texto

1. Abra un nuevo terminal y vaya a la carpeta `ui.frontend` dentro del proyecto. Ejecute `npm install` y luego `npm start` para iniciar el **servidor de desarrollo de webpack**:

   ```shell
   $ cd ui.frontend
   $ npm run start:mock
   ```

   El módulo `ui.frontend` está configurado actualmente para utilizar el modelo [burlar JSON](./integrate-spa.md#mock-json).

2. Debería ver una nueva ventana del explorador abierta a [http://localhost:4200/content/wknd-spa-angular/us/en/home.html](http://localhost:4200/content/wknd-spa-angular/us/en/home.html)

   ![Servidor de desarrollo de Webpack con contenido falso](assets/map-components/initial-start.png)

3. En el IDE que elija, abra el proyecto AEM para el SPA WKND. Expanda el módulo `ui.frontend` y abra el archivo **text.component.ts** en `ui.frontend/src/app/components/text/text.component.ts`:

   ![Código fuente de componente angular de Text.js](assets/map-components/vscode-ide-text-js.png)

4. El primer área a inspeccionar es `class TextComponent` en ~line 35:

   ```js
   export class TextComponent {
       @Input() richText: boolean;
       @Input() text: string;
       @Input() itemName: string;
   
       @HostBinding('innerHtml') get content() {
           return this.richText
           ? this.sanitizer.bypassSecurityTrustHtml(this.text)
           : this.text;
       }
       @HostBinding('attr.data-rte-editelement') editAttribute = true;
   
       constructor(private sanitizer: DomSanitizer) {}
   }
   ```

   [@Input()](https://angular.io/api/core/Input) decorator se utiliza para declarar campos cuyos valores se establecen mediante el objeto JSON asignado, revisado anteriormente.

   `@HostBinding('innerHtml') get content()` es un método que expone el contenido de texto creado del valor de  `this.text`. En el caso de que el contenido sea texto enriquecido (determinado por el indicador `this.richText`), se omite la seguridad integrada de Angular. El [DomSanitizer](https://angular.io/api/platform-browser/DomSanitizer) de Angular se usa para &quot;eliminar&quot; el HTML sin procesar y evitar vulnerabilidades de scripts en sitios múltiples. El método está enlazado a la propiedad `innerHtml` utilizando el decorador [@HostBinding](https://angular.io/api/core/HostBinding).

5. A continuación, revise el `TextEditConfig` en ~line 24:

   ```js
   const TextEditConfig = {
       emptyLabel: 'Text',
       isEmpty: cqModel =>
           !cqModel || !cqModel.text || cqModel.text.trim().length < 1
   };
   ```

   El código anterior es responsable de determinar cuándo procesar el marcador de posición en el entorno de creación AEM. Si el método `isEmpty` devuelve **true**, se representará el marcador de posición.

6. Finalmente, eche un vistazo a la llamada `MapTo` en ~line 53:

   ```js
   MapTo('wknd-spa-angular/components/text')(TextComponent, TextEditConfig );
   ```

   **** MapToes proporcionado por el SDK de JS del Editor de SPA de AEM (`@adobe/cq-angular-editable-components`). La ruta `wknd-spa-angular/components/text` representa el `sling:resourceType` del componente AEM. Esta ruta se compara con el `:type` expuesto por el modelo JSON observado anteriormente. **** MapTo analiza la respuesta del modelo JSON y pasa los valores correctos a las  `@Input()` variables del componente SPA.

   Puede encontrar la definición del componente AEM `Text` en `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/text`.

7. Experimente modificando el archivo **en.model.json** en `ui.frontend/src/mocks/json/en.model.json`.

   En ~line 62 actualice el primer valor `Text` para utilizar las etiquetas **`H1`** y **`u`** :

   ```json
       "text": {
           "text": "<h1><u>Hello World!</u></h1>",
           "richText": true,
           ":type": "wknd-spa-angular/components/text"
       }
   ```

   Vuelva al explorador para ver los efectos servidos por el **servidor de desarrollo de webpack**:

   ![Modelo de texto actualizado](assets/map-components/updated-text-model.png)

   Intente alternar la propiedad `richText` entre **true** / **false** para ver la lógica de renderización en acción.

8. Inspeccione **text.component.html** en `ui.frontend/src/app/components/text/text.component.html`.

   Este archivo está vacío, ya que la propiedad `innerHTML` establecerá todo el contenido del componente.

9. Inspeccione **app.module.ts** en `ui.frontend/src/app/app.module.ts`.

   ```js
   @NgModule({
   imports: [
       BrowserModule,
       SpaAngularEditableComponentsModule,
       AppRoutingModule
   ],
   providers: [ModelManagerService, { provide: APP_BASE_HREF, useValue: '/' }],
   declarations: [AppComponent, TextComponent, PageComponent, HeaderComponent],
   entryComponents: [TextComponent, PageComponent],
   bootstrap: [AppComponent]
   })
   export class AppModule {}
   ```

   El **TextComponent** no se incluye explícitamente, sino que se incluye dinámicamente mediante **AEMResponsiveGridComponent** proporcionado por el SDK JS del Editor de SPA de AEM. Por lo tanto, debe aparecer en la matriz **app.module.ts**&#39; [entryComponents](https://angular.io/guide/entry-components).

## Crear el componente de imagen

A continuación, cree un componente angular `Image` que esté asignado al componente de imagen [AEM](https://docs.adobe.com/content/help/es-ES/experience-manager-core-components/using/components/image.html). El componente `Image` es otro ejemplo de un componente **content**.

### Inspeccione el JSON

Antes de saltar al código SPA, revise el modelo JSON proporcionado por AEM.

1. Vaya a los [Ejemplos de imágenes de la biblioteca de componentes principales](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/image.html).

   ![JSON, componente principal de imagen](./assets/map-components/image-json.png)

   Las propiedades `src`, `alt` y `title` se utilizarán para rellenar el componente SPA `Image`.

   >[!NOTE]
   >
   > Hay otras propiedades de imagen expuestas (`lazyEnabled`, `widths`) que permiten a un desarrollador crear un componente adaptable y de carga diferida. El componente creado en este tutorial será sencillo y **no** utilizará estas propiedades avanzadas.

2. Vuelva a su IDE y abra `en.model.json` en `ui.frontend/src/mocks/json/en.model.json`. Dado que este es un nuevo componente para nuestro proyecto, necesitamos &quot;burlarnos&quot; del JSON de imagen.

   En ~line 70 añada una entrada JSON para el modelo `image` (no olvide la coma final `,` después de la segunda `text_386303036`) y actualice la matriz `:itemsOrder`.

   ```json
   ...
   ":items": {
               ...
               "text_386303036": {
                   "text": "<p>A new text component.</p>\r\n",
                   "richText": true,
                   ":type": "wknd-spa-angular/components/text"
                   },
               "image": {
                   "alt": "Rock Climber in New Zealand",
                   "title": "Rock Climber in New Zealand",
                   "src": "/mocks/images/adobestock-140634652.jpeg",
                   ":type": "wknd-spa-angular/components/image"
               }
           },
           ":itemsOrder": [
               "text",
               "text_386303036",
               "image"
           ],
   ```

   El proyecto incluye una imagen de ejemplo en `/mock-content/adobestock-140634652.jpeg` que se utilizará con el **servidor de desarrollo de webpack**.

   Puede ver el [en.model.json completo aquí](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/map-components-solution/ui.frontend/src/mocks/json/en.model.json).

3. Añada una foto de existencias para que la muestre el componente.

   Cree una nueva carpeta denominada **images** debajo de `ui.frontend/src/mocks`. Descargue [adobestock-140634652.jpeg](assets/map-components/adobestock-140634652.jpeg) y colóquelo en la carpeta **images** recién creada. Si lo desea, puede usar su propia imagen.

### Implementar el componente Imagen

1. Detenga el **servidor de desarrollo de webpack** si se inicia.
2. Cree un nuevo componente Imagen ejecutando el comando CLI `ng generate component` angular desde la carpeta `ui.frontend`:

   ```shell
   $ ng generate component components/image
   ```

3. En el IDE, abra **image.component.ts** en `ui.frontend/src/app/components/image/image.component.ts` y actualice como se indica a continuación:

   ```js
   import {Component, Input, OnInit} from '@angular/core';
   import {MapTo} from '@adobe/cq-angular-editable-components';
   
   const ImageEditConfig = {
   emptyLabel: 'Image',
   isEmpty: cqModel =>
       !cqModel || !cqModel.src || cqModel.src.trim().length < 1
   };
   
   @Component({
   selector: 'app-image',
   templateUrl: './image.component.html',
   styleUrls: ['./image.component.scss']
   })
   export class ImageComponent implements OnInit {
   
   @Input() src: string;
   @Input() alt: string;
   @Input() title: string;
   
   constructor() { }
   
   get hasImage() {
       return this.src && this.src.trim().length > 0;
   }
   
   ngOnInit() { }
   }
   
   MapTo('wknd-spa-angular/components/image')(ImageComponent, ImageEditConfig);
   ```

   `ImageEditConfig` es la configuración para determinar si se debe procesar el marcador de posición de autor en AEM, en función de si la  `src` propiedad se rellena.

   `@Input()` de  `src`,  `alt`, y  `title` son las propiedades asignadas desde la API de JSON.

   `hasImage()` es un método que determina si se debe representar la imagen.

   `MapTo` asigna el componente SPA al componente AEM ubicado en  `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/image`.

4. Abra **image.component.html** y actualícelo de la siguiente manera:

   ```html
   <ng-container *ngIf="hasImage">
       <img class="image" [src]="src" [alt]="alt" [title]="title"/>
   </ng-container>
   ```

   Esto renderizará el elemento `<img>` si `hasImage` devuelve **true**.

5. Abra **image.component.scss** y actualícelo de la siguiente manera:

   ```scss
   :host-context {
       display: block;
   }
   
   .image {
       margin: 1rem 0;
       width: 100%;
       border: 0;
   }
   ```

   >[!NOTE]
   >
   > La regla `:host-context` es **crítica** para que el marcador de posición del editor de SPA de AEM funcione correctamente. Todos los componentes de SPA que se pretendan crear en el editor de páginas de AEM necesitarán esta regla como mínimo.

6. Abra `app.module.ts` y añada `ImageComponent` a la matriz `entryComponents`:

   ```js
   entryComponents: [TextComponent, PageComponent, ImageComponent],
   ```

   Al igual que `TextComponent`, el `ImageComponent` se carga dinámicamente y debe incluirse en la matriz `entryComponents`.

7. Inicie el **servidor de desarrollo de webpack** para ver el `ImageComponent` renderizado.

   ```shell
   $ npm run start:mock
   ```

   ![Imagen agregada a la maqueta](assets/map-components/image-added-mock.png)

   *Imagen añadida a la SPA*

   >[!NOTE]
   >
   > **Desafío** para una bonificación: Implemente un nuevo método para mostrar el valor de  `title` como un rótulo debajo de la imagen.

## Actualización de directivas en AEM

El componente `ImageComponent` solo es visible en el **servidor de desarrollo de webpack**. A continuación, implemente la SPA actualizada en AEM y actualice las políticas de plantilla.

1. Detenga el **servidor de desarrollo de webpack** y, desde la **raíz** del proyecto, implemente los cambios en AEM con sus habilidades con Maven:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. En la pantalla Inicio de AEM, vaya a **[!UICONTROL Herramientas]** > **[!UICONTROL Plantillas]** > **[WKND SPA Angular](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd-spa-angular)**.

   Seleccione y edite la **Página de SPA**:

   ![Editar plantilla de página SPA](assets/map-components/edit-spa-page-template.png)

3. Seleccione el **Contenedor de diseño** y haga clic en su icono **directiva** para editar la política:

   ![Política de contenedor de diseño](./assets/map-components/layout-container-policy.png)

4. En **Componentes permitidos** > **WKND SPA Angular - Contenido** > compruebe el componente **Imagen**:

   ![Componente de imagen seleccionado](assets/map-components/check-image-component.png)

   En **Componentes predeterminados** > **Añadir asignación** y elija el componente **Imagen - WKND SPA Angular - Contenido**:

   ![Definición de componentes predeterminados](assets/map-components/default-components.png)

   Introduzca un **mime type** de `image/*`.

   Haga clic en **Listo** para guardar las actualizaciones de directivas.

5. En el **Contenedor de diseño** haga clic en el icono **directiva** para el componente **Texto**:

   ![Icono de política de componentes de texto](./assets/map-components/edit-text-policy.png)

   Cree una nueva directiva denominada **WKND SPA Text**. En **Plugins** > **Formatting**, marque todas las casillas para habilitar opciones de formato adicionales:

   ![Habilitar formato RTE](assets/map-components/enable-formatting-rte.png)

   En **Plugins** > **Estilos de párrafo**, marque la casilla **Habilitar estilos de párrafo**:

   ![Activar estilos de párrafo](./assets/map-components/text-policy-enable-paragraphstyles.png)

   Haga clic en **Listo** para guardar la actualización de la directiva.

6. Vaya a la **Página principal** [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html).

   También debe poder editar el componente `Text` y agregar estilos de párrafo adicionales en modo **pantalla completa**.

   ![Edición de texto enriquecido en pantalla completa](assets/map-components/full-screen-rte.png)

7. También debe poder arrastrar y soltar una imagen desde el **Buscador de recursos**:

   ![Arrastrar y soltar imagen](./assets/map-components/drag-drop-image.gif)

8. Añada sus propias imágenes a través de [AEM Assets](http://localhost:4502/assets.html/content/dam) o instale la base de código terminada para el [sitio de referencia WKND ](https://github.com/adobe/aem-guides-wknd/releases/latest) estándar. El [sitio de referencia de WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) incluye muchas imágenes que se pueden reutilizar en el SPA de WKND. El paquete se puede instalar utilizando el [Administrador de paquetes de AEM](http://localhost:4502/crx/packmgr/index.jsp).

   ![Package Manager instalar wknd.all](./assets/map-components/package-manager-wknd-all.png)

## Inspeccione el contenedor de diseño

El SDK del Editor de SPA de AEM proporciona automáticamente compatibilidad con el **Contenedor de diseño**. El **contenedor de diseño**, tal como indica el nombre, es un componente **contenedor**. Los componentes de contenedor son componentes que aceptan estructuras JSON que representan *otros* componentes y que crean instancias de ellos de forma dinámica.

Inspeccionemos más el contenedor de diseño.

1. En el IDE, abra **responsive-grid.component.ts** en `ui.frontend/src/app/components/responsive-grid`:

   ```js
   import { AEMResponsiveGridComponent,MapTo } from '@adobe/cq-angular-editable-components';
   
   MapTo('wcm/foundation/components/responsivegrid')(AEMResponsiveGridComponent);
   ```

   El `AEMResponsiveGridComponent` se implementa como parte del SDK del Editor de SPA de AEM y se incluye en el proyecto a través de `import-components`.

2. En un explorador, vaya a [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)

   ![API de modelo JSON: cuadrícula interactiva](./assets/map-components/responsive-grid-modeljson.png)

   El componente **Contenedor de diseño** tiene un `sling:resourceType` de `wcm/foundation/components/responsivegrid` y el Editor de SPA lo reconoce mediante la propiedad `:type` , al igual que los componentes `Text` y `Image` .

   Las mismas capacidades de cambiar el tamaño de un componente mediante [Modo de diseño](https://docs.adobe.com/content/help/en/experience-manager-65/authoring/siteandpage/responsive-layout.html#defining-layouts-layout-mode) están disponibles con el Editor de SPA.

3. Vuelva a [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html). Añada componentes **Image** adicionales e intente redimensionarlos con la opción **Layout**:

   ![Cambiar el tamaño de la imagen mediante el modo Diseño](./assets/map-components/responsive-grid-layout-change.gif)

4. Vuelva a abrir el modelo JSON [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json) y observe el `columnClassNames` como parte del JSON:

   ![Nombres de clase de columna](./assets/map-components/responsive-grid-classnames.png)

   El nombre de clase `aem-GridColumn--default--4` indica que el componente debe tener 4 columnas de ancho en función de una cuadrícula de 12 columnas. Puede encontrar más detalles sobre la [cuadrícula adaptable aquí](https://adobe-marketing-cloud.github.io/aem-responsivegrid/).

5. Vuelva al IDE y en el módulo `ui.apps` hay una biblioteca del lado del cliente definida en `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/clientlibs/clientlib-grid`. Abra el archivo `less/grid.less`.

   Este archivo determina los puntos de interrupción (`default`, `tablet` y `phone`) que utiliza el **contenedor de diseño**. Este archivo está diseñado para personalizarse según las especificaciones del proyecto. Actualmente, los puntos de interrupción están establecidos en `1200px` y `650px`.

6. Debe poder utilizar las capacidades adaptables y las políticas de texto enriquecido actualizadas del componente `Text` para crear una vista como la siguiente:

   ![Creación final de un ejemplo de capítulo](assets/map-components/final-page.png)

## Felicitaciones! {#congratulations}

Felicidades, ha aprendido a asignar componentes de SPA a componentes de AEM y ha implementado un nuevo componente `Image`. También tiene la oportunidad de explorar las capacidades de respuesta del **Contenedor de diseño**.

Siempre puede ver el código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/map-components-solution) o extraer el código localmente cambiando a la rama `Angular/map-components-solution`.

### Pasos siguientes {#next-steps}

[Navegación y enrutamiento](navigation-routing.md) : Descubra cómo se pueden admitir varias vistas en la SPA asignando páginas de AEM con el SDK del Editor de SPA. La navegación dinámica se implementa mediante Enrutador angular y se agrega a un componente de Encabezado existente.

## Bonus: Persista las configuraciones para el control de código fuente {#bonus}

En muchos casos, especialmente al principio de un proyecto de AEM, es importante mantener las configuraciones, como las plantillas y las políticas de contenido relacionadas, para el control de código fuente. Esto garantiza que todos los desarrolladores trabajen con el mismo conjunto de contenido y configuraciones, y puede garantizar una coherencia adicional entre entornos. Una vez que un proyecto alcanza un cierto nivel de madurez, la práctica de administrar plantillas se puede transferir a un grupo especial de usuarios avanzados.

Los siguientes pasos se llevarán a cabo utilizando Visual Studio Code IDE y [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync), pero podría estar utilizando cualquier herramienta y IDE que haya configurado para **extraer** o **importar** contenido de una instancia local de AEM.

1. En el IDE de código de Visual Studio, asegúrese de tener **VSCode AEM Sync** instalado mediante la extensión Marketplace:

   ![VSCode AEM Sync](./assets/map-components/vscode-aem-sync.png)

2. Expanda el módulo **ui.content** en el explorador de proyectos y vaya a `/conf/wknd-spa-angular/settings/wcm/templates`.

3. **Haga clic con el botón derecho** en la  `templates` carpeta y seleccione  **Importar desde AEM Server**:

   ![Plantilla de importación de VSCode](assets/map-components/import-aem-servervscode.png)

4. Repita los pasos para importar contenido pero seleccione la carpeta **policy** ubicada en `/conf/wknd-spa-angular/settings/wcm/policies`.

5. Inspeccione el archivo `filter.xml` ubicado en `ui.content/src/main/content/META-INF/vault/filter.xml`.

   ```xml
   <!--ui.content filter.xml-->
   <?xml version="1.0" encoding="UTF-8"?>
    <workspaceFilter version="1.0">
        <filter root="/conf/wknd-spa-angular" mode="merge"/>
        <filter root="/content/wknd-spa-angular" mode="merge"/>
        <filter root="/content/dam/wknd-spa-angular" mode="merge"/>
        <filter root="/content/experience-fragments/wknd-spa-angular" mode="merge"/>
    </workspaceFilter>
   ```

   El archivo `filter.xml` es responsable de identificar las rutas de los nodos que se instalarán con el paquete. Observe el `mode="merge"` en cada uno de los filtros que indica que el contenido existente no se modificará, solo se agregará contenido nuevo. Dado que los autores de contenido pueden estar actualizando estas rutas, es importante que una implementación de código **no** sobrescriba el contenido. Consulte la [Documentación de FileVault](https://jackrabbit.apache.org/filevault/filter.html) para obtener más información sobre cómo trabajar con elementos de filtro.

   Compare `ui.content/src/main/content/META-INF/vault/filter.xml` y `ui.apps/src/main/content/META-INF/vault/filter.xml` para comprender los diferentes nodos administrados por cada módulo.
