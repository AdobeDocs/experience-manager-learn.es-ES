---
title: Uso de la importación y exportación de metadatos en AEM Assets
description: Obtenga información sobre cómo utilizar las funciones de importación y exportación de metadatos de Adobe Experience Manager Assets. Las funciones de importación y exportación permiten a los autores de contenido actualizar de forma masiva los metadatos de los recursos existentes.
version: 6.3, 6.4, 6.5, cloud-service
topic: Administración de contenido
feature: Metadatos
role: Admin
level: Intermediate
kt: 647, 917
thumbnail: 22132.jpg
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '264'
ht-degree: 4%

---


# Uso de la importación y exportación de metadatos en AEM Assets {#metadata-import-and-export}

Obtenga información sobre cómo utilizar las funciones de importación y exportación de metadatos de Adobe Experience Manager Assets. Las funciones de importación y exportación permiten a los autores de contenido actualizar de forma masiva los metadatos de los recursos existentes.

## Exportación de metadatos {#metadata-export}

>[!VIDEO](https://video.tv.adobe.com/v/22132/?quality=12&learn=on)

## Importación de metadatos {#metadata-import}

>[!VIDEO](https://video.tv.adobe.com/v/21374/?quality=12&learn=on)

>[!NOTE]
>
> Al preparar un archivo CSV para importar, es más fácil generar un CSV con la lista de recursos mediante la función Exportación de metadatos. A continuación, puede modificar el archivo CSV generado e importarlo mediante la función Importar .

## Formato del archivo CSV de metadatos {#metadata-file-format}

### Primera fila

* La primera fila del archivo CSV define el esquema de metadatos.
* La primera columna toma el valor predeterminado `assetPath`, que contiene la ruta JCR absoluta de un recurso.

* Las columnas posteriores de la primera fila apuntan a otras propiedades de metadatos de un recurso.
   * Por ejemplo : `dc:title, dc:description, jcr:title`

* Formato de propiedad de un solo valor

   * `<metadata property name> {{<property type}}`
   * Si no se especifica el tipo de propiedad, el valor predeterminado es String.
   * Por ejemplo: `dc:title {{String}}`

* El nombre de propiedad distingue entre mayúsculas y minúsculas
   * Correcto : `dc:title {{String}}`
   * Incorrecto: `Dc:Title {{String}}`

* El tipo de propiedad no distingue entre mayúsculas y minúsculas
* Se admiten todos los tipos de propiedades [JCR](https://docs.adobe.com/content/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/PropertyType.html) válidos

* Formato de propiedad de varios valores: `<metadata property name> {{<property type : MULTI }}`

### Segunda fila a filas N

* La primera columna contiene la ruta JCR absoluta para un recurso. Por ejemplo: /content/dam/asset1.jpg
* Puede que falten valores en la propiedad de metadatos de un recurso en el archivo CSV. Las propiedades de metadatos que faltan para ese recurso en particular no se actualizan.
