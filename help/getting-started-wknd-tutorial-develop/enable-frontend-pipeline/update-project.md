---
title: Actualizar el proyecto de AEM de pila completa para utilizar la canalización de front-end
description: Aprenda a actualizar el proyecto de AEM de pila completa para habilitarlo para la canalización front-end, de modo que solo compile e implemente los artefactos front-end.
sub-product: sites
version: Cloud Service
type: Tutorial
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
kt: 10689
mini-toc-levels: 1
index: y
recommendations: disable
source-git-commit: 2e3615e9e9305165ca9c3c93b38ac7e9bdcc51fb
workflow-type: tm+mt
source-wordcount: '552'
ht-degree: 0%

---


# Actualizar el proyecto de AEM de pila completa para utilizar la canalización de front-end {#update-project-enable-frontend-pipeline}

En este capítulo, realizamos cambios de configuración en la variable __Proyecto WKND Sites__ para utilizar la canalización front-end para implementar JavaScript y CSS, en lugar de requerir una ejecución completa de la canalización de pila. Esto permite desvincular el ciclo de vida de desarrollo e implementación de artefactos front-end y back-end, lo que permite un proceso de desarrollo iterativo más rápido en general.

## Objetivos {#objectives}

* Actualizar el proyecto de pila completa para utilizar la canalización de front-end

## Resumen de los cambios de configuración en el proyecto de AEM de pila completa

>[!VIDEO](https://video.tv.adobe.com/v/3409419/)

## Requisitos previos {#prerequisites}

Este es un tutorial en varias partes y se da por hecho que ha revisado la variable [Módulo &quot;ui.frontend&quot;](./review-uifrontend-module.md).


## Cambios en el proyecto de AEM de pila completa

Hay tres cambios de configuración relacionados con el proyecto y un cambio de estilo para implementar en una ejecución de prueba, por lo que en total cuatro cambios específicos en el proyecto WKND para habilitarlo para el contrato de canalización del front-end.

1. Elimine el `ui.frontend` módulo del ciclo de compilación de pila completa

   * En, la raíz del proyecto WKND Sites `pom.xml` comente el `<module>ui.frontend</module>` entrada de submódulo.

   ```xml
       ...
       <modules>
       <module>all</module>
       <module>core</module>
       <!--
       <module>ui.frontend</module>
       -->                
       <module>ui.apps</module>
       ...
   ```

   * Y comentar la dependencia relacionada con el `ui.apps/pom.xml`

   ```xml
       ...
       <!-- ====================================================================== -->
       <!-- D E P E N D E N C I E S                                                -->
       <!-- ====================================================================== -->
           ...
       <!--
           <dependency>
               <groupId>com.adobe.aem.guides</groupId>
               <artifactId>aem-guides-wknd.ui.frontend</artifactId>
               <version>${project.version}</version>
               <type>zip</type>
           </dependency>
       -->    
       ...
   ```

1. Prepare el `ui.frontend` para el contrato de canalización front-end añadiendo dos nuevos archivos de configuración de webpack.

   * Copie el `webpack.common.js` como `webpack.theme.common.js`, cambiar `output` propiedad y `MiniCssExtractPlugin`, `CopyWebpackPlugin` parámetros de configuración del complemento como se muestra a continuación:

   ```javascript
   ...
   output: {
           filename: 'theme/js/[name].js', 
           path: path.resolve(__dirname, 'dist')
       }
   ...
   
   ...
       new MiniCssExtractPlugin({
               filename: 'clientlib-[name]/[name].css'
           }),
       new CopyWebpackPlugin({
           patterns: [
               { from: path.resolve(__dirname, SOURCE_ROOT + '/resources'), to: './clientlib-site' }
           ]
       })
   ...
   ```

   * Copie el `webpack.prod.js` como `webpack.theme.prod.js`y cambie el `common` ubicación de la variable en el archivo anterior como

   ```javascript
   ...
       const common = require('./webpack.theme.common.js');
   ...
   ```

   >[!NOTE]
   >
   >Los dos cambios de configuración de &quot;webpack&quot; anteriores son tener diferentes nombres de archivos de salida y carpetas, de modo que podemos diferenciar fácilmente entre los artefactos de front-end de la canalización generada por clientlib (Full-stack) y los generados por temas (front-end).
   >
   >Como supuso, los cambios anteriores también se pueden omitir para usar configuraciones de webpack existentes, pero se requieren los cambios siguientes.
   >
   >Depende de ti cómo quieres nombrarlos u organizarlos.


   * En el `package.json` , asegúrese de que la variable  `name` el valor de propiedad es el mismo que el nombre de sitio de la variable `/conf` nodo . Y debajo de `scripts` propiedad, `build` secuencia de comandos que indica cómo crear los archivos front-end desde este módulo.

   ```javascript
       {
       "name": "wknd",
       "version": "1.0.0",
       ...
   
       "scripts": {
           "build": "webpack --config ./webpack.theme.prod.js"
       }
   
       ...
       }
   ```

1. Prepare el `ui.content` para la canalización front-end añadiendo dos configuraciones de Sling.

   * Cree un nuevo archivo en `com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig` : esto incluye todos los archivos front-end que la variable `ui.frontend` el módulo genera en `dist` carpeta que utiliza el proceso de compilación de webpack.

   ```xml
   ...
       <css
       jcr:primaryType="nt:unstructured"
       element="link"
       location="header">
       <attributes
           jcr:primaryType="nt:unstructured">
           <as
               jcr:primaryType="nt:unstructured"
               name="as"
               value="style"/>
           <href
               jcr:primaryType="nt:unstructured"
               name="href"
               value="/theme/site.css"/>
   ...
   ```

   >[!TIP]
   >
   >    Consulte la [HtmlPageItemsConfig](https://github.com/adobe/aem-guides-wknd/blob/feature/frontend-pipeline/ui.content/src/main/content/jcr_root/conf/wknd/_sling_configs/com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig/.content.xml) en el __AEM proyecto WKND Sites__.


   * Segundo, `com.adobe.aem.wcm.site.manager.config.SiteConfig` con la variable `themePackageName` siendo el mismo que la variable `package.json` y `name` valor de propiedad y `siteTemplatePath` apuntando a un `/libs/wcm/core/site-templates/aem-site-template-stub-2.0.0` valor de ruta stub.

   ```xml
   ...
       <?xml version="1.0" encoding="UTF-8"?>
       <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
               jcr:primaryType="nt:unstructured"
               siteTemplatePath="/libs/wcm/core/site-templates/aem-site-template-stub-2.0.0"
               themePackageName="wknd">
       </jcr:root>
   ...
   ```

   >[!TIP]
   >
   >    Consulte la [SiteConfig](https://github.com/adobe/aem-guides-wknd/blob/feature/frontend-pipeline/ui.content/src/main/content/jcr_root/conf/wknd/_sling_configs/com.adobe.aem.wcm.site.manager.config.SiteConfig/.content.xml) en el __AEM proyecto WKND Sites__.

1. Un tema o estilos cambian para implementarse mediante canalización de front-end para una ejecución de prueba; estamos cambiando `text-color` a Adobe rojo (o puede elegir el suyo propio) actualizando la variable `ui.frontend/src/main/webpack/base/sass/_variables.scss`.

   ```css
       $black:     #a40606;
       ...
   ```

Finalmente, inserte estos cambios en el repositorio de Git de Adobe de su programa.


>[!AVAILABILITY]
>
> Estos cambios están disponibles en GitHub dentro de la variable [__canalización front-end__](https://github.com/adobe/aem-guides-wknd/tree/feature/frontend-pipeline) de __AEM proyecto WKND Sites__.


## Felicitaciones! {#congratulations}

Felicidades, ha actualizado el proyecto WKND Sites para habilitarlo para el contrato de canalización front-end.

## Pasos siguientes {#next-steps}

En el capítulo siguiente, [Implementar mediante la canalización front-end](create-frontend-pipeline.md), creará y ejecutará una canalización front-end y verificará cómo __alejado__ desde la entrega de recursos front-end basados en &quot;/etc.clientlibs&quot;.
