---
title: Importación de datos de un archivo de PDF a un formulario adaptable
description: Tutorial para rellenar un formulario adaptable mediante la importación de un archivo de PDF
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
source-git-commit: 17ab178f385619b589a9dde6089410bfa4515ffa
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 0%

---

# Implementar los recursos de muestra

Puede implementar los recursos de ejemplo para que esta solución funcione en su instancia local de AEM Forms

* [Importe la biblioteca de cliente y el componente personalizado para cargar el formulario pdf a través del Administrador de paquetes](./assets/client-libs-custom-component.zip)
* Descargue e implemente el paquete mediante la consola web OSGi[Paquete de servicios de documentos personalizados](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* Descargue e implemente el paquete mediante la consola web OSGi [Desarrollo con paquete de usuario de servicio](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* Descargue e implemente el paquete mediante la consola web OSGi[importación de datos desde archivo pdf](./assets/onlineToOffline.core-1.0.0-SNAPSHOT.jar)
* Agregar la entrada _**DesarrollarWithServiceUser.core:getresourceresolver=data**_ en el _**Servicio de asignador de usuarios del servicio Apache Sling**_ Consola de configuración OSGi