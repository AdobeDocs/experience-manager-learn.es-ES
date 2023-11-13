---
title: Análisis de proporción de aciertos de caché de CDN
description: AEM Obtenga información sobre cómo analizar los registros de CDN proporcionados de forma as a Cloud Service por el. Obtenga información, como la proporción de visitas en caché y las direcciones URL principales de los tipos de caché MISS y PASS para fines de optimización.
version: Cloud Service
feature: Operations, CDN Cache
topic: Administration, Performance
role: Admin, Architect, Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-11-10T00:00:00Z
jira: KT-13312
thumbnail: KT-13312.jpeg
source-git-commit: be503ba477d63a566b687866289a81a0aa7d01f7
workflow-type: tm+mt
source-wordcount: '1231'
ht-degree: 2%

---


# Análisis de proporción de aciertos de caché de CDN

AEM Obtenga información sobre cómo analizar los datos as a Cloud Service que se han proporcionado **Registros de CDN** y obtener perspectivas como **proporción de aciertos de caché**, y **las principales URL de _SEÑORITA_ y _PASE_ tipos de caché** para fines de optimización.


Los registros de CDN están disponibles en formato JSON, que contiene varios campos, incluidos los siguientes `url`, `cache`, para obtener más información, consulte la [Formato de registro de CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/logging.html?lang=en#cdn-log:~:text=Toggle%20Text%20Wrapping-,Log%20Format,-The%20CDN%20logs). El `cache` proporciona información sobre _estado de la caché_ y sus valores posibles son HIT, MISS o PASS. Revisemos los detalles de los valores posibles.

| Estado de la caché </br> Valor posible | Descripción |
|------------------------------------|:-----------------------------------------------------:|
| VISITA | Los datos solicitados son _se encuentra en la caché de CDN y no requiere realizar una recuperación_ AEM solicitud al servidor de la. |
| SEÑORITA | Los datos solicitados son _no se encuentra en la caché de CDN y debe solicitarse_ AEM del servidor de la. |
| PASE | Los datos solicitados son _establecido explícitamente para no almacenarse en caché_ AEM y siempre se recuperarán del servidor de la. |

Para este tutorial, la variable [AEM Proyecto de WKND de](https://github.com/adobe/aem-guides-wknd) AEM se implementa en el entorno as a Cloud Service de y se activa una pequeña prueba de rendimiento mediante [Apache JMeter](https://jmeter.apache.org/).

## Descargar registros de CDN

Para descargar los registros de CDN, siga estos pasos:

1. Inicie sesión en Cloud Manager en [my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com/) y seleccione su organización y programa.

1. Para un entorno AEM CS deseado, seleccione **Descargar registros** en el menú de los tres puntos.

   ![Descargar registros: Cloud Manager](assets/cdn-logs-analysis/download-logs.png){width="500" zoomable="yes"}

1. En el **Descargar registros** , seleccione la **Publish** Service en el menú desplegable y, a continuación, haga clic en el icono de descarga situado junto a la etiqueta **cdn** fila.

   ![Registros de CDN: Cloud Manager](assets/cdn-logs-analysis/download-cdn-logs.png){width="500" zoomable="yes"}


Si el archivo de registro descargado es de _hoy_ la extensión del archivo es `.log` de lo contrario, para los archivos de registro anteriores, la extensión es `.log.gz`.

## Analizar registros de CDN descargados

Para obtener información, como la proporción de visitas en caché y las direcciones URL principales de los tipos de caché MISS y PASS, analice el archivo de registro de CDN descargado. Estas perspectivas ayudan a optimizar la [Configuración de caché de CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html?lang=es) y mejorar el rendimiento del sitio.

Para analizar los registros de CDN, este artículo utiliza la variable **Elasticsearch, Logstash y Kibana (ELK)** [herramientas de tablero](https://github.com/adobe/AEMCS-CDN-Log-Analysis-ELK-Tool) y [Jupyter Notebook](https://jupyter.org/).


### Uso de las herramientas del panel

El [pila de ELK](https://www.elastic.co/elastic-stack) es un conjunto de herramientas que proporcionan una solución escalable para buscar, analizar y visualizar los datos. Consiste en Elasticsearch, Logstash y Kibana.

Para identificar los detalles clave, usemos el [AEMCS-CDN-Log-Analysis-ELK-Tool](https://github.com/adobe/AEMCS-CDN-Log-Analysis-ELK-Tool) proyecto de herramientas de tablero. Este proyecto proporciona un contenedor Docker de la pila ELK y un panel preconfigurado de Kibana para analizar los registros de CDN.

1. Siga los pasos de [Cómo configurar el contenedor ELK Docker](https://github.com/adobe/AEMCS-CDN-Log-Analysis-ELK-Tool#how-to-set-up-the-elk-docker-container) y asegúrese de importar el **Proporción de aciertos de caché de CDN** Tablero de Kibana.

1. Para identificar la proporción de visitas de caché de CDN y las direcciones URL principales, siga estos pasos:

   1. Copie los archivos de registro de CDN descargados dentro de la carpeta específica del entorno.

   1. Abra el **Proporción de aciertos de caché de CDN** Haga clic en el menú Hamburguesa > Analytics > Panel > Proporción de visitas de caché de CDN.

      ![CDN Cache Hit Ratio - Kibana Dashboard](assets/cdn-logs-analysis/cdn-cache-hit-ratio-dashboard.png){width="500" zoomable="yes"}

   1. Seleccione el intervalo de tiempo deseado en la esquina superior derecha.

      ![Rango de tiempo - Kibana Dashboard](assets/cdn-logs-analysis/time-range.png){width="500" zoomable="yes"}

   1. El **Proporción de aciertos de caché de CDN** El tablero de mandos se explica por sí mismo.

   1. El _Análisis de solicitudes totales_ muestra los siguientes detalles:
      - Proporciones de caché por tipo de caché
      - Recuentos de caché por tipo de caché

      ![Análisis de solicitudes totales: panel de Kibana](assets/cdn-logs-analysis/total-request-analysis.png){width="500" zoomable="yes"}

   1. El _Análisis por tipos de solicitud o MIME_ muestra los siguientes detalles:
      - Proporciones de caché por tipo de caché
      - Recuentos de caché por tipo de caché
      - Principales URL de MISS y PASS

      ![Análisis por tipos de solicitud o MIME - Kibana Dashboard](assets/cdn-logs-analysis/analysis-by-request-or-mime-types.png){width="500" zoomable="yes"}

#### Filtrado por nombre de entorno o ID de programa

Para filtrar los registros ingeridos por nombre de entorno, siga los siguientes pasos:

1. En el panel Proporción de visitas de caché de CDN, haga clic en **Añadir filtro** icono.

   ![Filtro - Tablero de Kibana](assets/cdn-logs-analysis/filter.png){width="500" zoomable="yes"}

1. En el **Añadir filtro** modal, seleccione la opción `aem_env_name.keyword` del menú desplegable y, a continuación, `is` operador y nombre de entorno deseado para el campo siguiente y, finalmente, haga clic en _Añadir filtro_.

   ![Añadir filtro - Tablero de Kibana](assets/cdn-logs-analysis/add-filter.png){width="500" zoomable="yes"}

#### Filtrado por nombre de host

Para filtrar los registros ingeridos por nombre de host, siga los siguientes pasos:

1. En el panel Proporción de visitas de caché de CDN, haga clic en **Añadir filtro** icono.

   ![Filtro - Tablero de Kibana](assets/cdn-logs-analysis/filter.png){width="500" zoomable="yes"}

1. En el **Añadir filtro** modal, seleccione la opción `host.keyword` del menú desplegable y, a continuación, `is` operador y nombre de host deseado para el campo siguiente y, finalmente, haga clic en _Añadir filtro_.

   ![Filtro de host - Tablero de Kibana](assets/cdn-logs-analysis/add-host-filter.png){width="500" zoomable="yes"}

Del mismo modo, agregue más filtros al panel en función de los requisitos de análisis.

### Uso de Jupyter Notebook

El [Jupyter Notebook](https://jupyter.org/) es una aplicación web de código abierto que permite crear documentos que contienen código, texto y visualización. Se utiliza para la transformación, visualización y modelado estadístico de datos.

Para acelerar el análisis de los registros de CDN, descargue el [AEM-as-a-CloudService: análisis de registros de CDN: Jupyter Notebook](./assets/cdn-logs-analysis/aemcs_cdn_logs_analysis.ipynb) archivo.

El descargado `aemcs_cdn_logs_analysis.ipynb` El archivo &quot;Interactive Python Notebook&quot; se explica por sí mismo, sin embargo, los aspectos destacados de cada sección son:

- **Instalación de bibliotecas adicionales**: instala el `termcolor` y `tabulate` Bibliotecas de Python.
- **Cargar registros de CDN**: carga el archivo de registro de CDN mediante `log_file` valor de la variable, asegúrese de actualizar su valor. También transforma este registro de CDN en la variable [DataFrame de Pandas](https://pandas.pydata.org/docs/reference/frame.html).
- **Realizar análisis**: el primer bloque de código es _Mostrar resultado de análisis para solicitudes totales, de HTML, JS/CSS e de imagen_Además, proporciona gráficos de porcentaje, barras y circulares de proporción de visitas a la caché.
El segundo bloque de código es _Las 5 direcciones URL de solicitud MISS y PASS principales para HTML, JS/CSS e imagen_, muestra las direcciones URL y sus recuentos en formato de tabla.

#### Ejecute Jupyter Notebook en Experience Platform.

>[!IMPORTANT]
>
>Si utiliza o tiene licencia para el Experience Platform, puede ejecutar Jupyter Notebook sin instalar software adicional.

Para ejecutar Jupyter Notebook en Experience Platform, siga estos pasos:

1. Inicie sesión en [Adobe Experience Cloud](https://experience.adobe.com/), en la Página de inicio > **Acceso rápido** sección > haga clic en **Experience Platform**

   ![Experience Platform](assets/cdn-logs-analysis/experience-platform.png){width="500" zoomable="yes"}

1. En la página de inicio de Adobe Experience Platform > Sección de ciencia de datos > , haga clic en **Notebooks** elemento de menú. Para iniciar el entorno de Jupyter Notebooks, haga clic en el **JupyterLab** pestaña.

   ![Actualización del valor del archivo de registro de Notebook](assets/cdn-logs-analysis/datascience-notebook.png){width="500" zoomable="yes"}

1. En el menú de JupyterLab, utilice el **Cargar archivos** , cargue el archivo de registro de CDN descargado y `aemcs_cdn_logs_analysis.ipynb` archivo.

   ![Cargar archivos - JupyteLab](assets/cdn-logs-analysis/jupyterlab-upload-file.png){width="500" zoomable="yes"}

1. Abra el `aemcs_cdn_logs_analysis.ipynb` haciendo doble clic en.

1. En el **Cargar archivo de registro de CDN** del bloc de notas, actualice el `log_file` valor.

   ![Actualización del valor del archivo de registro de Notebook](assets/cdn-logs-analysis/notebook-update-variable.png){width="500" zoomable="yes"}

1. Para ejecutar la celda seleccionada y avanzar, haga clic en **Reproducir** icono.

   ![Actualización del valor del archivo de registro de Notebook](assets/cdn-logs-analysis/notebook-run-cell.png){width="500" zoomable="yes"}

1. Después de ejecutar el **Mostrar el resultado del análisis para solicitudes totales, de HTML, JS/CSS e de imagen** en una celda de código, el resultado muestra los gráficos de porcentaje, barras y circulares de la proporción de visitas de la caché.

   ![Actualización del valor del archivo de registro de Notebook](assets/cdn-logs-analysis/output-cache-hit-ratio.png){width="500" zoomable="yes"}

1. Después de ejecutar el **Las 5 direcciones URL de solicitud MISS y PASS principales para HTML, JS/CSS e imagen** en la celda de código, el resultado muestra las 5 direcciones URL de solicitud FALSE y PASS principales.

   ![Actualización del valor del archivo de registro de Notebook](assets/cdn-logs-analysis/output-top-urls.png){width="500" zoomable="yes"}

Puede mejorar Jupyter Notebook para analizar los registros de CDN en función de sus necesidades.

## Optimizar configuración de caché de CDN

Después de analizar los registros de CDN, puede optimizar la configuración de la caché de CDN para mejorar el rendimiento del sitio. AEM La práctica recomendada es tener una proporción de visitas de caché del 90 % o superior.

Para obtener más información, consulte [Optimizar configuración de caché de CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#caching).

AEM El proyecto WKND de WKND tiene una configuración de CDN de referencia. Para obtener más información, consulte [Configuración de CDN](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost#L137-L190) desde el `wknd.vhost` archivo.
