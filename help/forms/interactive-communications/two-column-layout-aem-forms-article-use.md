---
title: Creación de dos diseños de columna para documentos de canal de impresión
description: Crear 2 diseños de columna para documento de canal de impresión
feature: Interactive Communication
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 416e3401-ba9f-4da3-8b07-2d39f9128571
last-substantial-update: 2019-07-07T00:00:00Z
duration: 49
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '218'
ht-degree: 0%

---

# Dos diseños de columna en un documento de canal de impresión

Este breve artículo resaltará los pasos necesarios para crear un diseño de 2 columnas en el canal Imprimir. El caso de uso es generar documentos de 2 páginas con una página 1 con un diseño de 2 columnas y una página 2 con un diseño de 1 columna estándar.

A continuación se indican los pasos de alto nivel necesarios para crear diseños de dos columnas con AEM Forms Designer.

* Crear 2 áreas de contenido en la página 1 página maestra
* Asigne a las 2 áreas de contenido el nombre &quot;left column&quot; y &quot;rightcolumn&quot;
* Crear una segunda página maestra con un área de contenido (esta es la predeterminada)
* Seleccione la pestaña paginación (Subformulario sin título) (página 1) y (Subformulario sin título) (página 2) y defina las propiedades como se muestra en las capturas de pantalla a continuación.

![página1](assets/untitledsubform_paginationproperties.gif)

![página2](assets/untitled_subformpage2.gif)

Una vez establecidas las propiedades de paginación, se pueden agregar subformularios o áreas de destino en (Subformulario sin título) (Página 1).

A continuación, podemos agregar fragmentos de documento a estos subformularios o áreas de destino. Cuando la columna izquierda esté llena, el contenido fluirá a la columna derecha.

Para probar esto en el servidor local, descargue los recursos relacionados con este artículo. Desplácese hacia abajo hasta la parte inferior de esta página

* [Descargue e instale un documento de canal de impresión de muestra mediante el administrador de paquetes](assets/print-channel-with-two-column-layout.zip)
* [Vista previa del documento de canal de impresión](http://localhost:4502/content/dam/formsanddocuments/2columnlayout/jcr:content?channel=print&amp;mode=preview&amp;dataRef=service%3A%2F%2FFnDTestData&amp;wcmmode=disabled)
