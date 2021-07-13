---
title: Comprender las razones para actualizar
description: Un desglose de alto nivel de las funciones clave para los clientes que se plantean actualizar a la última versión de Adobe Experience Manager.
version: 6.5
sub-product: recursos, cloud manager, comercio, servicios de contenido, Dynamic Media, formularios, fundación, pantallas, sitios
topics: best-practices, upgrade
audience: all
activity: understand
doc-type: article
topic: Actualización
role: Leader, Architect, Developer, Admin, User
level: Beginner
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '3541'
ht-degree: 3%

---


# Explicación de las razones para la actualización

Un desglose de alto nivel de las funciones clave para los clientes que se plantean actualizar a la última versión de Adobe Experience Manager.

## Funciones principales para la actualización a AEM 6.5

+ [Notas de la versión de Adobe Experience Manager 6.5](https://helpx.adobe.com/es/experience-manager/6-5/release-notes.html)

### Mejoras en la base

Adobe Experience Manager 6.5 sigue mejorando la estabilidad, el rendimiento y la compatibilidad del sistema mediante:

+ **Compatibilidad con Java 11**  (sin dejar de mantener la compatibilidad con Java 8).

### Creación y administración de sitios web

AEM Sites presenta una serie de funciones diseñadas para acelerar la creación y la creación de sitios web:

+ **SPA** Editorsupport permite que SPA (aplicaciones de una sola página) se autoricen completamente en AEM, lo que ofrece una experiencia de creación enriquecida y compatible con el mercado.
+_ **El SDK de JavaScript**, un Kit de inicio de proyecto SPA y herramientas de compilación compatibles, permiten a los desarrolladores de front-end desarrollar aplicaciones de página única compatibles con SPA Editor independientemente de AEM.
+ **Los** componentes principales añaden una multitud de componentes nuevos, una  **biblioteca de** componentes y diversas mejoras a los componentes principales existentes.
+ Más **Translations** mejoras simplifican la traducción de AEM Sites.

### Experiencias fluidas

AEM sigue adoptando Experiencias fluidas con herramientas nuevas y mejoradas que facilitan el uso de contenido fuera de AEM.

+ **Los** fragmentos de contenido admiten la comparación/diferencia de versiones y las anotaciones.
+ **AEM** API HTTP de Assets admite la exposición de  **fragmentos de** contenido directamente en DAM como  **JSON**.
   **Los** fragmentos de experiencia admiten la  **búsqueda** de texto completo y la  **AEM** invalidación de caché de Dispatcher para hacer referencia a las  **páginas**.

### Administración de activos

AEM Assets continúa aprovechando su completo conjunto de funcionalidades de administración de activos para mejorar el uso, la administración y la comprensión del DAM. AEM 6.5 sigue mejorando la integración entre Adobe Creative Cloud y los flujos de trabajo creativos.

+ **Adobe Asset** Linked conecta los elementos creativos directamente a AEM Assets desde las herramientas de Adobe Creative Cloud.
+ **Adobe** Stockintegration permite el acceso directo a las imágenes de Adobe Stock directamente desde la experiencia de AEM Assets, lo que crea una experiencia perfecta de detección de contenido.
+ **AEM Desktop** Appreleases versión 2.0 y reorganización a su vez, mejorando el rendimiento y la estabilidad.
+ **Los** recursos conectados admiten instancias discretas de AEM Sites para acceder y utilizar recursos sin problemas desde una instancia de AEM Assets diferente.
+ Se ha actualizado la compatibilidad con vídeo en **Dynamic Media**, que incluye **360 Video** y **Miniaturas de vídeo personalizadas**.

### Inteligencia de contenido

AEM continúa desarrollando su integración con tecnologías inteligentes, aprovechando el aprendizaje automático y la inteligencia artificial para mejorar todas las experiencias.

+ **Los** vínculos de recursos de Adobe  **añaden la búsqueda** de similitud visual, que permite descubrir y utilizar fácilmente imágenes similares en las herramientas **de** Adobe Creative Cloud.

### Integraciones

AEM aumenta su capacidad de integración con otros servicios de Adobe:

+ **Los** fragmentos de experiencias profundizan su integración con  **Adobe** Target, ya que admiten la  **exportación como** JSONa Adobe Target y la capacidad de  **eliminar** ofertas basadas en fragmentos de experiencias de  **Adobe Target**.

### AMS Cloud Manager

[Cloud Manager](https://adobe.ly/2HODmsv), exclusivo para los clientes de Adobe Managed Services (AMS), ofrece las siguientes funciones:

+ Cloud Manager admite la ampliación AEM compatibilidad con la implementación de AEM Sites a **AEM Assets**, incluidas las **pruebas de rendimiento automatizadas del procesamiento de recursos**.
+ **La** ampliación automática del nivel de AEM Publish a umbrales predefinidos garantiza una experiencia óptima para el usuario final.
+ **Las** canalizaciones que no son de producción permiten a los equipos de desarrollo aprovechar Cloud Manager para comprobar continuamente la calidad del código e implementarlo en entornos más bajos (desarrollo y control de calidad).
+ **Las** API de canalización de CI/CD permiten a los clientes interactuar mediante programación con Cloud Manager, lo que profundiza las posibilidades de integración con la infraestructura de desarrollo local.

## Funciones básicas

A continuación se muestra una matriz de funciones básicas clave que ofrece AEM. Algunas de estas funciones se introdujeron en versiones anteriores con mejoras incrementales añadidas en cada versión.

+ [Notas de la versión de AEM Foundation](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html)

***✔ <sup>+</sup> mejoras significativas en la función de esta versión.***

***✔ <sup></sup> SPdenota que la función está disponible a través de un Service Pack o Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Función de base</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6,1</td>
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
            <td>š</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://jackrabbit.apache.org/oak/docs/index.html" target="_blank">Repositorio de contenido Oak</a>: </strong> Proporciona un rendimiento y escalabilidad buenos y el predecesor Jackrabbit 2.</td>
            <td> </td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/indexing-via-the-oak-run-jar.html">compatibilidad con índices oak-run.jar</a>: </strong> reindexación mejorada, recopilación de estadísticas y verificación de consistencia de los índices Oak.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/queries-and-indexing.html" target="_blank">Índices</a> de búsqueda personalizados:  </strong>
                Posibilidad de agregar definiciones de índice personalizadas para optimizar el rendimiento de la consulta y la relevancia de la búsqueda.</td>
            <td> </td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/es/experience-manager/6-5/sites/deploying/using/revision-cleanup.html" target="_blank">Limpieza de revisiones en línea</a>:</strong>
                realice el mantenimiento del repositorio sin downtime del servidor.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/storage-elements-in-aem-6.html" target="_blank">Almacenamiento</a> de repositorios TarMK o MongoMK:</strong>
                <br> Opciones para utilizar almacenamiento de TarMK simple y con rendimiento basado en archivos (versión de próxima generación de TarPM) 
                <br> o escalar horizontalmente con un repositorio respaldado por MongoDB con MongoMK.</td>
            <td> </td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/aem-with-mongodb.html" target="_blank">Rendimiento y estabilidad</a> de MongoMK:</strong>
            Se han realizado mejoras continuas en MongoMK desde su introducción con AEM 6.0.</td>
            <td> </td>
            <td> </td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/data-store-config.html#AmazonS3DataStore">Amazon S3 DataStore</a>: </strong>
            aproveche la solución de almacenamiento en la nube ampliable para almacenar recursos binarios.</td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong>Paridad de funciones en la interfaz de usuario táctil: </strong>
                mejoras continuas en la IU de creación para aumentar la velocidad con el aumento de la productividad y la paridad de características con la IU clásica.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong>Omnisearch: </strong>
                busque y navegue AEM rápidamente.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/operations-dashboard.html" target="_blank">Tablero de operaciones</a>: </strong>
 realice tareas de mantenimiento, supervise el estado del servidor y analice el rendimiento desde AEM.</td>
            <td></td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/upgrade.html" target="_blank">Mejoras en la actualización</a>:</strong>
             Las mejoras en la actualización permiten realizar actualizaciones de AEM en el sitio de forma más sencilla y rápida.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/es/experience-manager/htl/using/overview.html" target="_blank">Lenguaje de plantilla HTL</a>:</strong>
            Motor de plantilla moderno que separa la presentación de la lógica. Reduce considerablemente el tiempo de desarrollo de los componentes. Funciones incrementales agregadas con cada versión.</td>
            <td> </td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://sling.apache.org/documentation/bundles/models.html" target="_blank">Modelos</a> de Sling:</strong>
             un marco flexible para modelar recursos JCR en objetos y lógica empresariales. Funciones incrementales agregadas con cada versión.
            </td>
            <td> </td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://adobe.ly/2HODmsv" target="_blank">Cloud Manager</a>:  </strong>
                Exclusivo para los clientes de Adobe Managed Services (AMS), Cloud Manager acelera el desarrollo y la implementación mediante una canalización de CD/CI de última generación.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
        </tr>
    </tbody>
</table>

## Funciones de seguridad

A continuación se muestra una matriz de funciones de seguridad clave que ofrece AEM. Algunas de estas funciones se introdujeron en versiones anteriores con mejoras incrementales añadidas en cada versión.

+ [Notas de la versión de seguridad](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html#Security)

***š indica que se han realizado mejoras significativas en la función de esta versión.***

***✔ <sup>+</sup>  indica que la función está disponible a través de un Service Pack o Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Función de seguridad</td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6,1</td>
            <td>6,2</td>
            <td>6,3</td>
            <td>6,4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html" target="_blank">Usuarios de servicio</a></strong>
            <br> Compartimenta los permisos para evitar el uso innecesario de privilegios de administrador.</td>
        <td></td>
        <td>š</td>
        <td>š</td>
        <td>š</td>
        <td>š</td>
        <td>š</td>
        <td>š</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/es/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank">Administración </a></strong>
            <br> de almacén de clavesAlmacén de confianza global, certificados y claves, todos administrados dentro del repositorio.</td>
        <td></td>
        <td>š</td>
        <td>š</td>
        <td>š</td>
        <td>š</td>
        <td>š</td>
        <td>š</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/csrf-protection.html" target="_blank"><strong></strong> <strong></strong></a>
            <br> CSRFprotectionProtección contra falsificaciones de solicitudes entre sitios fuera de la caja.</td>
        <td></td>
        <td></td>
        <td>š</td>
        <td>š</td>
        <td>š</td>
        <td>š</td>
        <td>š</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank"><strong></strong> <strong></strong></a>
            <br> CORSsupportCross-Origin Resource Sharing es compatible con la buena flexibilidad de las aplicaciones.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>š</td>
        <td>š</td>
        <td>š</td>
    </tr>
    <tr>
        <td><strong><a href="https://docs.adobe.com/docs/en/aem/6-5/administer/security/saml-2-0-authenticationhandler.html" target="_blank">Compatibilidad con autenticación SAML mejoradaSe han resuelto </a><br>
 </strong>problemas de redireccionamiento SAML, información de grupo optimizada y cifrado clave mejorados. 
            <br>
        </td>
        <td></td>
        <td></td>
        <td>š</td>
        <td>š</td>
        <td>š</td>
        <td>š</td>
        <td>š</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html" target="_blank">LDAP como </a><br>
 </strong>configuración OSGi Simplifica la administración y las actualizaciones de la autenticación LDAP.</td>
        <td></td>
        <td>š</td>
        <td>š</td>
        <td>š</td>
        <td>š</td>
        <td>š</td>
        <td>š</td>
    </tr>
    <tr>
        <td><strong>La compatibilidad con el cifrado OSGi para <br>
 </strong>contraseñas de texto sin formatoLas contraseñas y otros valores confidenciales se pueden guardar de forma cifrada y descifrar automáticamente.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>š</td>
        <td>š</td>
        <td>š</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/user-group-ac-admin.html" target="_blank">Mejoras del CUG</a><br>
 </strong>La implementación del grupo cerrado de usuarios se ha reescrito para abordar los problemas de rendimiento y escalabilidad.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>š</td>
        <td>✓<sup>+</sup></td>
        <td>š</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/platform-repository/using/ssl-wizard-technical-video-use.html" target="_blank">Interfaz de usuario del </a></strong>
            <br> Asistente SSL para simplificar la configuración y administración de SSL.</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>š</td>
        <td>š</td>
        <td>š</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/encapsulated-token.html" target="_blank">Compatibilidad con </a></strong>
            <br> tokens encapsuladosYa no es necesario en sesiones "duraderas" para admitir la autenticación horizontal en instancias de publicación.</td>
        <td> </td>
        <td> </td>
        <td>š</td>
        <td>š</td>
        <td>š</td>
        <td>š</td>
        <td>š</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ims-config-and-admin-console.html" target="_blank">Compatibilidad con la autenticación IMS de AdobeExclusivo para Adobe Managed Services (AMS), administre de forma centralizada el acceso a las instancias de AEM Author a través de Adobe IMS (Identity Management System). </a><br>
 </strong></td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>š</td>
        <td>š</td>
    </tr>
</tbody>
</table>

## Características de Sites

A continuación se muestra una matriz de las funciones clave de Sites que ofrece AEM. Algunas de estas funciones se introdujeron en versiones anteriores con mejoras incrementales añadidas en cada versión.

+ [Notas de la versión de AEM Sites](https://helpx.adobe.com/experience-manager/6-5/release-notes/sites.html)

***✔ <sup>+</sup> mejoras significativas en la función de esta versión.***

***✔ <sup></sup> SPdenota que la función está disponible a través de un Service Pack o Feature Pack.***

<table>
    <thead>
        <tr>
            <td><strong>Función Sitios</strong></td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6,1</td>
            <td>6,2</td>
            <td>6,3</td>
            <td>6,4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/page-editor-feature-video-use.html" target="_blank">Creación de páginas optimizada táctil</a>:</strong>
            permite a los editores aprovechar tabletas y equipos con pantallas táctiles.</td>
            <td></td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/responsive-layout.html" target="_blank">Creación interactiva de sitios</a>:</strong>
                el modo de diseño permite a los editores cambiar el tamaño de los componentes en función de la anchura del dispositivo para los sitios adaptables.</td>
            <td></td>
            <td></td>
            <td>š</td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/template-editor-feature-video-use.html" target="_blank">Plantillas editables</a>: </strong>
            permite que los autores especializados creen y editen plantillas de página.</td>
            <td></td>
            <td></td>
            <td></td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/core-components/user-guide.html" target="_blank">Componentes principales</a>: </strong>
            acelere el desarrollo del sitio. Disponible en GitHub para programación y flexibilidad frecuentes de versiones.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>✓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/spa-overview.html" target="_blank">SPA Editor</a>: </strong>
            cree experiencias web fiables y activas mediante marcos de aplicación de una sola página (SPA) basados en React o Angular.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✓<sup>+</sup></td>
            <td>✓<sup>+</sup></td>
            <td>✓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/es/experience-manager/6-5/release-notes/style-system-fp.html" target="_blank">Sistema de estilos</a>: </strong>
            aumente AEM reutilización de componentes definiendo su aspecto visual con el sistema de estilos en contexto.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✓<sup>SP</sup></td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/es-ES/experience-manager/6-5/sites/administering/using/msm.html" target="_blank">Multi-Site Manager (MSM)</a>: </strong>
            administre varios sitios web que compartan contenido común (es decir, varias marcas multilingües).</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/translation.html" target="_blank">Traducción de contenido</a>:</strong>
             El marco de Plug and Play se integra con los servicios de traducción de terceros líderes en el sector.</td>
            <td></td>
            <td></td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>✓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/contexthub.html" target="_blank">ContextHub</a>:</strong>
             marco de contexto del cliente de próxima generación para la personalización del contenido.</td>
            <td></td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/launches.html" target="_blank">Lanzamientos</a>:</strong>
             Desarrolle contenido para una versión futura sin interrumpir la creación diaria.</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/content-fragments-feature-video-understand.html" target="_blank">Fragmentos de contenido</a>: </strong>
            cree y depure contenido editorial desacoplado de la presentación para facilitar su reutilización.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>✓<sup>+</sup></td>
            <td>✓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-use.html" target="_blank">Fragmentos de experiencia</a>: </strong>
            cree experiencias y variaciones reutilizables optimizadas para canales de escritorio, móviles y sociales.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>✓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/release-notes/content-services-fragments-featurepack.html" target="_blank">Servicios de contenido</a>: </strong>
            exporte contenido desde AEM como JSON para consumo en dispositivos y aplicaciones.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✓<sup>SP</sup></td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong>Adobe Analytics Integration and Content Insights: </strong>
                integración sencilla de Adobe Analytics y DTM. Muestre información de rendimiento en el entorno de creación.</td>
            <td> </td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/content-targeting-touch.html" target="_blank">Integración de Adobe Target</a>: </strong>
            asistente paso a paso para crear experiencias segmentadas y bibliotecas de ofertas reutilizables.</td>
            <td> </td>
            <td> </td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/campaign.html" target="_blank">Integración de Adobe Campaign</a>:</strong>
            integración sencilla con la solución de campañas de correo electrónico de próxima generación.</td>
            <td> </td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/using/aem_launch_adobeio_integration.html" target="_blank">Integración de Adobe Launch</a>:</strong>
            Integre con el servicio en la nube de administración de etiquetas de próxima generación de Adobe.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/aem-screens-introduction.html" target="_blank">Pantallas</a>: </strong>
            administre experiencias para señalización digital y quioscos.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>š</td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>✓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ecommerce.html" target="_blank">Comercio electrónico</a>: </strong>
            ofrezca experiencias de compra personalizadas con marca en todos los puntos de contacto web, móviles y sociales.
            </td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>✓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/overview.html" target="_blank">Comunidades</a>: </strong>
            los foros, los comentarios en subprocesos, los calendarios de eventos y muchas otras funciones permiten una participación profunda con los visitantes del sitio.</td>
            <td>š</td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
        </tr>
    </tbody>
</table>

## Funciones de recursos

A continuación se muestra una matriz de las funciones clave de Assets que ofrece AEM. Algunas de estas funciones se introdujeron en versiones anteriores con mejoras incrementales añadidas en cada versión.

+ [Notas de la versión de AEM Assets](https://helpx.adobe.com/experience-manager/6-5/release-notes/assets.html)

***š indica que se han realizado mejoras significativas en la función de esta versión.***

***✔ <sup>+</sup>  indica que la función está disponible a través de un Service Pack o Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Función de recursos</td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6,1</td>
            <td>6,2</td>
            <td>6,3</td>
            <td>6,4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets-touch-ui.html" target="_blank">IU táctil optimizada</a>:</strong>
            Administre recursos en un equipo de escritorio o en dispositivos con capacidad táctil.</td>
            <td> </td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>✓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/metadata.html" target="_blank">Administración avanzada de metadatos</a>: </strong>
            plantillas de metadatos, editor de esquemas de metadatos y edición masiva de metadatos.</td>
            <td> </td>
            <td>š</td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>✓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/task-content.html" target="_blank"></a> Administración de  <a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects-with-workflows.html" target="_blank"></a> tareas y flujos de trabajo:</strong>
            flujos de trabajo y tareas pregenerados para la revisión y aprobación de recursos digitales aprovechando AEM proyectos.</td>
            <td> </td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong>Escalabilidad y rendimiento:</strong>
            Soporte mejorado para ingesta, carga y almacenamiento a escala.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>š</td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mac-api-assets.html" target="_blank">API</a> HTTP de recursos:</strong>
             Interactúe mediante programación con los recursos a través de HTTP y JSON.</td>
            <td> </td>
            <td> </td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/link-sharing.html" target="_blank">Uso compartido de vínculos</a>: </strong>
            uso compartido simple y específico de recursos digitales sin necesidad de iniciar sesión.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal.html" target="_blank">Brand Portal</a>:</strong>
            solución SAAS de Cloud service para un uso compartido y distribución sin problemas de recursos digitales.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/use-assets-across-connected-assets-instances.html" target="_blank">Recursos conectados</a>:</strong>
            las instancias de AEM Sites pueden acceder y utilizar recursos sin problemas desde una instancia de AEM Assets diferente.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/touch-ui-asset-insights.html" target="_blank">Asset Insights</a>: </strong>
            aproveche Adobe Analytics para capturar la interacción de los clientes con los recursos digitales y visualizarlos en AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/multilingual-assets.html" target="_blank">Recursos multilingües</a>:</strong>
            Compatibilidad de traducción de metadatos de recursos automáticamente con raíces de idioma.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/enhanced-smart-tags.html" target="_blank">Etiquetas inteligentes y moderación</a>: </strong>
            aproveche Adobe Sensei para etiquetar imágenes automáticamente con metadatos útiles.</td>
            <td> </td>
            <td></td>
            <td> </td>
            <td> </td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/assets/using/smart-translation-search-feature-video-use.html" target="_blank">Búsqueda de traducción inteligente</a>: </strong>
            traduce automáticamente los términos de búsqueda al buscar AEM Assets.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/indesign.html" target="_blank">Integración de Adobe InDesign Server</a>: </strong>
            genere catálogos de productos. Cree folletos, volantes e imprima anuncios basados en plantillas de InDesign.</td>
            <td> </td>
            <td> </td>
            <td>š</td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-desktop-app/using/using.html?lang=es" target="_blank">AEM aplicación de escritorio</a>: </strong>
            sincronice los recursos con el escritorio local para editarlos con productos de Creative Suite.
            </td>
            <td> </td>
            <td> </td>
            <td>š</td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/imaging-transcoding-library.html" target="_blank">Biblioteca de imágenes de Adobe</a>:</strong>
                <br> bibliotecas PDF de Photoshop y Acrobat utilizadas para la manipulación de archivos de alta calidad.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://www.adobe.com/creativecloud/business/enterprise/adobe-asset-link.html" target="_blank">Adobe Asset Link</a>: </strong>
            Acceda a AEM Assets directamente desde las aplicaciones de Adobe Create Cloud.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/aem-assets-adobe-stock.html" target="_blank">Integración de Adobe Stock</a>: </strong>
            acceda y use sin problemas imágenes de Adobe Stock directamente desde AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✓<sup>SP</sup></td>
            <td>š</td>
        </tr>
    </tbody>
</table>

### AEM Assets Dynamic Media

***✔ <sup>+</sup> mejoras significativas en la función de esta versión.***

***✔ <sup></sup> SPdenota que la función está disponible a través de un Service Pack o Feature Pack.***


<table>
    <thead>
        <tr>
            <td>Función Dynamic Media</td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6,1</td>
            <td>6,2</td>
            <td>6,3 +FP</td>
            <td>6,4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets.html" target="_blank">Imágenes</a>:</strong>
            distribuya de forma dinámica imágenes en diferentes tamaños y formatos, incluido el Recorte inteligente.</td>
            <td> </td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/video-profiles.html" target="_blank">Vídeo</a>: </strong>
            Codificación de vídeo avanzada y flujo de vídeo adaptable</td>
            <td> </td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>✓<sup>+</sup></td>
            <td>✓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/interactive-images.html" target="_blank">Medios interactivos</a>: </strong>
            cree letreros interactivos y vídeos con contenido en el que se pueda hacer clic para mostrar ofertas clave.
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>š</td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong>Conjuntos (<a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/image-sets.html" target="_blank">Imagen</a>,  <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/spin-sets.html" target="_blank">Giro</a>,  <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mixed-media-sets.html" target="_blank">Medios mixtos</a>): </strong>
            permite a los usuarios ampliar, reducir, rotar y simular una experiencia de visualización de 360 grados.</td>
            <td> </td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://docs.adobe.com/docs/es-ES/aem/6-5/administer/content/dynamic-media/viewer-presets.html" target="_blank">Visores</a>:</strong>
            reproductores de medios enriquecidos con marca personalizada y ajustes preestablecidos compatibles con distintas pantallas/dispositivos.</td>
            <td> </td>
            <td>š</td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/delivering-dynamic-media-assets.html" target="_blank">Envío</a>:</strong>
            opciones flexibles para vincular o integrar contenido de Dynamic Media y envío a través del protocolo HTTP/2.</td>
            <td> </td>
            <td>š</td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong>Actualización de Scene7 a Dynamic Media:</strong>
            Capacidad para migrar recursos maestros y seguir utilizando las direcciones URL de S7 existentes.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
        </tr>
    </tbody>
</table>

## Funciones de Forms

A continuación se muestra una matriz de las funciones clave del complemento de AEM Forms que ofrece AEM. Algunas de estas funciones se introdujeron en versiones anteriores con mejoras incrementales añadidas en cada versión.

+ [Notas de la versión de AEM Forms](https://helpx.adobe.com/experience-manager/6-5/release-notes/forms.html)

***✔ <sup>+</sup> mejoras significativas en la función de esta versión.***

***✔ <sup></sup> SPdenota que la función está disponible a través de un Service Pack o Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Función Forms</td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6,1</td>
            <td>6,2</td>
            <td>6,3</td>
            <td>6,4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/introduction-forms-authoring.html" target="_blank">Editor de Forms adaptable</a>: </strong>
            cree formularios interactivos, adaptables y adaptables en función de la configuración del dispositivo y el explorador.</td>
            <td> </td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>✓<sup>+</sup></td>
            <td>✓<sup>+</sup></td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/generate-document-of-record-for-non-xfa-based-adaptive-forms.html" target="_blank">Documento de registro</a>: </strong>
            cree un documento para garantizar el almacenamiento a largo plazo de una experiencia de captura de datos o de una versión lista para imprimir.</td>
            <td> </td>
            <td>š</td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/themes.html" target="_blank">Editor de temas</a>: </strong>
            cree temas reutilizables para aplicar estilo a componentes y paneles de un formulario.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>š</td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/template-editor.html" target="_blank">Editor de plantillas</a>: </strong>
            estandarice e implemente las prácticas recomendadas para formularios adaptables.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>š</td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedintegrationwithAdobeSign" target="_blank">Integración de Adobe Sign</a>: </strong>
            permite la implementación de escenarios de firma basados en formularios integrados de Adobe Sign.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/cm-overview.html" target="_blank">Gestión de Correspondencia</a>:</strong>
            con AEM Forms, puede crear, administrar y entregar correspondencia personalizada e interactiva con los clientes.
            </td>
            <td> </td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#AEMFormsdataintegration" target="_blank">Integración de datos de terceros</a>: </strong>
            al utilizar la integración de datos, los datos se recuperan de fuentes de datos dispares basadas en entradas del usuario en un formulario. Al enviar el formulario, los datos capturados se vuelven a escribir en los orígenes de datos.
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>✓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#FormscentricAEMWorkflowsforAEMFormsonOSGi" target="_blank">Flujo de trabajo (en OSGi) para procesamiento</a> de Forms: </strong>
            implementación simplificada de procesos de aprobación de formularios.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>✓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/user-guide.html?topic=/experience-manager/6-5/forms/morehelp/integrations.ug.js" target="_blank">Integración con Marketing Cloud</a>:</strong>
             Integración con Adobe Analytics y Adobe Target para mejorar y medir las experiencias de los clientes.</td>
            <td> </td>
            <td> </td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/introduction-managing-forms.html" target="_blank">Administrador de formularios</a>:</strong>
            Ubicación única para administrar todos los formularios, documentos o correspondencia, como habilitar análisis, traducción, pruebas A/B, revisiones y publicaciones.
            </td>
            <td> </td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/aem-forms-app.html" target="_blank">Aplicación de AEM Forms</a>: </strong>
            permite el procesamiento de formularios en línea o sin conexión dentro de una aplicación en iOS, Android o Windows.</td>
            <td> </td>
            <td>š</td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/adaptive-document.html" target="_blank">Comunicaciones interactivas</a>: </strong>
            cree comunicaciones enriquecidas, como instrucciones dirigidas, con elementos interactivos como gráficos (anteriormente conocidos como Documentos adaptables).</td>
            <td> </td>
            <td> </td>
            <td>š</td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>✓<sup>+</sup></td>
            <td>✓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/pdf/aem-forms/6-5/WorkbenchHelp.pdf" target="_blank">Flujo de trabajo (J2EE) para procesamiento</a> de Forms: </strong>
            genere formularios complejos/flujos de trabajo centrados en documentos utilizando un IDE intuitivo.</td>
            <td></td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedDocumentSecurity" target="_blank">AEM Forms Document Security</a>:</strong>
            Acceso seguro y autorización de documentos PDF y Office.
            </td>
            <td> </td>
            <td>š</td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#Simplifiedauthoringexperience" target="_blank">Marcos de prueba</a>: </strong>
            utilice el marco de Calvin y el complemento de Chrome para admitir y depurar formularios adaptables.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
        </tr>
    </tbody>
</table>

## Funciones de Communities

A continuación se muestra una matriz de las funciones clave del complemento de AEM Communities que ofrece AEM. Algunas de estas funciones se introdujeron en versiones anteriores con mejoras incrementales añadidas en cada versión.

+ [Resumen de las nuevas funciones de AEM Communities](https://helpx.adobe.com/experience-manager/6-5/communities/using/whats-new-aem-communities.html#main-pars_text)

***✔ <sup>+</sup> mejoras significativas en la función de esta versión.***

***✔ <sup></sup> SPdenota que la función está disponible a través de un Service Pack o Feature Pack.***

<table>
    <thead>
        <tr>
            <td> </td>
            <td>Función de Communities</td>
            <td>6,0</td>
            <td>6,1</td>
            <td>6,2</td>
            <td>6,3</td>
            <td>6,4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan="7">Funciones de Communities</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/forum.html" target="_blank">Foros</a>: </strong> (Marco de componentes de Social) cree nuevos temas o vea, siga, busque y mueva los temas existentes.</td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td>
                <p><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-qna.html" target="_blank">Preguntas</a>: </strong>
                Formule, vea y responda preguntas.</p>
            </td>
            <td></td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/blog-feature.html" target="_blank">Blogs</a>: </strong>
                cree artículos y comentarios en el blog del lado de publicación.
            </td>
            <td> </td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/ideation-feature.html" target="_blank">Ideación</a>: </strong>
                cree y comparta ideas con la comunidad, o vea, siga y comente las ideas existentes.
            </td>
            <td> </td>
            <td> </td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/calendar.html" target="_blank">Calendario</a>:</strong>
                 (Marco de componentes sociales) Proporcione información sobre eventos de la comunidad a los visitantes del sitio.
            </td>
            <td>✓<sup>+</sup></td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/file-library.html" target="_blank">Biblioteca de archivos</a>: </strong>
                cargue, administre y descargue archivos dentro del sitio de la comunidad.</td>
            <td> </td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/users.html#AboutCommunityGroups" target="_blank">Grupos</a> de usuarios: 
            </strong>Un conjunto de usuarios puede pertenecer a grupos de miembros y se les pueden asignar funciones colectivamente.</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong> </strong></td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resource.html" target="_blank">Asignación</a>: </strong>
            cree y asigne recursos de aprendizaje a los miembros de la comunidad.</td>
            <td> </td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td rowspan="5">Habilitación</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/catalog.html" target="_blank"></a> Administración de  <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resource.html" target="_blank">recursos y catálogos</a>:</strong>
             Acceda a los recursos de habilitación desde el catálogo.</td>
            <td> </td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resources.html#CreateaLearningPath" target="_blank">Administración de rutas de aprendizaje</a>:</strong>
            administre cursos o grupos de recursos de habilitación.</td>
            <td> </td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/reports.html#main-pars_text_1739724213" target="_blank">Informes de habilitación</a>: </strong>
            informes sobre recursos de habilitación y rutas de aprendizaje.</td>
            <td> </td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resources.html#main-pars_text_899882038" target="_blank">Participación en la habilitación</a>: </strong>
            agregue comentarios sobre los recursos de habilitación.</td>
            <td> </td>
            <td> </td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/analytics.html" target="_blank">Análisis de habilitación</a>: </strong>
            Análisis de vídeo, Informes de progreso e Informes de asignación</td>
            <td> </td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td rowspan="8">Commons</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/comments.html" target="_blank"></a> Comentarios y archivos adjuntos:</strong>
             (Marco de componentes sociales) Como miembro de la comunidad, comparte opiniones y conocimientos sobre el contenido del sitio de Communities.</td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong>Conversión de fragmentos de contenido: </strong>
            convierta las contribuciones UGC en fragmentos de contenido.</td>
            <td> </td>
            <td> </td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/reviews.html" target="_blank">Reseñas</a>:</strong>
                 (Marco de componentes sociales) Como miembro de la comunidad, revise un contenido usando una combinación de comentarios y funciones de clasificación.</td>
            <td>✓<sup>+</sup></td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/rating.html" target="_blank">Clasificaciones</a>:/strong&gt; (Marco de componentes sociales) Como miembro de la comunidad, clasifica un fragmento de contenido.</td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/voting.html" target="_blank">Votos</a>:</strong>
                 (Marco de componentes sociales) Como miembro de la comunidad, voten a favor o en contra de un contenido.</td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/tag-ugc.html" target="_blank">Etiquetas</a>: </strong>
            adjunte etiquetas (palabras clave o etiquetas) al contenido para localizar rápidamente el contenido.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/search.html" target="_blank">Búsqueda</a>:</strong>
            búsquedas predictivas y sugerentes.</td>
            <td> </td>
            <td> </td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/translate-ugc.html" target="_blank">Traducción</a>:</strong>
             traducción automática de contenido generado por el usuario.</td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td rowspan="10">Administración</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/create-site.html" target="_blank">Administración de sitios</a>:</strong>
            creación de sitios con funciones de comunidades.</td>
            <td> </td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sites.html" target="_blank">Plantillas</a>:</strong>
                <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sites.html" target="_blank"></a> Plantillas de  <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/tools-groups.html" target="_blank"></a> grupo y sitio para la creación basada en asistente de sitios comunitarios completamente funcionales.</td>
            <td> </td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong>Plantillas editables: </strong>
            habilite a los administradores de la comunidad para crear experiencias enriquecidas con plantillas editables de AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/creating-groups.html" target="_blank">Grupos o subcomunidades</a>: </strong>
            cree de forma dinámica subcomunidades dentro de los sitios de las comunidades.
            </td>
            <td> </td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/in-context.html" target="_blank">Moderación</a>:</strong>
             moderación del contenido generado por el usuario.
            </td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/moderation.html" target="_blank">Moderación masiva</a>:</strong>
            consola de moderación para administrar de forma masiva el contenido generado por el usuario.</td>
            <td> </td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/moderate-ugc.html#CommonModerationConcepts" target="_blank">Detección de correo no deseado y filtros</a> de lenguaje:</strong>
            detección automática de correo no deseado.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/members.html" target="_blank">Administración de miembros</a>: </strong>
            administre perfiles y grupos de usuarios desde el área de administración de miembros.</td>
            <td> </td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/overview.html#main-pars_text_866731966" target="_blank">Diseño</a> interactivo:</strong>
             los sitios de AEM Communities responden.
            </td>
            <td> </td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/analytics.html" target="_blank">Analytics</a>:</strong>
            Integre con Adobe Analytics para obtener información clave sobre el uso de los sitios de Communities.</td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td rowspan="4">Miembros</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/advanced.html" target="_blank">Puntuación y distintivo</a>:</strong>
             (Puntuación avanzada con tecnología de Adobe Sensei) Identifique a los miembros de la comunidad como expertos y premie a estos.</td>
            <td> </td>
            <td> </td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/activities.html" target="_blank"></a> Actividades y  <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/notifications.html" target="_blank">notificaciones</a>:</strong>
            vea el flujo de actividades recientes y obtenga notificaciones sobre eventos de interés.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/configure-messaging.html" target="_blank">Mensajes</a>:</strong>
            mensajería directa a usuarios y grupos.</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>✓<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/social-login.html" target="_blank">Inicios de sesión de Social</a>: </strong>
            inicie sesión con su cuenta de Facebook o Twitter.</td>
            <td> </td>
            <td> </td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td rowspan="5">Plataforma</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">MSRP (Mongo Storage)</a>:</strong>
            El contenido generado por el usuario (UGC) se mantiene directamente en una instancia local de MongoDB</td>
            <td> </td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">DSRP (Database Storage)</a>: </strong>
            el contenido generado por el usuario (UGC) se mantiene directamente en una instancia local de la base de datos MySQL.</td>
            <td> </td>
            <td> </td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">SRP (Cloud Storage)</a>:</strong>
                El contenido generado por el usuario (UGC) se mantiene de forma remota en un servicio en la nube alojado y administrado por Adobe.</td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank"><strong>JSRP</a>:</strong>
                el contenido de la comunidad se almacena en JCR y se puede acceder a UGC desde la instancia de autor (o publicación) en la que se publicó.</td>
            <td> </td>
            <td> </td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sync.html" target="_blank">Sincronización de usuarios y grupos</a>: </strong>
            sincronice usuarios y grupos en todas las instancias de publicación al usar una topología de granja de publicaciones.</td>
            <td>✓<sup>+</sup></td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
        </tr>
    </tbody>
</table>

AEM Communities agrega [mejoras](https://helpx.adobe.com/experience-manager/6-5/communities/using/whats-new-aem-communities.html) mediante versiones que permiten a las organizaciones interactuar y habilitar a sus usuarios mediante:

+ **@** mentionsupport en contenido generado por el usuario.
+ Mejoras de accesibilidad mediante **Navegación por teclado** en los componentes **Habilitación**.
+ Se ha mejorado la **Moderación masiva** mediante **Filtros personalizados**.
+ **Plantillas editables** para que los administradores de la comunidad puedan crear experiencias de comunidad enriquecidas en AEM.
+ Los usuarios ahora pueden enviar **mensajes directos de forma masiva** a todos los miembros de un grupo.
