---
title: Revise el módulo ui.frontend del proyecto de pila completa
description: Revise el desarrollo front-end, la implementación y el ciclo de vida de entrega de un proyecto de AEM Sites full-stack basado en Maven.
version: Cloud Service
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
jira: KT-10689
mini-toc-levels: 1
index: y
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: 65e8d41e-002a-4d80-a050-5366e9ebbdea
duration: 364
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '560'
ht-degree: 0%

---

# AEM Revise el módulo &#39;ui.frontend&#39; del proyecto de la pila completa de la {#aem-full-stack-ui-frontent}

AEM En, en este capítulo revisamos el desarrollo, la implementación y el envío de los artefactos front-end de un proyecto de pila completa, centrándonos en el módulo &quot;ui.frontend&quot; del __Proyecto de WKND Sites__.


## Objetivos {#objective}

* AEM Comprender el flujo de generación e implementación de artefactos front-end en un proyecto de pila completa de la
* AEM Revise el proyecto de pila completa de la `ui.frontend` del módulo [webpack](https://webpack.js.org/) configuraciones
* AEM proceso de generación de la biblioteca de cliente (también conocida como clientlibs)

## AEM Flujo de implementación front-end para proyectos de creación rápida de sitios y de pila completa de los que se ha realizado un seguimiento de la aplicación

>[!IMPORTANT]
>
>En este vídeo se explica y muestra el flujo front-end para ambos **Creación rápida y completa de sitios** proyectos para delinear la sutil diferencia en la generación, implementación y modelo de entrega de recursos front-end.

>[!VIDEO](https://video.tv.adobe.com/v/3409344?quality=12&learn=on)

## Requisitos previos {#prerequisites}


* Clonar el [AEM Proyecto de sitios WKND de WKND](https://github.com/adobe/aem-guides-wknd)
* AEM AEM Se creó e implementó el proyecto clonado de WKND Sites de WKND para as a Cloud Service.

AEM Consulte el proyecto del sitio de WKND de la [README.md](https://github.com/adobe/aem-guides-wknd/blob/main/README.md) para obtener más información.

## AEM Flujo de artefactos front-end del proyecto de pila completa {#flow-of-frontend-artifacts}

A continuación se presenta una representación de alto nivel del __desarrollo, implementación y entrega__ AEM flujo de los artefactos front-end en un proyecto de pila completa de la.

![Desarrollo, implementación y entrega de artefactos front-end](assets/Dev-Deploy-Delivery-AEM-Project.png)


Durante la fase de desarrollo, los cambios del front-end, como el estilo y el cambio de marca, se realizan actualizando los archivos CSS y JS de `ui.frontend/src/main/webpack` carpeta. A continuación, durante la compilación, la variable [webpack](https://webpack.js.org/) AEM module-bundler y el complemento de maven convierten estos archivos en clientlibs optimizados de la lista de distribución de la aplicación en la que se encuentran `ui.apps` módulo.

AEM Los cambios del front-end se implementan en el entorno as a Cloud Service cuando se ejecuta el [__De pila completa__ canalización en Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html).

Los recursos front-end se entregan a los navegadores web a través de rutas URI que comienzan por `/etc.clientlibs/`AEM , y suelen almacenarse en la caché de Dispatcher y CDN de la red de distribución de datos (CDN) de la.


>[!NOTE]
>
> Del mismo modo, en la variable __AEM Recorrido de creación rápida de sitios de__, el [cambios del front-end](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/customize-theme.html) AEM se implementan en el entorno as a Cloud Service de ejecutando el __Front-End__ canalización, consulte [Configurar la canalización](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/pipeline-setup.html)

### Revisar las configuraciones de Webpack en el proyecto de WKND Sites {#development-frontend-webpack-clientlib}

* Hay tres __webpack__ Archivos de configuración utilizados para agrupar los recursos front-end de WKND Sites.

   1. `webpack.common` - Contiene el __común__ para instruir el agrupamiento de recursos y la optimización de WKND. El __salida__ AEM indica a dónde emitir los archivos consolidados (también conocidos como paquetes JavaScript, pero que no se deben confundir con paquetes OSGi de la aplicación) que crea. El nombre predeterminado se establece en `clientlib-site/js/[name].bundle.js`.

  ```javascript
      ...
      output: {
              filename: 'clientlib-site/js/[name].bundle.js',
              path: path.resolve(__dirname, 'dist')
          }
      ...    
  ```

   1. `webpack.dev.js` contiene el __desarrollo__ para el webpack-dev-serve y señala a la plantilla de HTML que se va a utilizar. AEM También contiene una configuración proxy para una instancia de que se ejecuta en `localhost:4502`.

  ```javascript
      ...
      devServer: {
          proxy: [{
              context: ['/content', '/etc.clientlibs', '/libs'],
              target: 'http://localhost:4502',
          }],
      ...    
  ```

   1. `webpack.prod.js` contiene el __producción__ y utiliza los complementos para transformar los archivos de desarrollo en paquetes optimizados.

  ```javascript
      ...
      module.exports = merge(common, {
          mode: 'production',
          optimization: {
              minimize: true,
              minimizer: [
                  new TerserPlugin(),
                  new CssMinimizerPlugin({ ...})
          }
      ...    
  ```


* Los recursos agrupados se mueven al `ui.apps` módulo que utiliza [aem-clientlib-generator](https://www.npmjs.com/package/aem-clientlib-generator) , utilizando la configuración gestionada en el `clientlib.config.js` archivo.

```javascript
    ...
    const BUILD_DIR = path.join(__dirname, 'dist');
    const CLIENTLIB_DIR = path.join(
    __dirname,
    '..',
    'ui.apps',
    'src',
    'main',
    'content',
    'jcr_root',
    'apps',
    'wknd',
    'clientlibs'
    );
    ...
```

* El __frontend-maven-plugin__ de `ui.frontend/pom.xml` AEM organiza el agrupamiento de Webpack y la generación clientlib durante la generación de proyectos de.

`$ mvn clean install -PautoInstallSinglePackage`

### AEM Implementación en as a Cloud Service {#deployment-frontend-aemaacs}

El [__De pila completa__ canalización](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?#full-stack-pipeline) AEM implementa estos cambios en un entorno as a Cloud Service de.


### AEM Envío desde el as a Cloud Service {#delivery-frontend-aemaacs}

AEM Los recursos front-end implementados a través de la canalización de pila completa se entregan desde el sitio a los navegadores web de la siguiente manera: `/etc.clientlibs` archivos. Puede verificarlo en la página de [sitio WKND alojado públicamente](https://wknd.site/content/wknd/us/en.html) y ver el origen de la página web.

```html
    ....
    <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-181cd4102f7f49aa30eea548a7715c31-lc.min.css" type="text/css">

    ...

    <script async src="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-d4e7c03fe5c6a405a23b3ca1cc3dcd3d-lc.min.js"></script>
    ....
```

## Enhorabuena. {#congratulations}

Felicidades, ha revisado el módulo ui.frontend del proyecto de pila completa

## Pasos siguientes {#next-steps}

En el capítulo siguiente, [Actualizar proyecto para utilizar canalización front-end](update-project.md)AEM , actualizará el proyecto de WKND Sites de la para habilitarlo para el contrato de canalización front-end.
