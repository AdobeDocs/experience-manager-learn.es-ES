---
title: Integración de Adobe Experience Manager con Adobe Target mediante Experience Platform Launch y Adobe I/O
seo-title: Integración de Adobe Experience Manager con Adobe Target mediante Experience Platform Launch y Adobe I/O
description: Introducción paso a paso sobre cómo integrar Adobe Experience Manager con Adobe Target mediante Experience Platform Launch y Adobe I/O
seo-description: Introducción paso a paso sobre cómo integrar Adobe Experience Manager con Adobe Target mediante Experience Platform Launch y Adobe I/O
translation-type: tm+mt
source-git-commit: 1209064fd81238d4611369b8e5b517365fc302e3
workflow-type: tm+mt
source-wordcount: '1098'
ht-degree: 2%

---


# Uso de Adobe Experience Platform Launch mediante Adobe I/O Console

## Requisitos previos

* [AEM creación y publicación de ](./implementation.md#set-up-aem) la instalación en los puertos localhost 4502 y 4503 respectivamente
* **Experience Cloud**
   * Acceso a sus organizaciones Adobe Experience Cloud - <https://>`<yourcompany>`.experienceCloud.adobe.com
   * Experience Cloud aprovisionado con las siguientes soluciones
      * [Adobe Experience Platform Launch](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Adobe I/O Console](https://console.adobe.io)

      >[!NOTE]
      >Debe tener permiso para desarrollar, aprobar, publicar, administrar extensiones y administrar Entornos en Launch. Si no puede completar ninguno de estos pasos porque las opciones de la interfaz de usuario no están disponibles para usted, póngase en contacto con el administrador del Experience Cloud para solicitar acceso. Para obtener más información sobre los permisos de Launch, [consulte la documentación](https://docs.adobelaunch.com/administration/user-permissions).


* **Complementos del explorador**
   * Adobe Experience Cloud Debugger ([Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj))
   * Interruptor de inicio y DTM ([Chrome](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk))

## Usuarios involucrados

Para esta integración, es necesario involucrar las siguientes audiencias y, para realizar algunas tareas, es posible que necesite acceso administrativo.

* Desarrollador
* AEM administrador
* Experience Cloud Administrator

## Introducción

AEM ofertas y la integración lista para usar con Experience Platform Launch. Esta integración permite a los administradores de AEM configurar fácilmente el Experience Platform Launch mediante una interfaz fácil de usar, reduciendo así el nivel de esfuerzo y el número de errores al configurar estas dos herramientas. Y sólo agregando la extensión de Adobe Target al Experience Platform Launch nos ayudará a utilizar todas las funciones de Adobe Target en las páginas web AEM.

En esta sección, abarcaríamos los siguientes pasos de integración:

* Iniciar
   * Crear una propiedad de inicio
   * Añadir Extensión de grupo de destinatarios
   * Crear un elemento de datos
   * Crear una regla de página
   * Entornos de configuración
   * Generar y publicar
* AEM
   * Crear un Cloud Service
   * Crear

### Iniciar

#### Crear una propiedad de inicio

Una propiedad es un contenedor que se rellena con extensiones, reglas, elementos de datos y bibliotecas a medida que se implementan las etiquetas en el sitio.

1. Navegue a sus organizaciones [Adobe Experience Cloud](https://experiencecloud.adobe.com/) (<https://>`<yourcompany>`.experience ecloud.adobe.com)
2. Inicie sesión con su Adobe ID y asegúrese de que se encuentra en la organización correcta.
3. En el conmutador de soluciones, haga clic en **Iniciar** y, a continuación, seleccione el botón **Ir a iniciar**.

   ![Experience Cloud - Iniciar](assets/using-launch-adobe-io/exc-cloud-launch.png)

4. Asegúrese de que se encuentra en la organización correcta y, a continuación, continúe creando una propiedad Launch.
   ![Experience Cloud - Iniciar](assets/using-launch-adobe-io/launch-create-property.png)

   *Para obtener más información sobre la creación de propiedades, consulte  [Creación de una ](https://docs.adobelaunch.com/administration/companies-and-properties#create-a-property) propiedad en la documentación del producto.*
5. Haga clic en el botón **Nueva propiedad**
6. Asigne un nombre a la propiedad (por ejemplo, *Tutorial de Destinatario de AEM*)
7. Como dominio, escriba *localhost.com* porque es el dominio en el que se ejecuta el sitio de demostración de WKND. Aunque se requiere el campo &#39;*Dominio*&#39;, la propiedad Launch funcionará en cualquier dominio donde esté implementado. El objetivo principal de este campo es rellenar previamente las opciones de menú en el Generador de reglas.
8. Haga clic en el botón **Guardar**.

   ![Launch - Nueva propiedad](assets/using-launch-adobe-io/exc-launch-property.png)

9. Abra la propiedad que acaba de crear y haga clic en la ficha Extensiones.

#### Añadir Extensión de grupo de destinatarios

La extensión Adobe Target admite implementaciones de cliente mediante el SDK de JavaScript de Destinatario para la Web moderna, `at.js`. Los clientes que aún utilizan la biblioteca más antigua de Destinatario, `mbox.js`, [deben actualizar a at.js](https://docs.adobe.com/content/help/en/target/using/implement-target/client-side/upgrading-from-atjs-1x-to-atjs-20.html) para utilizar Launch.

La Extensión de grupo de destinatarios consta de dos partes principales:

* La configuración de extensión, que administra la configuración de la biblioteca principal
* Acciones de regla para hacer lo siguiente:
   * Cargar Destinatario (at.js)
   * Añadir parámetros a todos los mboxes
   * Añadir parámetros en mbox global
   * Fire Global Mbox

1. En **Extensiones**, puede ver la lista de extensiones que ya están instaladas para la propiedad Launch. ([Experience Platform Launch Core Extension](https://exchange.adobe.com/experiencecloud.details.100223.adobe-launch-core-extension.html) está instalada de forma predeterminada)
2. Haga clic en la opción **Catálogo de extensiones** y busque Destinatarios en el filtro.
3. Seleccione la versión más reciente de Adobe Target at.js y haga clic en la opción **Instalar**.
   ![Launch -Nueva propiedad](assets/using-launch-adobe-io/launch-target-extension.png)

4. Haga clic en el botón **Configurar** y podrá ver la ventana de configuración con las credenciales de cuenta de Destinatario importadas y la versión de at.js para esta extensión.
   ![Destinatario - Configuración de extensión](assets/using-launch-adobe-io/launch-target-extension-2.png)

   Cuando se implementa Destinatario mediante códigos incrustados de Launch asincrónicos, debe codificar un fragmento de ocultación previa en las páginas antes de los códigos incrustados de Launch para administrar el parpadeo del contenido. Más adelante aprenderemos más sobre el francotirador que se oculta previamente. Puede descargar el fragmento de ocultamiento previo [aquí](assets/using-launch-adobe-io/prehiding.js)

5. Haga clic en **Guardar** para completar la adición de la Extensión de grupo de destinatarios a la propiedad Launch y debería poder ver la Extensión de grupo de destinatarios en la lista **Installed** Extensions.

6. Repita los pasos anteriores para buscar la extensión &quot;Servicio de ID de Experience Cloud&quot; e instalarla.
   ![Extensión - Servicio de ID de Experience Cloud](assets/using-launch-adobe-io/launch-extension-experience-cloud.png)

#### Entornos de configuración

1. Haga clic en la ficha **Entorno** de la propiedad del sitio y podrá ver la lista de entornos que se crea para la propiedad del sitio. De forma predeterminada, tenemos una instancia creada para el desarrollo, ensayo y producción.

![Elemento Datos - Nombre de página](assets/using-launch-adobe-io/launch-environment-setup.png)

#### Generar y publicar

1. Haga clic en la ficha **Publicación** de la propiedad del sitio y crearemos una biblioteca para generar e implementar los cambios (elementos de datos, reglas) en un entorno de desarrollo.
   >[!VIDEO](https://video.tv.adobe.com/v/28412?quality=12&learn=on)
2. Publique los cambios desde el desarrollo en un entorno de ensayo.
   >[!VIDEO](https://video.tv.adobe.com/v/28419?quality=12&learn=on)
3. Ejecute la opción **Generar para ensayo**.
4. Una vez finalizada la compilación, ejecute **Aprobar para publicación**, lo cual mueve los cambios de un entorno de ensayo a un entorno de producción.
   ![Ensayo en producción](assets/using-launch-adobe-io/build-staging.png)
5. Finalmente, ejecute la opción **Generar y publicar en producción** para insertar los cambios en la producción.
   ![Generar y publicar en producción](assets/using-launch-adobe-io/build-and-publish.png)

### Adobe Experience Manager

>[!VIDEO](https://video.tv.adobe.com/v/28416?quality=12&learn=on)

>[!NOTE]
>
> Conceda a la integración de Adobe I/O el acceso a espacios de trabajo seleccionados con la función [adecuada para permitir que un equipo central realice cambios impulsados por API en sólo unos pocos espacios de trabajo](https://docs.adobe.com/content/help/en/target/using/administer/manage-users/enterprise/configure-adobe-io-integration.html).

1. Cree la integración de IMS en AEM con las credenciales de Adobe I/O. (01:12 a 03:55)
2. En Experience Platform Launch, cree una propiedad. (cubierto [por encima](#create-launch-property))
3. Con la integración de IMS del paso 1, cree la integración de Experience Platform Launch para importar la propiedad Launch.
4. En AEM, asigne la integración de Experience Platform Launch a un sitio mediante la configuración del explorador. (05:28 a 06:14)
5. Valide la integración manualmente. (06:15 a 06:33)
6. Uso del complemento del explorador Launch/DTM. (06:34 a 06:50)
7. Uso del complemento del navegador Adobe Experience Cloud Debugger. (06:51 a 07:22)

En este punto, ha integrado correctamente [AEM con Adobe Target mediante Adobe Experience Platform Launch](./using-aem-cloud-services.md#integrating-aem-target-options) como se detalla en la opción 1.

Para utilizar AEM ofertas de fragmentos de experiencia con el fin de potenciar sus actividades de personalización, pasemos al siguiente capítulo e integremos AEM con Adobe Target mediante los servicios de nube heredados.
