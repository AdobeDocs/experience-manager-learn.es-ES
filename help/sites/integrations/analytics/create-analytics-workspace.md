---
title: Analizar datos con Analysis Workspace
description: Obtenga información sobre cómo asignar datos capturados desde un sitio de Adobe Experience Manager a métricas y dimensiones en grupos de informes de Adobe Analytics. Obtenga información sobre cómo crear un tablero de informes detallado con la función Analysis Workspace de Adobe Analytics.
version: Cloud Service
topic: Integrations
feature: Adobe Client Data Layer
role: User
level: Intermediate
jira: KT-6409
thumbnail: KT-6296.jpg
doc-type: Tutorial
exl-id: b5722fe2-93bf-4b25-8e08-4cb8206771cb
badgeIntegration: label="Integración" type="positive"
last-substantial-update: 2022-06-15T00:00:00Z
duration: 601
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '2072'
ht-degree: 0%

---

# Analizar datos con Analysis Workspace

Obtenga información sobre cómo asignar datos capturados desde un sitio de Adobe Experience Manager a métricas y dimensiones en grupos de informes de Adobe Analytics. Obtenga información sobre cómo crear un tablero de informes detallado con la función Analysis Workspace de Adobe Analytics.

## Lo que va a generar {#what-build}

El equipo de marketing de WKND está interesado en saber cuál `Call to Action (CTA)` Los botones funcionan mejor en la página principal. En este tutorial, cree un proyecto en la **Analysis Workspace** para visualizar el rendimiento de diferentes botones de CTA y comprender el comportamiento del usuario en el sitio. La siguiente información se captura mediante Adobe Analytics cuando un usuario hace clic en un botón Llamada a la acción (CTA) en la página principal de WKND.

**Variables de Analytics**

A continuación se muestran las variables de Analytics de las que se está realizando un seguimiento:

* `eVar5` -  `Page template`
* `eVar6` - `Page Id`
* `eVar7` - `Page last modified date`
* `eVar8` - `CTA Button Id`
* `eVar9` - `Page Name`
* `event8` - `CTA Button Click event`
* `prop8` - `CTA Button Id`

![CTA Click Adobe Analytics](assets/create-analytics-workspace/page-analytics.png)

### Objetivos {#objective}

1. Cree un grupo de informes o utilice uno existente.
1. Configurar [Variables de conversión (eVars)](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/conversion-variables/conversion-var-admin.html) y [Eventos de éxito (Events)](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/conversion-variables/success-events/success-event.html) en el grupo de informes.
1. Crear un [proyecto de Analysis Workspace](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/home.html) para analizar datos con la ayuda de herramientas que le permiten crear, analizar y compartir perspectivas rápidamente.
1. Comparta el proyecto de Analysis Workspace con otros integrantes del equipo.

## Requisitos previos

Este tutorial es una continuación del [Seguimiento de componentes en los que se hizo clic con Adobe Analytics](./track-clicked-component.md) y supone que tiene lo siguiente:

* A **Propiedad de etiqueta** con el [Extensión de Adobe Analytics](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/analytics/overview.html) activado
* **Adobe Analytics** ID del grupo de informes de prueba/desarrollo y servidor de seguimiento. Consulte la siguiente documentación para [creación de un grupo de informes](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/new-report-suite.html).
* [Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html) extensión del explorador configurada con una propiedad de etiqueta cargada en la variable [Sitio WKND](https://wknd.site/us/es.html) AEM o un sitio de con la capa de datos de Adobe habilitada.

## Variables de conversión (eVars) y eventos de éxito (Event)

La variable de conversión de Custom Insight (o eVar) se coloca en el código de Adobe de las páginas web seleccionadas del sitio. Su principal función es segmentar las métricas de éxito de conversión en los informes de marketing personalizados. Un eVar puede basarse en visitas y funcionar de manera similar a las cookies. Los valores pasados a las variables de eVar siguen al usuario durante un período predeterminado.

Cuando un eVar está establecido en el valor de un visitante, Adobe recuerda automáticamente ese valor hasta que caduque. Cualquier evento de éxito que encuentre un visitante mientras el valor de eVar está activo se cuenta hacia el valor de eVar.

El mejor uso de las eVars es para medir causa y efecto, como:

* Qué campañas internas influyeron en los ingresos
* Qué anuncios de banner tuvieron como resultado final un registro
* El número de veces que se utilizó una búsqueda interna antes de realizar un pedido

Los eventos de éxito son acciones de las que se puede llevar un seguimiento. Usted determina lo que es un evento de éxito. Por ejemplo, si un visitante hace clic en un botón de CTA, el evento de clic podría considerarse un evento de éxito.

### Configurar eVars

1. En la página de inicio de Adobe Experience Cloud, seleccione su organización e inicie Adobe Analytics.

   ![Analytics AEP](assets/create-analytics-workspace/analytics-aep.png)

1. En la barra de herramientas de Analytics, haga clic en **Administrador** > **Grupos de informes** y busque su grupo de informes.

   ![Analytics Report Suite](assets/create-analytics-workspace/select-report-suite.png)

1. Seleccione el grupo de informes > **Editar configuración** > **Conversión** > **Variables de conversión**

   ![Variables de conversión de Analytics](assets/create-analytics-workspace/conversion-variables.png)

1. Uso del **Añadir nuevo** , vamos a crear Variables de conversión para asignar el esquema de la siguiente manera:

   * `eVar5` -  `Page Template`
   * `eVar6` - `Page ID`
   * `eVar7` - `Last Modified Date`
   * `eVar8` - `Button Id`
   * `eVar9` - `Page Name`

   ![Añadir nuevas eVars](assets/create-analytics-workspace/add-new-evars.png)

1. Proporcione un nombre y una descripción adecuados para cada eVar y **Guardar** sus cambios. En el proyecto de Analysis Workspace se utilizan las eVars con el nombre adecuado, por lo que un nombre fácil de usar hace que las variables sean fácilmente reconocibles.

   ![eVars](assets/create-analytics-workspace/evars.png)

### Configurar eventos de éxito

A continuación, vamos a crear un evento para rastrear el clic en el botón CTA.

1. Desde el **Administrador de grupos de informes** , seleccione la **ID del grupo de informes** y haga clic en **Editar configuración**.
1. Clic **Conversión** > **Eventos de éxito**
1. Uso del **Añadir nuevo** , cree un evento de éxito personalizado para rastrear el clic del botón CTA y, a continuación, **Guardar** sus cambios.
   * `Event` : `event8`
   * `Name`:`CTA Click`
   * `Type`:`Counter`

   ![eVars](assets/create-analytics-workspace/add-success-event.png)

## Creación de un proyecto en Analysis Workspace {#workspace-project}

Analysis Workspace es una herramienta de navegador flexible que le permite crear análisis y compartir perspectivas rápidamente. Con la interfaz de arrastrar y soltar, puede crear su análisis, agregar visualizaciones para dar vida a los datos, depurar un conjunto de datos, compartir y programar proyectos con cualquier persona de su organización.

A continuación, cree un [proyecto](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/build-workspace-project/freeform-overview.html#analysis-workspace) para crear un tablero que analice el rendimiento de los botones de CTA en todo el sitio.

1. En la barra de herramientas de Analytics, seleccione **Workspace** y haga clic para **Crear un nuevo proyecto**.

   ![Workspace](assets/create-analytics-workspace/create-workspace.png)

1. Elija comenzar desde una **proyecto en blanco** o bien, seleccione una de las plantillas creadas previamente, ya sea mediante Adobe o mediante plantillas personalizadas creadas por su organización. Hay varias plantillas disponibles, según el análisis o el caso de uso que tenga en mente. [Más información](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/build-workspace-project/starter-projects.html) acerca de las distintas opciones de plantilla disponibles.

   En el proyecto de Workspace, se accede a paneles, tablas, visualizaciones y componentes desde el carril izquierdo. Constituyen los componentes básicos del proyecto.

   * **[Componentes](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/components/analysis-workspace-components.html)** : los componentes son dimensiones, métricas, segmentos o intervalos de fechas que se pueden combinar en una tabla de forma libre para responder en inicio a las preguntas comerciales. Asegúrese de familiarizarse con cada tipo de componente antes de sumergirse en su análisis. Una vez dominada la terminología de los componentes, puede empezar a arrastrar y soltar para crear el análisis en una tabla de forma libre.
   * **[Visualizaciones](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/visualizations/freeform-analysis-visualizations.html)** : las visualizaciones, como una barra o un gráfico de líneas, se agregan a continuación sobre los datos para darle vida visualmente. En el carril del extremo izquierdo, seleccione el icono en el medio, Visualizaciones, para ver la lista completa de visualizaciones disponible.
   * **[Paneles](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/panels/panels.html)** : Un panel es una colección de tablas y visualizaciones. Puede acceder a los paneles desde el icono que hay en la parte superior izquierda de Workspace. Los paneles son útiles cuando desea organizar sus proyectos según períodos de tiempo, grupos de informes o casos de uso de análisis. Los siguientes tipos de panel están disponibles en Analysis Workspace:

   ![Selección de plantilla](assets/create-analytics-workspace/workspace-tools.png)

### Añadir visualización de datos con Analysis Workspace

A continuación, genere una tabla para crear una representación visual de cómo interactúan los usuarios con `Call to Action (CTA)` en la página de inicio del sitio WKND. Para crear una representación de este tipo, vamos a utilizar los datos recopilados en la [Seguimiento de componentes en los que se hizo clic con Adobe Analytics](./track-clicked-component.md). A continuación se muestra un resumen rápido de los datos rastreados para las interacciones del usuario con los botones de llamada a la acción para el sitio WKND.

* `eVar5` -  `Page template`
* `eVar6` - `Page Id`
* `eVar7` - `Page last modified date`
* `eVar8` - `CTA Button Id`
* `eVar9` - `Page Name`
* `event8` - `CTA Button Click event`
* `prop8` - `CTA Button Id`

1. Arrastre y suelte el **Página** Componente de dimensión en la tabla de forma libre. Ahora debería poder ver una visualización que muestre el Nombre de página (eVar9) y las Vistas de página correspondientes (Ocurrencias) mostrados dentro de la tabla.

   ![Dimension de página](assets/create-analytics-workspace/evar9-dimension.png)

1. Arrastre y suelte el **Clic CTA** (event8) en la métrica ocurrencias y reemplácela. Ahora puede ver una visualización que muestra el Nombre de página (eVar9) y un recuento correspondiente de eventos de clic CTA en una página.

   ![Métrica de página: clic en CTA](assets/create-analytics-workspace/evar8-cta-click.png)

1. Vamos a desglosar la página por tipo de plantilla. Seleccione la métrica de plantilla de página de los componentes y arrastre y suelte la métrica de plantilla de página en la dimensión Nombre de página. Ahora puede ver el nombre de página desglosado por su tipo de plantilla.

   * **Antes**
     ![EVAR 5](assets/create-analytics-workspace/evar5.png)

   * **Después**
     ![Métricas de eVar5](assets/create-analytics-workspace/evar5-metrics.png)

1. Para comprender cómo interactúan los usuarios con los botones de CTA cuando están en las páginas del sitio WKND, se necesita un desglose adicional añadiendo la métrica ID de botón (eVar 8).

   ![EVAR 8](assets/create-analytics-workspace/evar8.png)

1. A continuación, puede ver una representación visual del sitio WKND desglosada por su plantilla de página y desglosada aún más por la interacción del usuario con los botones de clic para acción del sitio WKND (CTA).

   ![EVAR 8](assets/create-analytics-workspace/evar8-metric.png)

1. Puede reemplazar el valor del ID del botón con un nombre más descriptivo mediante las clasificaciones de Adobe Analytics. Puede obtener más información sobre cómo crear una clasificación para una métrica específica [aquí](https://experienceleague.adobe.com/docs/analytics/components/classifications/c-classifications.html). En este caso, tenemos una métrica de clasificación `Button Section (Button ID)` configurar para `eVar8` que asigna el id del botón a un nombre descriptivo.

   ![Sección de botón](assets/create-analytics-workspace/button-section.png)

## Agregar clasificación a una variable analítica

### Clasificaciones de conversión

La clasificación de Analytics es una forma de categorizar los datos de variables de Analytics para luego mostrarlos de diferentes maneras cuando se generan los informes. Para mejorar la forma en que se muestra el ID de botón en el informe de Analytics Workspace, vamos a crear una variable de clasificación para el ID de botón (eVar 8). Al clasificar, se establece una relación entre la variable y los metadatos relacionados con ella.

A continuación, vamos a crear una variable Clasificación para Analytics.

1. Desde el **Administrador** menú de la barra de herramientas, seleccione **Grupos de informes**
1. Seleccione el **ID del grupo de informes** desde el **Administrador de grupos de informes** y haga clic en **Editar configuración** > **Conversión** > **Clasificaciones de conversión**

   ![Clasificación de conversión](assets/create-analytics-workspace/conversion-classification.png)

1. Desde el **Seleccionar tipo de clasificación** , seleccione la variable (ID de eVar 8 botones) para añadir una clasificación.
1. Haga clic en la flecha situada justo al lado de la variable Clasificación que aparece en la sección Clasificaciones para agregar una nueva Clasificación.

   ![Tipo de clasificación de conversión](assets/create-analytics-workspace/select-classification-variable.png)

1. En el **Editar una clasificación** , proporcione un nombre adecuado para la Clasificación de texto. Se creará un componente de dimensión con el nombre Clasificación de texto.

   ![Tipo de clasificación de conversión](assets/create-analytics-workspace/new-classification.png)

1. **Guardar** sus cambios.

### Importador de clasificaciones

Utilice el importador para cargar clasificaciones en Adobe Analytics. También puede exportar los datos para actualizarlos antes de importarlos. Los datos que se importan con la herramienta correspondiente deben tener un formato específico. Adobe permite descargar una plantilla de datos con todos los detalles de encabezado correctos en un archivo de datos delimitado por tabuladores. Puede agregar los nuevos datos a esta plantilla y, a continuación, importar el archivo de datos en el explorador a través del FTP.

#### Plantilla de clasificación

Antes de importar clasificaciones en informes de marketing, puede descargar una plantilla que le ayudará a crear un archivo de datos de clasificaciones. El archivo de datos utiliza las clasificaciones deseadas como encabezados de columna y, a continuación, organiza el conjunto de datos de informes con los encabezados de clasificación adecuados.

A continuación, descarguemos la plantilla de clasificación para la variable de ID de botón (eVar 8)

1. Vaya a **Administrador** > **Importador de clasificaciones**
1. Descargue una plantilla de clasificación para la variable de conversión desde el **Descargar plantilla** Ficha.
   ![Tipo de clasificación de conversión](assets/create-analytics-workspace/classification-importer.png)

1. En la pestaña Descargar plantilla, especifique la configuración de la plantilla de datos.
   * **Seleccionar grupo de informes** : seleccione el grupo de informes que se utilizará en la plantilla. El grupo de informes y el conjunto de datos deben coincidir.
   * **Conjunto de datos a clasificar** : seleccione el tipo de datos para el archivo de datos. El menú incluye todos los informes de los grupos de informes que se han configurado para las clasificaciones.
   * **Codificación** : seleccione la codificación de caracteres del archivo de datos. El formato de codificación predeterminado es UTF-8.

1. Clic **Descargar** y guarde el archivo de plantilla en el sistema local. El archivo de plantilla es un archivo de datos delimitado por tabuladores (con la extensión de nombre de archivo.tab) compatible con la mayoría de las aplicaciones de hojas de cálculo.
1. Abra el archivo de datos delimitado por tabuladores con un editor de su elección.
1. Agregue el ID de botón (eVar 9) y un nombre de botón correspondiente al archivo delimitado por tabuladores para cada valor de eVar 9 del paso 9 de la sección.

   ![Valor de clave](assets/create-analytics-workspace/key-value.png)

1. **Guardar** el archivo delimitado por tabuladores.
1. Vaya a **Importar archivo** pestaña.
1. Configure el Destino para la importación de archivos.
   * **Seleccionar grupo de informes** AEM : WKND Site (Grupo de informes)
   * **Conjunto de datos a clasificar** : ID del botón (eVar de variable de conversión8)
1. Haga clic en **Elegir archivo** para cargar el archivo delimitado por tabuladores desde el sistema y, a continuación, haga clic en **Importar archivo**

   ![Importador de archivos](assets/create-analytics-workspace/file-importer.png)

   >[!NOTE]
   >
   > Si la importación se realiza correctamente, se muestran inmediatamente los cambios correspondientes en una exportación. Sin embargo, los cambios de datos en los informes pueden tardar hasta cuatro horas si utiliza una importación mediante explorador y hasta 24 horas si utiliza una importación mediante FTP.

#### Reemplazar variable de conversión por variable de clasificación

1. En la barra de herramientas de Analytics, seleccione **Workspace** y abra el espacio de trabajo creado en [Creación de un proyecto en Analysis Workspace](#create-a-project-in-analysis-workspace) de este tutorial.

   ![Identificador de botón de Workspace](assets/create-analytics-workspace/workspace-report-button-id.png)

1. A continuación, sustituya el **Id De Botón** métrica del espacio de trabajo que muestra el ID de un botón Llamada a la acción (CTA) con el nombre de clasificación creado en el paso anterior.

1. En el buscador de componentes, busque **Botones de CTA de WKND** y arrastre y suelte el **Botones de CTA de WKND (ID de botón)** en la métrica ID de botón y reemplácela.

   * **Antes**
     ![Botón de Workspace antes de](assets/create-analytics-workspace/wknd-button-before.png)
   * **Después**
     ![Botón de Workspace después de](assets/create-analytics-workspace/wknd-button-after.png)

1. Puede ver que la métrica ID del botón que contenía el ID del botón de una llamada a la acción (CTA) ahora se reemplaza con el nombre correspondiente proporcionado en la plantilla de clasificación.
1. Vamos a comparar la tabla de Analytics Workspace con la página de inicio de WKND y comprender el recuento de clics en el botón CTA y su análisis. En función de los datos de la tabla de forma libre del espacio de trabajo, es evidente que 22 veces los usuarios han hecho clic en **ESQUÍ AHORA** y cuatro veces para la página principal de WKND Camping en Australia Occidental **Leer más** botón.

   ![Informe de CTA](assets/create-analytics-workspace/workspace-report-buttons-wknd.png)

1. Asegúrese de guardar el proyecto de Adobe Analytics Workspace y proporcione un nombre y una descripción adecuados. De forma opcional, puede agregar etiquetas a un proyecto de Workspace.

   ![Guardar proyecto](assets/create-analytics-workspace/save-project.png)

1. Después de guardar correctamente el proyecto, puede compartirlo con otros compañeros o compañeros de equipo mediante la opción Compartir.

   ![Compartir proyecto](assets/create-analytics-workspace/share.png)

## Enhorabuena.

Acaba de aprender a asignar datos capturados desde un sitio de Adobe Experience Manager a métricas y dimensiones en grupos de informes de Adobe Analytics. Además, realizó una clasificación para las métricas y creó un tablero de informes detallado con la función Analysis Workspace de Adobe Analytics.
