---
title: Mostrar varios documentos pdf
description: Desplazarse por varios documentos pdf en un formulario adaptable.
version: 6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
jira: KT-10292
exl-id: c1d248c3-8208-476e-b0ae-cab25575cd6a
last-substantial-update: 2021-10-12T00:00:00Z
duration: 92
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '320'
ht-degree: 2%

---

# Mostrar varios documentos pdf en un carrusel

Un caso de uso común es mostrar varios documentos de PDF al usuario que rellena el formulario para revisarlos antes de enviarlo.

Para llevar a cabo este caso de uso, hemos utilizado el [API de incrustación de Adobe PDF](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html).

[Una demostración en directo de esta muestra se puede experimentar aquí.](https://forms.enablementadobe.com/content/dam/formsanddocuments/wefinancecreditcard/jcr:content?wcmmode=disabled)

Se han realizado los siguientes pasos para completar la integración

## Crear un componente personalizado para mostrar varios documentos de PDF

Se ha creado un componente personalizado (pdf-carousel) para recorrer en ciclo los documentos pdf

## Biblioteca de cliente

Se creó una biblioteca de cliente para mostrar los PDF mediante la API de incrustación de Adobe PDF. Los PDF que se van a mostrar se especifican en los componentes de carrusel pdf.

## Crear formulario adaptable

Crear un formulario adaptable basado en algunas pestañas (este ejemplo tiene 3 pestañas) Agregar algunos componentes de formulario adaptable en las dos primeras pestañas Agregar el componente de carrusel de PDF en la tercera pestaña Configurar el componente de carrusel de PDF como se muestra en la captura de pantalla siguiente
![pdf-carrusel](assets/pdf-carousel-af-component.png)

**Incrustar clave API de PDF** : Esta es la clave que puede utilizar para incrustar el pdf. Esta clave solo funcionará con localhost. Puede crear [su propia clave](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html) y asociarlo a otro dominio.

**Especificar documentos de PDF** - Aquí puede especificar los documentos pdf que desea que se muestren en el carrusel.


## Implementar el ejemplo en el servidor

Para probar esto en el servidor local, siga los pasos:

1. [Importar la biblioteca de cliente](assets/pdf-carousel-client-lib.zip) AEM en la instancia local de la [uso del administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp)
1. [Importar el componente de carrusel PDF](assets/pdf-carousel-component.zip) AEM en la instancia local de la [uso del administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp)
1. [Importar el formulario adaptable](assets/adaptive-form-pdf-carousel.zip) AEM en la instancia local de la [uso del administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp)
1. [Importe los archivos PDF de ejemplo para mostrarlos](assets/pdf-carousel-sample-documents.zip) AEM en la instancia local de la [uso del vínculo de carga de archivos de recursos](http://localhost:4502/assets.html/content/dam)
1. [Previsualizar formulario adaptable](http://localhost:4502/content/dam/formsanddocuments/wefinancecreditcard/jcr:content?wcmmode=disabled)
1. Vaya a la pestaña Documentos para revisar. Debería ver tres documentos de PDF en el componente de carrusel.
