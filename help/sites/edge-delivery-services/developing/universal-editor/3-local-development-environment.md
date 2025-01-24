---
title: Configuración de un entorno de desarrollo local
description: Configure un entorno de desarrollo local para los sitios entregados con Edge Delivery Services y editables con el editor universal.
version: Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 700
source-git-commit: e8ce91b0be577ec6cf8f3ab07ba9ff09c7e7a6ab
workflow-type: tm+mt
source-wordcount: '925'
ht-degree: 1%

---


# Configuración de un entorno de desarrollo local

Un entorno de desarrollo local es esencial para el desarrollo rápido de sitios web ofrecidos por Edge Delivery Services. El entorno utiliza código desarrollado localmente mientras obtiene contenido de los Edge Delivery Services, lo que permite a los desarrolladores ver instantáneamente los cambios del código. Esta configuración admite pruebas y desarrollo rápidos e iterativos.

Las herramientas y los procesos de desarrollo de un proyecto de sitio web de Edge Delivery Services están diseñados para resultarles familiares a los desarrolladores web y proporcionar una experiencia de desarrollo rápida y eficaz.

## Topología de desarrollo

La topología de desarrollo para un proyecto de sitio web de Edge Delivery Services que se puede editar con el editor universal consta de los siguientes aspectos:

- **Repositorio de GitHub**:
   - **Propósito**: Aloja el código del sitio web (CSS y JavaScript).
   - **Estructura**: La **rama principal** contiene código listo para la producción, mientras que otras ramas contienen código en funcionamiento.
   - **Funcionalidad**: El código de cualquier rama se puede ejecutar en los entornos **production** o **preview** sin afectar el sitio web activo.

- AEM **Servicio de autor de**:
   - **Propósito**: sirve como repositorio de contenido canónico donde se edita y administra el contenido del sitio web.
   - **Funcionalidad**: El contenido lo lee y escribe el **Editor universal**. El contenido editado se ha publicado en **Edge Delivery Services** en **entornos de producción** o **vista previa**.

- **Editor universal**:
   - **Propósito**: Proporciona una interfaz de WYSIWYG para editar el contenido del sitio web.
   - AEM **Funcionalidad**: Lee y escribe en el servicio de **autor de la**. Se puede configurar para que use código de cualquier rama en el **repositorio de GitHub** para probar y validar cambios.

- **Edge Delivery Services**:
   - **Entorno de producción**:
      - **Propósito**: Envía el contenido y el código del sitio web activo a los usuarios finales.
      - AEM **Funcionalidad**: Sirve el contenido publicado desde el **servicio de autor de** con código de la **rama principal** del **repositorio de GitHub**.
   - **Entorno de vista previa**:
      - **Propósito**: proporciona un entorno de ensayo para probar y previsualizar el contenido y el código antes de la implementación.
      - AEM **Funcionalidad**: Sirve contenido publicado desde el servicio de autor de **** con código de cualquier rama del repositorio de **GitHub**, lo que permite realizar pruebas exhaustivas sin afectar al sitio web activo.

- **Entorno de desarrollador local**:
   - **Propósito**: permite a los desarrolladores escribir y probar código (CSS y JavaScript) localmente.
   - **Estructura**:
      - Un clon local del **repositorio de GitHub** para desarrollo basado en ramas.
      - AEM La **CLI de**, que actúa como servidor de desarrollo, aplica cambios de código local al **entorno de vista previa** para una experiencia de recarga en caliente.
   - **Flujo de trabajo**: Los desarrolladores escriben código localmente, confirman cambios en una rama de trabajo, insertan la rama en GitHub, la validan en el **Editor universal** (utilizando la rama especificada) y la combinan en la **rama principal** cuando estén listos para la implementación de producción.

## Requisitos previos

Antes de iniciar el desarrollo, instale lo siguiente en su equipo:

1. [Git](https://git-scm.com/)
1. [Node.js y npm](https://nodejs.org)
1. [Microsoft Visual Studio Code](https://code.visualstudio.com/) (o editor de código similar)

## Clone el repositorio de GitHub

AEM Clone el [repositorio de GitHub](./1-new-code-project.md) que contiene el proyecto de código de Edge Delivery Services de la en su entorno de desarrollo local.

![Clon del repositorio de GitHub](./assets/3-local-development-environment/github-clone.png)

```bash
$ cd ~/Code
$ git clone git@github.com:<YOUR_ORG>/aem-wknd-eds-ue.git
```

Se crea una nueva carpeta `aem-wknd-eds-ue` en el directorio `Code`, que sirve como raíz del proyecto. Aunque el proyecto se puede clonar en cualquier ubicación del equipo, este tutorial utiliza `~/Code` como directorio raíz.

## Instalar dependencias del proyecto

Vaya a la carpeta del proyecto e instale las dependencias necesarias con `npm install`. Aunque los proyectos de Edge Delivery Services no utilizan sistemas de compilación tradicionales de Node.js, como Webpack o Vite, siguen precisando varias dependencias para el desarrollo local.

```bash
# ~/Code/aem-wknd-eds-ue

$ npm install
```

## AEM Instalación de la CLI de

AEM La CLI de es una herramienta de línea de comandos de Node.js diseñada para optimizar el desarrollo de sitios web basados en Edge Delivery Services AEM, lo que proporciona un servidor de desarrollo local para el rápido desarrollo y prueba de su sitio web.

AEM Para instalar la CLI de, ejecute lo siguiente:

```bash
# ~/Code/aem-wknd-eds-ue

$ npm install @adobe/aem-cli
```

AEM También se puede instalar la CLI de la de forma global mediante `npm install --global @adobe/aem-cli`.

## AEM Inicio del servidor de desarrollo de la local

El comando `aem up` inicia el servidor de desarrollo local y abre automáticamente una ventana del explorador a la dirección URL del servidor. Este servidor actúa como un proxy inverso al entorno de los Edge Delivery Services, sirviendo contenido desde allí mientras utiliza su base de código local para el desarrollo.

```bash
$ cd ~/Code/aem-wknd-eds-ue 
$ aem up

    ___    ________  ___                          __      __ 
   /   |  / ____/  |/  /  _____(_)___ ___  __  __/ /___ _/ /_____  _____
  / /| | / __/ / /|_/ /  / ___/ / __ `__ \/ / / / / __ `/ __/ __ \/ ___/
 / ___ |/ /___/ /  / /  (__  ) / / / / / / /_/ / / /_/ / /_/ /_/ / /
/_/  |_/_____/_/  /_/  /____/_/_/ /_/ /_/\__,_/_/\__,_/\__/\____/_/

info: Starting AEM dev server version x.x.x
info: Local AEM dev server up and running: http://localhost:3000/
info: Enabled reverse proxy to https://main--aem-wknd-eds-ue--<YOUR_ORG>.aem.page
```

AEM La CLI de la abre el sitio web en el explorador en `http://localhost:3000/`. Los cambios en el proyecto se vuelven a cargar automáticamente en el explorador web, mientras que los cambios de contenido [requieren la publicación en el entorno de vista previa](./6-author-block.md) y la actualización del explorador web.

## Generar fragmentos de JSON

Los proyectos de Edge Delivery Services AEM, creados con la [plantilla XWalk de plantillas de plantillas ](https://github.com/adobe-rnd/aem-boilerplate-xwalk), dependen de las configuraciones de JSON que habilitan la creación de bloques en el editor universal.

- **Fragmentos JSON**: almacenados con sus bloques asociados y definen los modelos de bloques, definiciones y filtros.
   - **Fragmentos de modelo**: almacenado en `/blocks/example/_example.json`.
   - **Fragmentos de definición**: almacenado en `/blocks/example/_example.json`.
   - **Fragmentos de filtro**: almacenado en `/blocks/example/_example.json`.

Los scripts NPM compilan estos fragmentos JSON y los colocan en las ubicaciones adecuadas en la raíz del proyecto. Para crear archivos JSON, utilice los scripts NPM proporcionados. Por ejemplo, para compilar todos los fragmentos, ejecute:

```bash
# ~/Code/aem-wknd-eds-ue

npm run build:json
```

| Script NPM | Descripción |
|--------------------|-----------------------------------------------------------------------------|
| `build:json` | Genera todos los archivos JSON a partir de fragmentos y los agrega a los `component-*.json` archivos correspondientes. |
| `build:json:models` | Crea fragmentos JSON de bloque y los compila en `/component-models.json`. |
| `build:json:definitions` | Genera fragmentos JSON de página y los compila en `/component-definitions.json`. |
| `build:json:filters` | Genera fragmentos JSON de página y los compila en `/component-filters.json`. |

>[!TIP]
>
> Ejecute `npm run build:json` después de cualquier cambio en los archivos de fragmento para regenerar los archivos JSON principales.

## Linting

La vinculación garantiza la calidad y coherencia del código, que es necesaria para los proyectos de Edge Delivery Services antes de combinar los cambios en la rama `main`.

Los scripts NPM se pueden ejecutar a través de `npm run`, por ejemplo:

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint
```

| Script NPM | Descripción |
|------------------|--------------------------------------------------|
| `lint:js` | Ejecuta comprobaciones de vinculación de JavaScript. |
| `lint:css` | Ejecuta comprobaciones de vinculación CSS. |
| `lint` | Ejecuta comprobaciones de vinculación de JavaScript y CSS. |

### Corregir problemas de vinculación automáticamente

Puede resolver automáticamente los problemas de vinculación agregando los(as) siguientes `scripts` al(la) `package.json` del proyecto, y se pueden ejecutar a través de `npm run`:

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint:fix
```

AEM Estos scripts no vienen preconfigurados con la plantilla XWalk de plantillas de plantillas de plantillas, pero se pueden agregar al archivo `package.json`:

>[!BEGINTABS]

>[!TAB Scripts adicionales]

| Script NPM | Comando | Descripción |
|------------------|------------------------------------------------|-------------------------------------------------------|
| `lint:js:fix` | `npm run lint:js --fix` | Corrige automáticamente los problemas de vinculación de JavaScript. |
| `lint:css:fix` | `stylelint blocks/**/*.css styles/*.css --fix` | Corrige automáticamente los problemas de vinculación de CSS. |
| `lint:fix` | `npm run lint:js:fix && npm run lint:css:fix` | Ejecuta scripts de corrección de JS y CSS para realizar una limpieza rápida. |

>[!TAB ejemplo de package.json]

Las siguientes entradas de script se pueden agregar a la matriz `package.json` `scripts`.

```json
{
  ...
  "scripts": [
    ...,
    "lint:js:fix": "npm run lint:js --fix",
    "lint:css:fix": "npm run lint:css --fix",
    "lint:fix": "npm run lint:js:fix && npm run lint:css:fix",
    ...
  ]
  ...
}
```

>[!ENDTABS]

