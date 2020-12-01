---
title: Comprender las razones para la actualización
description: Un desglose de alto nivel de las funciones clave para los clientes que están considerando actualizar a la versión más reciente de Adobe Experience Manager.
version: 6.5
sub-product: recursos, cloud-manager, comercio, servicios de contenido, medios dinámicos, formularios, fundaciones, pantallas, sitios
topics: best-practices, upgrade
audience: all
activity: understand
doc-type: article
translation-type: tm+mt
source-git-commit: 1519856731758ece2860615c06fc0d64edb104a5
workflow-type: tm+mt
source-wordcount: '3540'
ht-degree: 2%

---


# Explicación de los motivos de la actualización

Un desglose de alto nivel de las funciones clave para los clientes que están considerando actualizar a la versión más reciente de Adobe Experience Manager.

## Características clave para la actualización a AEM 6.5

+ [Notas de la versión de Adobe Experience Manager 6.5](https://helpx.adobe.com/es/experience-manager/6-5/release-notes.html)

### Mejoras en la base

Adobe Experience Manager 6.5 sigue mejorando la estabilidad, el rendimiento y la compatibilidad del sistema mediante:

+ **Compatibilidad** con Java 11 (al mismo tiempo que se mantiene la compatibilidad con Java 8).

### Creación y administración de sitios Web

AEM Sites presenta una serie de funciones diseñadas para acelerar la creación y la creación de sitios web:

+ **SPA** Editorsupport permite la creación completa de SPA (aplicaciones de una sola página) en AEM, lo que permite una experiencia de creación rica y compatible con el mercado.
+_ **Los SDK de JavaScript**, un kit de Inicio de proyecto SPA y herramientas de compilación compatibles, permiten a los desarrolladores front-end desarrollar aplicaciones de página única compatibles con SPA editor independientemente de AEM.
+ **Los** componentes principales agregan una multitud de nuevos componentes, una biblioteca  **de componentes y** una variedad de mejoras a los componentes principales existentes.
+ Más **traducciones** optimizan la traducción de AEM Sites.

### Experiencias fluidas

AEM continúa adoptando Experiencias Fluidas con herramientas nuevas y mejoradas que facilitan el uso de contenido fuera de AEM.

+ **Los** fragmentos de contenido son compatibles con la comparación/diferencia de versiones y las anotaciones.
+ **AEM Assets HTTP** API **admite la exposición** de  **fragmentos de contenido directamente en DAM como**JSON.
   **Los** fragmentos de experiencia admiten  **la** búsqueda de texto completo y la  **AEM** invalidación de la caché del despachante para hacer referencia a  **las páginas**.

### Administración de activos

AEM Assets continúa aprovechando su rico conjunto de capacidades de administración de activos para mejorar el uso, la administración y la comprensión del DAM. AEM 6.5 sigue mejorando la integración entre Adobe Creative Cloud y los flujos de trabajo creativos.

+ **Adobe Asset** Links conecta elementos creativos directamente a AEM Assets desde las herramientas de Adobe Creative Cloud.
+ **La integración de Adobe** Stockintegration permite el acceso directo a imágenes de Adobe Stock directamente desde la experiencia de AEM Assets, lo que crea una experiencia de descubrimiento de contenido sin fisuras.
+ **AEM Desktop** Appreleases versión 2.0 y rediseño a sí mismo mientras mejora el rendimiento y la estabilidad.
+ **Los** recursos conectados admiten instancias discretas de AEM Sites para acceder a los recursos y utilizarlos sin problemas desde una instancia de AEM Assets diferente.
+ Se ha actualizado la compatibilidad con vídeo en **Dynamic Media**, incluidos **360 Video** y **Miniaturas de vídeo personalizadas**.

### Inteligencia de contenido

AEM continúa desarrollando su integración con tecnologías inteligentes, aprovechando el aprendizaje automático y la inteligencia artificial para mejorar todas las experiencias.

+ **Adobe Asset** Linkads  **Visual Similarity Search**, lo que permite descubrir y utilizar fácilmente imágenes similares dentro de las herramientas **de** Adobe Creative Cloud.

### Integraciones

AEM aumenta su capacidad de integración con otros servicios de Adobe:

+ **Los** fragmentos de experiencia profundizan su integración con  **Adobe** Target al admitir  **Exportar como** JSON a Adobe Target y la capacidad de  **eliminar** ofertas basadas en fragmentos de experiencia de  **Adobe Target**.

### Administrador de nube de AMS

[Cloud Manager](https://adobe.ly/2HODmsv), exclusivo para los clientes de los servicios gestionados de Adobe (AMS), oferta las siguientes funciones:

+ Cloud Manager admite la ampliación AEM la compatibilidad con la implementación desde AEM Sites a **AEM Assets**, incluidas **pruebas de rendimiento automatizadas del procesamiento de recursos**.
+ **La** ampliación automática del nivel de AEM Publish en umbrales predefinidos garantiza una experiencia óptima para el usuario final.
+ **Las** tuberías que no son de producción permiten a los equipos de desarrollo aprovechar Cloud Manager para comprobar continuamente la calidad del código e implementarlas en entornos más bajos (Desarrollo y control de calidad).
+ **Las** API de canalización de CD/CI permiten a los clientes participar mediante programación con Cloud Manager, lo que aumenta las posibilidades de integración con la infraestructura de desarrollo local.

## Funciones básicas

A continuación se presenta una matriz de las principales funciones básicas ofrecidas por AEM. Algunas de estas funciones se introdujeron en versiones anteriores y se añadieron mejoras incrementales en cada versión.

+ [Notas de la versión de AEM Foundation](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html)

***✔ <sup>+mejoras </sup> significativas en la función de esta versión.***

***✔ <sup></sup> SPindica que la función está disponible a través de un Service Pack o un Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Función de base</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>
                <strong>Compatibilidad con Java 11:</strong> AEM admite Java 11 (así como Java 8).
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>bito</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://jackrabbit.apache.org/oak/docs/index.html" target="_blank">Repositorio</a> de contenido Oak: </strong> Proporciona un rendimiento y una escalabilidad buenos con respecto al anterior Jackrabbit 2.</td>
            <td> </td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/indexing-via-the-oak-run-jar.html">compatibilidad con</a> índices oak-run.jar: </strong> se ha mejorado la reindexación, la recopilación de estadísticas y la comprobación de coherencia de los índices Oak.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/queries-and-indexing.html" target="_blank">Índices</a> de búsqueda personalizados:  </strong>
                Posibilidad de agregar definiciones de índice personalizadas para optimizar el rendimiento de la consulta y la relevancia de la búsqueda.</td>
            <td> </td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/revision-cleanup.html" target="_blank">Limpieza</a> de revisión en línea: </strong>
                realice el mantenimiento del repositorio sin downtime del servidor.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/storage-elements-in-aem-6.html" target="_blank">Almacenamiento</a> de repositorio TarMK o MongoMK:</strong>
                <br> Opciones para utilizar almacenamientos simples y de rendimiento basados en archivos de TarMK (versión de próxima generación de TarPM) 
                <br> o escalar horizontalmente con un repositorio respaldado por MongoDB con MongoMK.</td>
            <td> </td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/aem-with-mongodb.html" target="_blank">Rendimiento y estabilidad</a> de MongoMK: se han realizado </strong>
            continuas mejoras en MongoMK desde su introducción a AEM 6.0.</td>
            <td> </td>
            <td> </td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/data-store-config.html#AmazonS3DataStore">Amazon S3 DataStore</a>:</strong>
            aproveche la solución de almacenamiento en la nube ampliable para almacenar recursos binarios.</td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong>Paridad de funciones de la IU táctil: </strong>
                continuas mejoras en la interfaz de usuario de creación para aumentar la velocidad con mayor productividad y paridad de funciones con la IU clásica.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong>Omniture:</strong>
                Buscar y navegar AEM rápidamente.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/operations-dashboard.html" target="_blank">Panel</a> de operaciones: </strong>
 realice tareas de mantenimiento, supervise el estado del servidor y analice el rendimiento desde AEM.</td>
            <td></td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/upgrade.html" target="_blank">Mejoras</a> en la actualización: las mejoras en la </strong>
            actualización permiten actualizar la AEM de forma más sencilla y rápida.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/htl/using/overview.html" target="_blank">HTL Template Language</a>: </strong>
            un motor de creación de plantillas moderno que separa la presentación de la lógica. Reduce considerablemente el tiempo de desarrollo de los componentes. Se han añadido funciones incrementales con cada versión.</td>
            <td> </td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://sling.apache.org/documentation/bundles/models.html" target="_blank">Modelos</a> Sling: </strong>
            un marco flexible para modelar recursos JCR en objetos y lógica empresariales. Se han añadido funciones incrementales con cada versión.
            </td>
            <td> </td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://adobe.ly/2HODmsv" target="_blank">Administrador</a> de nube:  </strong>
                Exclusivamente para los clientes de los servicios gestionados de Adobe (AMS), Cloud Manager acelera el desarrollo y la implementación mediante una canalización de CD/CI de última generación.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
        </tr>
    </tbody>
</table>

## Funciones de seguridad

A continuación se muestra una matriz de las características de seguridad clave ofrecidas por AEM. Algunas de estas funciones se introdujeron en versiones anteriores y se añadieron mejoras incrementales en cada versión.

+ [Notas de la versión de seguridad](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html#Security)

***La sintaxis de la sección dice que en esta versión se han realizado mejoras significativas en la función.***

***✔ <sup>+</sup> indica que la función está disponible mediante un Service Pack o un Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Función de seguridad</td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6,3</td>
            <td>6,4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html" target="_blank">Usuarios de </a></strong>
            <br> serviciosLos permisos de la división evitan el uso innecesario de privilegios de administrador.</td>
        <td></td>
        <td>bito</td>
        <td>bito</td>
        <td>bito</td>
        <td>bito</td>
        <td>bito</td>
        <td>bito</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank">Administración </a></strong>
            <br> de almacenes de clavesAlmacén de confianza global, certificados y claves administrados en el repositorio.</td>
        <td></td>
        <td>bito</td>
        <td>bito</td>
        <td>bito</td>
        <td>bito</td>
        <td>bito</td>
        <td>bito</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/csrf-protection.html" target="_blank"><strong></strong> <strong></strong></a>
            <br> CSRFprotectionProtección contra falsificaciones de solicitudes entre sitios fuera de la caja.</td>
        <td></td>
        <td></td>
        <td>bito</td>
        <td>bito</td>
        <td>bito</td>
        <td>bito</td>
        <td>bito</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank"><strong></strong> <strong></strong></a>
            <br> CORSsupportCompatibilidad con el uso compartido de recursos entre Orígenes para buena flexibilidad de la aplicación.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>bito</td>
        <td>bito</td>
        <td>bito</td>
    </tr>
    <tr>
        <td><strong><a href="https://docs.adobe.com/docs/en/aem/6-5/administer/security/saml-2-0-authenticationhandler.html" target="_blank">Se ha mejorado la </a><br>
 </strong>compatibilidad con la autenticación SAMLha mejorado la redirección de SAML, se ha optimizado la información de grupo y se han resuelto los problemas de cifrado de claves. 
            <br>
        </td>
        <td></td>
        <td></td>
        <td>bito</td>
        <td>bito</td>
        <td>bito</td>
        <td>bito</td>
        <td>bito</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html" target="_blank">LDAP como </a><br>
 </strong>configuración OSGi Simplifica la administración y las actualizaciones de la autenticación LDAP.</td>
        <td></td>
        <td>bito</td>
        <td>bito</td>
        <td>bito</td>
        <td>bito</td>
        <td>bito</td>
        <td>bito</td>
    </tr>
    <tr>
        <td><strong>Compatibilidad con codificación OSGi para <br>
 </strong>contraseñas de texto sin formatoLas contraseñas y otros valores confidenciales se pueden guardar en forma cifrada y descifrar automáticamente.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>bito</td>
        <td>bito</td>
        <td>bito</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/user-group-ac-admin.html" target="_blank">Mejoras en el CUG</a><br>
 </strong>Se ha reescrito la implementación de grupos cerrados de usuarios para abordar problemas de rendimiento y escalabilidad.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>bito</td>
        <td>bito<sup>+</sup></td>
        <td>bito</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/platform-repository/using/ssl-wizard-technical-video-use.html" target="_blank">SSL </a></strong>
            <br> WizardUI para simplificar la configuración y administración de SSL.</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>bito</td>
        <td>bito</td>
        <td>bito</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/encapsulated-token.html" target="_blank">Compatibilidad con </a></strong>
            <br> tokens encapsuladosYa no es necesario para sesiones "adhesivas" para admitir la autenticación horizontal en todas las instancias de publicación.</td>
        <td> </td>
        <td> </td>
        <td>bito</td>
        <td>bito</td>
        <td>bito</td>
        <td>bito</td>
        <td>bito</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ims-config-and-admin-console.html" target="_blank">Compatibilidad </a><br>
 </strong>con la autenticación IMS de AdobeExclusivo para los servicios gestionados de Adobe (AMS), gestione de forma centralizada el acceso a las instancias de AEM Author mediante Adobe IMS (Identity Management System).</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>bito</td>
        <td>bito</td>
    </tr>
</tbody>
</table>

## Funciones del sitio

A continuación se muestra una matriz de las funciones de sitios principales ofrecidas por AEM. Algunas de estas funciones se introdujeron en versiones anteriores y se añadieron mejoras incrementales en cada versión.

+ [Notas de la versión de AEM Sites](https://helpx.adobe.com/experience-manager/6-5/release-notes/sites.html)

***✔ <sup>+mejoras </sup> significativas en la función de esta versión.***

***✔ <sup></sup> SPindica que la función está disponible a través de un Service Pack o un Feature Pack.***

<table>
    <thead>
        <tr>
            <td><strong>Función Sitios</strong></td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6,3</td>
            <td>6,4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/page-editor-feature-video-use.html" target="_blank">Creación</a> de páginas táctil optimizada: </strong>
            permite a los editores aprovechar tabletas y equipos con pantallas táctiles.</td>
            <td></td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/responsive-layout.html" target="_blank">Creación</a> de sitios adaptable: </strong>
                el modo de diseño permite a los editores cambiar el tamaño de los componentes según la anchura del dispositivo para los sitios interactivos.</td>
            <td></td>
            <td></td>
            <td>bito</td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/template-editor-feature-video-use.html" target="_blank">Plantillas</a> editables:</strong>
            permite que los autores especializados creen y editen plantillas de página.</td>
            <td></td>
            <td></td>
            <td></td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/core-components/user-guide.html" target="_blank">Componentes</a> principales: </strong>
            acelere el desarrollo del sitio. Disponible en GitHub para programación de lanzamientos frecuentes y flexibilidad.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/spa-overview.html" target="_blank">Editor</a> de SPA:</strong>
            cree experiencias web autorizadas y activas con marcos de aplicación de una sola página (SPA) creados en React o Angular.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>bito<sup>+</sup></td>
            <td>bito<sup>+</sup></td>
            <td>bito<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/release-notes/style-system-fp.html" target="_blank">Sistema</a> de estilos:</strong>
            aumente AEM reutilización de componentes definiendo su aspecto visual con el sistema de estilo en contexto.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>bito<sup>SP</sup></td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/msm.html" target="_blank">Multi-Site Manager (MSM)</a>:</strong>
            Administre varios sitios web que compartan contenido común (es decir, varias marcas multilingües).</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/translation.html" target="_blank">Traducción</a> de contenido:El marco de trabajo </strong>
            Plug and Play se integra con los servicios de traducción de terceros líderes en la industria.</td>
            <td></td>
            <td></td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/contexthub.html" target="_blank">ContextHub</a>: marco contextual de cliente de </strong>
            próxima generación para la personalización de contenido.</td>
            <td></td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/launches.html" target="_blank">Inicios</a>: </strong>
            desarrolle contenido para una versión futura sin interrumpir la creación diaria.</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/content-fragments-feature-video-understand.html" target="_blank">Fragmentos</a> de contenido: </strong>
            cree y revise el contenido editorial desacoplado de la presentación para facilitar su reutilización.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito<sup>+</sup></td>
            <td>bito<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-use.html" target="_blank">Fragmentos</a> de experiencia:</strong>
            cree experiencias y variaciones reutilizables optimizadas para canales de escritorio, móviles y sociales.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/release-notes/content-services-fragments-featurepack.html" target="_blank">Servicios</a> de contenido:</strong>
            Exporte contenido de AEM como JSON para consumo en dispositivos y aplicaciones.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>bito<sup>SP</sup></td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong>Adobe Analytics Integration and Content Insights: integración </strong>
                sencilla de Adobe Analytics y DTM. Muestre información de rendimiento en entorno de autor.</td>
            <td> </td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/content-targeting-touch.html" target="_blank">Integración</a> de Adobe Target:asistente </strong>
            paso a paso para crear experiencias con objetivos y bibliotecas de ofertas reutilizables.</td>
            <td> </td>
            <td> </td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/campaign.html" target="_blank">Integración</a> de Adobe Campaign: integración </strong>
            sencilla con la solución de campaña de correo electrónico de próxima generación.</td>
            <td> </td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/using/aem_launch_adobeio_integration.html" target="_blank">Integración</a> de lanzamiento de Adobe:</strong>
            Integración con el servicio en la nube de administración de etiquetas de próxima generación de Adobe.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/aem-screens-introduction.html" target="_blank">Pantallas</a>:</strong>
            Gestionar experiencias para señalización digital y quioscos.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>bito</td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ecommerce.html" target="_blank">Comercio electrónico</a>: </strong>
            ofrezca experiencias de compra personalizadas y con marca en todos los puntos de contacto web, móviles y sociales.
            </td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/overview.html" target="_blank">Comunidades</a>: </strong>
            los foros, los comentarios en subprocesos, los calendarios de eventos y muchas otras funciones permiten una participación profunda con los visitantes del sitio.</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
        </tr>
    </tbody>
</table>

## Funciones de recursos

A continuación se muestra una matriz de las funciones clave de Recursos ofrecidas por AEM. Algunas de estas funciones se introdujeron en versiones anteriores y se añadieron mejoras incrementales en cada versión.

+ [Notas de la versión de AEM Assets](https://helpx.adobe.com/experience-manager/6-5/release-notes/assets.html)

***La sintaxis de la sección dice que en esta versión se han realizado mejoras significativas en la función.***

***✔ <sup>+</sup> indica que la función está disponible mediante un Service Pack o un Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Función Recursos</td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6,3</td>
            <td>6,4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets-touch-ui.html" target="_blank">IU</a> táctil optimizada:</strong>
            Administre recursos en un equipo de escritorio o en dispositivos táctiles.</td>
            <td> </td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/metadata.html" target="_blank">Administración</a> avanzada de metadatos:Plantillas </strong>
            de metadatos, Editor de Esquemas de metadatos y Edición masiva de metadatos.</td>
            <td> </td>
            <td>bito</td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/task-content.html" target="_blank"></a> Taskand  <a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects-with-workflows.html" target="_blank"></a> WorkflowManagement: flujos de trabajo y tareas </strong>
            pregenerados para la revisión y aprobación de recursos digitales que utilizan AEM proyectos.</td>
            <td> </td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong>Escalabilidad y rendimiento: compatibilidad </strong>
            mejorada con la ingesta, carga y almacenamiento a escala.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>bito</td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mac-api-assets.html" target="_blank">API</a> HTTP de recursos:interactuar </strong>
            mediante programación con recursos mediante HTTP y JSON.</td>
            <td> </td>
            <td> </td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/link-sharing.html" target="_blank">Uso compartido</a> de vínculos: uso compartido </strong>
            sencillo y específico de recursos digitales sin necesidad de iniciar sesión.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal.html" target="_blank">Brand Portal</a>:solución SAAS de servicio de </strong>
            nube para compartir y distribuir recursos digitales sin problemas.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/use-assets-across-connected-assets-instances.html" target="_blank">Recursos</a> conectados: las instancias de </strong>
            AEM Sites pueden acceder a los recursos y utilizarlos sin problemas desde una instancia de AEM Assets diferente.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/touch-ui-asset-insights.html" target="_blank">Perspectivas</a> de recursos:</strong>
            aproveche Adobe Analytics para capturar la interacción del cliente con los recursos digitales y la vista en AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/multilingual-assets.html" target="_blank">Recursos</a> multilingües: compatibilidad con la </strong>
            traducción automática de metadatos de recursos con raíces de idioma.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/enhanced-smart-tags.html" target="_blank">Etiquetas inteligentes y moderación</a>: </strong>
            aproveche Adobe Sensei para etiquetar imágenes automáticamente con metadatos útiles.</td>
            <td> </td>
            <td></td>
            <td> </td>
            <td> </td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/assets/using/smart-translation-search-feature-video-use.html" target="_blank">Búsqueda</a> de traducción inteligente:Traducir </strong>
            automáticamente términos de búsqueda al buscar AEM Assets.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/indesign.html" target="_blank">Integración</a> de Adobe InDesign Server:</strong>
            Generación de catálogos de productos. Cree folletos, volantes y anuncios impresos basados en plantillas de InDesign.</td>
            <td> </td>
            <td> </td>
            <td>bito</td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/desktop-app/aem-desktop-app.html" target="_blank">Aplicación</a> de escritorio AEM:</strong>
            sincronice recursos con el escritorio local para editarlos con productos de Creative Suite.
            </td>
            <td> </td>
            <td> </td>
            <td>bito</td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/imaging-transcoding-library.html" target="_blank">Biblioteca</a> de imágenes de Adobe:bibliotecas PDF de </strong>
                <br> Photoshop y Acrobat utilizadas para la manipulación de archivos de alta calidad.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://www.adobe.com/creativecloud/business/enterprise/adobe-asset-link.html" target="_blank">Vínculo</a> de recursos de Adobe:</strong>
            Acceda a AEM Assets directamente desde las aplicaciones de Adobe Create Cloud.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/aem-assets-adobe-stock.html" target="_blank">Integración</a> de Adobe Stock: acceda </strong>
            sin problemas a las imágenes de Adobe Stock y utilícelas directamente desde AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>bito<sup>SP</sup></td>
            <td>bito</td>
        </tr>
    </tbody>
</table>

### AEM Assets Dynamic Media

***✔ <sup>+mejoras </sup> significativas en la función de esta versión.***

***✔ <sup></sup> SPindica que la función está disponible a través de un Service Pack o un Feature Pack.***


<table>
    <thead>
        <tr>
            <td>Función Dynamic Media</td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6,3 + FP</td>
            <td>6,4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets.html" target="_blank">Imágenes</a>:distribuya </strong>
            dinámicamente imágenes en diferentes tamaños y formatos, incluido Smart Crop.</td>
            <td> </td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/video-profiles.html" target="_blank">Vídeo</a>:codificación de vídeo </strong>
            avanzada y flujo continuo de vídeo adaptable</td>
            <td> </td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito<sup>+</sup></td>
            <td>bito<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/interactive-images.html" target="_blank">Medios</a> interactivos:</strong>
            cree letreros interactivos y vídeos con contenido en el que se puede hacer clic para mostrar ofertas clave.
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>bito</td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong>Conjuntos (<a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/image-sets.html" target="_blank">Imagen</a>,  <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/spin-sets.html" target="_blank">Giro</a>, Medios  <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mixed-media-sets.html" target="_blank">mixtos</a>):</strong>
            permite a los usuarios aplicar zoom, desplazarse, rotar y simular una experiencia de visualización de 360 grados.</td>
            <td> </td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://docs.adobe.com/docs/en/aem/6-5/administer/content/dynamic-media/viewer-presets.html" target="_blank">Visores</a>:ajustes preestablecidos y reproductores de medios enriquecidos con marca </strong>
            personalizada compatibles con diferentes pantallas/dispositivos.</td>
            <td> </td>
            <td>bito</td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/delivering-dynamic-media-assets.html" target="_blank">Envío</a>:opciones </strong>
            flexibles para vincular o incrustar contenido de Dynamic Media y envío a través del protocolo HTTP/2.</td>
            <td> </td>
            <td>bito</td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong>Actualizar de Scene7 a Dynamic Media:</strong>
            Capacidad para migrar recursos principales y seguir utilizando las URL de S7 existentes.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
        </tr>
    </tbody>
</table>

## Funciones de Forms

A continuación se muestra una matriz de las principales funciones de Añada de AEM Forms ofrecidas por AEM. Algunas de estas funciones se introdujeron en versiones anteriores y se añadieron mejoras incrementales en cada versión.

+ [Notas de la versión de AEM Forms](https://helpx.adobe.com/experience-manager/6-5/release-notes/forms.html)

***✔ <sup>+mejoras </sup> significativas en la función de esta versión.***

***✔ <sup></sup> SPindica que la función está disponible a través de un Service Pack o un Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Función Forms</td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6,3</td>
            <td>6,4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/introduction-forms-authoring.html" target="_blank">Editor</a> de Forms adaptable:</strong>
            cree formularios interactivos, adaptables y adaptables en función de la configuración del dispositivo y del explorador.</td>
            <td> </td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito<sup>+</sup></td>
            <td>bito<sup>+</sup></td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/generate-document-of-record-for-non-xfa-based-adaptive-forms.html" target="_blank">Documento de registro</a>: </strong>
            cree un documento para garantizar el almacenamiento a largo plazo de una experiencia de captura de datos o una versión preparada para imprimir.</td>
            <td> </td>
            <td>bito</td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/themes.html" target="_blank">Editor</a> de temas:</strong>
            Crear temáticas reutilizables para aplicar estilo a los componentes y paneles de un formulario.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>bito</td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/template-editor.html" target="_blank">Editor</a> de plantillas:</strong>
            estandarizar e implementar optimizaciones para formularios adaptables.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>bito</td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedintegrationwithAdobeSign" target="_blank">Integración</a> de Adobe Sign:</strong>
            permitir la implementación de escenarios de firma basados en formularios integrados de Adobe Sign.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/cm-overview.html" target="_blank">Administración</a> de correspondencia:</strong>
            con AEM Forms, puede crear, administrar y entregar correspondencia personalizada e interactiva con los clientes.
            </td>
            <td> </td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#AEMFormsdataintegration" target="_blank">Integración</a> de datos de terceros:</strong>
            al utilizar la integración de datos, los datos se recuperan de orígenes de datos dispares en función de las entradas de usuario en un formulario. Al enviar el formulario, los datos capturados se vuelven a escribir en los orígenes de datos.
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#FormscentricAEMWorkflowsforAEMFormsonOSGi" target="_blank">Flujo de trabajo (en OSGi) para procesamiento</a> de Forms: implementación </strong>
            simplificada de procesos de aprobación de formularios.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/user-guide.html?topic=/experience-manager/6-5/forms/morehelp/integrations.ug.js" target="_blank">Integración con Marketing Cloud</a>: </strong>
            integración con Adobe Analytics y Adobe Target para mejorar y medir las experiencias de los clientes.</td>
            <td> </td>
            <td> </td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/introduction-managing-forms.html" target="_blank">Administrador</a> de formularios:</strong>
            Ubicación única para administrar todos los formularios, documentos y correspondencia, como habilitar análisis, traducción, pruebas A/B, revisiones y publicaciones.
            </td>
            <td> </td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/aem-forms-app.html" target="_blank">Aplicación</a> de AEM Forms:</strong>
            Permitir el procesamiento de formularios en línea o sin conexión dentro de una aplicación en iOS, Android o Windows.</td>
            <td> </td>
            <td>bito</td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/adaptive-document.html" target="_blank">Comunicaciones</a> interactivas:</strong>
            cree comunicaciones enriquecidas, como afirmaciones con objetivos, con elementos interactivos como gráficos (anteriormente conocidos como Documentos adaptables).</td>
            <td> </td>
            <td> </td>
            <td>bito</td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito<sup>+</sup></td>
            <td>bito<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/pdf/aem-forms/6-5/WorkbenchHelp.pdf" target="_blank">Flujo de trabajo (J2EE) para procesamiento</a> de Forms: </strong>
            cree formularios complejos/flujos de trabajo centrados en el documento mediante un IDE intuitivo.</td>
            <td></td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedDocumentSecurity" target="_blank">Seguridad</a> de AEM Forms Documento: acceso </strong>
            seguro y autorización de documentos de PDF y Office.
            </td>
            <td> </td>
            <td>bito</td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#Simplifiedauthoringexperience" target="_blank">Marcos</a> de prueba:</strong>
            utilice el marco de Calvin y el complemento Chrome para admitir y depurar formularios adaptables.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
        </tr>
    </tbody>
</table>

## Funciones de comunidades

A continuación se muestra una matriz de las principales funciones de Añada de AEM Communities ofrecidas por AEM. Algunas de estas funciones se introdujeron en versiones anteriores y se añadieron mejoras incrementales en cada versión.

+ [Resumen de las nuevas funciones de AEM Communities](https://helpx.adobe.com/experience-manager/6-5/communities/using/whats-new-aem-communities.html#main-pars_text)

***✔ <sup>+mejoras </sup> significativas en la función de esta versión.***

***✔ <sup></sup> SPindica que la función está disponible a través de un Service Pack o un Feature Pack.***

<table>
    <thead>
        <tr>
            <td> </td>
            <td>Función de comunidades</td>
            <td>6,0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6,3</td>
            <td>6,4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan="7">Funciones de comunidades</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/forum.html" target="_blank">Foros</a>:</strong>  (Social Component Framework) Cree nuevos temas o vista, siga, busque y mueva los temas existentes.</td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td>
                <p><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-qna.html" target="_blank">QnA</a>: </strong>
                Preguntas, vista y respuestas.</p>
            </td>
            <td></td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/blog-feature.html" target="_blank">Blogs</a>: </strong>
                Cree artículos y comentarios en el lado de publicación.
            </td>
            <td> </td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/ideation-feature.html" target="_blank">Ideación</a>:</strong>
                Crear y compartir ideas con la comunidad, o vista, seguir y comentar ideas existentes.
            </td>
            <td> </td>
            <td> </td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/calendar.html" target="_blank">Calendario</a>:</strong>
                 (Social Component Framework) Proporciona información de eventos de la comunidad a los visitantes del sitio.
            </td>
            <td>bito<sup>+</sup></td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/file-library.html" target="_blank">Biblioteca</a> de archivos: </strong>
                cargue, administre y descargue archivos dentro del sitio de la comunidad.</td>
            <td> </td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/users.html#AboutCommunityGroups" target="_blank">Grupos</a> de usuarios: 
            </strong>Un conjunto de usuarios puede pertenecer a grupos de miembros y se pueden asignar funciones colectivamente.</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong> </strong></td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resource.html" target="_blank">Asignación</a>:</strong>
            Cree y asigne recursos de aprendizaje a los miembros de la comunidad.</td>
            <td> </td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td rowspan="5">Habilitación</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/catalog.html" target="_blank">Administración </a> de  <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resource.html" target="_blank">recursos y catálogos</a>:</strong>
            Acceso a recursos de habilitación desde el catálogo.</td>
            <td> </td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resources.html#CreateaLearningPath" target="_blank">Administración</a> de rutas de aprendizaje:</strong>
            Administre cursos o grupos de recursos de habilitación.</td>
            <td> </td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/reports.html#main-pars_text_1739724213" target="_blank">Sistema de informes</a> de habilitación:</strong>
            Sistema de informes en recursos de habilitación y rutas de aprendizaje.</td>
            <td> </td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resources.html#main-pars_text_899882038" target="_blank">Participación en la habilitación</a>:</strong>
            Añada comentarios sobre los recursos de habilitación.</td>
            <td> </td>
            <td> </td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/analytics.html" target="_blank">Análisis</a> de habilitación:Análisis </strong>
            de vídeo, Sistema de informes de progreso y Sistema de informes de asignación</td>
            <td> </td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td rowspan="8">Commons</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/comments.html" target="_blank"></a> Comentarios y anexos:</strong>
            (Marco de componentes sociales) Como miembro de la comunidad, comparte opiniones y conocimientos sobre el contenido en el sitio de Comunidades.</td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong>Conversión de fragmentos de contenido:</strong>
            Convertir las contribuciones UGC en fragmentos de contenido.</td>
            <td> </td>
            <td> </td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/reviews.html" target="_blank">Reseñas</a>:</strong>
                 (Marco de componentes sociales) Como miembro de la comunidad, revise un contenido mediante una combinación de comentarios y funciones de clasificación.</td>
            <td>bito<sup>+</sup></td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/rating.html" target="_blank">Clasificaciones</a>:/strong&gt; (Marco de componentes sociales) Como miembro de la comunidad califica un fragmento de contenido.</td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/voting.html" target="_blank">Votos</a>:</strong>
                (Marco de componentes sociales) Como miembro de la comunidad votará o rechazará un fragmento de contenido.</td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/tag-ugc.html" target="_blank">Etiquetas</a>:</strong>
            Adjunte etiquetas (palabras clave o etiquetas) con contenido para buscar contenido rápidamente.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/search.html" target="_blank">Búsqueda</a>: búsquedas </strong>
            predictivas y sugerentes.</td>
            <td> </td>
            <td> </td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/translate-ugc.html" target="_blank">Traducción</a>:Traducción </strong>
            automática de contenido generado por el usuario.</td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td rowspan="10">Administración</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/create-site.html" target="_blank">Administración</a> del sitio:</strong>
            Creación de sitios con funciones de comunidades.</td>
            <td> </td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sites.html" target="_blank">Plantillas</a>:Plantillas de </strong>
                <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sites.html" target="_blank"></a> sitio y  <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/tools-groups.html" target="_blank"></a> grupos para crear sitios de comunidad completamente funcionales basados en asistente.</td>
            <td> </td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong>Plantillas editables: </strong>
            habilite a los administradores de la comunidad para crear experiencias enriquecidas con plantillas editables de AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/creating-groups.html" target="_blank">Grupos o subcomunidades</a>: cree </strong>
            dinámicamente subcomunidades dentro de los sitios de las comunidades.
            </td>
            <td> </td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/in-context.html" target="_blank">Moderación</a>:</strong>
            Moderación del contenido generado por el usuario.
            </td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/moderation.html" target="_blank">Moderación</a> masiva:consola </strong>
            de moderación para administrar de forma masiva el contenido generado por el usuario.</td>
            <td> </td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/moderate-ugc.html#CommonModerationConcepts" target="_blank">Filtros</a> de detección de correo no deseado y de rentabilidad: detección </strong>
            automática de spam.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/members.html" target="_blank">Administración</a> de miembros:</strong>
            Administre perfiles de usuarios y grupos desde el área de administración de miembros.</td>
            <td> </td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/overview.html#main-pars_text_866731966" target="_blank">Diseño</a> adaptable: los sitios de </strong>
            AEM Communities responden.
            </td>
            <td> </td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/analytics.html" target="_blank">Analytics</a>:</strong>
            Integración con Adobe Analytics para obtener información clave sobre el uso de sitios de Communities.</td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td rowspan="4">Miembros</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/advanced.html" target="_blank">Puntuación y distintivo</a>:</strong>
             (Puntuación avanzada con tecnología de Adobe Sensei) Identifique a los miembros de la comunidad como expertos y premie a los mismos.</td>
            <td> </td>
            <td> </td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/activities.html" target="_blank"></a> Actividades y  <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/notifications.html" target="_blank">notificaciones</a>: flujo de actividades recientes de </strong>
            Vista y recibir notificaciones sobre eventos de interés.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/configure-messaging.html" target="_blank">Mensajes</a>:mensajes </strong>
            directos a usuarios y grupos.</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/social-login.html" target="_blank">Inicios de sesión</a> sociales:</strong>
            inicie sesión con su cuenta de Facebook o Twitter.</td>
            <td> </td>
            <td> </td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td rowspan="5">Plataforma</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">MSRP (Almacenamiento de Mongo)</a>:el contenido generado por el </strong>
            usuario (UGC) persiste directamente en una instancia de MongoDB local</td>
            <td> </td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">DSRP (Almacenamiento de la base de datos)</a>: el contenido generado por el </strong>
            usuario (UGC) persiste directamente en una instancia de base de datos MySQL local.</td>
            <td> </td>
            <td> </td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">SRP (Almacenamiento de nube)</a>:el contenido generado por el </strong>
                usuario (UGC) se mantiene de forma remota en un servicio en la nube alojado y administrado por Adobe.</td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank"><strong>JSRP</a>:el contenido </strong>
                de la comunidad se almacena en JCR y se puede acceder a él desde la instancia de creación (o publicación) en la que se publicó.</td>
            <td> </td>
            <td> </td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sync.html" target="_blank">Sincronización</a> de usuarios y grupos:</strong>
            Sincronice usuarios y grupos en distintas instancias de publicación al usar una topología de conjunto de publicaciones.</td>
            <td>bito<sup>+</sup></td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
            <td>bito</td>
        </tr>
    </tbody>
</table>

AEM Communities agrega [mejoras](https://helpx.adobe.com/experience-manager/6-5/communities/using/whats-new-aem-communities.html) a través de versiones para permitir que las organizaciones se involucren y habiliten a sus usuarios, mediante:

+ **@** mentionsupport en contenido generado por el usuario.
+ Mejoras de accesibilidad mediante **Navegación por teclado** en los componentes **Habilitación**.
+ Se ha mejorado la **Moderación masiva** mediante **Filtros personalizados**.
+ **Plantillas editables** para permitir a los administradores de la comunidad crear experiencias de comunidad enriquecidas en AEM.
+ Los usuarios ahora pueden enviar **mensajes directos de forma masiva** a todos los miembros de un grupo.
