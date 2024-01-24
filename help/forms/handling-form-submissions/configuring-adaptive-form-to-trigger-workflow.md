---
title: Información general sobre la configuración del formulario adaptable a déclencheur AEM flujo de trabajo
description: AEM Configurar las opciones de carga útil al activar el flujo de trabajo de la al enviar el formulario
feature: Workflow
doc-type: article
version: 6.4,6.5
jira: KT-5407
thumbnail: 40258.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 9f1dbd02-774a-4b84-90fa-02d4e468cbac
last-substantial-update: 2020-07-07T00:00:00Z
duration: 578
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '148'
ht-degree: 2%

---

# Configuración del formulario adaptable para el flujo de trabajo de déclencheur AEM

## Requisitos previos

AEM El formulario de ejemplo que se utiliza en este flujo de trabajo se basa en una plantilla de formulario adaptable personalizada que debe importarse en el servidor de la. El formulario de ejemplo proporcionado debe importarse después de importar la plantilla.

### Obtener las plantillas de formulario adaptable

* Descargar [Plantilla de formulario adaptable](assets/af-form-template.zip)
* [Importar la plantilla mediante el administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp)
* Cargar e instalar la plantilla de formulario adaptable

### Obtener el formulario adaptable de ejemplo

* Descargar [Formulario adaptable](assets/peak-application-form.zip)
* Navegar a [Formularios Y Documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Haga clic en Crear -> Cargar archivo
* El formulario adaptable de ejemplo se coloca en una carpeta llamada [Forms de aplicaciones](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments/applicationforms)

En el siguiente vídeo, se explica cómo configurar un formulario adaptable para almacenar en déclencheur AEM un flujo de trabajo de
>[!VIDEO](https://video.tv.adobe.com/v/40258?quality=12&learn=on)

El siguiente vídeo muestra la carga útil del flujo de trabajo y otros detalles en el repositorio crx

>[!VIDEO](https://video.tv.adobe.com/v/40259?quality=12&learn=on)
