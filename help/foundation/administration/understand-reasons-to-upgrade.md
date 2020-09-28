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

+ **Compatibilidad con Java 11** (al mismo tiempo que se mantiene la compatibilidad con Java 8).

### Creación y administración de sitios Web

AEM Sites presenta una serie de funciones diseñadas para acelerar la creación y la creación de sitios web:

+ **La compatibilidad con el Editor** de SPA permite que SPA (aplicaciones de una sola página) se cree completamente en AEM, lo que permite una experiencia de creación rica y compatible con el mercado.
+_ **JavaScript SDK**, un kit de Inicio de proyectos de SPA y herramientas de compilación compatibles, permiten a los desarrolladores de front-end desarrollar aplicaciones de página única compatibles con el Editor de SPA independientemente de AEM.
+ **Componentes** principales agrega una gran cantidad de componentes nuevos, una biblioteca **de** componentes y diversas mejoras a los componentes principales existentes.
+ Las mejoras adicionales en **traducciones** optimizan la traducción de AEM Sites.

### Experiencias fluidas

AEM continúa adoptando Experiencias Fluidas con herramientas nuevas y mejoradas que facilitan el uso de contenido fuera de AEM.

+ **Los fragmentos** de contenido admiten Comparación/Diferencia de versiones y Anotaciones.
+ **La API** HTTP de AEM Assets admite la exposición de fragmentos **de** contenido directamente en DAM como **JSON**.
   **Los fragmentos** de experiencia admiten la **búsqueda** de texto completo y la invalidación **de la caché del despachante de** AEM para hacer referencia a **las páginas**.

### Administración de activos

AEM Assets continúa aprovechando su rico conjunto de capacidades de administración de activos para mejorar el uso, la administración y la comprensión del DAM. AEM 6.5 sigue mejorando la integración entre Adobe Creative Cloud y los flujos de trabajo creativos.

+ **Adobe Asset Link** conecta elementos creativos directamente a AEM Assets desde las herramientas de Adobe Creative Cloud.
+ **La integración de Adobe Stock** permite el acceso directo a imágenes de Adobe Stock directamente desde la experiencia de AEM Assets, lo que crea una experiencia de descubrimiento de contenido sin problemas.
+ **AEM Desktop App** lanza la versión 2.0 y se rediseña a sí misma a la vez que mejora el rendimiento y la estabilidad.
+ **Recursos** conectados admite instancias discretas de AEM Sites para acceder a los recursos y utilizarlos sin problemas desde una instancia de AEM Assets diferente.
+ Se ha actualizado la compatibilidad con vídeo en **Dynamic Media**, incluidas las miniaturas de vídeo **** 360 y vídeo **personalizado**.

### Inteligencia de contenido

AEM continúa desarrollando su integración con tecnologías inteligentes, aprovechando el aprendizaje automático y la inteligencia artificial para mejorar todas las experiencias.

+ **Adobe Asset Link** agrega la búsqueda **por similitudes** visuales, lo que permite descubrir y utilizar fácilmente imágenes similares dentro de las herramientas **de** Adobe Creative Cloud.

### Integraciones

AEM aumenta su capacidad de integración con otros servicios de Adobe:

+ **Los fragmentos** de experiencia profundizan su integración con **Adobe Target** al admitir **Exportar como JSON** a Adobe Target y la capacidad de **eliminar ofertas** basadas en fragmentos de experiencia de **Adobe Target**.

### Administrador de nube de AMS

[Cloud Manager](https://adobe.ly/2HODmsv), exclusivo para los clientes de los servicios gestionados de Adobe (AMS), oferta las siguientes funciones:

+ Cloud Manager admite la ampliación AEM la compatibilidad de implementación de AEM Sites a **AEM Assets**, incluidas las pruebas de rendimiento **automatizadas del procesamiento** de recursos.
+ **El escalado** automático del nivel de AEM Publish en umbrales predefinidos garantiza una experiencia óptima para el usuario final.
+ **Las tuberías** que no son de producción permiten a los equipos de desarrollo aprovechar Cloud Manager para comprobar continuamente la calidad del código e implementarlas en entornos más bajos (Desarrollo y control de calidad).
+ **Las API** de canalización de CD/CI permiten a los clientes participar mediante programación con Cloud Manager, lo que aumenta las posibilidades de integración con la infraestructura de desarrollo local.

## Funciones básicas

A continuación se presenta una matriz de las principales funciones básicas ofrecidas por AEM. Algunas de estas funciones se introdujeron en versiones anteriores y se añadieron mejoras incrementales en cada versión.

+ [Notas de la versión de AEM Foundation](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html)

***✔<sup>+</sup>mejoras significativas en la función de esta versión.***

***✔<sup>SP</sup>indica que la función está disponible mediante un Service Pack o un Feature Pack.***

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
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://jackrabbit.apache.org/oak/docs/index.html" target="_blank">Repositorio</a>de contenido Oak:</strong> Proporciona un bueno rendimiento y escalabilidad con respecto al modelo anterior Jackrabbit 2.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/indexing-via-the-oak-run-jar.html">compatibilidad con</a>índices oak-run.jar:</strong> Se mejoró la reindexación, la recopilación de estadísticas y la comprobación de coherencia de los índices Oak.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/queries-and-indexing.html" target="_blank">Índices</a>de búsqueda personalizados: </strong>
                Posibilidad de agregar definiciones de índice personalizadas para optimizar el rendimiento de la consulta y la relevancia de la búsqueda.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/revision-cleanup.html" target="_blank">Limpieza</a>de revisión en línea:</strong>
                Realizar el mantenimiento del repositorio sin downtime del servidor.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/storage-elements-in-aem-6.html" target="_blank">Almacenamiento</a>del repositorio TarMK o MongoMK:</strong>
                <br> Opciones para utilizar almacenamiento simple y de rendimiento basado en archivos de TarMK (versión de próxima generación de TarPM) <br> o escalar horizontalmente con un repositorio respaldado por MongoDB con MongoMK.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/aem-with-mongodb.html" target="_blank">Rendimiento y estabilidad</a>de MongoMK:</strong>
            Se han realizado continuas mejoras en MongoMK desde su introducción con AEM 6.0.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/data-store-config.html#AmazonS3DataStore">Amazon S3 DataStore</a>:</strong>
            Aproveche la solución de almacenamiento en la nube ampliable para almacenar recursos binarios.</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Paridad de funciones de la IU táctil:</strong>
                Se han seguido realizando mejoras en la IU de creación para agilizar la creación con mayor productividad y paridad de funciones con la IU clásica.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Omnisearch:</strong>
                Busque y navegue AEM rápidamente.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/operations-dashboard.html" target="_blank">Panel</a>de operaciones:</strong>
 Realice tareas de mantenimiento, supervise el estado del servidor y analice el rendimiento desde dentro de AEM.</td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/upgrade.html" target="_blank">Mejoras</a>de actualización:</strong>
            Las mejoras en la actualización permiten realizar actualizaciones de AEM más sencillas y rápidas en el lugar.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/htl/using/overview.html" target="_blank">Lenguaje</a>de plantilla HTL:</strong>
            Motor de plantilla moderno que separa la presentación de la lógica. Reduce considerablemente el tiempo de desarrollo de los componentes. Se han añadido funciones incrementales con cada versión.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://sling.apache.org/documentation/bundles/models.html" target="_blank">Modelos</a>Sling:</strong>
            Un marco flexible para modelar recursos JCR en objetos y lógica de negocio. Se han añadido funciones incrementales con cada versión.
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://adobe.ly/2HODmsv" target="_blank">Administrador</a>de nube: </strong>
                Exclusivamente para los clientes de los servicios gestionados de Adobe (AMS), Cloud Manager acelera el desarrollo y la implementación mediante una canalización de CD/CI de última generación.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
    </tbody>
</table>

## Funciones de seguridad

A continuación se muestra una matriz de las características de seguridad clave ofrecidas por AEM. Algunas de estas funciones se introdujeron en versiones anteriores y se añadieron mejoras incrementales en cada versión.

+ [Notas de la versión de seguridad](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html#Security)

***La sintaxis de la sección dice que en esta versión se han realizado mejoras significativas en la función.***

***✔<sup>+</sup>indica que la función está disponible mediante un Service Pack o un Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Función de seguridad</td>
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
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html" target="_blank">Usuarios</a></strong>de servicios <br> Compartimentaliza permisos para evitar el uso innecesario de privilegios de administrador.</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank">Administración</a></strong>de almacenes de claves Almacén <br> de confianza global, certificados y claves, todos administrados dentro del repositorio.</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/csrf-protection.html" target="_blank"><strong>Protección</strong>de<strong>protección</strong></a>de CSRF <br> contra falsificaciones de solicitudes entre sitios fuera de la caja.</td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank"><strong>CORS</strong><strong>admite</strong></a>la compatibilidad con <br> el uso compartido de recursos entre Orígenes para buena flexibilidad de las aplicaciones.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://docs.adobe.com/docs/en/aem/6-5/administer/security/saml-2-0-authenticationhandler.html" target="_blank">Se mejoró la compatibilidad</a><br>con la autenticación SAML. Se </strong>mejoraron la redirección de SAML, la información de grupo optimizada y se resolvieron los problemas de cifrado de claves.
            <br>
        </td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html" target="_blank">LDAP como configuración</a><br>OSGi </strong>Simplifica la administración y las actualizaciones de la autenticación LDAP.</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong>La compatibilidad con el cifrado OSGi para contraseñas<br>de texto sin formato Las </strong>contraseñas y otros valores confidenciales se pueden guardar en forma cifrada y descifrar automáticamente.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/user-group-ac-admin.html" target="_blank">Mejoras</a><br>en el CUG Se ha reescrito la implementación del grupo de usuarios </strong>cerrado para abordar problemas de rendimiento y escalabilidad.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔<sup>+</sup></td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/platform-repository/using/ssl-wizard-technical-video-use.html" target="_blank">IU del Asistente</a></strong>SSL para <br> simplificar la configuración y la administración de SSL.</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/encapsulated-token.html" target="_blank">Compatibilidad</a></strong>con tokens encapsulados <br> ya no es necesaria para que las sesiones "adhesivas" admitan la autenticación horizontal en todas las instancias de publicación.</td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ims-config-and-admin-console.html" target="_blank">Compatibilidad</a><br>con la autenticación IMS de Adobe </strong>exclusiva para los servicios gestionados de Adobe (AMS), gestione de forma centralizada el acceso a las instancias de AEM Author mediante Adobe IMS (Identity Management System).</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
    </tr>
</tbody>
</table>

## Funciones del sitio

A continuación se muestra una matriz de las funciones de sitios principales ofrecidas por AEM. Algunas de estas funciones se introdujeron en versiones anteriores y se añadieron mejoras incrementales en cada versión.

+ [Notas de la versión de AEM Sites](https://helpx.adobe.com/experience-manager/6-5/release-notes/sites.html)

***✔<sup>+</sup>mejoras significativas en la función de esta versión.***

***✔<sup>SP</sup>indica que la función está disponible mediante un Service Pack o un Feature Pack.***

<table>
    <thead>
        <tr>
            <td><strong>Función Sitios</strong></td>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/page-editor-feature-video-use.html" target="_blank">Creación</a>de páginas táctil optimizada:</strong>
            Permite a los editores aprovechar tabletas y equipos con pantallas táctiles.</td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/responsive-layout.html" target="_blank">Creación</a>de sitios adaptable:</strong>
                El modo de diseño permite a los editores cambiar el tamaño de los componentes en función del ancho del dispositivo para los sitios interactivos.</td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/template-editor-feature-video-use.html" target="_blank">Plantillas</a>editables:</strong>
            Permite que los autores especializados creen y editen plantillas de página.</td>
            <td></td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/core-components/user-guide.html" target="_blank">Componentes</a>principales:</strong>
            Acelere el desarrollo del sitio. Disponible en GitHub para programación de lanzamientos frecuentes y flexibilidad.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/spa-overview.html" target="_blank">Editor</a>de SPA:</strong>
            Cree experiencias web fiables y atractivas mediante marcos de aplicación de una sola página (SPA) creados en React o Angular.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/release-notes/style-system-fp.html" target="_blank">Sistema</a>de estilos:</strong>
            Aumente AEM reutilización de componentes definiendo su aspecto visual mediante el sistema de estilo en contexto.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/es-ES/experience-manager/6-5/sites/administering/using/msm.html" target="_blank">Administrador multisitio (MSM)</a>:</strong>
            Administre varios sitios web que compartan contenido común (es decir, varias marcas multilingües).</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/translation.html" target="_blank">Traducción</a>de contenido:</strong>
            El módulo Plug and Play se integra con los servicios de traducción de terceros líderes en la industria.</td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/contexthub.html" target="_blank">ContextHub</a>:</strong>
            Entorno de contexto de cliente de próxima generación para la personalización de contenido.</td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/launches.html" target="_blank">Inicios</a>:</strong>
            Desarrolle contenido para una versión futura sin interrumpir la creación diaria.</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/content-fragments-feature-video-understand.html" target="_blank">Fragmentos</a>de contenido:</strong>
            Cree y revise el contenido editorial desacoplado de la presentación para facilitar su reutilización.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-use.html" target="_blank">Fragmentos</a>de experiencia:</strong>
            Cree experiencias y variaciones reutilizables optimizadas para canales de escritorio, móviles y sociales.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/release-notes/content-services-fragments-featurepack.html" target="_blank">Servicios</a>de contenido:</strong>
            Exporte contenido de AEM como JSON para consumo en dispositivos y aplicaciones.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Integración de Adobe Analytics y perspectivas de contenido:</strong>
                Fácil integración de Adobe Analytics y DTM. Muestre información de rendimiento en entorno de autor.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/es/experience-manager/6-5/sites/authoring/using/content-targeting-touch.html" target="_blank">Integración</a>de Adobe Target:</strong>
            Asistente paso a paso para crear experiencias con objetivos y bibliotecas de ofertas reutilizables.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/campaign.html" target="_blank">Integración</a>de Adobe Campaign:</strong>
            Fácil integración con la solución de campaña por correo electrónico de próxima generación.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/using/aem_launch_adobeio_integration.html" target="_blank">Integración</a>de inicio de Adobe:</strong>
            Integración con el servicio en la nube de administración de etiquetas de próxima generación de Adobe.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/aem-screens-introduction.html" target="_blank">Pantallas</a>:</strong>
            Administre experiencias para señalización digital y quioscos.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ecommerce.html" target="_blank">Comercio electrónico</a>:</strong>
            Ofrezca experiencias de compra personalizadas y con marca en todos los puntos de contacto web, móviles y sociales.
            </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/overview.html" target="_blank">Comunidades</a>:</strong>
            Los foros, los comentarios con subprocesos, los calendarios de eventos y muchas otras funciones permiten una participación profunda con los visitantes del sitio.</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
    </tbody>
</table>

## Funciones de recursos

A continuación se muestra una matriz de las funciones clave de Recursos ofrecidas por AEM. Algunas de estas funciones se introdujeron en versiones anteriores y se añadieron mejoras incrementales en cada versión.

+ [Notas de la versión de AEM Assets](https://helpx.adobe.com/experience-manager/6-5/release-notes/assets.html)

***La sintaxis de la sección dice que en esta versión se han realizado mejoras significativas en la función.***

***✔<sup>+</sup>indica que la función está disponible mediante un Service Pack o un Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Función Recursos</td>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets-touch-ui.html" target="_blank">IU</a>táctil optimizada:</strong>
            Administre recursos en un equipo de escritorio o en dispositivos táctiles.</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/metadata.html" target="_blank">Administración</a>avanzada de metadatos:</strong>
            Plantillas de metadatos, Editor de Esquemas de metadatos y Edición masiva de metadatos.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/task-content.html" target="_blank">Administración de tareas</a> y <a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects-with-workflows.html" target="_blank">flujos de trabajo</a> :</strong>
            Flujos de trabajo y tareas pregenerados para la revisión y aprobación de activos digitales que aprovechan AEM proyectos.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Escalabilidad y rendimiento:</strong>
            Compatibilidad mejorada para la ingestión, carga y almacenamiento a escala.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mac-api-assets.html" target="_blank">API</a>HTTP de recursos:</strong>
            Interactúe mediante programación con los recursos mediante HTTP y JSON.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/link-sharing.html" target="_blank">Vínculo compartido</a>:</strong>
            Uso compartido de recursos digitales simple y específico sin necesidad de iniciar sesión.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal.html" target="_blank">Brand Portal</a>:</strong>
            Solución SAAS de servicio de nube para compartir y distribuir recursos digitales sin problemas.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/use-assets-across-connected-assets-instances.html" target="_blank">Recursos</a>conectados:</strong>
            Las instancias de AEM Sites pueden acceder a los recursos y usarlos sin problemas desde una instancia de AEM Assets diferente.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/touch-ui-asset-insights.html" target="_blank">Perspectivas</a>de recursos:</strong>
            Aproveche Adobe Analytics para capturar la interacción de los clientes con los recursos digitales y la vista en AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/multilingual-assets.html" target="_blank">Recursos</a>multilingües:</strong>
            Compatibilidad de traducción de metadatos de recursos automáticamente con raíces de idioma.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/enhanced-smart-tags.html" target="_blank">Etiquetas inteligentes y moderación</a>:</strong>
            Aproveche Adobe Sensei para etiquetar imágenes automáticamente con metadatos útiles.</td>
            <td> </td>
            <td></td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/assets/using/smart-translation-search-feature-video-use.html" target="_blank">Búsqueda</a>de traducción inteligente:</strong>
            Traduzca automáticamente los términos de búsqueda al buscar AEM Assets.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/indesign.html" target="_blank">Integración</a>de Adobe InDesign Server:</strong>
            Genere catálogos de productos. Cree folletos, volantes y anuncios impresos basados en plantillas de InDesign.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/desktop-app/aem-desktop-app.html" target="_blank">AEM aplicación</a>de escritorio:</strong>
            Sincronice recursos con el escritorio local para editarlos con productos Creative Suite.
            </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/imaging-transcoding-library.html" target="_blank">Biblioteca</a>de imágenes de Adobe:</strong>
                <br> Bibliotecas PDF de Photoshop y Acrobat utilizadas para la manipulación de archivos de alta calidad.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://www.adobe.com/creativecloud/business/enterprise/adobe-asset-link.html" target="_blank">Vínculo</a>de recurso de Adobe:</strong>
            Acceda a AEM Assets directamente desde las aplicaciones de Adobe Create Cloud.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/aem-assets-adobe-stock.html" target="_blank">Integración</a>de Adobe Stock:</strong>
            Acceda y utilice sin problemas imágenes de Adobe Stock directamente desde AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
        </tr>
    </tbody>
</table>

### AEM Assets Dynamic Media

***✔<sup>+</sup>mejoras significativas en la función de esta versión.***

***✔<sup>SP</sup>indica que la función está disponible mediante un Service Pack o un Feature Pack.***


<table>
    <thead>
        <tr>
            <td>Función Dynamic Media</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6,3 + FP</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets.html" target="_blank">Imágenes</a>:</strong>
            Distribuya imágenes de forma dinámica en diferentes tamaños y formatos, incluido Smart Crop.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/video-profiles.html" target="_blank">Vídeo</a>:</strong>
            Codificación de vídeo avanzada y flujo de vídeo adaptable</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/interactive-images.html" target="_blank">Medios</a>interactivos:</strong>
            Cree letreros interactivos y vídeos con contenido en el que se puede hacer clic para mostrar ofertas clave.
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Conjuntos (<a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/image-sets.html" target="_blank">Imagen</a>, <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/spin-sets.html" target="_blank">Giro</a>, Medios <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mixed-media-sets.html" target="_blank">mixtos</a>):</strong>
            Permite a los usuarios aplicar zoom, recortar, rotar y simular una experiencia de visualización de 360 grados.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://docs.adobe.com/docs/en/aem/6-5/administer/content/dynamic-media/viewer-presets.html" target="_blank">Visores</a>:</strong>
            Reproductores y ajustes preestablecidos de medios enriquecidos con marca personalizada que admiten diferentes pantallas/dispositivos.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/delivering-dynamic-media-assets.html" target="_blank">Envío</a>:</strong>
            Opciones flexibles para vincular o incrustar contenido e envío de Dynamic Media a través del protocolo HTTP/2.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Actualizar de Scene7 a Dynamic Media:</strong>
            Capacidad para migrar recursos principales y seguir usando direcciones URL de S7 existentes.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
    </tbody>
</table>

## Funciones de Forms

A continuación se muestra una matriz de las principales funciones de Añada de AEM Forms ofrecidas por AEM. Algunas de estas funciones se introdujeron en versiones anteriores y se añadieron mejoras incrementales en cada versión.

+ [Notas de la versión de AEM Forms](https://helpx.adobe.com/experience-manager/6-5/release-notes/forms.html)

***✔<sup>+</sup>mejoras significativas en la función de esta versión.***

***✔<sup>SP</sup>indica que la función está disponible mediante un Service Pack o un Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Función Forms</td>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/introduction-forms-authoring.html" target="_blank">Editor</a>de Forms adaptable:</strong>
            Cree formularios atractivos, adaptables y adaptables en función de la configuración del dispositivo y del explorador.</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/generate-document-of-record-for-non-xfa-based-adaptive-forms.html" target="_blank">Documento del registro</a>:</strong>
            Cree un documento para garantizar el almacenamiento a largo plazo de una experiencia de captura de datos o una versión preparada para imprimir.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/themes.html" target="_blank">Editor</a>de temas:</strong>
            Cree temáticas reutilizables para aplicar estilo a los componentes y paneles de un formulario.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/template-editor.html" target="_blank">Editor</a>de plantillas:</strong>
            Estandarización e implementación de las optimizaciones para formularios adaptables.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedintegrationwithAdobeSign" target="_blank">Integración</a>de Adobe Sign:</strong>
            Permitir la implementación de escenarios de firma basados en formularios integrados de Adobe Sign.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/cm-overview.html" target="_blank">Administración</a>de correspondencia:</strong>
            Con AEM Forms, puede crear, administrar y entregar correspondencias personalizadas e interactivas de clientes.
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#AEMFormsdataintegration" target="_blank">Integración</a>de datos de terceros:</strong>
            Mediante la integración de datos, los datos se recuperan de orígenes de datos dispares en función de las entradas del usuario en un formulario. Al enviar el formulario, los datos capturados se vuelven a escribir en los orígenes de datos.
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#FormscentricAEMWorkflowsforAEMFormsonOSGi" target="_blank">Flujo de trabajo (en OSGi) para procesamiento</a>de Forms:</strong>
            Implementación simplificada de procesos de aprobación de formularios.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/user-guide.html?topic=/experience-manager/6-5/forms/morehelp/integrations.ug.js" target="_blank">Integración con Marketing Cloud</a>:</strong>
            Integración con Adobe Analytics y Adobe Target para mejorar y medir las experiencias de los clientes.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/introduction-managing-forms.html" target="_blank">Administrador</a>de formularios:</strong>
            Ubicación única para administrar todos los formularios, documentos y correspondencia, como habilitar análisis, traducción, pruebas A/B, revisiones y publicaciones.
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/aem-forms-app.html" target="_blank">Aplicación</a>AEM Forms:</strong>
            Permite el procesamiento de formularios en línea y sin conexión dentro de una aplicación en iOS, Android o Windows.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/adaptive-document.html" target="_blank">Comunicaciones</a>interactivas:</strong>
            Cree comunicaciones enriquecidas, como afirmaciones con objetivos, con elementos interactivos como gráficos (anteriormente conocidos como Documentos adaptables).</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/pdf/aem-forms/6-5/WorkbenchHelp.pdf" target="_blank">Flujo de trabajo (J2EE) para procesamiento</a>de Forms:</strong>
            Cree formularios complejos y flujos de trabajo centrados en el documento utilizando un IDE intuitivo.</td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedDocumentSecurity" target="_blank">Seguridad</a>de AEM Forms Documento:</strong>
            Acceso seguro y autorización de documentos de PDF y Office.
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#Simplifiedauthoringexperience" target="_blank">Marcos</a>de prueba:</strong>
            Utilice el marco Calvin y el complemento Chrome para admitir y depurar formularios adaptables.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
    </tbody>
</table>

## Funciones de comunidades

A continuación se muestra una matriz de las principales funciones de Añada de AEM Communities ofrecidas por AEM. Algunas de estas funciones se introdujeron en versiones anteriores y se añadieron mejoras incrementales en cada versión.

+ [Resumen de las nuevas funciones de AEM Communities](https://helpx.adobe.com/experience-manager/6-5/communities/using/whats-new-aem-communities.html#main-pars_text)

***✔<sup>+</sup>mejoras significativas en la función de esta versión.***

***✔<sup>SP</sup>indica que la función está disponible mediante un Service Pack o un Feature Pack.***

<table>
    <thead>
        <tr>
            <td> </td>
            <td>Función de comunidades</td>
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
            <td rowspan="7">Funciones de comunidades</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/forum.html" target="_blank">Foros</a>:</strong> (Marco de componentes sociales) Cree nuevos temas, o vista, siga, busque y mueva los temas existentes.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <p><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-qna.html" target="_blank">QnA</a>:</strong>
                Haga preguntas, vista y responda.</p>
            </td>
            <td></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/blog-feature.html" target="_blank">Blogs</a>:</strong>
                Cree artículos de blog y comentarios en el lado de publicación.
            </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/ideation-feature.html" target="_blank">Ideación</a>:</strong>
                Crear y compartir ideas con la comunidad, o vista, seguir y comentar ideas existentes.
            </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/calendar.html" target="_blank">Calendario</a>:</strong>
                (Marco de componentes sociales) Proporciona información sobre eventos de la comunidad a los visitantes del sitio.
            </td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/file-library.html" target="_blank">Biblioteca</a>de archivos:</strong>
                Cargue, administre y descargue archivos dentro del sitio de la comunidad.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/users.html#AboutCommunityGroups" target="_blank">Grupos</a>de usuarios:
            </strong>Un conjunto de usuarios puede pertenecer a grupos de miembros y se pueden asignar funciones colectivamente.</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong> </strong></td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resource.html" target="_blank">Asignación</a>:</strong>
            Cree y asigne recursos de aprendizaje a los miembros de la comunidad.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="5">Habilitación</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/catalog.html" target="_blank">Administración</a> de catálogos y <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resource.html" target="_blank">recursos</a>:</strong>
            Acceda a los recursos de habilitación desde el catálogo.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resources.html#CreateaLearningPath" target="_blank">Administración</a>de rutas de aprendizaje:</strong>
            Administre cursos o grupos de recursos de habilitación.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/reports.html#main-pars_text_1739724213" target="_blank">Sistema de informes</a>de habilitación:</strong>
            Sistema de informes en recursos de habilitación y rutas de aprendizaje.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resources.html#main-pars_text_899882038" target="_blank">Participación en la habilitación</a>:</strong>
            Añada comentarios sobre los recursos de habilitación.</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/analytics.html" target="_blank">Análisis</a>de habilitación:</strong>
            Análisis de vídeo, Sistema de informes de progreso y Sistema de informes de asignación</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="8">Commons</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/comments.html" target="_blank">Comentarios</a> y anexos:</strong>
            (Marco de componentes sociales) Como miembro de la comunidad, comparte opiniones y conocimientos sobre el contenido en el sitio de comunidades.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Conversión de fragmentos de contenido:</strong>
            Convierta las contribuciones UGC en fragmentos de contenido.</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/reviews.html" target="_blank">Reseñas</a>:</strong>
                (Marco de componentes sociales) Como miembro de la comunidad, revise un fragmento de contenido mediante una combinación de comentarios y funciones de clasificación.</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/rating.html" target="_blank">Clasificaciones</a>:/strong&gt; (Marco de componentes sociales) Como miembro de la comunidad califica un fragmento de contenido.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/voting.html" target="_blank">Votos</a>:</strong>
                (Marco de componentes sociales) Como miembro de la comunidad votará a favor o en contra de un contenido.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/tag-ugc.html" target="_blank">Etiquetas</a>:</strong>
            Adjunte etiquetas (palabras clave o etiquetas) con contenido para localizar contenido rápidamente.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/search.html" target="_blank">Buscar</a>:</strong>
            Búsquedas predictivas y sugerentes.</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/translate-ugc.html" target="_blank">Traducción</a>:</strong>
            Traducción automática de contenido generado por el usuario.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="10">Administración</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/create-site.html" target="_blank">Administración</a>del sitio:</strong>
            Creación de sitios con funciones de comunidades.</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sites.html" target="_blank">Plantillas</a>:</strong>
                <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sites.html" target="_blank">Plantillas de sitio</a> y <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/tools-groups.html" target="_blank">grupo</a> para la creación basada en asistente de sitios comunitarios completamente funcionales.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Plantillas editables:</strong>
            Permita a los administradores de la comunidad crear experiencias enriquecidas con plantillas editables de AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/creating-groups.html" target="_blank">Grupos o subcomunidades</a>:</strong>
            Crear dinámicamente subcomunidades dentro de los sitios de las comunidades.
            </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/in-context.html" target="_blank">Moderación</a>:</strong>
            Moderación del contenido generado por el usuario.
            </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/moderation.html" target="_blank">Moderación</a>masiva:</strong>
            Consola de moderación para administrar de forma masiva el contenido generado por el usuario.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/moderate-ugc.html#CommonModerationConcepts" target="_blank">Filtros</a>de detección de correo no deseado y de rentabilidad:</strong>
            Detección automática de spam.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/members.html" target="_blank">Administración</a>de miembros:</strong>
            Administre perfiles de usuario y grupos desde el área de administración de miembros.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/overview.html#main-pars_text_866731966" target="_blank">Diseño</a>adaptable:</strong>
            Los sitios de AEM Communities responden.
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/analytics.html" target="_blank">Analytics</a>:</strong>
            Integración con Adobe Analytics para obtener perspectivas clave en el uso de los sitios de Comunidades.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="4">Miembros</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/advanced.html" target="_blank">Puntuación y señalización</a>:</strong>
            (Puntuación avanzada con tecnología de Adobe Sensei) Identifique a los miembros de la comunidad como expertos y recompense a estos.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/activities.html" target="_blank">Actividades</a> y <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/notifications.html" target="_blank">notificaciones</a>:</strong>
            Vista las actividades recientes fluyen y recibe notificaciones sobre eventos de interés.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/configure-messaging.html" target="_blank">Mensajes</a>:</strong>
            Mensajería directa para usuarios y grupos.</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/social-login.html" target="_blank">Inicios de sesión</a>de Social:</strong>
            Inicie sesión con su cuenta de Facebook o Twitter.</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="5">Plataforma</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">MSRP (Almacenamiento de Mongo)</a>:</strong>
            El contenido generado por el usuario (UGC) persiste directamente en una instancia de MongoDB local</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">DSRP (Almacenamiento de base de datos)</a>:</strong>
            El contenido generado por el usuario (UGC) persiste directamente en una instancia de base de datos MySQL local.</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">SRP (Almacenamiento de nube)</a>:</strong>
                El contenido generado por el usuario (UGC) se mantiene de forma remota en un servicio en la nube alojado y administrado por Adobe.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank"><strong>JSRP</a>:</strong>
                El contenido de la comunidad se almacena en JCR y se puede acceder a él desde la instancia de creación (o publicación) en la que se publicó.</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sync.html" target="_blank">Sincronización</a>de usuarios y grupos:</strong>
            Sincronice usuarios y grupos en distintas instancias de publicación al usar una topología de conjunto de publicaciones.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
    </tbody>
</table>

AEM Communities agrega [mejoras](https://helpx.adobe.com/experience-manager/6-5/communities/using/whats-new-aem-communities.html) a través de las versiones para permitir que las organizaciones se involucren y habiliten a sus usuarios mediante:

+ **@uncia** compatibilidad con contenido generado por el usuario.
+ Mejoras de accesibilidad mediante la navegación **por** teclado en los componentes **Habilitación** .
+ Se mejoró la moderación **** masiva mediante Filtros **** personalizados.
+ **Plantillas** editables para que los administradores de la comunidad puedan crear experiencias de comunidad enriquecidas en AEM.
+ Ahora los usuarios pueden enviar mensajes **directos de forma masiva** a todos los miembros de un grupo.
