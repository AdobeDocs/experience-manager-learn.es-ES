---
title: Creación de HTML5 Forms
description: Crear y configurar formularios HTML5
feature: Mobile Forms
doc-type: article
version: Experience Manager 6.5
jira: KT-4419
thumbnail: kt-4419.jpg
topic: Development
role: User
level: Beginner
exl-id: 67a01c41-d284-4518-adb5-21702e22ccfa
last-substantial-update: 2019-07-07T00:00:00Z
duration: 101
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '462'
ht-degree: 11%

---

# Creación de formularios HTML5

HTML5 forms es una nueva funcionalidad de Adobe Experience Manager que ofrece el procesamiento de plantillas de formulario XFA (xdp) en formato HTML5. Esta capacidad permite procesar formularios tanto en dispositivos móviles como en exploradores de equipos de escritorio no compatibles con PDF basados en XFA. Los formularios HTML5 no solo son compatibles con las capacidades existentes de las plantillas de formulario XFA, sino que también agregan capacidades nuevas para dispositivos móviles, como la firma manuscrita.

## Requisitos previos

Asegúrese de que tiene una instancia de trabajo de AEM Forms. Siga la [guía de instalación](https://experienceleague.adobe.com/docs/experience-manager-65/forms/install-aem-forms/osgi-installation/installing-configuring-aem-forms-osgi.html?lang=es) para instalar y configurar AEM Forms

## Creación de su primer formulario de HTML5

1. [Descargue y extraiga el contenido del archivo zip](assets/assets.zip). El archivo zip contiene un xdp y un archivo de datos
2. [Navegar a Forms y documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
3. Haga clic en Crear -> Cargar archivo
4. Seleccione la plantilla xdp descargada en el paso 2

## Vista precia como HTML

El xdp se puede previsualizar en formato HTML5 o en formato PDF. Para obtener una vista previa del xdp en formato HTML5, siga estos pasos

* Pulse el xdp recién cargado y haga clic en _Vista previa -> Vista previa como HTML_. Debería ver el xdp representado como HTML5

>[!NOTE]
>Al seleccionar la opción _Vista previa como PDF_, el PDF procesado no se mostrará en el explorador porque AEM Forms procesa archivos PDF dinámicos que requieren el complemento de Acrobat. Tendrá que descargar el PDF y abrirlo con Adobe Acrobat/Reader para verlo


## Vista previa con datos

Para obtener una vista previa del xdp en formato HTML5 con archivo de datos, siga estos pasos:

* Pulse el xdp recién cargado y haga clic en _Vista previa -> Vista previa con datos_. Examine y seleccione el archivo de datos y haga clic en _Vista previa_.
* Debería ver la plantilla representada en formato HTML5 rellenada previamente con los datos

## Explorar las propiedades avanzadas de la plantilla xdp

Las propiedades avanzadas de la plantilla xdp permiten especificar la fecha de publicación, el controlador de envío, el perfil de procesamiento del formulario, el servicio de relleno previo, etc. Para ver las propiedades avanzadas de la plantilla, pulse xdp y haga clic en _propiedades -> Avanzadas_. Aquí encontrará una serie de propiedades. Algunas de estas propiedades se tratan aquí.

**Enviar URL**: Esta es la URL que administrará el envío del formulario HTML5. Lo explicaremos en la siguiente lección. Si no se especifica una dirección URL de envío aquí, se invoca el controlador de envío predeterminado, que devuelve los datos del formulario al explorador.

**Perfil de procesamiento de HTML**: los formularios HTML5 tienen la noción Perfiles que se exponen como extremos REST para habilitar el procesamiento móvil de plantillas de formulario. La mayoría de las veces, el perfil de procesamiento predeterminado debe ser suficiente para procesar el formulario. Si el perfil de procesamiento predeterminado no satisface sus necesidades, se puede crear un [perfil personalizado](https://experienceleague.adobe.com/docs/experience-manager-65/forms/html5-forms/custom-profile.html) y asociarlo al formulario.

**Servicio de relleno previo**: el servicio de relleno previo se suele usar para rellenar el formulario con datos recuperados de una fuente de datos back-end.
