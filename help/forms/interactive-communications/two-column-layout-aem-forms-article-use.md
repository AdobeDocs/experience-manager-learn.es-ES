---
title: Creación de dos diseños de columna para documentos de canal de impresión
seo-title: Creación de dos diseños de columna para documentos de canal de impresión
description: Creación de dos diseños de columna para el documento de canal de impresión
seo-description: Creación de dos diseños de columna para el documento de canal de impresión
feature: comunicación interactiva
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '239'
ht-degree: 0%

---


# Dos diseños de columna en el documento de canal de impresión

Este breve artículo resaltará los pasos necesarios para crear un diseño de dos columnas en el canal de impresión. El caso de uso es generar 2 documentos de página con la página 1 con 2 columnas de diseño y la página 2 con el diseño estándar de 1 columna.

A continuación se indican los pasos de alto nivel necesarios para crear diseños de dos columnas con AEM Forms Designer.

* Crear 2 áreas de contenido en la página 1 página de formato
* Asigne a las dos áreas de contenido el nombre &quot;columna izquierda&quot; y &quot;columna derecha&quot;.
* Crear una segunda página de formato con un área de contenido (el valor predeterminado)
* Seleccione la ficha paginación (Subformulario sin título) (página 1) y (Subformulario sin título) (página 2) y defina las propiedades como se muestra en las capturas de pantalla siguientes.

![page1](assets/untitledsubform_paginationproperties.gif)

![page2](assets/untitled_subformpage2.gif)

Una vez definidas las propiedades de paginación, se pueden agregar subformularios o áreas de destino en (Subformulario sin título) (Página 1).

A continuación, se pueden agregar fragmentos de documento a estos subformularios o áreas de destino. Cuando la columna izquierda está llena, el contenido fluye a la columna derecha.

Para probar esto en el servidor local, descargue los recursos relacionados con este artículo. Desplácese hacia abajo hasta la parte inferior de esta página

* [Descargue e instale el documento de canal de impresión de ejemplo mediante el administrador de paquetes](assets/print-channel-with-two-column-layout.zip)
* [Vista previa del documento de canal de impresión](http://localhost:4502/content/dam/formsanddocuments/2columnlayout/jcr:content?channel=print&amp;mode=preview&amp;dataRef=service%3A%2F%2FFnDTestData&amp;wcmmode=disabled)
