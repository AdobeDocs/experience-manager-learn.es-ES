---
title: Configuración de AEM para SPA Editor y SPA remoto
description: Se requiere un proyecto AEM para los requisitos de configuración y contenido compatibles con la configuración a fin de permitir AEM SPA Editor crear un SPA remoto.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7631
thumbnail: kt-7631.jpeg
exl-id: 0bdb93c9-5070-483c-a34c-f2b348bfe5ae
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1215'
ht-degree: 1%

---

# Configuración de AEM para SPA Editor

Aunque el código base de SPA se administra fuera de AEM, se requiere un proyecto de AEM para configurar los requisitos de configuración y contenido. Este capítulo explica la creación de un proyecto AEM que contenga las configuraciones necesarias:

+ AEM proxies de componentes principales de WCM
+ AEM Remote SPA Page proxy
+ AEM plantillas de página SPA remotas
+ Páginas de AEM de SPA remoto de línea de base
+ Subproyecto para definir SPA a AEM asignaciones de URL
+ Carpetas de configuración de OSGi

## Creación de un proyecto AEM

Cree un proyecto AEM en el que se administren las configuraciones y el contenido de línea de base.

_Utilice siempre la versión más reciente de [Tipo de archivo AEM](https://github.com/adobe/aem-project-archetype)._


```
$ mkdir -p ~/Code/wknd-app
$ cd ~/Code/wknd-app
$ mvn -B archetype:generate \
 -D archetypeGroupId=com.adobe.aem \
 -D archetypeArtifactId=aem-project-archetype \
 -D archetypeVersion=27 \
 -D aemVersion=cloud \
 -D appTitle="WKND App" \
 -D appId="wknd-app" \
 -D groupId="com.adobe.aem.guides.wkndapp" \
 -D frontendModule="react"
$ mv ~/Code/wknd-app/wknd-app ~/Code/wknd-app/com.adobe.aem.guides.wknd-app
```

_El último comando simplemente cambia el nombre de la carpeta del proyecto de AEM para que quede claro que es el proyecto de AEM y no debe confundirse con SPA remoto__

While `frontendModule="react"` se especifica, la variable `ui.frontend` proyecto no se utiliza para el caso de uso de SPA remoto. El SPA se desarrolla y administra externamente para AEM y solo utiliza AEM como API de contenido. La variable `frontendModule="react"` se requiere un indicador para que el proyecto incluya la variable  `spa-project` AEM las dependencias de Java™ y configure las plantillas de página de SPA remotas.

El tipo de archivo del proyecto AEM genera los siguientes elementos que se utilizaron para configurar AEM para la integración con el SPA.

+ __AEM proxies de componentes principales de WCM__ at `ui.apps/src/.../apps/wknd-app/components`
+ __AEM SPA del proxy de página remota__ at `ui.apps/src/.../apps/wknd-app/components/remotepage`
+ __Plantillas de página AEM__ at `ui.content/src/.../conf/wknd-app/settings/wcm/templates`
+ __Subproyecto para definir asignaciones de contenido__ at `ui.content/src/...`
+ __Páginas de AEM de SPA remoto de línea de base__ at `ui.content/src/.../content/wknd-app`
+ __Carpetas de configuración de OSGi__ at `ui.config/src/.../apps/wknd-app/osgiconfig`

Con la generación del proyecto de AEM base, algunos ajustes garantizan SPA compatibilidad del Editor con SPA remoto.

## Quitar proyecto ui.frontend

Dado que el SPA es un SPA remoto, asumamos que se ha desarrollado y administrado fuera del proyecto AEM. Para evitar conflictos, elimine la variable `ui.frontend` proyecto desde la implementación. Si la variable `ui.frontend` El proyecto no se elimina, dos SPA, la SPA predeterminada que se proporciona en la `ui.frontend` El proyecto y el SPA remoto se cargan al mismo tiempo en el AEM SPA Editor.

1. Abra el proyecto AEM (`~/Code/wknd-app/com.adobe.aem.guides.wknd-app`) en su IDE
1. Abra la raíz `pom.xml`
1. Comente el `<module>ui.frontend</module` fuera de `<modules>` list

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

   La variable `pom.xml` debe tener el siguiente aspecto:

   ![Quitar el módulo ui.frontend del pom del reactor](./assets/aem-project/uifrontend-reactor-pom.png)

1. Abra el `ui.apps/pom.xml`
1. Comente el `<dependency>` en `<artifactId>wknd-app.ui.frontend</artifactId>`

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

   La variable `ui.apps/pom.xml` debe tener el siguiente aspecto:

   ![Elimine la dependencia ui.frontend de ui.apps](./assets/aem-project/uifrontend-uiapps-pom.png)

Si el proyecto de AEM se creó antes de estos cambios, elimine manualmente la variable `ui.frontend` biblioteca de cliente generada desde la variable `ui.apps` proyecto en `ui.apps/src/main/content/jcr_root/apps/wknd-app/clientlibs/clientlib-react`.

## Asignación de contenido AEM

Para AEM cargar el SPA remoto en el Editor de SPA, se deben establecer asignaciones entre las rutas de SPA y las páginas de AEM utilizadas para abrir y crear contenido.

La importancia de esta configuración se analiza más adelante.

La asignación se puede realizar con [Asignación de Sling](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html#root-level-mappings-1) definido en `/etc/map`.

1. En el IDE, abra el `ui.content` subproyecto
1. Vaya a  `src/main/content/jcr_root`
1. Cree una carpeta  . `etc`
1. En `etc`, crear una carpeta `map`
1. En `map`, crear una carpeta `http`
1. En `http`, crear un archivo `.content.xml` con el contenido:

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="sling:Mapping">
       <localhost_any/>
   </jcr:root>
   ```

1. En `http` , crear una carpeta `localhost_any`
1. En `localhost_any`, crear un archivo `.content.xml` con el contenido:

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="sling:Mapping"
       sling:match="localhost\\.\\d+">
       <wknd-app-routes-adventure/>
   </jcr:root>
   ```

1. En `localhost_any` , crear una carpeta `wknd-app-routes-adventure`
1. En `wknd-app-routes-adventure`, crear un archivo `.content.xml` con el contenido:

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

1. Añadir los nodos de asignación a `ui.content/src/main/content/META-INF/vault/filter.xml` a que se incluyen en el paquete AEM.

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

La estructura de carpetas y `.context.xml` Los archivos deben tener el siguiente aspecto:

![Asignación de Sling](./assets/aem-project/sling-mapping.png)

La variable `filter.xml` debe tener el siguiente aspecto:

![Asignación de Sling](./assets/aem-project/sling-mapping-filter.png)

Ahora, cuando se implementa el proyecto AEM, estas configuraciones se incluyen automáticamente.

Los efectos de asignación de Sling AEM se ejecutan en `http` y `localhost`, de modo que solo admita el desarrollo local. Al implementar en AEM as a Cloud Service, se deben agregar asignaciones de Sling similares al destino `https` y los dominios as a Cloud Service AEM correspondientes. Para obtener más información, consulte la [Documentación de Sling Mapping](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html).

## Políticas de seguridad de Cross-Origin Resource Sharing

A continuación, configure AEM para proteger el contenido de modo que solo este SPA pueda acceder al contenido AEM. Configurar [Uso compartido de recursos de origen cruzado en AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/develop-for-cross-origin-resource-sharing.html).

1. En su IDE, abra el `ui.config` Subproyecto Maven
1. Navegar `src/main/content/jcr_root/apps/wknd-app/osgiconfig/config`
1. Crear un archivo con el nombre `com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-app_remote-spa.cfg.json`
1. Agregue lo siguiente al archivo :

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

La variable `com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-app_remote-spa.cfg.json` debe tener el siguiente aspecto:

![Configuración de CORS del Editor SPA](./assets/aem-project/cors-configuration.png)

Los elementos de configuración clave son:

+ `alloworigin` especifica qué hosts pueden recuperar contenido de AEM.
   + `localhost:3000` se agrega para admitir la SPA que se ejecuta localmente
   + `https://external-hosted-app` actúa como marcador de posición para reemplazarlo por el dominio en el que está alojado SPA remoto.
+ `allowedpaths` especifique qué rutas de AEM están cubiertas por esta configuración CORS. El valor predeterminado permite acceder a todo el contenido de AEM, aunque solo se puede tratar en el ámbito de las rutas específicas a las que el SPA puede acceder, por ejemplo: `/content/wknd-app`.

## Establecer AEM página como plantilla SPA página remota

El tipo de archivo del proyecto AEM genera un proyecto preparado para AEM integración con un SPA remoto, pero requiere un ajuste pequeño, pero importante, de la estructura de AEM página generada automáticamente. La página de AEM generada automáticamente debe cambiar su tipo a __Página SPA remota__, en lugar de __SPA página__.

1. En su IDE, abra el `ui.content` subproyecto
1. Abrir a `src/main/content/jcr_root/content/wknd-app/us/en/home/.content.xml`
1. Actualice esto `.content.xml` con:

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

Los cambios clave son las actualizaciones de la variable `jcr:content` del nodo:

+ `cq:template` hasta `/conf/wknd-app/settings/wcm/templates/spa-remote-page`
+ `sling:resourceType` hasta `wknd-app/components/remotepage`

La variable `src/main/content/jcr_root/content/wknd-app/us/en/home/.content.xml` debe tener el siguiente aspecto:

![Actualizaciones de la página principal .content.xml](./assets/aem-project/home-content-xml.png)

Estos cambios permiten que esta página, que actúa como SPA raíz en AEM, cargue la SPA remota en SPA Editor.

>[!NOTE]
>
>Si este proyecto se implementó anteriormente en AEM, asegúrese de eliminar la página AEM como __Sites > Aplicación WKND > us > es > Página de inicio de la aplicación WKND__, como `ui.content`  proyecto está definido como __combinar__ en lugar de __actualizar__.

Esta página también se puede eliminar y volver a crear como una página de SPA remota en AEM, sin embargo, ya que esta página se crea automáticamente en la variable `ui.content` es mejor actualizarlo en la base de código.

## Implementación del proyecto AEM en AEM SDK

1. Asegúrese de que el servicio AEM Author se esté ejecutando en el puerto 4502
1. Desde la línea de comandos, vaya a la raíz del proyecto AEM Maven.
1. Utilice Maven para implementar el proyecto en el servicio de creación del SDK de AEM local.

   ```
   $ mvn clean install -PautoInstallSinglePackage
   ```

   ![mvn clean install -PautoInstallSinglePackage](./assets/aem-project/mvn-install.png)

## Configuración de la página AEM raíz

Con el proyecto AEM implementado, hay un último paso para preparar SPA Editor para cargar nuestro SPA remoto. En AEM, marque la página de AEM que corresponde a la raíz de SPA,`/content/wknd-app/us/en/home`, generado por el tipo de archivo del proyecto de AEM.

1. Iniciar sesión en AEM Author
1. Vaya a __Sites > Aplicación WKND > us > es__
1. Seleccione el __Página principal de la aplicación WKND__ y toque __Propiedades__

   ![Página principal de la aplicación WKND: propiedades](./assets/aem-content/edit-home-properties.png)

1. Vaya a la __SPA__ ficha
1. Complete el __Configuración de SPA remoto__
   + __URL del host SPA__: `http://localhost:3000`
      + La URL a la raíz del SPA remoto

   ![Página principal de la aplicación WKND: configuración de SPA remota](./assets/aem-content/remote-spa-configuration.png)

1. Toque __Guardar y cerrar__

Recuerde que hemos cambiado el tipo de esta página por el de un __Página SPA remota__, que es lo que nos permite ver la variable __SPA__ en su __Propiedades de página__.

Esta configuración solo debe configurarse en la página AEM que corresponda a la raíz de la SPA. Todas las páginas AEM debajo de esta página heredan el valor.

## Felicitaciones

Ya ha preparado AEM configuraciones e implementado en su autor de AEM local. Ahora sabe cómo:

+ Elimine la SPA generada por el tipo de archivo del proyecto de AEM, comentando las dependencias en `ui.frontend`
+ Agregue asignaciones de Sling a AEM que asignen las rutas de SPA a los recursos en AEM
+ Configure AEM directivas de seguridad de Cross-Origin Resource Sharing que permitan que el SPA remoto consuma contenido de AEM
+ Implemente el proyecto de AEM en el servicio local AEM SDK Author
+ Marcar una página AEM como la raíz de SPA remota usando la propiedad de página URL de host SPA

## Siguientes pasos

Con AEM configurado, podemos centrarnos en [arrancar el SPA remoto](./spa-bootstrap.md) con compatibilidad con áreas editables mediante AEM SPA Editor!
