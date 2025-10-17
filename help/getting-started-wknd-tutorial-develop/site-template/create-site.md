---
title: Crear un sitio | Creación rápida de sitios de AEM
description: Aprenda a utilizar el Asistente para la creación de sitios para generar un nuevo sitio web. La plantilla de sitio estándar proporcionada por Adobe es un punto de partida para el nuevo sitio.
version: Experience Manager as a Cloud Service
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
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '959'
ht-degree: 100%

---

# Creación de un sitio {#create-site}

Como parte de la Creación rápida de sitios, utilice el Asistente para la creación de sitios de Adobe Experience Manager, AEM, para generar un nuevo sitio web. La plantilla de sitio estándar proporcionada por Adobe se utiliza como punto de partida para el nuevo sitio.

## Requisitos previos {#prerequisites}

Los pasos de este capítulo se realizarán en un entorno de Adobe Experience Manager as a Cloud Service. Asegúrese de que tiene acceso administrativo al entorno de AEM. Se recomienda usar un [programa de zona protegida](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/getting-access/sandbox-programs/introduction-sandbox-programs.html?lang=es) y un [entorno de desarrollo](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/manage-environments.html?lang=es) al completar este tutorial.

Los entornos de [Programa de producción](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs.html?lang=es) también se pueden usar para este tutorial; sin embargo, asegúrese de que las actividades de este tutorial no afecten al trabajo que se está realizando en los entornos de destino, ya que este tutorial implementa contenido y código en el entorno de AEM de destino.

[AEM SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=es) se puede usar como parte de este tutorial. Los aspectos de este tutorial que dependen de los servicios en la nube, como [implementar temáticas con la canalización front-end de Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/theming.html?lang=es), no se pueden ejecutar en AEM SDK.

Consulte la [documentación de incorporación](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html?lang=es) para obtener más información.

## Objetivo {#objective}

1. Aprenda a utilizar el Asistente para la creación de sitios para generar un nuevo sitio.
1. Comprenda la función de las plantillas de sitio.
1. Explore el sitio de AEM generado.

## Iniciar sesión en Adobe Experience Manager Author {#author}

Como primer paso, inicie sesión en su entorno de AEM as a Cloud Service. Los entornos de AEM se dividen entre **Servicio de creación** y **Servicio de publicación**.

* **Servicio de creación**: donde se crea, administra y actualiza el contenido del sitio. Normalmente, solo los usuarios internos tienen acceso al **Servicio de creación**, que se encuentra detrás de una pantalla de inicio de sesión.
* **Servicio de publicación**: aloja el sitio web en tiempo real. Este es el servicio que ven los usuarios finales y que suele estar disponible públicamente.

La mayoría del tutorial se realizará mediante el **Servicio de creación**.

1. Vaya a Adobe Experience Cloud [https://experience.adobe.com/](https://experience.adobe.com/). Inicie sesión con su cuenta personal o con una cuenta de empresa/escuela.
1. Asegúrese de que esté seleccionada la organización correcta en el menú y haga clic en **Experience Manager**.

   ![Inicio de Experience Cloud](assets/create-site/experience-cloud-home-screen.png)

1. En **Cloud Manager**, haga clic en **Iniciar**.
1. Pase el ratón sobre el programa que quiera usar y luego haga clic en el icono **Programa de Cloud Manager**.

   ![Icono del programa Cloud Manager](assets/create-site/cloud-manager-program-icon.png)

1. En el menú superior, haga clic en **Entornos** para ver los entornos aprovisionados.

1. Busque el entorno que desea usar y haga clic en la **URL del autor**.

   ![Autor de desarrollo de acceso](assets/create-site/access-dev-environment.png)

   >[!NOTE]
   >
   >Se recomienda usar un entorno de **Desarrollo** para este tutorial.

1. Se inicia una nueva pestaña en el **servicio de creación** de AEM. Haga clic en **Iniciar sesión con Adobe** y deberá iniciar sesión automáticamente con las mismas credenciales de Experience Cloud.

1. Después de ser redirigido y autenticado, ahora debería ver la pantalla de inicio de AEM.

   ![Pantalla de inicio de AEM](assets/create-site/aem-start-screen.png)

>[!NOTE]
>
> ¿Tiene problemas para acceder a Experience Manager? Revise la [documentación de incorporación](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html?lang=es)

## Descargar la plantilla de sitio básica

Una plantilla de sitio proporciona el punto de partida para un sitio nuevo. Una plantilla de sitio incluye algunas temáticas básicas, plantillas de página, configuraciones y contenido de muestra. Exactamente lo que se incluye en la plantilla del sitio depende del desarrollador. Adobe proporciona una **plantilla básica del sitio** para acelerar las nuevas implementaciones.

1. Abra una nueva pestaña del explorador y vaya al proyecto Plantilla de sitio básica en GitHub: [https://github.com/adobe/aem-site-template-standard](https://github.com/adobe/aem-site-template-standard). El proyecto es de código abierto y tiene licencia para ser utilizado por cualquier persona.
1. Haga clic en **Versiones** y vaya a la [última versión](https://github.com/adobe/aem-site-template-standard/releases/latest).
1. Expanda el menú desplegable **Recursos** y descargue el archivo zip de la plantilla:

   ![Archivo zip de la plantilla básica del sitio](assets/create-site/template-basic-zip-file.png)

   Este archivo zip se utilizará en el siguiente ejercicio.

   >[!NOTE]
   >
   > Este tutorial se ha escrito con la versión **1.1.0** de la plantilla básica del sitio. Al iniciar un nuevo proyecto para su uso en producción, siempre se recomienda utilizar la versión más reciente.

## Creación de un nuevo sitio

A continuación, genere un nuevo sitio con la plantilla de sitio del ejercicio anterior.

1. Vuelva al entorno de AEM. En la pantalla de inicio de AEM, vaya a **Sitios**.
1. En la esquina superior derecha, haga clic en **Crear** > **Sitio (plantilla)**. Aparecerá el **Asistente para crear sitio**.
1. En **Seleccionar una plantilla de sitio**, haga clic en el botón **Importar**.

   Cargue el archivo de plantilla **.zip** descargado del ejercicio anterior.

1. Seleccione la **Plantilla básica del sitio AEM** y haga clic en **Siguiente**.

   ![Seleccionar una plantilla de sitio](assets/create-site/select-site-template.png)

1. En **Detalles del sitio** > **Título del sitio**, escriba `WKND Site`.

   En una implementación real, “Sitio WKND” se sustituiría por el nombre de marca de su compañía u organización. En este tutorial, simulamos la creación de un sitio para una marca ficticia de estilo de vida “WKND”.

1. En **Nombre del sitio**, escriba `wknd`.

   ![Detalles de la plantilla de sitio](assets/create-site/site-template-details.png)

   >[!NOTE]
   >
   > Si usa un entorno de AEM compartido, añada un identificador único al **Nombre del sitio**. Por ejemplo, `wknd-site-johndoe`. Esto garantizará que varios usuarios puedan completar el mismo tutorial, sin ningún conflicto.

1. Haga clic en **Crear** para generar el sitio. Haga clic en **Listo** en el cuadro de diálogo **Éxito** cuando AEM haya terminado de crear el sitio web.

## Explore el nuevo sitio

1. Vaya a la consola de AEM Sites, si aun no está allí.
1. Se ha generado un nuevo **sitio WKND**. Incluirá una estructura de sitio con una jerarquía multilingüe.
1. Para abrir la página **Inglés** > **Inicio**, seleccione la página y haga clic en el botón **Editar** de la barra de menús:

   ![Jerarquía del sitio de WKND](assets/create-site/wknd-site-starter-hierarchy.png)

1. El contenido de inicio ya se ha creado y hay varios componentes disponibles para añadirlos a una página. Experimente con estos componentes para hacerse una idea de la funcionalidad. Aprenderá los conceptos básicos de un componente en el capítulo siguiente.

   ![Iniciar página principal](assets/create-site/start-home-page.png)

   *Contenido de muestra proporcionado por la plantilla del sitio*

## Enhorabuena. {#congratulations}

Enhorabuena. Acaba de crear su primer sitio de AEM.

### Próximos pasos {#next-steps}

Use el editor de páginas en Adobe Experience Manager, AEM, para actualizar el contenido del sitio en el capítulo [Crear de contenido y publicar](author-content-publish.md). Descubra cómo se pueden configurar los componentes atómicos para actualizar el contenido. Comprenda la diferencia entre los entornos de AEM Author y Publish, y aprenda a publicar actualizaciones en el sitio en directo.
