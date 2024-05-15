---
title: Implementar los recursos localmente
description: AEM Implementación de los recursos del tutorial en la instancia local de
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-01-04T00:00:00Z
exl-id: d23b51ba-1efb-4505-b5b3-44a02177e467
duration: 27
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '140'
ht-degree: 0%

---

# Implementar en el sistema

AEM Siga los pasos que se indican a continuación para que este caso de uso funcione en su instancia de local.

* [Implementar el paquete DevelopersWithServiceUser](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip) incluido en el archivo zip.

* Agregue la siguiente entrada en el servicio asignador de usuarios del servicio Apache Sling **DesarrollarWithServiceUser.core:getformsresourceresolver=fd-service** uso del [configMgr](http://localhost:4502/system/console/configMgr).

* [Implementar el paquete de boletines](assets/Newsletters.core-1.0.0-SNAPSHOT.jar). Este paquete contiene el código para enumerar el contenido de la carpeta y combinar los boletines seleccionados.

* [Importe el paquete mediante el Administrador de paquetes](assets/newsletter.zip). Este paquete contiene una biblioteca de cliente y archivos PDF de ejemplo para probar la solución.

* [Importar el formulario adaptable de ejemplo](assets/sample-adaptive-form.zip). Este formulario muestra los boletines que se pueden seleccionar.

* [Previsualización del formulario](http://localhost:4502/content/dam/formsanddocuments/downloadarchivednewsletters/jcr:content?wcmmode=disabled).
Seleccione un par de boletines para descargar. Los boletines seleccionados se combinarán en un PDF y se le devolverán.
