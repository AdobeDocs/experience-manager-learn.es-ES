---
title: Integración de Adobe Experience Manager con Adobe Target mediante Experience Platform Launch y Adobe I/O
seo-title: Integración de Adobe Experience Manager con Adobe Target mediante Experience Platform Launch y Adobe I/O
description: Explicación paso a paso sobre cómo integrar Adobe Experience Manager con Adobe Target mediante Experience Platform Launch y Adobe I/O
seo-description: Explicación paso a paso sobre cómo integrar Adobe Experience Manager con Adobe Target mediante Experience Platform Launch y Adobe I/O
feature: Fragmentos de experiencias
topic: Personalización
role: Developer
level: Intermediate
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '1098'
ht-degree: 2%

---


# Uso de Adobe Experience Platform Launch mediante la consola Adobe I/O

## Requisitos previos

* [AEM creación y publicación de ](./implementation.md#set-up-aem) instancias en los puertos localhost 4502 y 4503 respectivamente
* **Experience Cloud**
   * Acceso a sus organizaciones Adobe Experience Cloud - <https://>`<yourcompany>`.experiencecloud.adobe.com
   * Experience Cloud aprovisionado con las siguientes soluciones
      * [Adobe Experience Platform Launch](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Consola Adobe I/O](https://console.adobe.io)

      >[!NOTE]
      >Debe tener permiso para desarrollar, aprobar, publicar, administrar extensiones y administrar entornos en Launch. Si no puede completar estos pasos porque las opciones de la interfaz de usuario no están disponibles, póngase en contacto con el administrador del Experience Cloud para solicitar el acceso. Para obtener más información sobre los permisos de Launch, [consulte la documentación](https://docs.adobelaunch.com/administration/user-permissions).


* **Complementos del explorador**
   * Adobe Experience Cloud Debugger ([Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj))
   * Conmutador de Launch y DTM ([Chrome](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk))

## Usuarios implicados

Para esta integración, es necesario que participen las siguientes audiencias y, para realizar algunas tareas, es posible que necesite acceso administrativo.

* Desarrollador
* Administrador de AEM
* Administrador de Experience Cloud

## Introducción

AEM ofrece una integración predeterminada con Experience Platform Launch. Esta integración permite a los administradores de AEM configurar fácilmente el Experience Platform Launch mediante una interfaz fácil de usar, lo que reduce el nivel de esfuerzo y el número de errores al configurar estas dos herramientas. Y solo añadiendo la extensión de Adobe Target al Experience Platform Launch nos ayudará a utilizar todas las funciones de Adobe Target en las páginas web AEM.

En esta sección, se tratan los siguientes pasos de integración:

* Iniciar
   * Crear una propiedad de Launch
   * Añadir extensión de Target
   * Crear un elemento de datos
   * Crear una regla de página
   * Configuración de entornos
   * Generar y publicar
* AEM
   * Creación de un Cloud Service
   * Crear

### Iniciar

#### Crear una propiedad de Launch

Una propiedad es un contenedor que se rellena con extensiones, reglas, elementos de datos y bibliotecas al implementar etiquetas en el sitio.

1. Vaya a sus organizaciones [Adobe Experience Cloud](https://experiencecloud.adobe.com/) (<https://>`<yourcompany>`.experiencecloud.adobe.com)
2. Inicie sesión con su Adobe ID y asegúrese de que se encuentra en la organización correcta.
3. En el conmutador de soluciones, haga clic en **Launch** y, a continuación, seleccione el botón **Ir a Launch**.

   ![Experience Cloud: Launch](assets/using-launch-adobe-io/exc-cloud-launch.png)

4. Asegúrese de que está en la organización correcta y, a continuación, continúe creando una propiedad de Launch.
   ![Experience Cloud: Launch](assets/using-launch-adobe-io/launch-create-property.png)

   *Para obtener más información sobre la creación de propiedades, consulte  [Crear una ](https://docs.adobelaunch.com/administration/companies-and-properties#create-a-property) propiedad en la documentación del producto.*
5. Haga clic en el botón **New Property**
6. Proporcione un nombre para su propiedad (por ejemplo, *AEM Tutorial de Target*)
7. Como dominio, introduzca *localhost.com* ya que es el dominio en el que se está ejecutando el sitio de demostración de WKND. Aunque el campo &#39;*Domain*&#39; es obligatorio, la propiedad Launch funcionará en cualquier dominio en el que esté implementada. El objetivo principal de este campo es rellenar previamente las opciones de menú en el Generador de reglas.
8. Haga clic en el botón **Save**.

   ![Launch: Nueva propiedad](assets/using-launch-adobe-io/exc-launch-property.png)

9. Abra la propiedad que acaba de crear y haga clic en la pestaña Extensiones .

#### Añadir extensión de Target

La extensión de Adobe Target es compatible con implementaciones del lado del cliente mediante el SDK de JavaScript de Target para la web moderna, `at.js`. Los clientes que aún utilicen la biblioteca anterior de Target, `mbox.js`, [deben actualizar a at.js](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/upgrading-from-atjs-1x-to-atjs-20.html) para utilizar Launch.

La extensión de Target consta de dos partes principales:

* La configuración de la extensión, que administra la configuración de la biblioteca principal
* Acciones de regla para hacer lo siguiente:
   * Load Target (at.js)
   * Agregar parámetros a todos los mboxes
   * Añadir parámetros al mbox global
   * Fire Global Mbox

1. En **Extensions**, puede ver la lista de extensiones que ya están instaladas para la propiedad de Launch. ([La extensión principal del Experience Platform Launch](https://exchange.adobe.com/experiencecloud.details.100223.adobe-launch-core-extension.html) está instalada de forma predeterminada)
2. Haga clic en la opción **Extension Catalog** y busque Target en el filtro.
3. Seleccione la última versión de Adobe Target at.js y haga clic en la opción **Instalar**.
   ![Launch: Nueva propiedad](assets/using-launch-adobe-io/launch-target-extension.png)

4. Haga clic en el botón **Configure** y puede observar la ventana de configuración con las credenciales de la cuenta de Target importadas y la versión de at.js para esta extensión.
   ![Target: configuración de extensión](assets/using-launch-adobe-io/launch-target-extension-2.png)

   Cuando Target se implementa mediante códigos de incrustación de Launch asincrónicos, debe codificar mediante código duro un fragmento de ocultamiento previo en las páginas antes de los códigos de incrustación de Launch para administrar el parpadeo del contenido. Más adelante aprenderemos más sobre el francotirador preocultado. Puede descargar el fragmento de ocultamiento previo [aquí](assets/using-launch-adobe-io/prehiding.js)

5. Haga clic en **Save** para completar la adición de la extensión de Target a la propiedad de Launch y debería ver la extensión de Target en la lista **Installed** Extensions.

6. Repita los pasos anteriores para buscar la extensión &quot;Servicio de ID de Experience Cloud&quot; e instalarla.
   ![Extensión: Servicio de ID de Experience Cloud](assets/using-launch-adobe-io/launch-extension-experience-cloud.png)

#### Configuración de entornos

1. Haga clic en la ficha **Entorno** de la propiedad del sitio y podrá ver la lista de entornos que se crean para la propiedad del sitio. De forma predeterminada, tenemos una instancia cada una creada para desarrollo, ensayo y producción.

![Elemento de datos - Nombre de página](assets/using-launch-adobe-io/launch-environment-setup.png)

#### Generar y publicar

1. Haga clic en la pestaña **Publicación** de la propiedad del sitio y creemos una biblioteca para crear e implementar nuestros cambios (elementos de datos, reglas) en un entorno de desarrollo.
   >[!VIDEO](https://video.tv.adobe.com/v/28412?quality=12&learn=on)
2. Publique los cambios del entorno Desarrollo a un entorno de ensayo.
   >[!VIDEO](https://video.tv.adobe.com/v/28419?quality=12&learn=on)
3. Ejecute la opción **Build for Staging**.
4. Una vez finalizada la compilación, ejecute **Aprobar para publicación**, que mueve los cambios de un entorno de ensayo a un entorno de producción.
   ![Ensayo en producción](assets/using-launch-adobe-io/build-staging.png)
5. Finalmente, ejecute la opción **Generar y publicar en producción** para insertar los cambios en la producción.
   ![Generar y publicar en producción](assets/using-launch-adobe-io/build-and-publish.png)

### Adobe Experience Manager

>[!VIDEO](https://video.tv.adobe.com/v/28416?quality=12&learn=on)

>[!NOTE]
>
> Conceda a la integración de Adobe I/O acceso para seleccionar espacios de trabajo con la función [adecuada para permitir que un equipo central realice cambios impulsados por API en solo unos pocos espacios de trabajo](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/configure-adobe-io-integration.html).

1. Cree la integración de IMS en AEM usando las credenciales de Adobe I/O. (01:12 a 03:55)
2. En Experience Platform Launch, cree una propiedad. (cubierto [arriba](#create-launch-property))
3. Con la integración de IMS del paso 1, cree una integración de Experience Platform Launch para importar la propiedad de Launch.
4. En AEM, asigne la integración del Experience Platform Launch a un sitio mediante la configuración del explorador. (05:28 a 06:14)
5. Valide la integración manualmente. (06:15 a 06:33)
6. Uso del complemento del explorador Launch/DTM. (06:34 a 06:50)
7. Uso del complemento del explorador Adobe Experience Cloud Debugger. (06:51 a 07:22)

En este punto, ha integrado correctamente [AEM con Adobe Target mediante Adobe Experience Platform Launch](./using-aem-cloud-services.md#integrating-aem-target-options) como se detalla en la Opción 1.

Para utilizar AEM ofertas de fragmentos de experiencia para potenciar sus actividades de personalización, pasemos al capítulo siguiente e integremos AEM con Adobe Target mediante los servicios de nube heredados.
