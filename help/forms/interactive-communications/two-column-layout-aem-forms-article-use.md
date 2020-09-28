---
title: Creación de dos diseños de columna para documentos de canal de impresión
seo-title: Creación de dos diseños de columna para documentos de canal de impresión
description: Creación de dos diseños de columna para documento de canal de impresión
seo-description: Creación de dos diseños de columna para documento de canal de impresión
feature: interactive-communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: 314f798f7a80f9c554e5bea052f8a64ae397d0de
workflow-type: tm+mt
source-wordcount: '238'
ht-degree: 0%

---


# Dos diseños de columna en el Documento Imprimir Canal

Este breve artículo resalta los pasos necesarios para crear una maquetación de dos columnas en el canal de impresión. El caso de uso es generar 2 documentos de página con la página 1 con el diseño de 2 columnas y la página 2 con el diseño de 1 columna estándar.

A continuación se indican los pasos de alto nivel necesarios para crear dos diseños de columna con AEM Forms Designer.

* Crear 2 áreas de contenido en la página de formato 1
* Asigne a las 2 áreas de contenido el nombre &quot;columna izquierda&quot; y &quot;columna derecha&quot;
* Crear una segunda página de formato con un área de contenido (este es el valor predeterminado)
* Seleccione la ficha paginación (Subformulario sin título) (página 1) y (Subformulario sin título) (página 2) y defina las propiedades como se muestra en las capturas de pantalla siguientes.

![page1](assets/untitledsubform_paginationproperties.gif)

![page2](assets/untitled_subformpage2.gif)

Una vez definidas las propiedades de paginación, podemos agregar subformularios o áreas de destinatario en (Subformulario sin título) (Página 1).

A continuación, podemos agregar fragmentos de documento a estos subformularios o áreas de destinatario. Cuando la columna izquierda está llena, el contenido fluirá a la columna derecha.

Para probar esto en el servidor local, descargue los recursos relacionados con este artículo. Desplácese hacia abajo hasta la parte inferior de esta página

* [Descargar e instalar el Documento de Canal de impresión de muestra mediante el administrador de paquetes](assets/print-channel-with-two-column-layout.zip)
* [Previsualización del Documento del Canal de impresión](http://localhost:4502/content/dam/formsanddocuments/2columnlayout/jcr:content?channel=print&amp;mode=preview&amp;dataRef=service%3A%2F%2FFnDTestData&amp;wcmmode=disabled)
