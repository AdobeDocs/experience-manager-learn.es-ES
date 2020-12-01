---
title: Uso de la importación y exportación de metadatos en AEM Assets
seo-title: Uso de la importación y exportación de metadatos en AEM Assets
description: Las funciones de importación y exportación de metadatos de AEM Assets permiten a los autores de contenido mover fácilmente los metadatos de los recursos dentro y fuera de AEM y aprovechar el poder de Microsoft Excel para manipular los metadatos a escala, lo que facilita la actualización masiva de los metadatos de los recursos existentes en AEM.
seo-description: Las funciones de importación y exportación de metadatos de AEM Assets permiten a los autores de contenido mover fácilmente los metadatos de los recursos dentro y fuera de AEM y aprovechar el poder de Microsoft Excel para manipular los metadatos a escala, lo que facilita la actualización masiva de los metadatos de los recursos existentes en AEM.
uuid: db7e57a4-b0c1-4a48-906d-802c19964313
discoiquuid: 72dd9230-73e1-454e-a3e0-9281e621d901
topics: metadata
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '306'
ht-degree: 3%

---


# Uso de la importación y exportación de metadatos en AEM Assets{#using-metadata-import-and-export-in-aem-assets}

Las funciones de importación y exportación de metadatos de AEM Assets permiten a los autores de contenido mover fácilmente los metadatos de los recursos dentro y fuera de AEM y aprovechar el poder de Microsoft Excel para manipular los metadatos a escala, lo que facilita la actualización masiva de los metadatos de los recursos existentes en AEM.

## Exportación de metadatos {#metadata-export}

>[!VIDEO](https://video.tv.adobe.com/v/22132/?quality=9&learn=on)

## Importación de metadatos {#metadata-import}

>[!VIDEO](https://video.tv.adobe.com/v/21374/?quality=9&learn=on)

Descargar [Carpeta deportiva WeRetail](assets/we-retail-sports.zip)

Descargar [paquete de metadatos de recursos](assets/we-retail-sports-asset-metadata.zip)

## Formato de archivo de metadatos {#metadata-file-format}

### Formato de archivo CSV

#### Primera fila

* La primera fila del archivo CSV define el esquema de metadatos.
* La primera columna tiene el valor predeterminado `assetPath`, que contiene la ruta JCR absoluta para un recurso.

* Las columnas posteriores de la primera fila apuntan a otras propiedades de metadatos de un recurso.

   * Por ejemplo : `dc:title, dc:description, jcr:title`

* Formato de propiedad de un solo valor

   * `<metadata property name> {{<property type}}`
   * Si no se especifica el tipo de propiedad, el valor predeterminado es String.
   * Por ejemplo: `dc:title {{String}}`

* El nombre de propiedad distingue entre mayúsculas y minúsculas
   * Correcto : `dc:title {{String}}`
   * Incorrecto: `Dc:Ttle {{String}}`

* El tipo de propiedad distingue entre mayúsculas y minúsculas
* Se admiten todos los tipos de propiedades [JCR válidos](https://docs.adobe.com/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/PropertyType.html)

* Formato de propiedad de varios valores: `<metadata property name> {{<property type : MULTI }}`

#### Segunda fila a N filas

* La primera columna contiene la ruta JCR absoluta para un recurso. Por ejemplo: /content/dam/asset1.jpg
* Puede que falten valores en la propiedad de metadatos de un recurso en el archivo CSV. No se actualizará la propiedad de metadatos que falta para ese recurso concreto.