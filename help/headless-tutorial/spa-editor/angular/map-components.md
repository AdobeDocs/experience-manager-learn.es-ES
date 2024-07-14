---
title: SPA AEM Asignación de componentes de a componentes de | AEM SPA Introducción al Editor y al Angular de la
description: Obtenga información sobre cómo asignar componentes de Angular a componentes de Adobe Experience Manager AEM AEM SPA () con el SDK de JS de Editor de. SPA AEM SPA AEM La asignación de componentes permite a los usuarios realizar actualizaciones dinámicas en los componentes de la de componentes del Editor de componentes, de forma similar a la creación tradicional de los componentes de la.
feature: SPA Editor
version: Cloud Service
jira: KT-5311
thumbnail: 5311-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 19a8917c-a1e7-4293-9ce1-9f4c1a565861
duration: 509
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '2213'
ht-degree: 0%

---

# SPA AEM Asignación de componentes de a componentes de {#map-components}

Obtenga información sobre cómo asignar componentes de Angular a componentes de Adobe Experience Manager AEM AEM SPA () con el SDK de JS de Editor de. SPA AEM SPA AEM La asignación de componentes permite a los usuarios realizar actualizaciones dinámicas en los componentes de la de componentes del Editor de componentes, de forma similar a la creación tradicional de los componentes de la.

AEM AEM En este capítulo se profundiza en la API del modelo JSON de y en cómo el contenido JSON expuesto por un componente de la se puede insertar automáticamente en un componente de Angular como props.

## Objetivo

1. AEM SPA Obtenga información sobre cómo asignar componentes de la a componentes de la aplicación.
2. Comprenda la diferencia entre los componentes **Contenedor** y los componentes **Contenido**.
3. Cree un nuevo componente de Angular AEM que se asigne a un componente de existente.

## Qué va a generar

SPA AEM En este capítulo se analizará cómo se asigna el componente de `Text` proporcionado al componente `Text`de la aplicación de la aplicación de la aplicación de la aplicación de la aplicación de la aplicación de la aplicación de la aplicación. SPA SPA AEM Se crea un nuevo componente de la `Image` que se puede usar en el y crear en el mismo. Las características predeterminadas de las directivas **Contenedor de diseño** y **Editor de plantillas** también se usarán para crear una vista que tenga un aspecto un poco más variado.

![Creación final de ejemplo de capítulo](./assets/map-components/final-page.png)

## Requisitos previos

Revise las herramientas y las instrucciones necesarias para configurar un [entorno de desarrollo local](overview.md#local-dev-environment).

### Obtener el código

1. Descargue el punto de partida para este tutorial mediante Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/map-components-start
   ```

2. AEM Implemente el código base en una instancia de local mediante Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   AEM Si usa [6.x](overview.md#compatibility), agregue el perfil `classic`:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Siempre puede ver el código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/map-components-solution) o desprotegerlo localmente cambiando a la rama `Angular/map-components-solution`.

## Método de asignación

SPA AEM El concepto básico es asignar un componente de a un componente de. AEM Los componentes de, que ejecutan del lado del servidor, exportan contenido como parte de la API del modelo JSON. SPA El contenido JSON lo consume el usuario, que se ejecuta en el lado del cliente en el explorador. SPA AEM Se crea una asignación 1:1 entre los componentes de la y un componente de la.

AEM ![Información general de alto nivel sobre la asignación de un componente de a un componente de Angular](./assets/map-components/high-level-approach.png)

AEM *Información general de alto nivel sobre la asignación de un componente de a un componente de Angular*

## Inspect el componente Texto

AEM AEM El [Arquetipo de proyecto de](https://github.com/adobe/aem-project-archetype) proporciona un componente `Text` que está asignado al componente de texto [. ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html) AEM Este es un ejemplo de un componente **content**, en el sentido de que procesa *content* de la.

Veamos cómo funciona el componente.

### Inspect el modelo JSON

1. SPA AEM Antes de saltar al código de la, es importante comprender el modelo JSON que proporciona el usuario de la interfaz de usuario de JSON de la que se proporciona la aplicación. Vaya a la [Biblioteca de componentes principales](https://www.aemcomponents.dev/content/core-components-examples/library/core-content/text.html) y vea la página del componente Texto. AEM La biblioteca de componentes principales proporciona ejemplos de todos los componentes principales de la.
2. Seleccione la ficha **JSON** para uno de los ejemplos:

   ![Modelo JSON de texto](./assets/map-components/text-json.png)

   Debería ver tres propiedades: `text`, `richText` y `:type`.

   AEM `:type` es una propiedad reservada que enumera `sling:resourceType` (o ruta de acceso) del componente de la. AEM SPA El valor de `:type` es lo que se usa para asignar el componente de la al componente de la.

   SPA `text` y `richText` son propiedades adicionales que se exponen al componente de la.

### Inspect el componente Texto

1. Abra un nuevo terminal y vaya a la carpeta `ui.frontend` dentro del proyecto. Ejecute `npm install` y luego `npm start` para iniciar el **servidor de desarrollo de Webpack**:

   ```shell
   $ cd ui.frontend
   $ npm run start:mock
   ```

   El módulo `ui.frontend` está configurado actualmente para usar el [modelo JSON ficticio](./integrate-spa.md#mock-json).

2. Debería ver una nueva ventana del explorador abierta a [http://localhost:4200/content/wknd-spa-angular/us/en/home.html](http://localhost:4200/content/wknd-spa-angular/us/en/home.html)

   ![Servidor de desarrollo de Webpack con contenido ficticio](assets/map-components/initial-start.png)

3. AEM SPA En el IDE de su elección, abra el Proyecto de para el WKND. Expanda el módulo `ui.frontend` y abra el archivo **text.component.ts** en `ui.frontend/src/app/components/text/text.component.ts`:

   ![Código Source del componente de Angular Text.js](assets/map-components/vscode-ide-text-js.png)

4. El primer área a inspeccionar es el `class TextComponent` en ~line 35:

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

   El decorador [@Input()](https://angular.io/api/core/Input) se usa para declarar campos cuyos valores se establecen a través del objeto JSON asignado, revisado anteriormente.

   `@HostBinding('innerHtml') get content()` es un método que expone el contenido de texto creado del valor de `this.text`. En caso de que el contenido sea texto enriquecido (determinado por el indicador `this.richText`), se omite la seguridad integrada del Angular. El [DomSanitizer](https://angular.io/api/platform-browser/DomSanitizer) de Angular se usa para &quot;eliminar&quot; el HTML sin procesar y evitar vulnerabilidades de ejecución de scripts en sitios múltiples. El método está enlazado a la propiedad `innerHtml` mediante el decorador [@HostBinding](https://angular.io/api/core/HostBinding).

5. A continuación, inspeccione `TextEditConfig` en ~line 24:

   ```js
   const TextEditConfig = {
       emptyLabel: 'Text',
       isEmpty: cqModel =>
           !cqModel || !cqModel.text || cqModel.text.trim().length < 1
   };
   ```

   AEM El código anterior es responsable de determinar cuándo procesar el marcador de posición en el entorno de autor de la. Si el método `isEmpty` devuelve **true**, se procesará el marcador de posición.

6. Finalmente, eche un vistazo a la llamada de `MapTo` en la línea 53:

   ```js
   MapTo('wknd-spa-angular/components/text')(TextComponent, TextEditConfig );
   ```

   AEM SPA **MapTo** lo proporciona el SDK de JS del Editor de la aplicación de código de tiempo de ejecución de la aplicación de la aplicación de código abierto de la aplicación de (`@adobe/cq-angular-editable-components`). AEM La ruta de acceso `wknd-spa-angular/components/text` representa el `sling:resourceType` del componente de la. Esta ruta de acceso coincide con el `:type` expuesto por el modelo JSON observado anteriormente. SPA **MapTo** analiza la respuesta del modelo JSON y pasa los valores correctos a las variables `@Input()` del componente de.

   AEM Puede encontrar la definición del componente `Text` de la en `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/text`.

7. Experimente modificando el archivo **en.model.json** en `ui.frontend/src/mocks/json/en.model.json`.

   En la línea 62, actualice el primer valor `Text` para que utilice las etiquetas **`H1`** y **`u`**:

   ```json
       "text": {
           "text": "<h1><u>Hello World!</u></h1>",
           "richText": true,
           ":type": "wknd-spa-angular/components/text"
       }
   ```

   Vuelva al explorador para ver los efectos que produce el **servidor de desarrollo de Webpack**:

   ![Modelo de texto actualizado](assets/map-components/updated-text-model.png)

   Intente alternar la propiedad `richText` entre **true** / **false** para ver la lógica de procesamiento en acción.

8. Inspect **text.component.html** a las `ui.frontend/src/app/components/text/text.component.html`.

   Este archivo está vacío porque la propiedad `innerHTML` establece todo el contenido del componente.

9. Inspect **app.module.ts** en `ui.frontend/src/app/app.module.ts`.

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

   AEM SPA **TextComponent** no se incluye explícitamente, sino de forma dinámica a través de **AEMResponsiveGridComponent** proporcionado por el SDK de JS de Editor de de trabajo de la aplicación. Por lo tanto, debe aparecer en la matriz **app.module.ts**&#39; [entryComponents](https://angular.io/guide/entry-components).

## Creación del componente de imagen

A continuación, cree un componente de Angular AEM `Image` que esté asignado al componente de imagen [Image](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html?lang=es) de la. El componente `Image` es otro ejemplo de un componente **content**.

### Inspect el JSON

SPA AEM Antes de saltar al código de la, revise el modelo JSON proporcionado por el administrador de códigos de.

1. Vaya a los [ejemplos de imagen de la biblioteca de componentes principales](https://www.aemcomponents.dev/content/core-components-examples/library/core-content/image.html).

   ![Componente principal de imagen JSON](./assets/map-components/image-json.png)

   SPA Las propiedades de `src`, `alt` y `title` se usan para rellenar el componente de `Image` de la.

   >[!NOTE]
   >
   > Hay otras propiedades de imagen expuestas (`lazyEnabled`, `widths`) que permiten a un desarrollador crear un componente de carga diferida y adaptable. El componente integrado en este tutorial es sencillo y **no** utiliza estas propiedades avanzadas.

2. Vuelva a su IDE y abra `en.model.json` a las `ui.frontend/src/mocks/json/en.model.json`. Dado que este es un nuevo componente para nuestro proyecto, necesitamos &quot;burlarnos&quot; del JSON de imagen.

   En la línea ~line 70 agregue una entrada JSON para el modelo `image` (no se olvide de la coma final `,` después del segundo `text_386303036`) y actualice la matriz `:itemsOrder`.

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

   El proyecto incluye una imagen de muestra en `/mock-content/adobestock-140634652.jpeg` que se usa con el **servidor de desarrollo de Webpack**.

   Puedes ver [en.model.json aquí](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/map-components-solution/ui.frontend/src/mocks/json/en.model.json).

3. Añada una foto estándar para que la muestre el componente.

   Cree una nueva carpeta llamada **images** debajo de `ui.frontend/src/mocks`. Descargue [adobestock-140634652.jpeg](assets/map-components/adobestock-140634652.jpeg) y colóquelo en la carpeta **imágenes** recién creada. Siéntase libre de usar su propia imagen, si lo desea.

### Implementación del componente de imagen

1. Detenga el **servidor de desarrollo de Webpack** si se ha iniciado.
2. Cree un nuevo componente de imagen ejecutando el comando `ng generate component` de CLI de Angular desde la carpeta `ui.frontend`:

   ```shell
   $ ng generate component components/image
   ```

3. En el IDE, abra **image.component.ts** en `ui.frontend/src/app/components/image/image.component.ts` y actualícelo de la siguiente manera:

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

   AEM `ImageEditConfig` es la configuración para determinar si se debe procesar el marcador de posición de autor en, en función de si se ha rellenado la propiedad `src`.

   `@Input()` de `src`, `alt` y `title` son las propiedades asignadas desde la API JSON.

   `hasImage()` es un método que determinará si se debe procesar la imagen.

   SPA AEM `MapTo` asigna el componente de la al componente de la ubicado en `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/image`.

4. Abra **image.component.html** y actualícelo de la siguiente manera:

   ```html
   <ng-container *ngIf="hasImage">
       <img class="image" [src]="src" [alt]="alt" [title]="title"/>
   </ng-container>
   ```

   Esto procesará el elemento `<img>` si `hasImage` devuelve **true**.

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
   > AEM SPA La regla `:host-context` es **crítica** para que el marcador de posición del editor de funcione correctamente. SPA AEM Todos los componentes de la que se vayan a crear en el editor de páginas de la página necesitarán esta regla como mínimo.

6. Abra `app.module.ts` y agregue `ImageComponent` a la matriz `entryComponents`:

   ```js
   entryComponents: [TextComponent, PageComponent, ImageComponent],
   ```

   Al igual que `TextComponent`, `ImageComponent` se carga dinámicamente y debe incluirse en la matriz `entryComponents`.

7. Inicie **webpack dev server** para ver el procesamiento de `ImageComponent`.

   ```shell
   $ npm run start:mock
   ```

   ![Imagen agregada al simulacro](assets/map-components/image-added-mock.png)

   SPA *Imagen agregada a la página de inicio de sesión*

   >[!NOTE]
   >
   > **Desafío adicional**: Implemente un nuevo método para mostrar el valor de `title` como pie de ilustración debajo de la imagen.

## AEM Actualizar directivas en el

El componente `ImageComponent` solo está visible en el **servidor de desarrollo de Webpack**. SPA AEM A continuación, implemente la plantilla actualizada para implementar y actualizar las directivas de la plantilla.

1. AEM Detenga el **servidor de desarrollo de Webpack** y, desde la **raíz** del proyecto, implemente los cambios para usar sus habilidades con Maven:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. AEM SPA Desde la pantalla Inicio de la, vaya a **[!UICONTROL Herramientas]** > **[!UICONTROL Plantillas]** > **[Angular de trabajo de WKND](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd-spa-angular)**.

   SPA Seleccione y edite la **Página de la**:

   SPA ![Editar plantilla de página de la página](assets/map-components/edit-spa-page-template.png)

3. Seleccione el **contenedor de diseño** y haga clic en su icono **directiva** para editar la directiva:

   ![Política del contenedor de diseño](./assets/map-components/layout-container-policy.png)

4. SPA En **Componentes permitidos** > **Angular de trabajo de WKND - Contenido** > compruebe el componente **Imagen**:

   ![Componente de imagen seleccionado](assets/map-components/check-image-component.png)

   SPA En **Componentes predeterminados** > **Agregar asignación** y elija el componente **Imagen - Angular de trabajo de WKND - Contenido**:

   ![Establecer componentes predeterminados](assets/map-components/default-components.png)

   Escriba un **tipo mime** de `image/*`.

   Haga clic en **Listo** para guardar las actualizaciones de la directiva.

5. En el **contenedor de diseño**, haga clic en el icono **directiva** para el componente **Texto**:

   ![Icono de directiva de componente de texto](./assets/map-components/edit-text-policy.png)

   SPA Cree una nueva directiva con el nombre **WKND Texto de la**. En **Complementos** > **Formato** > marque todas las casillas para habilitar opciones de formato adicionales:

   ![Habilitar formato RTE](assets/map-components/enable-formatting-rte.png)

   En **Complementos** > **Estilos de párrafo** > marque la casilla para **habilitar estilos de párrafo**:

   ![Activar estilos de párrafo](./assets/map-components/text-policy-enable-paragraphstyles.png)

   Haga clic en **Listo** para guardar la actualización de la directiva.

6. Vaya a **Página principal** [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html).

   También debería poder editar el componente `Text` y agregar estilos de párrafo adicionales en el modo **pantalla completa**.

   ![Edición de texto enriquecido en pantalla completa](assets/map-components/full-screen-rte.png)

7. También debería poder arrastrar y soltar una imagen desde **Buscador de recursos**:

   ![Arrastrar y soltar imagen](./assets/map-components/drag-drop-image.gif)

8. Agregue sus propias imágenes a través de [AEM Assets](http://localhost:4502/assets.html/content/dam) o instale la base de código final para el [sitio de referencia WKND estándar](https://github.com/adobe/aem-guides-wknd/releases/latest). SPA El [sitio de referencia WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) incluye muchas imágenes que se pueden reutilizar en el sitio de referencia de WKND. AEM El paquete se puede instalar usando el [Administrador de paquetes de](http://localhost:4502/crx/packmgr/index.jsp).

   ![Administrador de paquetes instala wknd.all](./assets/map-components/package-manager-wknd-all.png)

## Inspect el contenedor de diseño

AEM SPA El SDK del editor de diseños proporciona automáticamente compatibilidad con el **contenedor de diseños**. El **contenedor de diseño**, tal como indica el nombre, es un componente **contenedor**. Los componentes de contenedor son componentes que aceptan estructuras JSON que representan *otros* componentes y las instancian dinámicamente.

Vamos a inspeccionar más el contenedor de diseño.

1. En el IDE, abra **responsive-grid.component.ts** a las `ui.frontend/src/app/components/responsive-grid`:

   ```js
   import { AEMResponsiveGridComponent,MapTo } from '@adobe/cq-angular-editable-components';
   
   MapTo('wcm/foundation/components/responsivegrid')(AEMResponsiveGridComponent);
   ```

   AEM SPA `AEMResponsiveGridComponent` se implementó como parte del SDK del Editor de código de tiempo de la y se incluyó en el proyecto a través de `import-components`.

2. En un explorador, vaya a [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)

   ![API de modelo JSON: cuadrícula interactiva](./assets/map-components/responsive-grid-modeljson.png)

   SPA El componente **Contenedor de diseño** tiene un `sling:resourceType` de `wcm/foundation/components/responsivegrid` y el Editor de recursos lo reconoce usando la propiedad `:type`, al igual que los componentes `Text` y `Image`.

   SPA Las mismas capacidades para cambiar el tamaño de un componente mediante [Modo de diseño](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html#defining-layouts-layout-mode) están disponibles con el Editor de elementos de diseño de la página de la página de la página de la página de la página de la página de la página de la página de inicio de la página de.

3. Volver a [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html). Agregue componentes adicionales de **Image** e intente cambiar su tamaño con la opción **Diseño**:

   ![Cambiar el tamaño de la imagen mediante el modo Diseño](./assets/map-components/responsive-grid-layout-change.gif)

4. Vuelva a abrir el modelo JSON [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json) y observe `columnClassNames` como parte del JSON:

   ![Nombres de clase de columna](./assets/map-components/responsive-grid-classnames.png)

   El nombre de clase `aem-GridColumn--default--4` indica que el componente debe tener 4 columnas de ancho basado en una cuadrícula de 12 columnas. Encontrará más detalles sobre la cuadrícula [adaptable aquí](https://adobe-marketing-cloud.github.io/aem-responsivegrid/).

5. Vuelva al IDE y en el módulo `ui.apps` hay una biblioteca del lado del cliente definida en `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/clientlibs/clientlib-grid`. Abra el archivo `less/grid.less`.

   Este archivo determina los puntos de interrupción (`default`, `tablet` y `phone`) utilizados por el **contenedor de diseño**. El propósito de este archivo es personalizarlo según las especificaciones del proyecto. Actualmente los puntos de interrupción están establecidos en `1200px` y `650px`.

6. Debe poder utilizar las capacidades adaptables y las directivas de texto enriquecido actualizadas del componente `Text` para crear una vista como la siguiente:

   ![Creación final de ejemplo de capítulo](assets/map-components/final-page.png)

## Enhorabuena. {#congratulations}

SPA AEM Felicidades, ha aprendido a asignar componentes de la a componentes de la y ha implementado un nuevo componente `Image`. También tiene la oportunidad de explorar las capacidades adaptables de **Contenedor de diseño**.

Siempre puede ver el código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/map-components-solution) o desprotegerlo localmente cambiando a la rama `Angular/map-components-solution`.

### Siguientes pasos {#next-steps}

SPA AEM SPA [Navegación y enrutamiento](navigation-routing.md): descubra cómo se pueden admitir varias vistas en la asignando a páginas de la lista de distribución con el SDK del Editor de la página de la lista de distribución (SDK) del editor de la lista de distribución. La navegación dinámica se implementa mediante Angular Router y se añade a un componente de encabezado existente.

## Bonificación: mantener configuraciones para el control de código fuente {#bonus}

AEM En muchos casos, especialmente al principio de un proyecto de, es importante mantener las configuraciones, como las plantillas y las directivas de contenido relacionadas, para el control de código fuente. Esto garantiza que todos los desarrolladores trabajen con el mismo conjunto de contenido y configuraciones y puede garantizar una coherencia adicional entre entornos. Una vez que un proyecto alcanza un cierto nivel de madurez, la práctica de administrar plantillas se puede transferir a un grupo especial de usuarios avanzados.

AEM AEM Los siguientes pasos se llevarán a cabo con el IDE de código de Visual Studio y la sincronización de código de VSC [VSCode](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync), pero podrían realizarse con cualquier herramienta y cualquier IDE que haya configurado para **extraer** o **importar** contenido de una instancia local de.

1. AEM En el IDE de código de Visual Studio, asegúrese de que tiene **VSCode Sync** instalado mediante la extensión Marketplace:

   AEM ![VSCodeSincronización de](./assets/map-components/vscode-aem-sync.png)

2. Expanda el módulo **ui.content** en el explorador del proyecto y vaya a `/conf/wknd-spa-angular/settings/wcm/templates`.

3. AEM **Haga clic con el botón derecho** en la carpeta `templates` y seleccione **Importar desde el servidor**:

   ![Plantilla de importación de VSCode](assets/map-components/import-aem-servervscode.png)

4. Repita los pasos para importar contenido, pero seleccione la carpeta **policies** ubicada en `/conf/wknd-spa-angular/settings/wcm/policies`.

5. Inspect agregó el archivo `filter.xml` ubicado en `ui.content/src/main/content/META-INF/vault/filter.xml`.

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

   El archivo `filter.xml` es responsable de identificar las rutas de acceso de los nodos instalados con el paquete. Observe el `mode="merge"` en cada uno de los filtros que indica que el contenido existente no se modificará, solo se agregará contenido nuevo. Dado que los autores de contenido pueden estar actualizando estas rutas, es importante que una implementación de código sobrescriba el contenido **no**. Consulte la [documentación de FileVault](https://jackrabbit.apache.org/filevault/filter.html) para obtener más información sobre cómo trabajar con elementos de filtro.

   Compare `ui.content/src/main/content/META-INF/vault/filter.xml` y `ui.apps/src/main/content/META-INF/vault/filter.xml` para comprender los diferentes nodos administrados por cada módulo.
