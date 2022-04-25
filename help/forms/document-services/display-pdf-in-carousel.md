---
title: Mostrar varios documentos pdf
description: Recorra varios documentos pdf en forma adaptable.
version: 6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
kt: 10292
source-git-commit: cb5b3eb77a57fa8a2918710b7dbcd1b0a58b74bd
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 4%

---

# Mostrar varios documentos pdf en un carrusel

Un caso de uso común es mostrar varios documentos de PDF al usuario que rellena el formulario para revisarlos antes de enviarlo.

Para lograr este caso de uso, hemos utilizado la variable [API de incrustación de Adobe PDF](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html).

[Una demostración en vivo de esta muestra se puede experimentar aquí.](https://forms.enablementadobe.com/content/dam/formsanddocuments/wefinancecreditcard/jcr:content?wcmmode=disabled)

Se realizaron los siguientes pasos para completar la integración

## Crear un componente personalizado para mostrar varios documentos del PDF

Se creó un componente personalizado (pdf-carousel) para recorrer los documentos pdf

## Biblioteca de cliente

Se creó una biblioteca de cliente para mostrar los PDF mediante la API de incrustación de Adobe PDF. Los PDF que se van a mostrar se especifican en los componentes pdf-carrusel.

## Crear formulario adaptable

Crear un formulario adaptable basado en algunas pestañas (Este ejemplo tiene 3 fichas) Añadir algunos componentes de formulario adaptable en las dos primeras pestañas Añadir el componente de carrusel pdf en la tercera pestaña Configurar el componente de carrusel pdf como se muestra en la captura de pantalla siguiente
![pdf-carousel](assets/pdf-carousel-af-component.png)

**Incrustar clave de API del PDF** - Esta es la clave que puede utilizar para incrustar el pdf. Esta clave solo funciona con localhost. Puede crear [su propia clave](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html) y asociarlo a otro dominio.

**Especificar documentos del PDF** - Aquí puede especificar los documentos pdf que desea que se muestren en el carrusel.


## Implementar el ejemplo en el servidor

Para probar esto en el servidor local, siga los pasos:

1. [Importar la biblioteca del cliente](assets/pdf-carousel-client-lib.zip) en la instancia de AEM local [uso del administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp)
1. [Importación del componente de carrusel pdf](assets/pdf-carousel-component.zip) en la instancia de AEM local [uso del administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp)
1. [Importar el formulario adaptable ](assets/adaptive-form-pdf-carousel.zip) en la instancia de AEM local [uso del administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp)
1. [Importe los pdf de muestra que desea mostrar](assets/pdf-carousel-sample-documents.zip) en la instancia de AEM local [uso del vínculo de carga del archivo de recursos](http://localhost:4502/assets.html/content/dam)
1. [Vista previa del formulario adaptable](http://localhost:4502/content/dam/formsanddocuments/wefinancecreditcard/jcr:content?wcmmode=disabled)
1. Tabule en la ficha Documentos para revisar . Debería ver tres documentos PDF en el componente carrusel.
