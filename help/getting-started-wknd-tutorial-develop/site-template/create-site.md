---
title: Crear un sitio
seo-title: 'Introducción a AEM Sites: Creación de un sitio'
description: Utilice el Asistente para la creación de sitios en Adobe Experience Manager, AEM, para generar un nuevo sitio web. La plantilla de sitio estándar proporcionada por el Adobe, se utiliza como punto de partida para el nuevo sitio.
sub-product: sitios
version: Cloud Service
type: Tutorial
topic: Administración de contenido
feature: Componentes principales, Editor de páginas
role: Developer
level: Beginner
kt: 7496
thumbnail: KT-7496.jpg
translation-type: tm+mt
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '902'
ht-degree: 0%

---


# Crear un sitio {#create-site}

>[!CAUTION]
>
> Las funciones de creación rápida de sitios que se muestran aquí se lanzarán en el segundo semestre de 2021. La documentación relacionada está disponible con fines de vista previa.

Este capítulo trata la creación de un nuevo sitio en Adobe Experience Manager. La plantilla de sitio estándar, proporcionada por Adobe, se utiliza como punto de partida.

## Requisitos previos {#prerequisites}

Los pasos de este capítulo se llevarán a cabo en un entorno de Adobe Experience Manager as a Cloud Service. Asegúrese de tener acceso administrativo al entorno de AEM. Se recomienda utilizar un [programa de espacio aislado](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/getting-access/sandbox-programs/introduction-sandbox-programs.html) y [entorno de desarrollo](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/manage-environments.html) al completar este tutorial.

Consulte la [documentación de integración](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html) para obtener más información.

## Objetivo {#objective}

1. Aprenda a utilizar el Asistente para la creación de sitios para generar un nuevo sitio.
1. Comprender la función de las plantillas del sitio.
1. Explorar el sitio de AEM generado.

## Inicie sesión en Adobe Experience Manager Author {#author}

Como primer paso, inicie sesión en su AEM como entorno de Cloud Service. AEM entornos se dividen entre un **Author Service** y un **Publish Service**.

* **Servicio de autor** : donde el contenido del sitio se crea, administra y actualiza. Normalmente, solo los usuarios internos tienen acceso al **servicio de autor** y se encuentran detrás de una pantalla de inicio de sesión.
* **Servicio de publicación** : aloja el sitio web activo. Este es el servicio que verán los usuarios finales y que suele estar disponible públicamente.

La mayoría del tutorial se realizará utilizando el **servicio de autor**.

1. Vaya a Adobe Experience Cloud [https://experience.adobe.com/](https://experience.adobe.com/). Inicie sesión con su cuenta personal o una cuenta de empresa/escuela.
1. Asegúrese de que la organización correcta esté seleccionada en el menú y haga clic en **Experience Manager**.

   ![Página principal del Experience Cloud](assets/create-site/experience-cloud-home-screen.png)

1. En **Cloud Manager** haga clic en **Iniciar**.
1. Pase el ratón sobre el programa que desee utilizar y haga clic en el icono **Cloud Manager Program**.

   ![Icono del programa de Cloud Manager](assets/create-site/cloud-manager-program-icon.png)

1. En el menú superior, haga clic en **Entornos** para ver los entornos aprovisionados.

1. Busque el entorno que desea utilizar y haga clic en **Author URL**.

   ![Autor de desarrollo de acceso](assets/create-site/access-dev-environment.png)

   >[!NOTE]
   >
   >Se recomienda utilizar un entorno **Development** para este tutorial.

1. Se iniciará una nueva pestaña en el AEM **Author Service**. Haga clic en **Iniciar sesión con Adobe** y debe iniciar sesión automáticamente con las mismas credenciales de Experience Cloud.

1. Después de ser redirigido y autenticado, debería ver la pantalla de inicio AEM.

   ![AEM pantalla Inicio](assets/create-site/aem-start-screen.png)

>[!NOTE]
>
> ¿Tiene problemas para acceder al Experience Manager? Revise la [documentación de integración](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html)

## Descargar la plantilla de sitio básica

Una plantilla de sitio proporciona un punto de partida para un nuevo sitio. Una plantilla de sitio incluye temas básicos, plantillas de página, configuraciones y contenido de muestra. Exactamente lo que se incluye en la plantilla del sitio corresponde al desarrollador. Adobe proporciona una **Plantilla básica del sitio** para acelerar las nuevas implementaciones.

1. Abra una nueva pestaña del explorador y vaya al proyecto de plantilla de sitio básica en GitHub: [https://github.com/adobe/aem-site-template-basic](https://github.com/adobe/aem-site-template-basic). El proyecto es de código abierto y tiene licencia para ser utilizado por cualquier persona.
1. Haga clic en **Versiones** y vaya a la [última versión](https://github.com/adobe/aem-site-template-basic/releases/latest).
1. Expanda la lista desplegable **Assets** y descargue el archivo zip de la plantilla:

   ![Código postal básico del sitio](assets/create-site/template-basic-zip-file.png)

   Este archivo zip se utilizará en el siguiente ejercicio.

   >[!NOTE]
   >
   > Este tutorial se escribe con la versión **5.0.0** de la plantilla del sitio básica. Si se inicia un nuevo proyecto, siempre se recomienda utilizar la versión más reciente.

## Crear un nuevo sitio

A continuación, genere un nuevo sitio utilizando la plantilla de sitio del ejercicio anterior.

1. Vuelva al entorno de AEM. En la pantalla Inicio de AEM, vaya a **Sitios**.
1. En la esquina superior derecha, haga clic en **Crear** > **Sitio (plantilla)**. Esto mostrará el **Asistente para crear sitio**.
1. En **Seleccione una plantilla de sitio** haga clic en el botón **Importar**.

   Cargue el archivo de plantilla **.zip** descargado del ejercicio anterior.

1. Seleccione **Plantilla básica AEM sitio** y haga clic en **Siguiente**.

   ![Seleccionar plantilla de sitio](assets/create-site/select-site-template.png)

1. En **Detalles del sitio** > **Título del sitio** introduzca `WKND Site`.
1. En **Nombre del sitio** introduzca `wknd`.

   ![Detalles de la plantilla del sitio](assets/create-site/site-template-details.png)

   >[!NOTE]
   >
   > Si utiliza un entorno de AEM compartido, anexe un identificador único al **Nombre del sitio**. Por ejemplo `wknd-johndoe`. Esto garantizará que varios usuarios puedan completar el mismo tutorial, sin conflictos.

1. Haga clic en **Crear** para generar el sitio. Haga clic en **Finalizado** en el cuadro de diálogo **Éxito** cuando AEM haya terminado de crear el sitio web.

## Explorar el nuevo sitio

1. Vaya a la consola de AEM Sites, si no está allí.
1. Se ha generado un nuevo **sitio WKND**. Incluirá una estructura de sitio con una jerarquía multilingüe.
1. Abra la página **English** > **Home** seleccionando la página y haciendo clic en el botón **Edit** en la barra de menús:

   ![Jerarquía del sitio WKND](assets/create-site/wknd-site-starter-hierarchy.png)

1. Ya se ha creado contenido de inicio y hay varios componentes disponibles para añadirlos a una página. Experimente con estos componentes para hacerse una idea de la funcionalidad. En el capítulo siguiente aprenderá los conceptos básicos de un componente.

   ![Iniciar página principal](assets/create-site/start-home-page.png)

   *Contenido de muestra proporcionado por la plantilla de sitio*

## Felicitaciones! {#congratulations}

¡Felicidades, acaba de crear su primer Sitio AEM!

### Pasos siguientes {#next-steps}

Utilice el editor de páginas de Adobe Experience Manager, AEM, para actualizar el contenido del sitio en el capítulo [Author content and publish](author-content-publish.md). Descubra cómo se pueden configurar los componentes atómicos para actualizar contenido. Comprenda la diferencia entre los entornos de AEM Author y Publish y aprenda a publicar actualizaciones en el sitio activo.
