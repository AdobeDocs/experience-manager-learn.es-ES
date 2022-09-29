---
title: Información general sobre la configuración del formulario adaptable al déclencheur AEM flujo de trabajo
description: Configurar las opciones de carga útil al activar AEM flujo de trabajo al enviar el formulario
sub-product: forms
feature: Workflow
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
kt: 5407
thumbnail: 40258.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 9f1dbd02-774a-4b84-90fa-02d4e468cbac
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '165'
ht-degree: 3%

---

# Configuración del formulario adaptable al flujo de trabajo AEM déclencheur

## Requisitos previos

El formulario de ejemplo utilizado en este flujo de trabajo se basa en una plantilla de formulario adaptable personalizada que debe importarse en el servidor de AEM. El formulario de ejemplo que se proporciona debe importarse después de importar la plantilla.

### Obtener las plantillas de formulario adaptables

* Descargar [Plantilla de formulario adaptable](assets/af-form-template.zip)
* [Importación de la plantilla mediante el administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp)
* Cargar e instalar la plantilla de formulario adaptable

### Obtener el formulario adaptable de ejemplo

* Descargar [Formulario adaptable](assets/peak-application-form.zip)
* Vaya a [Formularios Y Documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Haga clic en Crear -> Cargar archivo
* El formulario adaptable de ejemplo se coloca en una carpeta llamada [Forms de aplicaciones](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments/applicationforms)

En el siguiente vídeo se explica cómo configurar un formulario adaptable para el déclencheur de un flujo de trabajo AEM
>[!VIDEO](https://video.tv.adobe.com/v/40258/?quality=9&learn=on)

El siguiente vídeo muestra la carga útil del flujo de trabajo y otros detalles en el repositorio crx

>[!VIDEO](https://video.tv.adobe.com/v/40259/?quality=9&learn=on)
