---
title: Uso de la importación y exportación de metadatos en AEM Assets
description: Obtenga información sobre cómo utilizar las funciones de importación y exportación de metadatos de Adobe Experience Manager Assets. Las funciones de importación y exportación permiten a los autores de contenido actualizar de forma masiva los metadatos de los recursos existentes.
version: 6.4, 6.5, Cloud Service
topic: Content Management
feature: Metadata
role: Admin
level: Intermediate
kt: 647, 917
thumbnail: 22132.jpg
exl-id: 0681e2c4-8661-436c-9170-9aa841a6fa27
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '260'
ht-degree: 3%

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
* El valor predeterminado de la primera columna es `assetPath`, que contiene la ruta JCR absoluta para un recurso.

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
* Todas son válidas [Tipos de propiedades JCR](https://www.adobe.io/experience-manager/reference-materials/spec/jsr170/javadocs/jcr-2.0/javax/jcr/PropertyType.html) son compatibles

* Formato de propiedad de varios valores - `<metadata property name> {{<property type : MULTI }}`

### Segunda fila a filas N

* La primera columna contiene la ruta JCR absoluta para un recurso. Por ejemplo: /content/dam/asset1.jpg
* Puede que falten valores en la propiedad de metadatos de un recurso en el archivo CSV. Las propiedades de metadatos que faltan para ese recurso en particular no se actualizan.
