---
title: Uso de la importación y exportación de metadatos en AEM Assets
description: Obtenga información sobre cómo utilizar las funciones de importación y exportación de metadatos de Adobe Experience Manager Assets. Las funciones de importación y exportación permiten a los autores de contenido actualizar de forma masiva los metadatos de los recursos existentes.
version: Experience Manager 6.4, Experience Manager 6.5, Experience Manager as a Cloud Service
topic: Content Management
feature: Metadata
role: Admin
level: Intermediate
kt: 647, 917
thumbnail: 22132.jpg
last-substantial-update: 2022-06-13T00:00:00Z
doc-type: Feature Video
exl-id: 0681e2c4-8661-436c-9170-9aa841a6fa27
duration: 419
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '378'
ht-degree: 1%

---

# Uso de la importación y exportación de metadatos en AEM Assets {#metadata-import-and-export}

Obtenga información sobre cómo utilizar las funciones de importación y exportación de metadatos de Adobe Experience Manager Assets. Las funciones de importación y exportación permiten a los autores de contenido actualizar de forma masiva los metadatos de los recursos existentes.

## Exportación de metadatos {#metadata-export}

>[!VIDEO](https://video.tv.adobe.com/v/22132?quality=12&learn=on)

>[!TIP]
>
> Al abrir el archivo CSV de exportación de metadatos en Excel, utilice [Excel importer](https://support.microsoft.com/en-us/office/import-data-from-a-csv-html-or-text-file-b62efe49-4d5b-4429-b788-e1211b5e90f6) en lugar de hacer doble clic en el archivo para evitar problemas con los archivos CSV codificados con UTF-8.
>
> Para abrir el archivo CSV de exportación de metadatos en Excel, siga estos pasos:
> 
> 1. Abra Microsoft Excel
> 1. Seleccione __Archivo > Nuevo__ para crear una hoja de cálculo vacía
> 1. Con la hoja de cálculo vacía abierta, seleccione __Archivo > Importar__
> 1. Seleccione el archivo __Text__ y haga clic en __Importar__
> 1. Seleccione el archivo CSV exportado del sistema de archivos y haga clic en __Obtener datos__
> 1. En el paso 1 del asistente para importar, seleccione __Delimitado__, establezca __Origen de archivo__ en __Unicode (UTF-8)__ y haga clic en __Siguiente__
> 1. En el paso 2, establezca los __Delimitadores__ en __Coma__ y haga clic en __Siguiente__
> 1. En el paso 3, deje el __formato de datos de columna__ tal cual y haga clic en __Finalizar__
> 1. Seleccione __Importar__ para agregar los datos a la hoja de cálculo

## Importación de metadatos {#metadata-import}

>[!VIDEO](https://video.tv.adobe.com/v/21374?quality=12&learn=on)

>[!NOTE]
>
> Al preparar un archivo CSV para importar, es más fácil generar un CSV con la lista de recursos mediante la función de exportación de metadatos. A continuación, puede modificar el archivo CSV generado e importarlo mediante la función Importar.

## Formato de archivo CSV de metadatos {#metadata-file-format}

### Primera fila

* La primera fila del archivo CSV define el esquema de metadatos.
* El valor predeterminado de la primera columna es `assetPath`, que contiene la ruta JCR absoluta de un recurso.

* Las columnas posteriores de la primera fila apuntan a otras propiedades de metadatos de un recurso.
   * Por ejemplo: `dc:title, dc:description, jcr:title`

* Formato de propiedad de valor único

   * `<metadata property name> {{<property type}}`
   * Si no se especifica el tipo de propiedad, el valor predeterminado es Cadena.
   * Por ejemplo: `dc:title {{String}}`

* El nombre de propiedad distingue entre mayúsculas y minúsculas
   * Correcto: `dc:title {{String}}`
   * Incorrecto: `Dc:Title {{String}}`

* El tipo de propiedad no distingue entre mayúsculas y minúsculas
* Se admiten todos los [tipos de propiedad JCR](https://www.adobe.io/experience-manager/reference-materials/spec/jsr170/javadocs/jcr-2.0/javax/jcr/PropertyType.html) válidos

* Formato de propiedad de varios valores: `<metadata property name> {{<property type : MULTI }}`

### Fila segunda a N filas

* La primera columna contiene la ruta JCR absoluta de un recurso. Por ejemplo: /content/dam/asset1.jpg
* Es posible que falten valores en la propiedad de metadatos de un recurso en el archivo CSV. Las propiedades de metadatos que faltan para ese recurso en particular no se actualizan.
