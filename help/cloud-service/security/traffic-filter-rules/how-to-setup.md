---
title: Cómo configurar las reglas del filtro de tráfico, incluidas las reglas de WAF
description: Obtenga información sobre cómo configurar para crear, implementar, probar y analizar los resultados de las reglas de filtro de tráfico, incluidas las reglas de WAF.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-26T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
exl-id: b67bf642-3341-48d0-8ea9-5f262febf414
duration: 292
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '575'
ht-degree: 3%

---

# Cómo configurar las reglas del filtro de tráfico, incluidas las reglas de WAF

Obtenga información sobre **cómo configurar** reglas de filtro de tráfico, incluidas las reglas de WAF. Obtenga información sobre la creación, la implementación, las pruebas y el análisis de resultados.

>[!VIDEO](https://video.tv.adobe.com/v/3425407?quality=12&learn=on)

## Configuración

El proceso de configuración implica lo siguiente:

- _creando reglas_ con una estructura de proyecto y un archivo de configuración de AEM adecuados.
- _implementar reglas_ mediante la canalización de configuración de Adobe Cloud Manager.
- _probar reglas_ con diversas herramientas para generar tráfico.
- _analizando los resultados_ mediante registros de CDN de AEM CS y herramientas de tablero.

### Creación de reglas en el proyecto de AEM

Para crear reglas, siga estos pasos:

1. En el nivel superior del proyecto de AEM, cree una carpeta `config`.

1. Dentro de la carpeta `config`, cree un nuevo archivo llamado `cdn.yaml`.

1. Agregue los siguientes metadatos al archivo `cdn.yaml`:

```yaml
kind: CDN
version: '1'
metadata:
  envTypes:
    - dev
    - stage
    - prod
data:
  trafficFilters:
    rules:
```

Vea un ejemplo del archivo `cdn.yaml` dentro del proyecto WKND Sites de AEM Guides:

![Archivo y carpeta de reglas del proyecto WKND AEM](./assets/wknd-rules-file-and-folder.png){width="800" zoomable="yes"}

### Implementación de reglas mediante Cloud Manager {#deploy-rules-through-cloud-manager}

Para implementar las reglas, siga estos pasos:

1. Inicie sesión en Cloud Manager en [my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com/) y seleccione la organización y programa adecuados.

1. Vaya a la tarjeta _Canalizaciones_ de la página _Resumen del programa_, haga clic en el botón **+Agregar** y seleccione el tipo de canalización que desee.

   ![Tarjeta de canalizaciones de Cloud Manager](./assets/cloud-manager-pipelines-card.png)

   En el ejemplo anterior, para fines de demostración, se ha seleccionado _Agregar canalización que no sea de producción_ porque se utiliza un entorno de desarrollo.

1. En el cuadro de diálogo _Agregar canalización que no sea de producción_, elija e introduzca los siguientes detalles:

   1. Paso de configuración:

      - **Tipo**: Canalización de implementación
      - **Nombre de canalización**: Dev-Config

      ![Cuadro de diálogo de canalización de configuración de Cloud Manager](./assets/cloud-manager-config-pipeline-step1-dialog.png)

   2. Paso Código Source:

      - **Código para implementar**: implementación dirigida
      - **Incluir**: Configuración
      - **Entorno de implementación**: Nombre de su entorno, por ejemplo, wknd-program-dev.
      - **Repositorio**: El repositorio Git desde el que la canalización debe recuperar el código; por ejemplo, `wknd-site`
      - **Rama de Git**: El nombre de la rama del repositorio de Git.
      - **Ubicación del código**: `/config`, correspondiente a la carpeta de configuración de nivel superior creada en el paso anterior.

      ![Cuadro de diálogo de canalización de configuración de Cloud Manager](./assets/cloud-manager-config-pipeline-step2-dialog.png)

### Prueba de reglas mediante la generación de tráfico

Para probar las reglas, hay varias herramientas de terceros disponibles y es posible que su organización tenga una herramienta preferida. Para el propósito de demostración, vamos a utilizar las siguientes herramientas:

- [Curl](https://curl.se/) para pruebas básicas como invocar una URL y comprobar el código de respuesta.

- [Vegeta](https://github.com/tsenart/vegeta) para realizar denegación de servicio (DOS). Siga las instrucciones de instalación de [Vegeta GitHub](https://github.com/tsenart/vegeta#install).

- [Nikto](https://github.com/sullo/nikto/wiki) para encontrar posibles problemas y vulnerabilidades de seguridad como XSS, inyección SQL y más. Siga las instrucciones de instalación de [Nikto GitHub](https://github.com/sullo/nikto).

- Compruebe que las herramientas están instaladas y disponibles en el terminal ejecutando los siguientes comandos:

  ```shell
  # Curl version check
  $ curl --version
  
  # Vegeta version check
  $ vegeta -version
  
  # Nikto version check
  $ cd <PATH-OF-CLONED-REPO>/program
  ./nikto.pl -Version
  ```

### Analizar los resultados con las herramientas del panel

Después de crear, implementar y probar las reglas, puede analizar los resultados mediante los registros **CDN** y **AEMCS-CDN-Log-Analysis-Tool**. La herramienta proporciona un conjunto de paneles para visualizar los resultados de la pila de Splunk y ELK (Elasticsearch, Logstash y Kibana).

Las herramientas se pueden clonar desde el repositorio de GitHub [AEMCS-CDN-Log-Analysis-Tooling](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling). A continuación, siga las instrucciones para instalar y cargar los paneles de **Tráfico de CDN** y **Tablero de WAF** para su herramienta de observación preferida.

En este tutorial, vamos a utilizar la pila ELK. Siga las instrucciones de [ELK Docker container for AEMCS CDN Log Analysis](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/ELK/README.md) para configurar la pila ELK.

- Después de cargar el tablero de muestra, la página de la herramienta Tablero elástico debería tener el siguiente aspecto:

  ![Tablero de reglas de filtro de tráfico ELK](./assets/elk-dashboard.png)

>[!NOTE]
>
>    Dado que aún no se han introducido registros de CDN de AEM CS, el panel está vacío.


## Siguiente paso

Aprenda a declarar reglas de filtro de tráfico, incluidas las reglas de WAF, en el capítulo [Ejemplos y análisis de resultados](./examples-and-analysis.md), mediante el proyecto WKND Sites de AEM.
