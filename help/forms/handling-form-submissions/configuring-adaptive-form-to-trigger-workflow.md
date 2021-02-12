---
title: Configuración del flujo de trabajo de formulario adaptable a déclencheur AEM
description: Configurar las opciones de carga útil al activar AEM flujo de trabajo al enviar el formulario
sub-product: formularios
feature: workflow
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
kt: 5407
thumbnail: 40258.jpg
translation-type: tm+mt
source-git-commit: 449202af47b6bbcd9f860d5c5391d1f7096d489e
workflow-type: tm+mt
source-wordcount: '166'
ht-degree: 0%

---


# Configuración del flujo de trabajo de formulario adaptable a déclencheur AEM

## Requisitos previos

El formulario de ejemplo utilizado en este flujo de trabajo se basa en una plantilla de formulario adaptable personalizada que debe importarse en el servidor de AEM. El formulario de ejemplo proporcionado debe importarse después de importar la plantilla.

### Obtener las plantillas de formulario adaptables

* Descargar [Plantilla de formulario adaptable](assets/af-form-template.zip)
* [Importar la plantilla mediante el administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp)
* Cargar e instalar la plantilla Formulario adaptable

### Obtener el formulario adaptable de ejemplo

* Descargar [Formulario adaptable](assets/peak-application-form.zip)
* Vaya a [Formulario y Documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Haga clic en Crear -> Carga de archivo
* El formulario adaptable de ejemplo se colocará en una carpeta denominada [Aplicación Forms](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments/applicationforms)

En el siguiente vídeo se explica cómo configurar un formulario adaptable para el déclencheur de un flujo de trabajo AEM
>[!VIDEO](https://video.tv.adobe.com/v/40258/?quality=9&learn=on)

El siguiente vídeo muestra la carga útil del flujo de trabajo y otros detalles en el repositorio crx

>[!VIDEO](https://video.tv.adobe.com/v/40259/?quality=9&learn=on)


