---
title: Implementar los recursos localmente
description: Implementar los recursos del tutorial en la instancia de AEM local
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-01-04T00:00:00Z
source-git-commit: ddef90067d3ae4a3c6a705b5e109e474bab34f6d
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 5%

---

# Implementar en el sistema

Siga los pasos que se indican a continuación para que este caso de uso funcione en la instancia de AEM local.

* [Implementar el paquete DevelopingWithServiceUser](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip) contenido en el archivo zip.

* Añada la siguiente entrada en el servicio de asignador de usuarios del servicio Apache Sling **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service** usando la variable [configMgr](http://localhost:4502/system/console/configMgr).

* [Implementar el paquete de boletines](assets/Newsletters.core-1.0.0-SNAPSHOT.jar). Este paquete contiene el código para enumerar el contenido de la carpeta y ensamblar el boletín seleccionado.

* [Importación del paquete mediante el Administrador de paquetes](assets/newsletter.zip). Este paquete contiene archivos pdf de muestra y biblioteca de cliente para probar la solución.

* [Importación del formulario adaptable de ejemplo](assets/sample-adaptive-form.zip). Este formulario muestra los boletines que se pueden seleccionar.

* [Obtener una vista previa del formulario](http://localhost:4502/content/dam/formsanddocuments/downloadarchivednewsletters/jcr:content?wcmmode=disabled).
Seleccione un par de boletines para descargar.Los boletines seleccionados se combinarán en un pdf y se le devolverán.




