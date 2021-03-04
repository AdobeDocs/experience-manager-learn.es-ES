---
title: Crear formularios HTML5
description: Creación y configuración de formularios HTML5
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 4419
thumbnail: kt-4419.jpg
topic: Desarrollo
role: Profesional empresarial
level: Principiante
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '491'
ht-degree: 5%

---


# Creación de formularios HTML5

HTML5 forms es una nueva funcionalidad en Adobe Experience Manager que ofrece la representación de plantillas de formulario XFA (xdp) en formato HTML5. Esta capacidad permite procesar formularios tanto en dispositivos móviles como en el explorador de un ordenador de sobremesa no compatible con PDF basados en XFA. Los formularios HTML5 no solo admiten las capacidades existentes de las plantillas de formulario XFA, sino que también agregan nuevas funciones, como la firma de anotaciones, para dispositivos móviles.

## Requisitos previos

Asegúrese de tener una instancia de AEM Forms en funcionamiento. Siga la [guía de instalación](https://docs.adobe.com/content/help/en/experience-manager-65/forms/install-aem-forms/osgi-installation/installing-configuring-aem-forms-osgi.html) para instalar y configurar AEM Forms

## Crear el primer formulario HTML5

1. [Descargue y extraiga el contenido del archivo](assets/assets.zip) zip. El archivo zip contiene xdp y el archivo de datos
2. [Navegar a formularios y documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
3. Haga clic en Crear -> Carga de archivo
4. Seleccione la plantilla xdp descargada en el paso 2

## Vista previa como HTML

La vista previa del xdp se puede realizar en formato HTML5 o PDF. Para obtener una vista previa del xdp en formato HTML5, siga los siguientes pasos

* Pulse en el xdp recién cargado y haga clic en _Vista previa -> Vista previa como HTML_. Debería ver el xdp representado como HTML5

>[!NOTE]
>Cuando selecciona la opción _Vista previa como PDF_, el PDF procesado no se muestra en el explorador, ya que los AEM Forms procesan archivos pdf dinámicos que requieren el complemento de Acrobat. Tendrá que descargar el PDF y abrirlo con Adobe Acrobat/Reader para verlo


## Vista previa con datos

Para previsualizar el xdp en formato HTML5 con archivo de datos, siga los siguientes pasos:

* Pulse en el xdp recién cargado y haga clic en _Vista previa -> Vista previa con datos_. Busque y seleccione el archivo de datos y haga clic en _Preview_.
* Debería ver la plantilla procesada en formato HTML5 previamente rellenada con los datos

## Explorar las propiedades avanzadas de la plantilla xdp

Las propiedades avanzadas de la plantilla xdp permiten especificar la fecha de publicación, el controlador de envío, el perfil de procesamiento del formulario, el servicio de cumplimentación previa, etc. Para ver las propiedades avanzadas de la plantilla, pulse en el xdp y haga clic en _properties -> Advanced_. Aquí encontrará varias propiedades. Algunas de estas propiedades están cubiertas aquí.

**Dirección URL de envío** : es la dirección URL que gestiona el envío de formulario HTML5. En la siguiente lección trataremos esto. Si no se especifica una dirección URL de envío, se invoca el controlador de envío predeterminado, que devuelve los datos del formulario al explorador.

**Perfil de procesamiento HTML** : los formularios HTML5 tienen la noción de Perfiles que se exponen como extremos REST para permitir el procesamiento móvil de plantillas de formulario. La mayoría de las veces el perfil de procesamiento predeterminado debe ser suficiente para procesar el formulario. Si el perfil de renderización predeterminado no satisface sus necesidades, se puede crear un [perfil personalizado](https://docs.adobe.com/content/help/en/experience-manager-64/forms/html5-forms/custom-profile.html) y asociarlo al formulario.

**Servicio de relleno previo** : el servicio de cumplimentación previa suele utilizarse para rellenar el formulario con datos recuperados de un origen de datos de servidor.

