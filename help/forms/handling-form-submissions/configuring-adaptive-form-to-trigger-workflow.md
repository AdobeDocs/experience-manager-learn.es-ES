---
title: Información general sobre la configuración del formulario adaptable al flujo de trabajo AEM de déclencheur
description: Configurar las opciones de carga útil al activar el flujo de trabajo de AEM al enviar el formulario
feature: Workflow
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-5407
thumbnail: 40258.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 9f1dbd02-774a-4b84-90fa-02d4e468cbac
last-substantial-update: 2020-07-07T00:00:00Z
duration: 573
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '148'
ht-degree: 2%

---

# Configurar formularios adaptables para almacenar en déclencheur el flujo de trabajo de AEM

## Requisitos previos

El formulario de ejemplo utilizado en este flujo de trabajo se basa en una plantilla de formulario adaptable personalizada que debe importarse en el servidor de AEM. El formulario de ejemplo proporcionado debe importarse después de importar la plantilla.

### Obtener las plantillas de formulario adaptable

* Descargar [plantilla de formulario adaptable](assets/af-form-template.zip)
* [Importe la plantilla mediante el administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp)
* Cargar e instalar la plantilla de formulario adaptable

### Obtener el formulario adaptable de ejemplo

* Descargar [formulario adaptable](assets/peak-application-form.zip)
* Examinar [Formulario Y Documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Haga clic en Crear -> Cargar archivo
* El formulario adaptable de ejemplo se coloca en una carpeta llamada [Application Forms](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments/applicationforms)

En el siguiente vídeo se explica cómo configurar un formulario adaptable para almacenar en déclencheur un flujo de trabajo de AEM
>[!VIDEO](https://video.tv.adobe.com/v/40258?quality=12&learn=on)

El siguiente vídeo muestra la carga útil del flujo de trabajo y otros detalles en el repositorio crx

>[!VIDEO](https://video.tv.adobe.com/v/40259?quality=12&learn=on)
