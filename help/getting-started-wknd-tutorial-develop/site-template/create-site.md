---
title: Crear un sitio | AEM Creación rápida de sitios de
description: Aprenda a utilizar el Asistente para la creación de sitios para generar un nuevo sitio web. La plantilla de sitio estándar proporcionada por Adobe es un punto de partida para el nuevo sitio.
version: Cloud Service
topic: Content Management
feature: Core Components, Page Editor
role: Developer
level: Beginner
jira: KT-7496
thumbnail: KT-7496.jpg
doc-type: Tutorial
exl-id: 6d0fdc4d-d85f-4966-8f7d-d53506a7dd08
recommendations: noDisplay, noCatalog
duration: 198
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '959'
ht-degree: 1%

---

# Crear un sitio {#create-site}

Como parte de la Creación rápida de sitios, utilice el Asistente para la creación de sitios de Adobe Experience Manager AEM, en la documentación de, para generar un nuevo sitio web. La plantilla de sitio estándar proporcionada por Adobe se utiliza como punto de partida para el nuevo sitio.

## Requisitos previos {#prerequisites}

Los pasos de este capítulo se realizarán en un entorno de Adobe Experience Manager as a Cloud Service. AEM Asegúrese de que tiene acceso administrativo al entorno de la. Se recomienda usar un [programa de espacio aislado](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/getting-access/sandbox-programs/introduction-sandbox-programs.html) y [entorno de desarrollo](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/manage-environments.html) al completar este tutorial.

AEM Los entornos [Production program](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs.html) también se pueden usar para este tutorial; sin embargo, asegúrese de que las actividades de este tutorial no afecten al trabajo que se está realizando en los entornos de destino, ya que este tutorial implementa contenido y código en el entorno de destino.

AEM El [SDK de](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html) se puede usar en partes de este tutorial. Los aspectos de este tutorial que dependen de los servicios en la nube, como [la implementación de temáticas con la canalización front-end de Cloud Manager AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/theming.html), no se pueden ejecutar en el SDK de la.

Revise la [documentación de incorporación](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html?lang=es) para obtener más detalles.

## Objetivo {#objective}

1. Aprenda a utilizar el Asistente para la creación de sitios para generar un nuevo sitio.
1. Comprender la función de las plantillas de sitio.
1. AEM Explore el sitio de la generado.

## Iniciar sesión en Adobe Experience Manager Author {#author}

Como primer paso, inicie sesión en su entorno de AEM as a Cloud Service. AEM Los entornos de se dividen entre **Servicio de creación** y **Servicio de Publish**.

* **Servicio de creación**: donde se crea, administra y actualiza el contenido del sitio. Normalmente, solo los usuarios internos tienen acceso al **servicio de creación** y se encuentra detrás de una pantalla de inicio de sesión.
* **Servicio de Publish**: aloja el sitio web activo. Este es el servicio que ven los usuarios finales y que suele estar disponible públicamente.

La mayoría del tutorial se realizará mediante el **servicio de creación**.

1. Vaya a Adobe Experience Cloud [https://experience.adobe.com/](https://experience.adobe.com/). Inicie sesión con su cuenta personal o con una cuenta de empresa/escuela.
1. Asegúrese de que la organización correcta esté seleccionada en el menú y haga clic en **Experience Manager**.

   ![Página principal de Experience Cloud](assets/create-site/experience-cloud-home-screen.png)

1. En **Cloud Manager**, haga clic en **Iniciar**.
1. Pase el ratón sobre el programa que quiera usar y luego haga clic en el icono **Programa de Cloud Manager**.

   ![Icono del programa Cloud Manager](assets/create-site/cloud-manager-program-icon.png)

1. En el menú superior, haga clic en **Entornos** para ver los entornos aprovisionados.

1. Busque el entorno que desea usar y haga clic en la **URL del autor**.

   ![Autor de desarrollo de acceso](assets/create-site/access-dev-environment.png)

   >[!NOTE]
   >
   >Se recomienda usar un entorno **Development** para este tutorial.

1. AEM Se inicia una nueva pestaña en el servicio de creación de **.** Haga clic en **Iniciar sesión con el Adobe** y deberá iniciar sesión automáticamente con las mismas credenciales de Experience Cloud.

1. AEM Después de ser redirigido y autenticado, ahora debería ver la pantalla de inicio de la.

   AEM ![Pantalla de inicio de](assets/create-site/aem-start-screen.png)

>[!NOTE]
>
> ¿Tiene problemas para acceder al Experience Manager? Revisar la [documentación de incorporación](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html?lang=es)

## Descargar la plantilla básica del sitio

Una plantilla de sitio proporciona un punto de partida para un nuevo sitio. Una plantilla de sitio incluye algunas temáticas básicas, plantillas de página, configuraciones y contenido de muestra. Exactamente lo que se incluye en la plantilla del sitio depende del desarrollador. El Adobe proporciona una **plantilla básica del sitio** para acelerar las nuevas implementaciones.

1. Abra una nueva pestaña del explorador y vaya al proyecto Plantilla de sitio básica en GitHub: [https://github.com/adobe/aem-site-template-standard](https://github.com/adobe/aem-site-template-standard). El proyecto es de código abierto y tiene licencia para ser utilizado por cualquier persona.
1. Haga clic en **Versiones** y vaya a la [última versión](https://github.com/adobe/aem-site-template-standard/releases/latest).
1. Expanda el menú desplegable **Assets** y descargue el archivo zip de plantilla:

   ![Código postal de plantilla básica del sitio](assets/create-site/template-basic-zip-file.png)

   Este archivo zip se utilizará en el siguiente ejercicio.

   >[!NOTE]
   >
   > Este tutorial se ha escrito con la versión **1.1.0** de la plantilla de sitio básica. Al iniciar un nuevo proyecto para su uso en producción, siempre se recomienda utilizar la versión más reciente.

## Crear un nuevo sitio

A continuación, genere un nuevo sitio con la plantilla de sitio del ejercicio anterior.

1. AEM Vuelva al entorno de la. AEM Desde la pantalla Inicio de la, vaya a **Sitios**.
1. En la esquina superior derecha, haga clic en **Crear** > **Sitio (plantilla)**. Esto mostrará el **Asistente para crear sitio**.
1. En **Seleccionar una plantilla de sitio**, haga clic en el botón **Importar**.

   Cargar el archivo de plantilla **.zip** descargado del ejercicio anterior.

1. AEM Seleccione la **Plantilla básica del sitio de la** y haga clic en **Siguiente**.

   ![Seleccionar plantilla de sitio](assets/create-site/select-site-template.png)

1. En **Detalles del sitio** > **Título del sitio**, escriba `WKND Site`.

   En una implementación real, &quot;Sitio WKND&quot; se sustituiría por el nombre de marca de su empresa u organización. En este tutorial, simulamos la creación de un sitio para una marca ficticia de estilo de vida &quot;WKND&quot;.

1. En **Nombre del sitio**, escriba `wknd`.

   ![Detalles de plantilla del sitio](assets/create-site/site-template-details.png)

   >[!NOTE]
   >
   > AEM Si usa un entorno de compartido, agregue un identificador único al **Nombre del sitio**. Por ejemplo, `wknd-site-johndoe`. Esto garantizará que varios usuarios puedan completar el mismo tutorial, sin ningún conflicto.

1. Haga clic en **Crear** para generar el sitio. AEM Haga clic en **Listo** en el cuadro de diálogo **Éxito** cuando haya terminado de crear el sitio web, por lo que el usuario debe hacer clic en el botón de diálogo Éxito.

## Explorar el nuevo sitio

1. Vaya a la consola de AEM Sites, si no está allí.
1. Se ha generado un nuevo **sitio WKND**. Incluirá una estructura de sitio con una jerarquía multilingüe.
1. Para abrir la página **English** > **Home**, seleccione la página y haga clic en el botón **Editar** de la barra de menús:

   ![Jerarquía del sitio WKND](assets/create-site/wknd-site-starter-hierarchy.png)

1. El contenido de inicio ya se ha creado y hay varios componentes disponibles para añadirlos a una página. Experimente con estos componentes para hacerse una idea de la funcionalidad. Aprenderá los conceptos básicos de un componente en el capítulo siguiente.

   ![Iniciar página de inicio](assets/create-site/start-home-page.png)

   *Contenido de muestra proporcionado por la plantilla del sitio*

## Enhorabuena. {#congratulations}

AEM ¡Felicidades, acabas de crear tu primer sitio en la red de la red de la red de Internet

### Siguientes pasos {#next-steps}

Use el Editor de páginas en Adobe Experience Manager AEM, en la, para actualizar el contenido del sitio en el capítulo [Crear contenido y publicar](author-content-publish.md). Descubra cómo se pueden configurar los componentes atómicos para actualizar el contenido. AEM Comprenda la diferencia entre los entornos de un autor de y de Publish, y aprenda a publicar actualizaciones en el sitio en directo.
