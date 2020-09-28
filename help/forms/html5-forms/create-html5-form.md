---
title: Crear HTML5 Forms
description: Creación y configuración de formularios HTML5
feature: mobile-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 4419
thumbnail: kt-4419.jpg
translation-type: tm+mt
source-git-commit: c60a46027cc8d71fddd41aa31dbb569e4df94823
workflow-type: tm+mt
source-wordcount: '485'
ht-degree: 4%

---


# Creación de formularios HTML5

Los formularios HTML5 son una nueva función de Adobe Experience Manager que oferta la representación de plantillas de formulario XFA (xdp) en formato HTML5. Esta capacidad permite procesar formularios tanto en dispositivos móviles como en el explorador de un ordenador de sobremesa no compatible con PDF basados en XFA. Los formularios HTML5 no sólo admiten las capacidades existentes de las plantillas de formulario XFA, sino que también agregan nuevas funciones, como la firma de garabatos, para dispositivos móviles.

## Requisitos previos

Asegúrese de que tiene una instancia de AEM Forms que funciona correctamente. Siga la guía [de](https://docs.adobe.com/content/help/en/experience-manager-65/forms/install-aem-forms/osgi-installation/installing-configuring-aem-forms-osgi.html) instalación para instalar y configurar AEM Forms

## Crear el primer formulario HTML5

1. [Descargue y extraiga el contenido del archivo](assets/assets.zip)zip. El archivo zip contiene xdp y el archivo de datos
2. [Navegar a Forms y Documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
3. Haga clic en Crear -> Carga de archivo
4. Seleccione la plantilla xdp descargada en el paso 2

## Previsualización como HTML

El xdp se puede previsualizar en formato HTML5 o PDF. Para previsualización del xdp en formato HTML5, siga los siguientes pasos

* Toque en el xdp recién cargado y haga clic en _Previsualización -> Previsualización como HTML_. Debería ver el xdp procesado como HTML5

>[!NOTE]
>Al seleccionar la opción _Previsualización como PDF_ , el PDF procesado no se mostrará en el navegador porque AEM Forms procesa archivos PDF dinámicos que requieren el complemento Acrobat.Tendrá que descargar el PDF y abrirlo con Adobe Acrobat/Reader para vista


## Vista previa con datos

Para previsualización de xdp en formato HTML5 con archivo de datos, siga los siguientes pasos:

* Toque en el xdp recién cargado y haga clic en _Previsualización -> Previsualización con datos_. Busque y seleccione el archivo de datos y haga clic en la _Previsualización_.
* Debe ver la plantilla procesada en formato HTML5 previamente rellenada con los datos

## Explorar las propiedades avanzadas de la plantilla xdp

Las propiedades avanzadas de la plantilla xdp permiten especificar la fecha de publicación, el controlador de envío, el perfil de procesamiento del formulario, el servicio de cumplimentación previa, etc. Para realizar la vista de las propiedades avanzadas de la plantilla, toque el xdp y haga clic en _propiedades -> Avanzadas_. Aquí encontrará una serie de propiedades. Algunas de estas propiedades se tratan aquí.

**Enviar URL** : es la URL que gestionará el envío de formulario HTML5. En la próxima lección trataremos esto. Si no se especifica una URL de envío aquí, se invoca el controlador de envío predeterminado, que devuelve los datos del formulario al explorador.

**PERFIL** de procesamiento HTML: los formularios HTML5 tienen la noción de Perfiles que se exponen como extremos REST para habilitar el procesamiento móvil de las plantillas de formulario. La mayoría de las veces el perfil de procesamiento predeterminado debe ser suficiente para procesar el formulario. Si el perfil de procesamiento predeterminado no satisface sus necesidades, se puede crear un perfil [](https://docs.adobe.com/content/help/en/experience-manager-64/forms/html5-forms/custom-profile.html) personalizado y asociarlo al formulario.

**Servicio** de cumplimentación previa: el servicio de cumplimentación previa se suele utilizar para rellenar el formulario con datos recuperados de un origen de datos de back-end.

