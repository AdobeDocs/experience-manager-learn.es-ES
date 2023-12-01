---
title: Integración de AEM Sites con Adobe Target
description: Un artículo que explica cómo configurar Adobe Experience Manager con Adobe Target para diferentes escenarios.
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="Integración" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 54a30cd9-d94a-4de5-82a1-69ab2263980d
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '661'
ht-degree: 8%

---

# Integración de AEM Sites con Adobe Target

En esta sección, analizaremos cómo configurar Adobe Experience Manager Sites con Adobe Target para diferentes escenarios. En función de su escenario y de los requisitos de la organización.

* **Agregar la biblioteca JavaScript de Adobe Target (necesario para todos los casos)**
AEM Para los sitios alojados en, puede agregar bibliotecas de Target a su sitio mediante, [Launch](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html). Launch ofrece una alternativa sencilla para implementar y gestionar todas las etiquetas necesarias para potenciar las importantes experiencias del cliente.
* **Añadir los Cloud Service de Adobe Target (requerido para el escenario de Fragmentos de experiencias)**
AEM Para los clientes de, que desean utilizar ofertas de fragmentos de experiencias para crear una actividad en Adobe Target, deberán integrar Adobe Target AEM con los Cloud Service de. AEM Esta integración es necesaria para insertar los fragmentos de experiencias de en Target como ofertas de HTML AEM/JSON, y para mantener las ofertas sincronizadas con la. *Esta integración es necesaria para implementar el escenario 1.*

## Requisitos previos

* **Adobe Experience Manager AEM (){#aem}**
   * AEM,5 (*Se recomienda el paquete de servicio más reciente*)
   * AEM Descargar paquetes de sitios de referencia de WKND de WKND
      * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
      * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
      * [Componentes principales](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
      * [Capa de datos digital](assets/implementation/digital-data-layer.zip)

* **Experience Cloud**
   * Acceso a Adobe Experience Cloud de sus organizaciones: `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud aprovisionado con las siguientes soluciones
      * [Adobe Experience Platform Launch](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Consola de Adobe I/O](https://console.adobe.io)

* **Entorno**
   * Java 1.8 o Java 11 (solo AEM 6.5 o posterior)
   * Apache Maven (3.3.9 o posterior)
   * Chrome

>[!NOTE]
>
> El cliente debe recibir el Experience Platform Launch y el Adobe I/O de [compatibilidad con Adobe](https://helpx.adobe.com/es/contact/enterprise-support.ec.html) o póngase en contacto con el administrador del sistema

### AEM Configuración de la{#set-up-aem}

AEM Se necesita una instancia de autor y publicación para completar este tutorial. La instancia de autor se está ejecutando en `http://localhost:4502` instancia de publicación y que se ejecuta en `http://localhost:4503`. Para obtener más información, consulte: [AEM Configuración de un entorno de desarrollo de Local](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/local-aem-dev-environment-article-setup.html).

#### AEM Configuración de instancias de autor y publicación de

1. Obtenga una copia del [AEM Jar de inicio rápido y licencia.](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingtheSoftware)
2. Cree una estructura de carpetas en el equipo como la siguiente:
   ![Estructura de carpetas](assets/implementation/aem-setup-1.png)
3. Cambie el nombre del JAR de inicio rápido a `aem-author-p4502.jar` y colóquelo debajo de la `/author` directorio. Añada el `license.properties` debajo del archivo `/author` directorio.
   ![AEM Instancia de autor de](assets/implementation/aem-setup-author.png)
4. Haga una copia del JAR de inicio rápido y cambie su nombre a `aem-publish-p4503.jar` y colóquelo debajo de la `/publish` directorio. Añada una copia del `license.properties` debajo del archivo `/publish` directorio.
   ![AEM Instancia de publicación de](assets/implementation/aem-setup-publish.png)
5. Haga doble clic en `aem-author-p4502.jar` para instalar la instancia de autor. Esto iniciará la instancia de autor, que se ejecutará en el puerto 4502 del equipo local.
6. AEM Inicie sesión con las credenciales que se indican a continuación y, cuando el inicio de sesión se haya realizado correctamente, se le dirigirá a la pantalla de la página de inicio de la sesión de.
nombre de usuario : **administrador**
contraseña : **administrador**
   ![AEM Instancia de publicación de](assets/implementation/aem-author-home-page.png)
7. Haga doble clic en `aem-publish-p4503.jar` para instalar una instancia de publicación. Puede ver que se abre una nueva pestaña en el explorador para la instancia de publicación, que se ejecuta en el puerto 4503 y muestra la página de inicio de WeRetail. Estamos utilizando el sitio de referencia de WKND para este tutorial y vamos a instalar los paquetes en la instancia de autor.
8. AEM Vaya a Autor de la en el explorador web en `http://localhost:4502`. AEM En la pantalla Inicio de la, vaya a *[Herramientas > Implementación > Paquetes](http://localhost:4502/crx/packmgr/index.jsp)*.
9. AEM Descargue y cargue los paquetes para su uso (enumerados arriba en ). *[AEM Requisitos previos >](#aem)*)
   * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
   * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
   * [core.wcm.components.all-2.5.0.zip](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
   * [digital-data-layer.zip](assets/implementation/digital-data-layer.zip)

   >[!VIDEO](https://video.tv.adobe.com/v/28377?quality=12&learn=on)
10. AEM AEM Después de instalar los paquetes en el Autor de la, seleccione cada paquete cargado en el Administrador de paquetes y, a continuación, seleccione **Más > Replicar** AEM para asegurarse de que los paquetes se implementan en Publicación de la.
11. En este punto, ha instalado correctamente su sitio de referencia de WKND y todos los paquetes adicionales necesarios para este tutorial.

[CAPÍTULO SIGUIENTE](./using-launch-adobe-io.md)AEM : En el capítulo siguiente, integrará Launch con el servicio de integración de.
