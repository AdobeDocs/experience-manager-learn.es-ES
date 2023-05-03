---
title: Crear HTML5 Forms
description: Creación y configuración de formularios HTML5
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
kt: 4419
thumbnail: kt-4419.jpg
topic: Development
role: User
level: Beginner
exl-id: 67a01c41-d284-4518-adb5-21702e22ccfa
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 53af8fbc20ff21abf8778bbc165b5ec7fbdf8c8f
workflow-type: tm+mt
source-wordcount: '481'
ht-degree: 12%

---

# Creación de formularios de HTML5

HTML5 forms es una nueva funcionalidad en Adobe Experience Manager que ofrece la representación de plantillas de formulario XFA (xdp) en formato HTML5. Esta capacidad permite procesar formularios tanto en dispositivos móviles como en exploradores de equipos de escritorio no compatibles con PDF basados en XFA. Los formularios HTML5 no solo son compatibles con las capacidades existentes de las plantillas de formulario XFA, sino que también agregan capacidades nuevas para dispositivos móviles, como la firma manuscrita.

## Requisitos previos

Asegúrese de que tiene una instancia de AEM Forms en funcionamiento. Siga las [guía de instalación](https://experienceleague.adobe.com/docs/experience-manager-65/forms/install-aem-forms/osgi-installation/installing-configuring-aem-forms-osgi.html) para instalar y configurar AEM Forms

## Crear el primer formulario de HTML 5

1. [Descargar y extraer el contenido del archivo zip](assets/assets.zip). El archivo zip contiene xdp y el archivo de datos
2. [Navegar a Forms y documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
3. Haga clic en Crear -> Carga de archivo
4. Seleccione la plantilla xdp descargada en el paso 2

## Vista precia como HTML

La vista previa del xdp se puede realizar en formato HTML5 o PDF. Para obtener una vista previa del xdp en formato HTML5, siga los siguientes pasos

* Pulse en el xdp recién cargado y haga clic en _Vista previa -> Vista previa como HTML_. Debería ver el xdp representado como HTML5

>[!NOTE]
>Al seleccionar _Vista previa como PDF_ el PDF procesado no se mostrará en el explorador porque AEM Forms procesa archivos pdf dinámicos que requieren el complemento de Acrobat. Tendrá que descargar el PDF y abrirlo con Adobe Acrobat/Reader para verlo


## Vista previa con datos

Para obtener una vista previa del xdp en formato HTML5 con el archivo de datos, siga los siguientes pasos:

* Pulse en el xdp recién cargado y haga clic en _Vista previa -> Vista previa con datos_. Busque y seleccione el archivo de datos y haga clic en _Vista previa_.
* Debería ver la plantilla procesada en formato HTML5 previamente rellenada con los datos

## Explorar las propiedades avanzadas de la plantilla xdp

Las propiedades avanzadas de la plantilla xdp permiten especificar la fecha de publicación, el controlador de envío, el perfil de procesamiento del formulario, el servicio de cumplimentación previa, etc. Para ver las propiedades avanzadas de la plantilla, pulse en el xdp y haga clic en _propiedades -> Avanzadas_. Aquí encontrará varias propiedades. Algunas de estas propiedades están cubiertas aquí.

**Dirección URL de envío** - Esta es la dirección URL que administrará el envío del formulario de HTML5. En la siguiente lección trataremos esto. Si no se especifica una dirección URL de envío, se invoca el controlador de envío predeterminado, que devuelve los datos del formulario al explorador.

**Perfil de procesamiento del HTML** - Los formularios HTML5 tienen la noción de Perfiles que se exponen como extremos de REST para permitir el procesamiento móvil de plantillas de formulario. La mayoría de las veces el perfil de procesamiento predeterminado debe ser suficiente para procesar el formulario. Si el perfil de renderización predeterminado no se adapta a sus necesidades, [perfil personalizado](https://experienceleague.adobe.com/docs/experience-manager-65/forms/html5-forms/custom-profile.html) se puede crear y asociar al formulario.

**Servicio de precarga** - El servicio de cumplimentación previa suele utilizarse para rellenar el formulario con datos recuperados de un origen de datos de servidor.
