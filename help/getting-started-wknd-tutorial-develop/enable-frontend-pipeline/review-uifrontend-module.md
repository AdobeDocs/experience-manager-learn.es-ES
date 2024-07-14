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

AEM En, en este capítulo revisamos el desarrollo, la implementación y el envío de los artefactos front-end de un proyecto de pila completa, centrándonos en el módulo &quot;ui.frontend&quot; del __proyecto WKND Sites__.


## Objetivos {#objective}

* AEM Comprender el flujo de generación e implementación de artefactos front-end en un proyecto de pila completa de la
* AEM Revise las configuraciones [webpack](https://webpack.js.org/) del módulo `ui.frontend` del proyecto de pila completa de la
* AEM proceso de generación de la biblioteca de cliente (también conocida como clientlibs)

## AEM Flujo de implementación front-end para proyectos de creación rápida de sitios y de pila completa de los que se ha realizado un seguimiento de la aplicación

>[!IMPORTANT]
>
>En este vídeo se explica y muestra el flujo front-end de los proyectos de **Creación rápida y de pila completa** para esbozar la sutil diferencia en el modelo de generación, implementación y entrega de recursos front-end.

>[!VIDEO](https://video.tv.adobe.com/v/3409344?quality=12&learn=on)

## Requisitos previos {#prerequisites}


* AEM Clonar el [proyecto de sitios WKND de](https://github.com/adobe/aem-guides-wknd)
* AEM Se creó e implementó el proyecto clonado de WKND Sites de la en AEM as a Cloud Service.

AEM Consulte el proyecto de sitio de WKND de la [LÉAME.md](https://github.com/adobe/aem-guides-wknd/blob/main/README.md) para obtener más información.

## AEM Flujo de artefactos front-end del proyecto de pila completa {#flow-of-frontend-artifacts}

AEM A continuación se muestra una representación de alto nivel del flujo de __desarrollo, implementación y entrega__ de los artefactos front-end en un proyecto de pila completa de la.

![Desarrollo, implementación y entrega de artefactos front-end](assets/Dev-Deploy-Delivery-AEM-Project.png)


Durante la fase de desarrollo, se realizan cambios en el front-end como el estilo y el cambio de marca mediante la actualización de los archivos CSS, JS de la carpeta `ui.frontend/src/main/webpack`. AEM A continuación, durante el tiempo de compilación, el complemento module-bundler y maven de [webpack](https://webpack.js.org/) convierte estos archivos en clientlibs optimizados en el módulo `ui.apps` de .

Los cambios del front-end se implementan en el entorno de AEM as a Cloud Service al ejecutar la canalización [__Full-stack__ en Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html).

AEM Los recursos front-end se entregan a los exploradores web a través de rutas de URI que comienzan por `/etc.clientlibs/` y, por lo general, se almacenan en caché en Dispatcher y CDN de la.


>[!NOTE]
>
> AEM Del mismo modo, en el __Recorrido de creación rápida de sitios__, los [cambios del front-end](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/customize-theme.html) se implementan en el entorno de AEM as a Cloud Service ejecutando la canalización __Front-End__, consulte [Configurar la canalización](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/pipeline-setup.html)

### Revisar las configuraciones de Webpack en el proyecto de WKND Sites {#development-frontend-webpack-clientlib}

* Hay tres archivos de configuración __webpack__ utilizados para agrupar los recursos front-end de los sitios WKND.

   1. `webpack.common`: contiene la configuración __común__ para indicar al paquete de recursos WKND y la optimización. La propiedad __output__ indica a dónde emitir los archivos consolidados (también conocidos como paquetes JavaScript AEM, pero que no se deben confundir con paquetes OSGi) que crea. El nombre predeterminado es `clientlib-site/js/[name].bundle.js`.

  ```javascript
      ...
      output: {
              filename: 'clientlib-site/js/[name].bundle.js',
              path: path.resolve(__dirname, 'dist')
          }
      ...    
  ```

   1. `webpack.dev.js` contiene la configuración de __desarrollo__ para el webpack-dev-serve y señala a la plantilla de HTML que se va a usar. AEM También contiene una configuración de proxy para una instancia de que se ejecuta en `localhost:4502`.

  ```javascript
      ...
      devServer: {
          proxy: [{
              context: ['/content', '/etc.clientlibs', '/libs'],
              target: 'http://localhost:4502',
          }],
      ...    
  ```

   1. `webpack.prod.js` contiene la configuración de __production__ y usa los complementos para transformar los archivos de desarrollo en paquetes optimizados.

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


* Los recursos agrupados se mueven al módulo `ui.apps` mediante el complemento [aem-clientlib-generator](https://www.npmjs.com/package/aem-clientlib-generator), utilizando la configuración administrada en el archivo `clientlib.config.js`.

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

* AEM __frontend-maven-plugin__ de `ui.frontend/pom.xml` organiza el agrupamiento de Webpack y la generación de clientlib durante la compilación de proyectos de la red de distribución de contenido (clientlib) durante la creación de la misma.

`$ mvn clean install -PautoInstallSinglePackage`

### Implementación en AEM as a Cloud Service {#deployment-frontend-aemaacs}

La canalización [__Full-stack__](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?#full-stack-pipeline) implementa estos cambios en un entorno de AEM as a Cloud Service.


### Envío desde AEM as a Cloud Service {#delivery-frontend-aemaacs}

AEM Los recursos front-end implementados a través de la canalización de pila completa se entregan desde el sitio a los navegadores web como `/etc.clientlibs` archivos. Puede comprobarlo si visita el [sitio WKND alojado públicamente](https://wknd.site/content/wknd/us/en.html) y ve el origen de la página web.

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

AEM En el capítulo siguiente, [Actualizar proyecto para utilizar canalización front-end](update-project.md), actualizará el proyecto de sitios WKND de la para habilitarlo para el contrato de canalización front-end.
