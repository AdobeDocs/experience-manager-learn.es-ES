---
title: Integración de Adobe Experience Manager con Adobe Target mediante etiquetas y Adobe Developer
description: Guía paso a paso sobre cómo integrar Adobe Experience Manager con Adobe Target mediante etiquetas y Adobe Developer
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="Integración" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: b1d7ce04-0127-4539-a5e1-802d7b9427dd
duration: 663
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '985'
ht-degree: 2%

---

# Uso de etiquetas mediante Adobe Developer Console

## Requisitos previos

* AEM [instancia de autor y publicación de](./implementation.md#set-up-aem) que se ejecuta en el puerto localhost 4502 y 4503 respectivamente
* **Experience Cloud**
   * Acceso a Adobe Experience Cloud de sus organizaciones - `https://<yourcompany>.experiencecloud.adobe.com`
   * Aprovisionamiento del Experience Cloud con las siguientes soluciones
      * [Recopilación de datos](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Adobe Developer Console](https://developer.adobe.com/console/)

     >[!NOTE]
     >Debe tener permiso para desarrollar, aprobar, Publish, gestionar extensiones y entornos en la recopilación de datos. Si no puede completar estos pasos porque ninguna de las opciones de la interfaz de usuario está disponible, póngase en contacto con su Experience Cloud de para solicitar el acceso. Para obtener más información sobre los permisos de etiquetas, [consulte la documentación](https://experienceleague.adobe.com/docs/experience-platform/tags/admin/user-permissions.html).

* **Extensiones de explorador Chrome**
   * Adobe Experience Cloud Debugger(https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)

## Usuarios implicados

Para esta integración, deben participar las siguientes audiencias y, para realizar algunas tareas, es posible que necesite acceso administrativo.

* Desarrollador
* AEM Administrador de
* Experience Cloud Administrator

## Introducción

AEM ofrece una integración predeterminada con etiquetas de. AEM Esta integración permite a los administradores de la configurar fácilmente las etiquetas a través de una interfaz fácil de usar, lo que reduce el nivel de esfuerzo y el número de errores al configurar estas dos herramientas. Y solo añadiendo la extensión de Adobe Target a las etiquetas nos ayudará a utilizar todas las funciones de Adobe Target AEM en las páginas web de la.

En esta sección, se tratarán los siguientes pasos de integración:

* Etiquetas
   * Crear una propiedad de etiquetas
   * Añadir extensión de Target
   * Crear un elemento de datos
   * Crear una regla de página
   * Configurar entornos
   * Generar y publicar
* AEM
   * Creación de un Cloud Service
   * Crear

### Etiquetas

#### Creación de una propiedad de etiquetas

Una propiedad es un contenedor que se rellena con extensiones, reglas, elementos de datos y bibliotecas al implementar etiquetas en el sitio.

1. Vaya a sus organizaciones [Adobe Experience Cloud](https://experiencecloud.adobe.com/) (`https://<yourcompany>.experiencecloud.adobe.com`)
1. Inicie sesión con su Adobe ID y asegúrese de que se encuentra en la organización correcta.
1. En el conmutador de soluciones, haga clic en **Experience Platform**, luego en la sección **Recopilación de datos** y seleccione **Etiquetas**.

![Experience Cloud - tags](assets/using-launch-adobe-io/exc-cloud-launch.png)

1. Asegúrese de que se encuentra en la organización correcta y, a continuación, proceda a crear una propiedad de etiquetas.
   ![Experience Cloud - tags](assets/using-launch-adobe-io/launch-create-property.png)

   *Para obtener más información sobre cómo crear propiedades, consulte [Crear una propiedad](https://experienceleague.adobe.com/docs/experience-platform/tags/admin/companies-and-properties.html?lang=en#create-or-configure-a-property) en la documentación del producto.*
1. Haz clic en el botón **Nueva propiedad**
1. AEM Asigne un nombre a su propiedad (por ejemplo, *Tutorial de Target de la aplicación*)
1. Como dominio, introduzca *localhost.com*, ya que es el dominio en el que se ejecuta el sitio de demostración de WKND. Aunque el campo &#39;*Dominio*&#39; es obligatorio, la propiedad tags funcionará en cualquier dominio donde esté implementada. El propósito principal de este campo es rellenar previamente las opciones de menú en el generador de reglas.
1. Haga clic en el botón **Guardar**.

   ![etiquetas - Nueva propiedad](assets/using-launch-adobe-io/exc-launch-property.png)

1. Abra la propiedad que acaba de crear y haga clic en la pestaña Extensiones.

#### Añadir la extensión de Target

La extensión de Adobe Target admite implementaciones del lado del cliente mediante el SDK de JavaScript de Target para la web moderna, `at.js`. Los clientes que todavía usan la biblioteca antigua de Target `mbox.js`, [deben actualizarse a at.js](https://experienceleague.adobe.com/docs/target-dev/developer/client-side/at-js-implementation/upgrading-from-atjs-1x-to-atjs-20.html) para usar etiquetas.

La extensión de Target consta de dos partes principales:

* La configuración de la extensión, que administra la configuración de la biblioteca principal
* Acciones de regla para hacer lo siguiente:
   * Load Target (at.js)
   * Añadir parámetros a todos los mboxes
   * Añadir parámetros a mbox global
   * Fire Global Mbox

1. En **Extensiones**, puede ver la lista de extensiones instaladas para la propiedad de etiquetas. ([La extensión principal de Adobe Launch](https://exchange.adobe.com/apps/ec/100223/adobe-launch-core-extension) está instalada de manera predeterminada)
2. Haga clic en la opción **Catálogo de extensiones** y busque Target en el filtro.
3. Seleccione la última versión de Adobe Target at.js y haga clic en la opción **Instalar**.
   ![Etiquetas - Nueva propiedad](assets/using-launch-adobe-io/launch-target-extension.png)

4. Haga clic en el botón **Configurar** y verá la ventana de configuración con las credenciales de la cuenta de Target importadas y la versión de at.js para esta extensión.
   ![Target - Configuración de extensión](assets/using-launch-adobe-io/launch-target-extension-2.png)

   Cuando Target se implementa mediante códigos incrustados de etiquetas asincrónicas, debe codificar de forma rígida un fragmento de ocultamiento previo en las páginas antes de los códigos incrustados de etiquetas para administrar el parpadeo del contenido. Más adelante aprenderemos más sobre el francotirador preocultado. Puede descargar el fragmento de ocultamiento previo [aquí](assets/using-launch-adobe-io/prehiding.js)

5. Haga clic en **Guardar** para completar la adición de la extensión de Target a su propiedad de etiquetas y ahora debería poder ver la extensión de Target en la lista de extensiones **Instaladas**.

6. Repita los pasos anteriores para buscar la extensión &quot;Servicio de ID de Experience Cloud&quot; e instálela.
   ![Extensión - Servicio de ID de Experience Cloud](assets/using-launch-adobe-io/launch-extension-experience-cloud.png)

#### Configurar entornos

1. Haga clic en la ficha **Entorno** de la propiedad del sitio y verá la lista de entornos que se crean para ella. De forma predeterminada, tenemos una instancia creada para desarrollo, ensayo y producción.

![Elemento de datos - Nombre de página](assets/using-launch-adobe-io/launch-environment-setup.png)

#### Generar y publicar

1. Haga clic en la ficha **Publicación** de la propiedad del sitio. Vamos a crear una biblioteca para generar e implementar nuestros cambios (elementos de datos y reglas) en un entorno de desarrollo.
   >[!VIDEO](https://video.tv.adobe.com/v/28412?quality=12&learn=on)
2. Publish cambia del entorno de desarrollo a un entorno de ensayo.
   >[!VIDEO](https://video.tv.adobe.com/v/28419?quality=12&learn=on)
3. Ejecute la opción **Generar para ensayo**.
4. Una vez completada la compilación, ejecute **Aprobar para publicación**, que mueve los cambios de un entorno de ensayo a un entorno de producción.
   ![Ensayo en producción](assets/using-launch-adobe-io/build-staging.png)
5. Finalmente, ejecute la opción **Generar y Publish en producción** para insertar los cambios en producción.
   ![Generar y Publish en producción](assets/using-launch-adobe-io/build-and-publish.png)

### Adobe Experience Manager

>[!VIDEO](https://video.tv.adobe.com/v/28416?quality=12&learn=on)

>[!NOTE]
>
> Conceda acceso a la integración de Adobe Developer para seleccionar espacios de trabajo con la función [adecuada para permitir que un equipo central realice cambios controlados por API solo en unos pocos espacios de trabajo](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/configure-adobe-io-integration.html).

1. AEM Cree la integración de IMS en el con las credenciales de Adobe Developer. (1:12 a 03:55)
2. En Recopilación de datos, cree una propiedad. (cubierto [arriba](#create-launch-property))
3. Con la integración de IMS del paso 1, cree la integración de etiquetas para importar la propiedad de etiquetas.
4. AEM En, asigne la integración de etiquetas a un sitio mediante la configuración del explorador. (05:28 a 06:14)
5. Valide la integración manualmente. (06:15 a 06:33)
6. Uso del complemento del explorador de Adobe Experience Cloud Debugger. (06:51 a 07:22)

AEM En este punto, ha integrado correctamente [con Adobe Target mediante etiquetas](./using-aem-cloud-services.md#integrating-aem-target-options), tal como se detalla en la Opción 1.

AEM AEM Para utilizar ofertas de fragmentos de experiencias de la aplicación para ofrecerle posibilidades en las actividades de personalización, continúe con el siguiente capítulo e integre la aplicación con Adobe Target mediante los servicios en la nube heredados.
