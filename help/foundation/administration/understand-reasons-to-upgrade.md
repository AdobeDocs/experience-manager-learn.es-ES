---
title: Comprender las razones para actualizar
description: Un desglose de alto nivel de las funciones clave para los clientes que se plantean actualizar a la última versión de Adobe Experience Manager 6.
version: Experience Manager 6.5
topic: Upgrade
feature: Release Information
role: Leader, Developer, Admin, User
level: Beginner
doc-type: Article
exl-id: bf4030b0-67c4-4b00-af95-f63e6f79e995
duration: 538
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '2576'
ht-degree: 2%

---

# Explicación de los motivos para actualizar

Un desglose de alto nivel de las funciones clave para los clientes que se plantean actualizar a la última versión de Adobe Experience Manager 6.

## Características principales para actualizar a AEM 6.5

+ [Notas de la versión de Adobe Experience Manager 6.5](https://helpx.adobe.com/es/experience-manager/6-5/release-notes.html)

### Mejoras de base

Adobe Experience Manager 6.5 sigue mejorando la estabilidad, el rendimiento y la compatibilidad del sistema mediante:

+ Compatibilidad con **Java 11** (mientras se mantiene la compatibilidad con Java 8).

### Creación y administración de sitios web

AEM Sites presenta una serie de funciones diseñadas para acelerar la creación y la creación de sitios web:

+ La compatibilidad con el **Editor de SPA** permite que el SPA (aplicaciones de una sola página) se cree completamente en AEM, lo que ofrece una experiencia de creación enriquecida y fácil de usar para los especialistas en marketing.
+_ **JavaScript SDK**, un kit de inicio de proyectos de SPA y herramientas de compilación compatibles, permiten a los desarrolladores de front-end desarrollar aplicaciones de una sola página compatibles con el Editor de SPA independientemente de AEM.
+ **Componentes principales** agrega una gran cantidad de componentes nuevos, una **Biblioteca de componentes**, así como una variedad de mejoras a los Componentes principales existentes.
+ Más **traducciones** mejoras optimizan la traducción de AEM Sites.

### Experiencias fluidas

AEM sigue adoptando las Experiencias fluidas con herramientas nuevas y mejoradas que facilitan el uso de contenido fuera de AEM.

+ **Fragmentos de contenido** admiten Comparación de versiones/Diferencias y anotaciones.
+ La API HTTP de Assets de **AEM** admite la exposición de **fragmentos de contenido** directamente en DAM como **JSON**.
  **Fragmentos de experiencia** admiten **Búsqueda de texto completo** e **Invalidación de caché de AEM Dispatcher** para hacer referencia a **Páginas**.

### Administración de recursos

AEM Assets sigue aprovechando su completo conjunto de funcionalidades de administración de recursos para mejorar el uso, la administración y la comprensión de DAM. AEM 6.5 sigue mejorando la integración entre Adobe Creative Cloud y los flujos de trabajo creativos.

+ **Adobe Asset Link** conecta a los creativos directamente con los AEM Assets desde las herramientas de Adobe Creative Cloud.
+ La integración de **Adobe Stock** permite el acceso directo a las imágenes de Adobe Stock desde la experiencia de los AEM Assets, lo que crea una experiencia perfecta de detección de contenido.
+ **AEM Desktop App** lanza la versión 2.0 y se rediseña a la vez que mejora el rendimiento y la estabilidad.
+ **Assets conectado** admite instancias de AEM Sites discretas para acceder y utilizar recursos sin problemas desde una instancia de AEM Assets diferente.
+ Se ha actualizado la compatibilidad con vídeo en **Dynamic Media**, que incluye **360 vídeos** y **miniaturas de vídeo personalizadas**.

### Inteligencia de contenido

AEM sigue desarrollando su integración con tecnologías inteligentes, aprovechando el aprendizaje automático y la inteligencia artificial para mejorar todas las experiencias.

+ **Adobe Asset Link** agrega **Búsqueda por similitud visual**, lo que permite que imágenes similares se descubran y utilicen fácilmente en **herramientas de Adobe Creative Cloud**.

### Integraciones

AEM aumenta su capacidad de integración con otros servicios de Adobe:

+ **Los fragmentos de experiencias** profundizan su integración con **Adobe Target** al admitir **Exportar como JSON** a Adobe Target y la capacidad de **eliminar ofertas basadas en fragmentos de experiencias** de **Adobe Target**.

### AMS CLOUD MANAGER

[Cloud Manager](https://adobe.ly/2HODmsv), una aplicación exclusiva para los clientes de Adobe Managed Services (AMS), ofrece las siguientes características:

+ Cloud Manager admite y extiende la compatibilidad con la implementación de AEM de AEM Sites a **AEM Assets**, incluidas las **pruebas de rendimiento automatizadas del procesamiento de recursos**.
+ **Escalado automático** del nivel de publicación de AEM en los umbrales predefinidos, garantiza una experiencia óptima para el usuario final.
+ **Las canalizaciones que no son de producción** permiten a los equipos de desarrollo aprovechar Cloud Manager para comprobar continuamente la calidad del código e implementarlas en entornos más bajos (desarrollo y control de calidad).
+ Las **API de canalización de CI/CD** permiten a los clientes interactuar mediante programación con Cloud Manager, lo que aumenta las posibilidades de integración con la infraestructura de desarrollo local.

## Funciones de base

A continuación se muestra una matriz de funciones básicas clave que ofrece AEM. Algunas de estas funciones se introdujeron en versiones anteriores y se agregaron mejoras incrementales en cada versión.

+ [Notas de la versión de AEM Foundation](https://helpx.adobe.com/es/experience-manager/6-5/release-notes/wcm-platform.html)

***✔<sup>+</sup> mejoras significativas en la característica de esta versión.***

***✔<sup>SP</sup> indica que la característica está disponible a través de un Service Pack o Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Función Base</td>
            <td>5.6.x</td>
            <td>6,0</td>
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
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://jackrabbit.apache.org/oak/docs/index.html" target="_blank">Repositorio de contenido de Oak</a>:</strong> Proporciona un rendimiento y una escalabilidad mucho mayores que los del predecesor Jackrabbit 2.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/es/experience-manager/6-5/sites/deploying/using/indexing-via-the-oak-run-jar.html">compatibilidad con el índice oak-run.jar</a>:</strong> Se mejoró la reindexación, la recopilación de estadísticas y la comprobación de consistencia de los índices Oak.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/es/experience-manager/6-5/sites/deploying/using/queries-and-indexing.html" target="_blank">Índices de búsqueda personalizada</a>: </strong>
                Capacidad para agregar definiciones de índice personalizadas para optimizar el rendimiento de las consultas y la relevancia de la búsqueda.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/es/experience-manager/6-5/sites/deploying/using/revision-cleanup.html" target="_blank">Limpieza de revisión en línea</a>:</strong>
                Realice el mantenimiento del repositorio sin tiempo de inactividad del servidor.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/es/experience-manager/6-5/sites/deploying/using/storage-elements-in-aem-6.html" target="_blank">Almacenamiento del repositorio TarMK o MongoMK</a>:</strong>
                <br> Opciones para utilizar el almacenamiento simple y eficaz basado en archivos de TarMK (versión de próxima generación de TarPM)
                <br> o escalar horizontalmente con un repositorio respaldado por MongoDB con MongoMK.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/es/experience-manager/6-5/sites/deploying/using/aem-with-mongodb.html" target="_blank">Rendimiento y estabilidad de MongoMK</a>:</strong>
            Se han realizado mejoras continuas en MongoMK desde su introducción con AEM 6.0.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/es/experience-manager/6-5/sites/deploying/using/data-store-config.html#AmazonS3DataStore">Almacén de datos de Amazon S3</a>:</strong>
            Aproveche la solución de almacenamiento en la nube expandible para almacenar recursos binarios.</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Paridad de característica de IU táctil:</strong>
                Mejoras continuas en la IU de creación para aumentar la velocidad con una mayor productividad y paridad de características con la IU clásica.</td>
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
                Busque y navegue rápidamente por AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/es/experience-manager/6-5/sites/administering/using/operations-dashboard.html" target="_blank">Tablero de operaciones</a>:</strong>
 Realizar tareas de mantenimiento, supervisar el estado del servidor y analizar el rendimiento desde AEM.</td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/es/experience-manager/6-5/sites/deploying/using/upgrade.html" target="_blank">Mejoras en la actualización</a>:</strong>
            Las mejoras de actualización permiten actualizaciones in situ más sencillas y rápidas de AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/es/experience-manager/htl/using/overview.html" target="_blank">Idioma de plantilla HTL</a>:</strong>
            Motor de creación de plantillas moderno que separa la presentación de la lógica. Reduce considerablemente el tiempo de desarrollo de componentes. Funciones incrementales añadidas con cada versión.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://sling.apache.org/documentation/bundles/models.html?lang=es" target="_blank">Modelos Sling</a>:</strong>
            Un marco flexible para modelar recursos JCR en objetos y lógica empresarial. Funciones incrementales añadidas con cada versión.
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
            <td><strong><a href="https://adobe.ly/2HODmsv" target="_blank">Cloud Manager</a>: </strong>
                Exclusivo para los clientes de Adobe Managed Services (AMS), Cloud Manager acelera el desarrollo y la implementación a través de una canalización de CD/CI de última generación.</td>
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

A continuación se muestra una matriz de las funciones de seguridad clave que ofrece AEM. Algunas de estas funciones se introdujeron en versiones anteriores y se agregaron mejoras incrementales en cada versión.

+ [Notas de la versión de seguridad](https://helpx.adobe.com/es/experience-manager/6-5/release-notes/wcm-platform.html#Security)

***✔indica que se han realizado mejoras significativas en la característica en esta versión.***

***✔<sup>+</sup> indica que la característica está disponible a través de un Service Pack o Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Función de seguridad</td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6,1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
        <td><strong><a href="https://helpx.adobe.com/es/experience-manager/6-5/sites/administering/using/security-service-users.html" target="_blank">Usuarios de servicio</a></strong>
            <br> Compartimenta los permisos para evitar el uso innecesario de los privilegios de administrador.</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/es/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank">Administración de almacén de claves</a></strong>
            <br> Almacén de confianza global, certificados y claves administrados en el repositorio.</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/es/experience-manager/6-5/sites/developing/using/csrf-protection.html" target="_blank"><strong>CSRF</strong> <strong>protección</strong></a>
            <br> solicitud entre sitios Protección contra falsificación lista para usar.</td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/es/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank"><strong>CORS</strong> <strong>soporte</strong></a>
            <br> Compatibilidad de uso compartido de recursos de origen cruzado para una mayor flexibilidad de la aplicación.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://experienceleague.adobe.com/docs/?lang=es" target="_blank">Se mejoró la compatibilidad con la autenticación SAML</a><br>
 </strong>Se han resuelto los problemas de redireccionamiento de SAML, información de grupo optimizada y cifrado de claves.
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
        <td><strong><a href="https://helpx.adobe.com/es/experience-manager/6-5/sites/administering/using/ldap-config.html" target="_blank">LDAP como una configuración OSGi</a><br>
 </strong>Simplifica la administración y las actualizaciones de la autenticación LDAP.</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong>Compatibilidad con cifrado OSGi para contraseñas de texto sin formato<br>
 </strong>Las contraseñas y otros valores confidenciales se pueden guardar cifrados y descifrar automáticamente.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/es/experience-manager/6-5/sites/administering/using/user-group-ac-admin.html" target="_blank">mejoras de CUG</a><br>
 Se ha vuelto a escribir la implementación de </strong>grupo de usuarios cerrados para abordar los problemas de rendimiento y escalabilidad.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔<sup>+</sup></td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/platform-repository/using/ssl-wizard-technical-video-use.html" target="_blank">Asistente SSL</a></strong>
            Interfaz de usuario <br> para simplificar la configuración y administración de SSL.</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/es/experience-manager/6-5/sites/administering/using/encapsulated-token.html" target="_blank">Compatibilidad con token encapsulado</a></strong>
            <br> Ya no es necesario para sesiones "fijas" que admitan autenticación horizontal en instancias de publicación.</td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/es/experience-manager/6-5/sites/administering/using/ims-config-and-admin-console.html" target="_blank">Compatibilidad con autenticación IMS de Adobe</a><br>
 </strong>Exclusivo para Adobe Managed Services (AMS), administra de forma centralizada el acceso a las instancias de autor de AEM a través de Adobe IMS (Identity Management System).</td>
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

## Funciones de sitios

A continuación se muestra una matriz de las funciones clave de Sites que ofrece AEM. Algunas de estas funciones se introdujeron en versiones anteriores y se agregaron mejoras incrementales en cada versión.

+ [Notas de la versión de AEM Sites](https://helpx.adobe.com/es/experience-manager/6-5/release-notes/sites.html)

***✔<sup>+</sup> mejoras significativas en la característica de esta versión.***

***✔<sup>SP</sup> indica que la característica está disponible a través de un Service Pack o Feature Pack.***

<table>
    <thead>
        <tr>
            <td><strong>Función de Sites</strong></td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6,1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/page-editor-feature-video-use.html" target="_blank">Creación de página táctil optimizada</a>:</strong>
            Permite a los editores aprovechar las tabletas y los equipos con pantallas táctiles.</td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/es/experience-manager/6-5/sites/authoring/using/responsive-layout.html" target="_blank">Creación de sitios adaptable</a>:</strong>
                El modo de diseño permite a los editores cambiar el tamaño de los componentes en función de los anchos de los dispositivos para sitios adaptables.</td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/template-editor-feature-video-use.html" target="_blank">Plantillas editables</a>:</strong>
            Permite a los autores especializados crear y editar plantillas de página.</td>
            <td></td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/es/experience-manager/core-components/user-guide.html" target="_blank">Componentes principales</a>:</strong>
            Acelerar el desarrollo del sitio. Disponible en GitHub para flexibilidad y programación frecuentes de versiones.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/es/experience-manager/6-5/sites/developing/using/spa-overview.html" target="_blank">Editor de SPA</a>:</strong>
            Cree experiencias web atractivas y legibles con marcos de aplicación de una sola página (SPA) basados en React.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong>Sistema de estilos:</strong>
            Aumente la reutilización de componentes de AEM definiendo su apariencia visual mediante el sistema de estilos en contexto.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/es-ES/experience-manager/6-5/sites/administering/using/msm.html" target="_blank">Administrador de varios sitios (MSM)</a>:</strong>
            Administre varios sitios web que compartan contenido común (es decir, multilingües, varias marcas).</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/es/experience-manager/6-5/sites/administering/using/translation.html" target="_blank">Traducción de contenido</a>:</strong>
            El marco Plug and Play se integra con los servicios de traducción de terceros líderes del sector.</td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/es/experience-manager/6-5/sites/developing/using/contexthub.html" target="_blank">ContextHub</a>:</strong>
            Marco de trabajo de cliente de próxima generación para la personalización de contenido.</td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/es/experience-manager/6-5/sites/authoring/using/launches.html" target="_blank">Lanzamientos</a>:</strong>
            Desarrollar contenido para una versión futura sin interrumpir la creación diaria.</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Fragmentos de contenido:</strong>
            Cree y depure contenido editorial disociado de la presentación para facilitar su reutilización.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-use.html" target="_blank">Fragmentos de experiencias</a>:</strong>
            Cree experiencias reutilizables y variaciones optimizadas para escritorio, móvil y canales sociales.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong>Servicios de contenido:</strong>
            Exporte contenido de AEM como JSON para utilizarlo en todos los dispositivos y aplicaciones.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Perspectivas de contenido e integración de Adobe Analytics:</strong>
                Fácil integración de Adobe Analytics y DTM. Mostrar información de rendimiento en el entorno de Author.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/es/experience-manager/6-5/sites/authoring/using/content-targeting-touch.html" target="_blank">Integración de Adobe Target</a>:</strong>
            Asistente paso a paso para crear experiencias segmentadas, crear bibliotecas de ofertas reutilizables.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/es/experience-manager/6-5/sites/administering/using/campaign.html" target="_blank">Integración de Adobe Campaign</a>:</strong>
            Integración sencilla con la solución de campaña de correo electrónico de próxima generación.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html?lang=es" target="_blank">Etiquetas en la integración de Adobe Experience Platform</a>:</strong>
            Integre con el servicio en la nube de administración de etiquetas de próxima generación de Adobe.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Screens:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/es/experience-manager/6-5/sites/administering/using/ecommerce.html" target="_blank">comercio electrónico</a>:</strong>
            Ofrezca experiencias de compra personalizadas y de marca en puntos de contacto web, móviles y sociales.
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
            <td><strong><a href="https://helpx.adobe.com/es/experience-manager/6-5/communities/using/overview.html" target="_blank">Comunidades</a>:</strong>
            Los foros, los comentarios enlazados, los calendarios de eventos y muchas otras funciones permiten una participación profunda con los visitantes del sitio.</td>
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

## Funciones de Assets

A continuación se muestra una matriz de las funciones clave de Assets que ofrece AEM. Algunas de estas funciones se introdujeron en versiones anteriores y se agregaron mejoras incrementales en cada versión.

+ [Notas de la versión de AEM Assets](https://helpx.adobe.com/es/experience-manager/6-5/release-notes/assets.html)

***✔indica que se han realizado mejoras significativas en la característica en esta versión.***

***✔<sup>+</sup> indica que la característica está disponible a través de un Service Pack o Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Función Assets</td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6,1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/es/experience-manager/6-5/assets/using/managing-assets-touch-ui.html" target="_blank">IU táctil optimizada</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/es/experience-manager/6-5/assets/using/metadata.html" target="_blank">Administración avanzada de metadatos</a>:</strong>
            Plantillas de metadatos, Editor de esquemas de metadatos y Edición masiva de metadatos.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/es/experience-manager/6-5/sites/authoring/using/task-content.html" target="_blank">Administración de tareas</a> y flujos de trabajo:</strong>
            Flujos de trabajo y tareas creados previamente para revisar y aprobar recursos digitales que aprovechan los proyectos de AEM.</td>
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
            Compatibilidad mejorada con la ingesta, carga y almacenamiento a escala.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/es/experience-manager/6-5/assets/using/mac-api-assets.html" target="_blank">API HTTP Assets</a>:</strong>
            Interactuar mediante programación con recursos a través de HTTP y JSON.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/es/experience-manager/6-5/assets/using/link-sharing.html" target="_blank">Vínculo compartido</a>:</strong>
            Uso compartido simple y ad hoc de recursos digitales sin necesidad de iniciar sesión.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/es/experience-manager/brand-portal/using/brand-portal.html" target="_blank">Brand Portal</a>:</strong>
            Solución SAAS de Cloud Service para un uso compartido y una distribución de recursos digitales sin problemas.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/es/experience-manager/6-5/assets/using/use-assets-across-connected-assets-instances.html" target="_blank">Assets conectado</a>:</strong>
            Las instancias de AEM Sites pueden acceder a los recursos de una instancia de AEM Assets diferente y utilizarlos sin problemas.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/es/experience-manager/6-5/assets/using/touch-ui-asset-insights.html" target="_blank">Información de recursos</a>:</strong>
            Aproveche Adobe Analytics para capturar la interacción de los clientes con los recursos digitales y verlos en AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/es/experience-manager/6-5/assets/using/multilingual-assets.html" target="_blank">Assets multilingüe</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/es/experience-manager/6-5/assets/using/enhanced-smart-tags.html" target="_blank">Etiquetas inteligentes y moderación</a>:</strong>
            Aproveche la IA de Adobe para etiquetar imágenes automáticamente con metadatos útiles.</td>
            <td> </td>
            <td></td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/assets/using/smart-translation-search-feature-video-use.html" target="_blank">Búsqueda de traducción inteligente</a>:</strong>
            Traducir automáticamente términos de búsqueda al buscar AEM Assets.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/es/experience-manager/6-5/assets/using/indesign.html" target="_blank">Integración de Adobe InDesign Server</a>:</strong>
            Generar catálogos de productos. Cree folletos, prospectos y anuncios impresos basados en plantillas de InDesign.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-desktop-app/using/using.html?lang=es" target="_blank">Aplicación de escritorio de AEM</a>:</strong>
            Sincronice los recursos con el escritorio local para editarlos con los productos de Creative Suite.
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
            <td><strong><a href="https://helpx.adobe.com/es/experience-manager/6-5/assets/using/imaging-transcoding-library.html" target="_blank">Biblioteca de imágenes de Adobe</a>:</strong>
                <br> bibliotecas de Photoshop y Acrobat PDF utilizadas para la manipulación de archivos de alta calidad.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://www.adobe.com/es/creativecloud/business/enterprise/adobe-asset-link.html" target="_blank">Vínculo de recursos de Adobe</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/es/experience-manager/6-5/assets/using/aem-assets-adobe-stock.html" target="_blank">Integración de Adobe Stock</a>:</strong>
            Acceda y utilice imágenes de Adobe Stock directamente desde AEM.</td>
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

***✔<sup>+</sup> mejoras significativas en la característica de esta versión.***

***✔<sup>SP</sup> indica que la característica está disponible a través de un Service Pack o Feature Pack.***


<table>
    <thead>
        <tr>
            <td>Función de Dynamic Media</td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6,1</td>
            <td>6.2</td>
            <td>6.3 +FP</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/es/experience-manager/6-5/assets/using/managing-assets.html" target="_blank">Imágenes</a>:</strong>
            Distribuye de forma dinámica imágenes en diferentes tamaños y formatos, incluido el Recorte inteligente.</td>
            <td> </td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/es/experience-manager/6-5/assets/using/video-profiles.html" target="_blank">Vídeo</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/es/experience-manager/6-5/assets/using/interactive-images.html" target="_blank">Medios interactivos</a>:</strong>
            Cree titulares interactivos y vídeos con contenido en el que se puede hacer clic para mostrar las ofertas clave.
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
            <td><strong>Conjuntos (<a href="https://helpx.adobe.com/es/experience-manager/6-5/assets/using/image-sets.html" target="_blank">Imagen</a>, <a href="https://helpx.adobe.com/es/experience-manager/6-5/assets/using/spin-sets.html" target="_blank">Giro</a>, <a href="https://helpx.adobe.com/es/experience-manager/6-5/assets/using/mixed-media-sets.html" target="_blank">Medios Mixtos</a>):</strong>
            Permite a los usuarios hacer zoom, panoramizar, rotar y simular una experiencia de visualización de 360 grados.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/?lang=es" target="_blank">Visores</a>:</strong>
            Reproductor multimedia enriquecido de marca personalizada y ajustes preestablecidos compatibles con diferentes pantallas o dispositivos.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/es/experience-manager/6-5/assets/using/delivering-dynamic-media-assets.html" target="_blank">Envío</a>:</strong>
            Opciones flexibles para vincular o incrustar contenido y envíos de Dynamic Media a través del protocolo HTTP/2.</td>
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
            Capacidad para migrar recursos principales y seguir utilizando las URL de S7 existentes.</td>
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

A continuación se muestra una matriz de las funciones clave de complementos de AEM Forms que ofrece AEM. Algunas de estas funciones se introdujeron en versiones anteriores y se agregaron mejoras incrementales en cada versión.

***✔<sup>+</sup> mejoras significativas en la característica de esta versión.***

***✔<sup>SP</sup> indica que la característica está disponible a través de un Service Pack o Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Función Forms</td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6,1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/es/experience-manager/6-5/forms/using/introduction-forms-authoring.html" target="_blank">Editor de Forms adaptable</a>:</strong>
            Cree formularios atractivos, interactivos y adaptables basados en la configuración del dispositivo y el explorador.</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/es/experience-manager/6-5/forms/using/generate-document-of-record-for-non-xfa-based-adaptive-forms.html" target="_blank">Documento de registro</a>:</strong>
            Cree un documento para garantizar el almacenamiento a largo plazo de una experiencia de captura de datos o una versión lista para imprimir.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/es/experience-manager/6-5/forms/using/themes.html" target="_blank">Editor de temas</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/es/experience-manager/6-5/forms/using/template-editor.html" target="_blank">Editor de plantillas</a>:</strong>
            Estandarizar e implementar las prácticas recomendadas para los formularios adaptables.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/es/aem-forms/6-5/whats-new.html#EnhancedintegrationwithAdobeSign" target="_blank">Integración de Acrobat Sign</a>:</strong>
            Permitir la implementación de formularios integrados de Acrobat Sign basados en escenarios de firma.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/es/experience-manager/6-5/forms/using/cm-overview.html" target="_blank">Administración de correspondencia</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/es/aem-forms/6-5/whats-new.html#AEMFormsdataintegration" target="_blank">Integración de datos de terceros</a>:</strong>
            Con la integración de datos de, los datos se recuperan de fuentes de datos dispares en función de las entradas del usuario en un formulario. Al enviar el formulario, los datos capturados se vuelven a escribir en las fuentes de datos.
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
            <td><strong><a href="https://helpx.adobe.com/es/aem-forms/6-5/whats-new.html#FormscentricAEMWorkflowsforAEMFormsonOSGi" target="_blank">Flujo de trabajo (en OSGi) para el procesamiento de Forms</a>:</strong>
            Implementación simplificada de los procesos de aprobación de formularios.</td>
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
            <td><strong><a href="https://helpx.adobe.com/es/experience-manager/6-5/forms/using/introduction-managing-forms.html" target="_blank">Administrador de formularios</a>:</strong>
            Ubicación única para administrar todo el formulario, documento o correspondencia, como habilitar el análisis, la traducción, las pruebas A/B, las revisiones y la publicación.
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
            <td><strong><a href="https://helpx.adobe.com/es/experience-manager/6-5/forms/using/aem-forms-app.html" target="_blank">Aplicación de AEM Forms</a>:</strong>
            Permitir el procesamiento de formularios en línea/sin conexión dentro de una aplicación en iOS, Android o Windows.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/es/aem-forms/6-5/adaptive-document.html" target="_blank">Comunicaciones interactivas</a>:</strong>
            Crear comunicaciones enriquecidas como instrucciones segmentadas con elementos interactivos como gráficos (anteriormente conocidos como documentos adaptables).</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong>Flujo de trabajo (J2EE) para el procesamiento de Forms:</strong>
            Cree formularios/flujos de trabajo centrados en documentos complejos mediante un IDE intuitivo.</td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/es/aem-forms/6-5/whats-new.html#EnhancedDocumentSecurity" target="_blank">Seguridad de documentos de AEM Forms</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/es/aem-forms/6-5/whats-new.html#Simplifiedauthoringexperience" target="_blank">Marcos de pruebas</a>:</strong>
            Utilice el marco de Calvin y el complemento de Chrome para admitir y depurar formularios adaptables.</td>
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

