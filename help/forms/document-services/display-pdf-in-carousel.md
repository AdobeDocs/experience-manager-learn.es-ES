---
title: Mostrar varios documentos pdf
description: Desplazarse por varios documentos pdf en un formulario adaptable.
version: Experience Manager 6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
jira: KT-10292
exl-id: c1d248c3-8208-476e-b0ae-cab25575cd6a
last-substantial-update: 2021-10-12T00:00:00Z
duration: 66
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '320'
ht-degree: 2%

---

# Mostrar varios documentos pdf en un carrusel

Un caso de uso común es mostrar varios documentos de PDF al usuario que rellena el formulario para revisarlos antes de enviarlo.

Para llevar a cabo este caso de uso, se ha utilizado la [API de incrustación de Adobe PDF](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html).

[Aquí se puede experimentar una demostración en vivo de esta muestra.](https://forms.enablementadobe.com/content/dam/formsanddocuments/wefinancecreditcard/jcr:content?wcmmode=disabled)

Se han realizado los siguientes pasos para completar la integración

## Crear un componente personalizado para mostrar varios documentos de PDF

Se ha creado un componente personalizado (pdf-carousel) para recorrer en ciclo los documentos pdf

## Biblioteca de cliente

Se ha creado una biblioteca de cliente para mostrar los archivos PDF mediante la API de incrustación de Adobe PDF. Los PDF que se van a mostrar se especifican en los componentes de carrusel de PDF.

## Crear formulario adaptable

Crear un formulario adaptable basado en algunas pestañas (este ejemplo tiene 3 pestañas)
Agregar algunos componentes de formulario adaptable en las dos primeras pestañas
Añada el componente de carrusel pdf en la tercera pestaña
Configure el componente pdf-carrusel como se muestra en la captura de pantalla siguiente
![pdf-carousel](assets/pdf-carousel-af-component.png)

**Incrustar clave de API de PDF**: esta es la clave que puede usar para incrustar el PDF. Esta clave solo funcionará con localhost. Puede crear [su propia clave](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html) y asociarla a otro dominio.

**Especificar documentos de PDF**: aquí puede especificar los documentos PDF que desea que se muestren en el carrusel.


## Implementar el ejemplo en el servidor

Para probar esto en el servidor local, siga los pasos:

1. [Importe la biblioteca de cliente](assets/pdf-carousel-client-lib.zip) en su instancia de AEM local [mediante el administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp)
1. [Importe el componente de carrusel pdf](assets/pdf-carousel-component.zip) en su instancia local de AEM [mediante el administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp)
1. [Importe el formulario adaptable](assets/adaptive-form-pdf-carousel.zip) en su instancia de AEM local [mediante el administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp)
1. [Importe los archivos PDF de ejemplo para mostrar](assets/pdf-carousel-sample-documents.zip) en la instancia local de AEM [mediante el vínculo de carga de archivos de recursos](http://localhost:4502/assets.html/content/dam)
1. [Vista previa de formulario adaptable](http://localhost:4502/content/dam/formsanddocuments/wefinancecreditcard/jcr:content?wcmmode=disabled)
1. Vaya a la pestaña Documentos para revisar. Debería ver tres documentos de PDF en el componente de carrusel.
