---
title: Seleccionar y descargar el contenido de la carpeta DAM
description: Implementar los recursos del tutorial en la instancia de AEM local
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-01-04T00:00:00Z
source-git-commit: a2bbb26751c9182056b4fe6d36eeeec964001df8
workflow-type: tm+mt
source-wordcount: '152'
ht-degree: 1%

---

# Implementar en el sistema

Siga los pasos que se indican a continuación para que este caso de uso funcione en la instancia de AEM local.

* [Configure para utilizar un usuario de fd-service siguiendo los pasos mencionados en este artículo](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html?lang=en). Asegúrese de haber implementado el paquete DevelopingWithServiceUser .

* [Implementar el paquete de boletines](assets/Newsletters.core-1.0.0-SNAPSHOT.jar). Este paquete contiene el código para enumerar el contenido de la carpeta y ensamblar el boletín seleccionado.

* [Importación del paquete mediante el Administrador de paquetes](assets/newsletter.zip). Este paquete contiene archivos pdf de muestra y biblioteca de cliente para probar la solución.

* [Importación del formulario adaptable de ejemplo](assets/sample-adaptive-form.zip). Este formulario muestra los boletines que se pueden seleccionar.

* [Obtener una vista previa del formulario](http://localhost:4502/content/dam/formsanddocuments/downloadarchivednewsletters/jcr:content?wcmmode=disabled).
Seleccione un par de boletines para descargar.Los boletines seleccionados se combinarán en un pdf y se le devolverán.




