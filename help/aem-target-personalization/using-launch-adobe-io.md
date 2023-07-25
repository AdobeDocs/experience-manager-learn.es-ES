---
title: Integración de Adobe Experience Manager con Adobe Target mediante Experience Platform Launch y Adobe I/O
seo-title: Integrating Adobe Experience Manager with Adobe Target using Experience Platform Launch and Adobe I/O
description: Guía paso a paso sobre cómo integrar Adobe Experience Manager con Adobe Target mediante Experience Platform Launch y Adobe I/O
seo-description: Step by step walk-through on how to integrate Adobe Experience Manager with Adobe Target using Experience Platform Launch and Adobe I/O
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="Integración" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
exl-id: b1d7ce04-0127-4539-a5e1-802d7b9427dd
source-git-commit: b044c9982fc9309fb73509dd3117f5467903bd6a
workflow-type: tm+mt
source-wordcount: '1067'
ht-degree: 4%

---

# Uso de Adobe Experience Platform Launch a través de la consola de Adobe I/O

## Requisitos previos

* [AEM Instancia de autor y publicación de](./implementation.md#set-up-aem) ejecutándose en el puerto localhost 4502 y 4503 respectivamente
* **Experience Cloud**
   * Acceso a Adobe Experience Cloud de sus organizaciones: `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud aprovisionado con las siguientes soluciones
      * [Adobe Experience Platform Launch](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Consola de Adobe I/O](https://console.adobe.io)

     >[!NOTE]
     >Debe tener permiso para desarrollar, aprobar, publicar, gestionar extensiones y entornos en Launch. Si no puede completar estos pasos porque ninguna de las opciones de la interfaz de usuario está disponible, póngase en contacto con su Experience Cloud de para solicitar el acceso. Para obtener más información sobre los permisos de Launch, [consulte la documentación](https://experienceleague.adobe.com/docs/experience-platform/tags/admin/user-permissions.html).

* **Complementos del explorador**
   * Adobe Experience Cloud Debugger ([Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj))
   * Conmutador de Launch y DTM ([Chrome](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk))

## Usuarios implicados

Para esta integración, deben participar las siguientes audiencias y, para realizar algunas tareas, puede necesitar acceso administrativo.

* Desarrollador
* AEM Administrador de
* Experience Cloud Administrator

## Introducción

AEM ofrece una integración predeterminada con Experience Platform Launch. AEM Esta integración permite a los administradores de la aplicación configurar fácilmente el Experience Platform Launch mediante una interfaz fácil de usar, lo que reduce el nivel de esfuerzo y el número de errores al configurar estas dos herramientas. Y solo añadiendo la extensión de Adobe Target a Experience Platform Launch nos ayudará a utilizar todas las funciones de Adobe Target AEM en las páginas web de la.

En esta sección, se tratarán los siguientes pasos de integración:

* Iniciar
   * Crear una propiedad de Launch
   * Añadir extensión de Target
   * Crear un elemento de datos
   * Crear una regla de página
   * Configurar entornos
   * Generar y publicar
* AEM
   * Creación de un Cloud Service
   * Crear

### Iniciar

#### Crear una propiedad de Launch

Una propiedad es un contenedor que se rellena con extensiones, reglas, elementos de datos y bibliotecas al implementar etiquetas en el sitio.

1. Navegar a sus organizaciones [Adobe Experience Cloud](https://experiencecloud.adobe.com/) (`https://<yourcompany>.experiencecloud.adobe.com`)
2. Inicie sesión con su Adobe ID y asegúrese de que se encuentra en la organización correcta.
3. En el conmutador de soluciones, haga clic en **Launch** y luego seleccione la **Ir a Launch** botón.

   ![Experience Cloud - Inicio](assets/using-launch-adobe-io/exc-cloud-launch.png)

4. Asegúrese de que está en la organización correcta y, a continuación, continúe con la creación de una propiedad de Launch.
   ![Experience Cloud - Inicio](assets/using-launch-adobe-io/launch-create-property.png)

   *Para obtener más información sobre la creación de propiedades, consulte [Crear una propiedad](https://experienceleague.adobe.com/docs/experience-platform/tags/admin/companies-and-properties.html?lang=en#create-or-configure-a-property) en la documentación del producto.*
5. Haga clic en **Nueva propiedad** botón
6. Proporcione un nombre para la propiedad (por ejemplo, *AEM Tutorial de Target*)
7. Como dominio, introduzca *localhost.com* ya que es el dominio en el que se ejecuta el sitio de demostración de WKND. Aunque el &#39;*Dominio*&#39; es obligatorio, la propiedad Launch funcionará en cualquier dominio en el que esté implementada. El propósito principal de este campo es rellenar previamente las opciones de menú en el generador de reglas.
8. Haga clic en **Guardar** botón.

   ![Launch: nueva propiedad](assets/using-launch-adobe-io/exc-launch-property.png)

9. Abra la propiedad que acaba de crear y haga clic en la pestaña Extensiones.

#### Añadir extensión de Target

La extensión de Adobe Target es compatible con implementaciones del lado del cliente mediante el SDK de JavaScript de Target para la web moderna, `at.js`. Clientes que aún utilizan la biblioteca antigua de Target, `mbox.js`, [debe actualizar a at.js](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/upgrading-from-atjs-1x-to-atjs-20.html) para utilizar Launch.

La extensión de Target consta de dos partes principales:

* La configuración de la extensión, que administra la configuración de la biblioteca principal
* Acciones de regla para hacer lo siguiente:
   * Load Target (at.js)
   * Añadir parámetros a todos los mboxes
   * Añadir parámetros a mbox global
   * Fire Global Mbox

1. En **Extensiones**, puede ver la lista de extensiones que ya están instaladas para la propiedad de Launch. ([Extensión principal de Experience Platform Launch](https://exchange.adobe.com/experiencecloud.details.100223.adobe-launch-core-extension.html) está instalado de forma predeterminada)
2. Haga clic en **Catálogo de extensiones** y busque Target en el filtro.
3. Seleccione la última versión de Adobe Target at.js y haga clic en **Instalar** opción.
   ![Launch: Nueva propiedad](assets/using-launch-adobe-io/launch-target-extension.png)

4. Haga clic en **Configurar** y verá la ventana de configuración con las credenciales de la cuenta de Target importadas y la versión de at.js para esta extensión.
   ![Target: configuración de extensión](assets/using-launch-adobe-io/launch-target-extension-2.png)

   Cuando Target se implementa mediante códigos de incrustación de Launch asincrónicos, debe codificar un fragmento de ocultamiento previo en las páginas antes de los códigos de incrustación de Launch para administrar el parpadeo del contenido. Más adelante aprenderemos más sobre el francotirador preocultado. Puede descargar el fragmento de ocultamiento previo [aquí](assets/using-launch-adobe-io/prehiding.js)

5. Clic **Guardar** para completar la adición de la extensión de Target a la propiedad de Launch. Ahora debería poder ver la extensión de Target en la lista de la propiedad **Instalado** lista de extensiones.

6. Repita los pasos anteriores para buscar la extensión &quot;Servicio de ID de Experience Cloud&quot; e instálela.
   ![Extensión: servicio de ID de Experience Cloud](assets/using-launch-adobe-io/launch-extension-experience-cloud.png)

#### Configurar entornos

1. Haga clic en **Entorno** para la propiedad del sitio y puede ver la lista de entornos que se crean para la propiedad del sitio. De forma predeterminada, tenemos una instancia creada para desarrollo, ensayo y producción.

![Elemento de datos - Nombre de página](assets/using-launch-adobe-io/launch-environment-setup.png)

#### Generar y publicar

1. Haga clic en **Publicación** para su propiedad del sitio y vamos a crear una biblioteca para crear e implementar nuestros cambios (elementos de datos, reglas) en un entorno de desarrollo.
   >[!VIDEO](https://video.tv.adobe.com/v/28412?quality=12&learn=on)
2. Publique los cambios del entorno de desarrollo en un entorno de ensayo.
   >[!VIDEO](https://video.tv.adobe.com/v/28419?quality=12&learn=on)
3. Ejecute el **Opción Generar para ensayo**.
4. Una vez completada la generación, ejecute **Aprobar para publicación**, que cambia de un entorno de ensayo a un entorno de producción.
   ![Ensayo en producción](assets/using-launch-adobe-io/build-staging.png)
5. Finalmente, ejecute el **Generar y publicar en producción** para insertar los cambios en producción.
   ![Generar y publicar en producción](assets/using-launch-adobe-io/build-and-publish.png)

### Adobe Experience Manager

>[!VIDEO](https://video.tv.adobe.com/v/28416?quality=12&learn=on)

>[!NOTE]
>
> Conceder acceso a la integración de Adobe I/O para seleccionar espacios de trabajo con el [función para permitir que un equipo central realice cambios controlados por API solo en unos pocos espacios de trabajo](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/configure-adobe-io-integration.html).

1. AEM Cree la integración de IMS en la con las credenciales de la Adobe I/O. (1:12 a 03:55)
2. En Experience Platform Launch, cree una propiedad. (cubierto [superior](#create-launch-property))
3. Con la integración de IMS del paso 1, cree una integración de Experience Platform Launch para importar la propiedad de Launch.
4. AEM En, asigne la integración del Experience Platform Launch a un sitio mediante la configuración del explorador. (05:28 a 06:14)
5. Valide la integración manualmente. (06:15 a 06:33)
6. Uso del complemento del explorador Launch/DTM. (06:34 a 06:50)
7. Uso del complemento del explorador de Adobe Experience Cloud Debugger. (06:51 a 07:22)

En este punto, se ha integrado correctamente [AEM con Adobe Target usando Adobe Experience Platform Launch](./using-aem-cloud-services.md#integrating-aem-target-options) como se detalla en la opción 1.

AEM AEM Para utilizar ofertas de fragmentos de experiencias de la aplicación para ofrecerle posibilidades en las actividades de personalización, continúe con el siguiente capítulo e integre la aplicación con Adobe Target mediante los servicios en la nube heredados.
