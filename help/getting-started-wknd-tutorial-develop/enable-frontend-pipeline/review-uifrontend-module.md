---
title: Revisión del módulo ui.frontend del proyecto de pila completa
description: Revise el desarrollo front-end, la implementación y el ciclo de vida de envío de un proyecto de AEM Sites de pila completa basado en Maven.
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
exl-id: 65e8d41e-002a-4d80-a050-5366e9ebbdea
duration: 364
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '560'
ht-degree: 100%

---

# Revisión del módulo “ui.frontend” del proyecto de AEM de pila completa {#aem-full-stack-ui-frontent}

En este capítulo vamos a revisar el desarrollo, la implementación y el envío de los artefactos front-end de un proyecto de AEM de pila completa, centrándonos en el módulo “ui.frontend” del __proyecto WKND Sites__.


## Objetivos {#objective}

* Comprender el flujo de generación e implementación de artefactos front-end en un proyecto de pila completa de AEM
* Revisar las configuraciones [webpack](https://webpack.js.org/) del módulo `ui.frontend` del proyecto de pila completa de AEM.
* Proceso de generación de la biblioteca de cliente de AEM (también conocida como clientlibs)

## Flujo de implementación front-end para proyectos de pila completa y creación rápida de sitios de AEM

>[!IMPORTANT]
>
>En este vídeo se explica y muestra el flujo front-end de los proyectos de **Creación rápida y de pila completa** para esbozar la sutil diferencia en el modelo de generación, implementación y entrega de recursos front-end.

>[!VIDEO](https://video.tv.adobe.com/v/3409344?quality=12&learn=on)

## Requisitos previos {#prerequisites}


* Clonar el [proyecto WKND Sites de AEM](https://github.com/adobe/aem-guides-wknd)
* Haber creado e implementado el proyecto WKND Sites de AEM clonado en AEM as a Cloud Service.

Consulte el proyecto del sitio WKND de AEM [README.md](https://github.com/adobe/aem-guides-wknd/blob/main/README.md) para obtener más información.

## Flujo de artefactos front-end del proyecto de pila completa de AEM {#flow-of-frontend-artifacts}

A continuación se muestra una representación de alto nivel del flujo de __desarrollo, implementación y envío__ de los artefactos front-end en un proyecto de AEM de pila completa.

![Desarrollo, implementación y envío de artefactos front-end](assets/Dev-Deploy-Delivery-AEM-Project.png)


Durante la fase de desarrollo, se realizan cambios en el front-end como el estilo y el cambio de marca mediante la actualización de los archivos CSS, JS de la carpeta `ui.frontend/src/main/webpack`. A continuación, durante el tiempo de compilación, el empaquetador de módulos [webpack](https://webpack.js.org/) y el complemento maven convierten estos archivos en clientlibs de AEM optimizados bajo el módulo `ui.apps`.

Los cambios de front-end se implementan en el entorno de AEM as a Cloud Service al ejecutar la canalización de [__pila completa__ en Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?lang=es).

Los recursos front-end se envían a los exploradores web a través de rutas URI que comienzan por `/etc.clientlibs/` y, por lo general, se almacenan en la caché en AEM Dispatcher y CDN.


>[!NOTE]
>
> Del mismo modo, en el __Recorrido de creación rápida de sitios de AEM__, los [cambios de front-end](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/customize-theme.html?lang=es) se implementan en el entorno de AEM as a Cloud Service ejecutando la canalización __Front-End__; consulte [Configurar la canalización](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/pipeline-setup.html?lang=es)

### Revisión de las configuraciones de webpack en el proyecto WKND Sites {#development-frontend-webpack-clientlib}

* Hay tres archivos de configuración __webpack__ que se utilizan para agrupar los recursos front-end de los sitios WKND.

   1. `webpack.common`: contiene la configuración __común__ para indicar al paquete de recursos WKND y la optimización. La propiedad __output__ indica dónde emitir los archivos consolidados (también conocidos como paquetes JavaScript, pero que no se deben confundir con los paquetes OSGi de AEM) que crea. El nombre del valor predeterminado se establece en `clientlib-site/js/[name].bundle.js`.

  ```javascript
      ...
      output: {
              filename: 'clientlib-site/js/[name].bundle.js',
              path: path.resolve(__dirname, 'dist')
          }
      ...    
  ```

   1. `webpack.dev.js` contiene la configuración de __desarrollo__ para el webpack-dev-serve y señala a la plantilla html que se va a utilizar. También contiene una configuración proxy para una instancia de AEM que se ejecuta en `localhost:4502`.

  ```javascript
      ...
      devServer: {
          proxy: [{
              context: ['/content', '/etc.clientlibs', '/libs'],
              target: 'http://localhost:4502',
          }],
      ...    
  ```

   1. `webpack.prod.js` contiene la configuración de __producción__ y usa los complementos para transformar los archivos de desarrollo en paquetes optimizados.

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

* El __frontend-maven-plugin__ de `ui.frontend/pom.xml` coordina el agrupamiento de webpack y la generación de clientlib durante la compilación del proyecto de AEM.

`$ mvn clean install -PautoInstallSinglePackage`

### Implementación en AEM as a Cloud Service {#deployment-frontend-aemaacs}

La canalización de [__pila completa__](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?lang=es#full-stack-pipeline) implementa estos cambios en un entorno de AEM as a Cloud Service.


### Envío desde AEM as a Cloud Service {#delivery-frontend-aemaacs}

Los recursos front-end implementados a través de la canalización de pila completa se envían desde el sitio de AEM a los exploradores web como archivos `/etc.clientlibs`. Puede comprobarlo si visita el [sitio WKND alojado públicamente](https://wknd.site/content/wknd/us/en.html) y ve el origen de la página web.

```html
    ....
    <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-181cd4102f7f49aa30eea548a7715c31-lc.min.css" type="text/css">

    ...

    <script async src="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-d4e7c03fe5c6a405a23b3ca1cc3dcd3d-lc.min.js"></script>
    ....
```

## Enhorabuena. {#congratulations}

Enhorabuena. Ha revisado el módulo ui.frontend del proyecto de pila completa

## Próximos pasos {#next-steps}

En el próximo capítulo, [Actualización del proyecto para utilizar la canalización front-end](update-project.md), actualizará el proyecto WKND Sites de AEM para habilitarlo para el contrato de canalización front-end.
