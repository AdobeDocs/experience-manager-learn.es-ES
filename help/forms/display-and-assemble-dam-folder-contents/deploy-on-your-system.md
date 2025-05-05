---
title: Implementar los recursos localmente
description: Implementación de los recursos del tutorial en la instancia local de AEM
feature: Adaptive Forms
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-01-04T00:00:00Z
exl-id: d23b51ba-1efb-4505-b5b3-44a02177e467
duration: 27
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '140'
ht-degree: 0%

---

# Implementar en el sistema

Siga los pasos que se indican a continuación para que este caso de uso funcione en su instancia local de AEM.

* [Implementar el paquete DevelopersWithServiceUser](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip?lang=es) contenido en el archivo zip.

* Agregue la siguiente entrada en el servicio de asignador de usuarios del servicio Apache Sling **DesarrollandoConUsuarioServicio.core:getformsresourceresolver=fd-service** mediante [configMgr](http://localhost:4502/system/console/configMgr).

* [Implementar el paquete de boletines](assets/Newsletters.core-1.0.0-SNAPSHOT.jar). Este paquete contiene el código para enumerar el contenido de la carpeta y combinar los boletines seleccionados.

* [Importe el paquete mediante el Administrador de paquetes](assets/newsletter.zip). Este paquete contiene una biblioteca de cliente y archivos PDF de ejemplo para probar la solución.

* [Importe el formulario adaptable de ejemplo](assets/sample-adaptive-form.zip). Este formulario muestra los boletines que se pueden seleccionar.

* [Vista previa del formulario](http://localhost:4502/content/dam/formsanddocuments/downloadarchivednewsletters/jcr:content?wcmmode=disabled).
Seleccione un par de boletines para descargar. Los boletines seleccionados se combinarán en un PDF y se le devolverán.
