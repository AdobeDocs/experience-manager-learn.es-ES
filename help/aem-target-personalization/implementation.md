---
title: Integración de Adobe Experience Manager con Adobe Target
seo-title: Un artículo que cubre diferentes maneras de integrar Adobe Experience Manager(AEM) con Adobe Target para ofrecer contenido personalizado.
description: Un artículo que explica cómo configurar Adobe Experience Manager con Adobe Target para diferentes situaciones.
seo-description: Un artículo que explica cómo configurar Adobe Experience Manager con Adobe Target para diferentes situaciones.
feature: Fragmentos de experiencias
topic: Personalización
role: Developer
level: Intermediate
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '700'
ht-degree: 5%

---


# Integración de Adobe Experience Manager con Adobe Target

En esta sección, analizaremos cómo configurar Adobe Experience Manager con Adobe Target para diferentes escenarios. Según el escenario y los requisitos organizativos.

* **Agregar la biblioteca JavaScript de Adobe Target (necesaria en todos los casos)**
En los sitios alojados en AEM, puede agregar las bibliotecas de Target a su sitio mediante  [Launch](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html). Launch ofrece una alternativa sencilla para implementar y gestionar todas las etiquetas necesarias para ofrecer al cliente experiencias más relevantes.
* **Añadir los Cloud Services de Adobe Target (necesarios para el escenario Fragmentos de experiencia)**
Para AEM clientes que deseen utilizar ofertas de fragmento de experiencia para crear una actividad en Adobe Target, deberá integrar Adobe Target con AEM mediante los Cloud Services heredados. Esta integración es necesaria para insertar los fragmentos de experiencias de AEM a Target como ofertas HTML/JSON y para mantener las ofertas sincronizadas con AEM. 
*Esta integración es necesaria para implementar el escenario 1.*

## Requisitos previos

* **Adobe Experience Manager (AEM){#aem}**
   * AEM 6.5 (*se recomienda el último Service Pack*)
   * Descarga AEM paquetes del sitio de referencia WKND
      * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
      * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
      * [Componentes principales](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
      * [Capa de datos digital](assets/implementation/digital-data-layer.zip)

* **Experience Cloud**
   * Acceso a sus organizaciones Adobe Experience Cloud - <https://>`<yourcompany>`.experiencecloud.adobe.com
   * Experience Cloud aprovisionado con las siguientes soluciones
      * [Adobe Experience Platform Launch](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Consola Adobe I/O](https://console.adobe.io)

* **creación**
   * Java 1.8 o Java 11 (solo AEM 6.5 o posterior)
   * Apache Maven (3.3.9 o posterior)
   * Chrome

>[!NOTE]
>
> El cliente debe estar aprovisionado con el Experience Platform Launch y el Adobe I/O de [Adobe support](https://helpx.adobe.com/es/contact/enterprise-support.ec.html) o ponerse en contacto con el administrador del sistema

### Configurar AEM{#set-up-aem}

AEM instancia de creación y publicación es necesaria para completar este tutorial. La instancia de autor se ejecuta en `http://localhost:4502` y la instancia de publicación se ejecuta en `http://localhost:4503`. Para obtener más información, consulte: [Configure un Entorno de desarrollo de AEM local](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/local-aem-dev-environment-article-setup.html).

#### Configuración de instancias de AEM Author y Publish

1. Obtenga una copia del [AEM Jar de inicio rápido y una licencia.](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingtheSoftware)
2. Cree una estructura de carpetas en el equipo como la siguiente:
   ![Estructura de carpetas](assets/implementation/aem-setup-1.png)
3. Cambie el nombre del jar de inicio rápido a `aem-author-p4502.jar` y colóquelo debajo del directorio `/author`. Añada el archivo `license.properties` debajo del directorio `/author`.
   ![Instancia de autor de AEM](assets/implementation/aem-setup-author.png)
4. Haga una copia del jar de inicio rápido, renómbrelo a `aem-publish-p4503.jar` y colóquelo debajo del directorio `/publish`. Agregue una copia del archivo `license.properties` debajo del directorio `/publish`.
   ![Instancia de AEM Publish](assets/implementation/aem-setup-publish.png)
5. Haga doble clic en el archivo `aem-author-p4502.jar` para instalar la instancia de autor. Esto iniciará la instancia de autor, ejecutándose en el puerto 4502 del equipo local.
6. Inicie sesión con las credenciales que se indican a continuación y, una vez que haya iniciado sesión correctamente, se le dirigirá a la pantalla de la página principal de AEM.
username : **admin**
contraseña : **admin**
   ![Instancia de AEM Publish](assets/implementation/aem-author-home-page.png)
7. Haga doble clic en el archivo `aem-publish-p4503.jar` para instalar una instancia de publicación. Puede observar una nueva pestaña abierta en el explorador para su instancia de publicación, que se ejecuta en el puerto 4503 y muestra la página de inicio de WeRetail. Usaremos el sitio de referencia WKND para este tutorial e instalaremos los paquetes en la instancia de autor.
8. Vaya a Autor de AEM en su navegador web en `http://localhost:4502`. En la pantalla Inicio de AEM, vaya a *[Herramientas > Implementación > Paquetes](http://localhost:4502/crx/packmgr/index.jsp)*.
9. Descargue y cargue los paquetes para AEM (enumerados arriba en *[Requisitos previos > AEM](#aem)*)
   * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
   * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
   * [core.wcm.components.all-2.5.0.zip](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
   * [digital-data-layer.zip](assets/implementation/digital-data-layer.zip)

   >[!VIDEO](https://video.tv.adobe.com/v/28377?quality=12&learn=on)
10. Después de instalar los paquetes en AEM Author, seleccione cada paquete cargado en AEM Administrador de paquetes y seleccione **Más > Replicar** para asegurarse de que los paquetes se implementan en AEM Publish.
11. En este punto, ha instalado correctamente su sitio de referencia WKND y todos los paquetes adicionales necesarios para este tutorial.

[SIGUIENTE CAPÍTULO](./using-launch-adobe-io.md): En el capítulo siguiente, integrará Launch con AEM.
