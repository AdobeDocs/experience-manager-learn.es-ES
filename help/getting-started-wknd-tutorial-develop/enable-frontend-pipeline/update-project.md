---
title: Actualizar el proyecto de AEM de pila completa para utilizar la canalización front-end
description: Obtenga información sobre cómo actualizar un proyecto de AEM de pila completa para habilitarlo para la canalización front-end, de modo que solo genere e implemente los artefactos front-end.
version: Experience Manager as a Cloud Service
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
jira: KT-10689
mini-toc-levels: 1
index: y
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: c4a961fb-e440-4f78-b40d-e8049078b3c0
duration: 307
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '595'
ht-degree: 0%

---

# Actualizar el proyecto de AEM de pila completa para utilizar la canalización front-end {#update-project-enable-frontend-pipeline}

En este capítulo, realizamos cambios de configuración en el __proyecto de WKND Sites__ para utilizar la canalización front-end para implementar JavaScript y CSS, en lugar de requerir una ejecución de canalización de pila completa. Esto desvincula el ciclo vital de desarrollo e implementación de los artefactos front-end y back-end, lo que permite un proceso de desarrollo más rápido e iterativo en general.

## Objetivos {#objectives}

* Actualizar el proyecto de pila completa para utilizar la canalización front-end

## Información general sobre los cambios de configuración en el proyecto de AEM de pila completa

>[!VIDEO](https://video.tv.adobe.com/v/3409419?quality=12&learn=on)

## Requisitos previos {#prerequisites}

Este es un tutorial de varias partes y se supone que ha revisado el módulo [&#39;ui.frontend&#39;](./review-uifrontend-module.md).


## Cambios en el proyecto de AEM de pila completa

Hay tres cambios de configuración relacionados con el proyecto y un cambio de estilo que se deben implementar para una ejecución de prueba, por lo que en total hay cuatro cambios específicos en el proyecto WKND para habilitarlo para el contrato de canalización front-end.

1. Quitar el módulo `ui.frontend` del ciclo de compilación de pila completa

   * En, la raíz `pom.xml` del proyecto de WKND Sites comenta la entrada del submódulo `<module>ui.frontend</module>`.

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

   * Y dependencia relacionada con los comentarios de `ui.apps/pom.xml`

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

1. Prepare el módulo `ui.frontend` para el contrato de canalización front-end agregando dos nuevos archivos de configuración de Webpack.

   * Copie el(la) `webpack.common.js` existente(a) como `webpack.theme.common.js`, y cambie la propiedad `output` y los parámetros de configuración del complemento `MiniCssExtractPlugin`, `CopyWebpackPlugin` de la siguiente manera:

   ```javascript
   ...
   output: {
           filename: 'theme/js/[name].js', 
           path: path.resolve(__dirname, 'dist')
       }
   ...
   
   ...
       new MiniCssExtractPlugin({
               filename: 'theme/[name].css'
           }),
       new CopyWebpackPlugin({
           patterns: [
               { from: path.resolve(__dirname, SOURCE_ROOT + '/resources'), to: './clientlib-site' }
           ]
       })
   ...
   ```

   * Copie la `webpack.prod.js` existente como `webpack.theme.prod.js` y cambie la ubicación de la variable `common` al archivo anterior como

   ```javascript
   ...
       const common = require('./webpack.theme.common.js');
   ...
   ```

   >[!NOTE]
   >
   >Los dos cambios de configuración anteriores del &quot;webpack&quot; deben tener nombres de archivo y carpeta de salida diferentes, para que podamos diferenciar fácilmente entre los artefactos clientlib (full-stack) y los generados por el tema (front-end) del front-end de la canalización.
   >
   >Como ha adivinado, los cambios anteriores se pueden omitir para utilizar también las configuraciones de Webpack existentes, pero los cambios siguientes son obligatorios.
   >
   >Depende de usted cómo desee ponerles nombre u organizarlos.


   * En el archivo `package.json`, asegúrese de que el valor de la propiedad `name` sea el mismo que el nombre de sitio del nodo `/conf`. Y en la propiedad `scripts`, un script `build` que indica cómo generar los archivos front-end a partir de este módulo.

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

1. Prepare el módulo `ui.content` para la canalización front-end agregando dos configuraciones de Sling.

   * Crear un archivo en `com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig`: esto incluye todos los archivos front-end que el módulo `ui.frontend` genera en la carpeta `dist` mediante el proceso de generación de Webpack.

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
   >    Ver [HtmlPageItemsConfig](https://github.com/adobe/aem-guides-wknd/blob/feature/frontend-pipeline/ui.content/src/main/content/jcr_root/conf/wknd/_sling_configs/com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig/.content.xml) completo en el __proyecto WKND Sites de AEM__.


   * Segundo, `com.adobe.aem.wcm.site.manager.config.SiteConfig` con el valor `themePackageName` que es el mismo que el valor de propiedad `package.json` y `name`, y `siteTemplatePath` que señala a un valor de ruta de acceso auxiliar `/libs/wcm/core/site-templates/aem-site-template-stub-2.0.0`.

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
   >    Vea, el [SiteConfig](https://github.com/adobe/aem-guides-wknd/blob/feature/frontend-pipeline/ui.content/src/main/content/jcr_root/conf/wknd/_sling_configs/com.adobe.aem.wcm.site.manager.config.SiteConfig/.content.xml) completo en el __proyecto WKND Sites de AEM__.

1. Se ha cambiado un tema o estilos para implementarlos a través de la canalización front-end en una ejecución de prueba. Estamos cambiando `text-color` a Adobe red (o puede elegir el suyo propio) al actualizar `ui.frontend/src/main/webpack/base/sass/_variables.scss`.

   ```css
       $black:     #a40606;
       ...
   ```

Finalmente, inserte estos cambios en el repositorio de Git de Adobe de su programa.


>[!AVAILABILITY]
>
> Estos cambios están disponibles en GitHub dentro de la [__canalización front-end__](https://github.com/adobe/aem-guides-wknd/tree/feature/frontend-pipeline) rama del __proyecto WKND Sites de AEM__.


## Precaución: _Botón Habilitar canalización front-end_

La opción [Sitio](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/getting-started/basic-handling.html?lang=es) del selector de carril [1&rbrace; muestra el botón **Habilitar canalización front-end** al seleccionar la raíz del sitio o la página del sitio. ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/getting-started/basic-handling.html?lang=es) Al hacer clic en el botón **Habilitar canalización front-end**, se anularán las **configuraciones de Sling** anteriores. Asegúrese de **no hacer clic** en este botón después de implementar los cambios anteriores a través de la ejecución de la canalización de Cloud Manager.

![Botón Habilitar canalización front-end](assets/enable-front-end-Pipeline-button.png)

Si se hace clic en él por error, debe volver a ejecutar las canalizaciones para asegurarse de que se restauran el contrato y los cambios de la canalización front-end.

## Enhorabuena. {#congratulations}

¡Enhorabuena! Ha actualizado el proyecto de WKND Sites para habilitarlo para el contrato de canalización front-end.

## Pasos siguientes {#next-steps}

En el siguiente capítulo, [Implementar mediante la canalización front-end](create-frontend-pipeline.md), creará y ejecutará una canalización front-end y verificará cómo __nos hemos alejado__ de la entrega de recursos front-end basada en &#39;/etc.clientlibs&#39;.
