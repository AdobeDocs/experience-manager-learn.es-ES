---
title: AEM SPA SPA Configuración de la para el Editor de y el Control remoto
description: AEM AEM SPA SPA Se requiere un proyecto de para configurar la compatibilidad con los requisitos de configuración y contenido a fin de permitir que el Editor de la cree un elemento remoto de la configuración de la aplicación de la.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
jira: KT-7631
thumbnail: kt-7631.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: 0bdb93c9-5070-483c-a34c-f2b348bfe5ae
duration: 297
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1230'
ht-degree: 0%

---

# AEM SPA Configuración de la para el editor de segmentos

SPA AEM AEM Aunque la base de código de la se administra fuera de la base de datos, se requiere un proyecto de la para configurar los requisitos de configuración y contenido de soporte. AEM En este capítulo se explica la creación de un proyecto de que contiene las configuraciones necesarias:

+ AEM Proxies de componentes principales de WCM
+ AEM SPA Proxy de página de remoto
+ AEM SPA Plantillas de página de remoto
+ SPA AEM Páginas de remotas de línea base
+ SPA AEM Subproyecto para definir la a las asignaciones de URL de la
+ Carpetas de configuración de OSGi

## Descargar el proyecto base desde GitHub

Descargue la `aem-guides-wknd-graphql` proyecto de Github.com. Contiene algunos archivos de línea de base utilizados en este proyecto.

```
$ mkdir -p ~/Code
$ git clone https://github.com/adobe/aem-guides-wknd-graphql.git
$ cd remote-spa-tutorial
```

## AEM Creación de un proyecto de

AEM Cree un proyecto de en el que se administren las configuraciones y el contenido de línea de base. Este proyecto se generará dentro del `aem-guides-wknd-graphql` del proyecto `remote-spa-tutorial` carpeta.

_Utilice siempre la versión más reciente de [AEM Tipo de archivo de](https://github.com/adobe/aem-project-archetype)._

```
$ cd ~/Code/aem-guides-wknd-graphql/remote-spa-tutorial
$ mvn -B archetype:generate \
 -D archetypeGroupId=com.adobe.aem \
 -D archetypeArtifactId=aem-project-archetype \
 -D archetypeVersion=39 \
 -D aemVersion=cloud \
 -D appTitle="WKND App" \
 -D appId="wknd-app" \
 -D groupId="com.adobe.aem.guides.wkndapp" \
 -D frontendModule="react"
$ mv ~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/wknd-app ~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/com.adobe.aem.guides.wknd-app
```

_AEM AEM SPA El último comando simplemente cambia el nombre de la carpeta de proyecto de la para que quede claro que es el proyecto de la y no se debe confundir con el de la carpeta de proyectos de la carpeta de proyectos de la__

While `frontendModule="react"` se especifica, la variable `ui.frontend` SPA El proyecto no se utiliza para el caso de uso de Remote. SPA AEM AEM La se desarrolla y administra de forma externa para que se pueda usar en la forma de un solo como API de contenido. El `frontendModule="react"` se requiere un indicador para que el proyecto incluya el  `spa-project` AEM SPA las dependencias de Java™ y configure las plantillas de página de remoto.

AEM AEM SPA El tipo de archivo del proyecto de genera los siguientes elementos que se utilizan para configurar la integración de los proyectos de con el.

+ __AEM Proxies de componentes principales de WCM__ en `ui.apps/src/.../apps/wknd-app/components`
+ __AEM SPA proxy de página remota__ en `ui.apps/src/.../apps/wknd-app/components/remotepage`
+ __AEM Plantillas de página de__ en `ui.content/src/.../conf/wknd-app/settings/wcm/templates`
+ __Subproyecto para definir asignaciones de contenido__ en `ui.content/src/...`
+ __SPA AEM Páginas de remotas de línea base__ en `ui.content/src/.../content/wknd-app`
+ __Carpetas de configuración de OSGi__ en `ui.config/src/.../apps/wknd-app/osgiconfig`

AEM SPA SPA Con el proyecto de base de datos generado, algunos ajustes garantizan la compatibilidad del Editor de la con el Editor de elementos de la interfaz de usuario de la interfaz de usuario remota

## Eliminar proyecto ui.frontend

SPA SPA AEM Dado que la es un proyecto remoto, asuma que se desarrolla y administra fuera del proyecto de la. Para evitar conflictos, elimine la `ui.frontend` de implementación del proyecto. Si la variable `ui.frontend` SPA SPA proyecto no se elimina, dos veces, el valor predeterminado que se proporciona en la lista de valores de la lista no se ha eliminado `ui.frontend` SPA AEM SPA proyecto y el remoto, se carga al mismo tiempo en el editor de la de trabajo.

1. AEM Abra el proyecto de la (`~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/com.adobe.aem.guides.wknd-app`) en su IDE
1. Abra la raíz `pom.xml`
1. Comente el `<module>ui.frontend</module` fuera de `<modules>` lista

   ```
   <modules>
       <module>all</module>
       <module>core</module>
   
       <!-- <module>ui.frontend</module> -->
   
       <module>ui.apps</module>
       <module>ui.apps.structure</module>
       <module>ui.config</module>
       <module>ui.content</module>
       <module>it.tests</module>
       <module>dispatcher</module>
       <module>ui.tests</module>
       <module>analyse</module>
   </modules>
   ```

   El `pom.xml` el archivo debe tener un aspecto similar al siguiente:

   ![Quitar el módulo ui.frontend del pom del reactor](./assets/aem-project/uifrontend-reactor-pom.png)

1. Abra el `ui.apps/pom.xml`
1. Comente el `<dependency>` el `<artifactId>wknd-app.ui.frontend</artifactId>`

   ```
   <dependencies>
   
       <!-- Remote SPA project will provide all frontend resources
       <dependency>
           <groupId>com.adobe.aem.guides.wkndapp</groupId>
           <artifactId>wknd-app.ui.frontend</artifactId>
           <version>${project.version}</version>
           <type>zip</type>
       </dependency>
       --> 
   </dependencies>
   ```

   El `ui.apps/pom.xml` el archivo debe tener un aspecto similar al siguiente:

   ![Eliminar la dependencia ui.frontend de ui.apps](./assets/aem-project/uifrontend-uiapps-pom.png)

AEM Si el proyecto de se creó antes de estos cambios, elimine manualmente la variable `ui.frontend` Biblioteca de cliente generada desde el `ui.apps` proyecto en `ui.apps/src/main/content/jcr_root/apps/wknd-app/clientlibs/clientlib-react`.

## AEM Asignación de contenido de

AEM SPA SPA SPA AEM Para cargar de forma remota la remota en el Editor de, se deben establecer asignaciones entre las rutas de la y las Páginas de la página utilizadas para abrir y crear contenido.

La importancia de esta configuración se analiza más adelante.

La asignación se puede realizar con [Asignación de Sling](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html#root-level-mappings-1) definido en `/etc/map`.

1. En el IDE, abra el `ui.content` subproyecto
1. Vaya a  `src/main/content/jcr_root`
1. Crear una carpeta `etc`
1. Entrada `etc`, cree una carpeta `map`
1. Entrada `map`, cree una carpeta `http`
1. Entrada `http`, cree un archivo `.content.xml` con el contenido:

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="sling:Mapping">
       <localhost_any/>
   </jcr:root>
   ```

1. Entrada `http` , cree una carpeta `localhost_any`
1. Entrada `localhost_any`, cree un archivo `.content.xml` con el contenido:

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="sling:Mapping"
       sling:match="localhost\\.\\d+">
       <wknd-app-routes-adventure/>
   </jcr:root>
   ```

1. Entrada `localhost_any` , cree una carpeta `wknd-app-routes-adventure`
1. Entrada `wknd-app-routes-adventure`, cree un archivo `.content.xml` con el contenido:

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   
   <!--
   The 'wknd-app-routes-adventure' mapping, maps requests to the SPA's adventure route 
   to it's corresponding page in AEM at /content/wknd-app/us/en/home/adventure/xxx.
   
   Note the adventure AEM pages are created directly in AEM.
   -->
   
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="sling:Mapping"
       sling:match="adventure:.*/([^/]+)/?$"
       sling:internalRedirect="/content/wknd-app/us/en/home/adventure/$1"/>
   ```

1. Añadir los nodos de asignación a `ui.content/src/main/content/META-INF/vault/filter.xml` AEM a que se incluyan en el paquete de.

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <workspaceFilter version="1.0">
       <filter root="/conf/wknd-app" mode="merge"/>
       <filter root="/content/wknd-app" mode="merge"/>
       <filter root="/content/dam/wknd-app/asset.jpg" mode="merge"/>
       <filter root="/content/experience-fragments/wknd-app" mode="merge"/>
   
       <!-- Add the Sling Mapping rules for the WKND App -->
       <filter root="/etc/map" mode="merge"/>
   </workspaceFilter>
   ```

La estructura de carpetas y `.context.xml` los archivos deben tener este aspecto:

![Asignación de Sling](./assets/aem-project/sling-mapping.png)

El `filter.xml` el archivo debe tener un aspecto similar al siguiente:

![Asignación de Sling](./assets/aem-project/sling-mapping-filter.png)

AEM Ahora, cuando se implementa el proyecto de la, estas configuraciones se incluyen automáticamente.

AEM Los efectos de la asignación de Sling se ejecutan en `http` y `localhost`, por lo que solo es compatible con el desarrollo local. AEM Al implementar en el as a Cloud Service de la, se deben agregar asignaciones de Sling similares a ese destino `https` AEM y el dominio o dominios as a Cloud Service correspondientes. Para obtener más información, consulte la [Documentación de asignación de Sling](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html).

## Políticas de seguridad de Intercambio de recursos de origen cruzado

AEM SPA AEM A continuación, configure la protección del contenido para que solo este pueda acceder al contenido de la de forma que solo pueda acceder a él. Configurar [AEM Intercambio de recursos de origen cruzado en la](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/develop-for-cross-origin-resource-sharing.html).

1. En su IDE, abra el `ui.config` Subproyecto Maven
1. Navegar `src/main/content/jcr_root/apps/wknd-app/osgiconfig/config`
1. Cree un archivo llamado `com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-app_remote-spa.cfg.json`
1. Añada lo siguiente al archivo:

   ```
   {
       "supportscredentials":true,
       "exposedheaders":[
           ""
       ],
       "supportedmethods":[
           "GET",
           "HEAD",
           "POST",
           "OPTIONS"
       ],
       "alloworigin":[
           "https://external-hosted-app", "localhost:3000"
       ],
       "maxage:Integer":1800,
       "alloworiginregexp":[
           ".*"
       ],
       "allowedpaths":[
           ".*"
       ],
       "supportedheaders":[
           "Origin",
           "Accept",
           "X-Requested-With",
           "Content-Type",
           "Access-Control-Request-Method",
           "Access-Control-Request-Headers",
           "authorization"
       ]
   }
   ```

El `com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-app_remote-spa.cfg.json` el archivo debe tener un aspecto similar al siguiente:

![SPA Configuración del Editor CORS de](./assets/aem-project/cors-configuration.png)

Los elementos clave de la configuración son:

+ `alloworigin` AEM especifica los hosts que pueden recuperar contenido de las listas de distribución de los recursos
   + `localhost:3000` SPA se ha agregado para admitir la ejecución local de la
   + `https://external-hosted-app` SPA actúa como un marcador de posición que se reemplazará con el dominio en el que está alojado el servicio remoto de.
+ `allowedpaths` AEM especifique las rutas en las que se encuentra la configuración de CORS que abarca la configuración de la aplicación AEM SPA El valor predeterminado permite el acceso a todo el contenido en las listas, aunque esto solo se puede definir para las rutas específicas a las que el usuario puede acceder, por ejemplo, a las que se puede acceder a través de los siguientes elementos: `/content/wknd-app`.

## AEM SPA Establecer página de como plantilla de página de remoto

AEM AEM SPA AEM El tipo de archivo del proyecto de genera un proyecto preparado para la integración de la con un tipo de archivo remoto, pero que requiere un ajuste pequeño pero importante de la estructura de la página generada automáticamente. AEM El tipo de la página de generada automáticamente debe cambiarse a __SPA Página de__, en lugar de a __SPA página de__.

1. En su IDE, abra el `ui.content` subproyecto
1. Abrir en `src/main/content/jcr_root/content/wknd-app/us/en/home/.content.xml`
1. Actualizar esto `.content.xml` archivo con:

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
           jcr:primaryType="cq:Page">
       <jcr:content
           cq:template="/conf/wknd-app/settings/wcm/templates/spa-remote-page"
           jcr:primaryType="cq:PageContent"
           jcr:title="WKND App Home Page"
           sling:resourceType="wknd-app/components/remotepage">
           <root
               jcr:primaryType="nt:unstructured"
               sling:resourceType="wcm/foundation/components/responsivegrid">
               <responsivegrid
                   jcr:primaryType="nt:unstructured"
                   sling:resourceType="wcm/foundation/components/responsivegrid">
                   <text
                       jcr:primaryType="nt:unstructured"
                       sling:resourceType="wknd-app/components/text"
                       text="&lt;p>Hello World!&lt;/p>"
                       textIsRich="true">
                       <cq:responsive jcr:primaryType="nt:unstructured"/>
                   </text>
               </responsivegrid>
           </root>
       </jcr:content>
   </jcr:root>
   ```

Los cambios clave son las actualizaciones del `jcr:content` de nodo:

+ `cq:template` hasta `/conf/wknd-app/settings/wcm/templates/spa-remote-page`
+ `sling:resourceType` hasta `wknd-app/components/remotepage`

El `src/main/content/jcr_root/content/wknd-app/us/en/home/.content.xml` el archivo debe tener un aspecto similar al siguiente:

![Actualizaciones del archivo .content.xml de la página de inicio](./assets/aem-project/home-content-xml.png)

SPA AEM SPA SPA Estos cambios permiten que esta página, que actúa como raíz de la en la que se realiza la acción, cargue el control remoto en el Editor de.

>[!NOTE]
>
>AEM AEM Si este proyecto se implementó anteriormente para la, asegúrese de eliminar la página de la aplicación de forma que se eliminen los elementos de la página de la página de la. __Sites > Aplicación WKND > us > es > Página de inicio de la aplicación WKND__, como el `ui.content`  el proyecto está configurado en __fusionar__ nodos, en lugar de __actualizar__.

SPA AEM Esta página también se puede eliminar y volver a crear como una página de remoto en sí misma, aunque, dado que esta página se crea automáticamente en el `ui.content` proyecto es mejor actualizarlo en la base de código.

## AEM AEM Implementar el proyecto de en el SDK de la

1. AEM Asegúrese de que el servicio de autor de la se esté ejecutando en el puerto 4502
1. AEM Desde la línea de comandos, vaya a la raíz del proyecto de Maven, que se encuentra en la parte superior de la lista de proyectos de Maven.
1. AEM Utilice Maven para implementar el proyecto en el servicio local de creación de SDK de la

   ```
   $ mvn clean install -PautoInstallSinglePackage
   ```

   ![mvn clean install: PautoInstallSinglePackage](./assets/aem-project/mvn-install.png)

## AEM Configuración de la página raíz de la

AEM SPA SPA Con el proyecto de implementado, hay un último paso para preparar a Editor de la para cargar nuestro proyecto de forma remota. AEM AEM SPA En el caso de los usuarios, marque la página de la página de la que corresponda,`/content/wknd-app/us/en/home`AEM , generado por el tipo de archivo del proyecto de.

1. AEM Iniciar sesión en el autor de la
1. Vaya a __Sites > Aplicación WKND > us > es__
1. Seleccione el __Página de inicio de la aplicación WKND__ y pulse __Propiedades__

   ![Página de inicio de la aplicación WKND: propiedades](./assets/aem-content/edit-home-properties.png)

1. Vaya a __SPA__ pestaña
1. Rellene el __SPA Configuración de remoto__
   + __SPA URL de host__: `http://localhost:3000`
      + SPA La dirección URL a la raíz de la interfaz de usuario de la aplicación remota

   ![SPA Página de inicio de la aplicación WKND: configuración de remota](./assets/aem-content/remote-spa-configuration.png)

1. Tocar __Guardar y cerrar__

Recuerde que hemos cambiado el tipo de esta página por el de una __SPA Página de remota__, que es lo que nos permite ver el __SPA__ pestaña en su __Propiedades de página__.

AEM SPA Esta configuración solo debe configurarse en la página de que corresponda a la raíz de la. AEM Todas las páginas de la página que se encuentran debajo de esta página heredan el valor.

## Felicitaciones

AEM AEM Ahora ha preparado configuraciones de la y las ha implementado en el autor local de la misma. Ahora ya sabe cómo:

+ AEM SPA Elimine el archivo generado por el tipo de archivo del proyecto de comentando las dependencias en `ui.frontend`
+ AEM SPA AEM Agregue asignaciones de Sling para que las asignaciones de los recursos de Sling se asignen a las rutas de los recursos de la de trabajo.
+ AEM SPA AEM Configurar políticas de seguridad de uso compartido de recursos de origen cruzado de la que permitan al usuario remoto consumir contenido de los recursos de origen cruzado de la
+ AEM AEM Implemente el proyecto de en el servicio local de creación de SDK de
+ AEM SPA SPA Marcar una página de como raíz de la remota mediante la propiedad de la página URL del host de la aplicación

## Siguientes pasos

AEM Con la configuración de la plataforma, podemos centrarnos en lo siguiente: [SPA Inicio de la aplicación remota de](./spa-bootstrap.md) AEM SPA con compatibilidad para áreas editables mediante el Editor de de trabajo.
