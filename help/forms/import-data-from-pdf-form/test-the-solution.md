---
title: Importación de datos de un archivo PDF a un formulario adaptable
description: Tutorial para rellenar un formulario adaptable mediante la importación de un archivo PDF
feature: Adaptive Forms
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
exl-id: f21753b2-f065-4556-add4-b1983fb57031
duration: 21
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 0%

---

# Implementar los recursos de muestra

Puede implementar los recursos de ejemplo para que esta solución funcione en su instancia local de AEM Forms

* [Importe la biblioteca de cliente y el componente personalizado para cargar el formulario pdf a través del Administrador de paquetes](./assets/client-libs-custom-component.zip)
* Descargue e implemente el paquete usando la consola web de OSGi [Paquete de servicios de documentos personalizados](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* Descargue e implemente el paquete usando la consola web de OSGi [Desarrollo con el paquete de usuario de servicio](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* Descargue e implemente el paquete usando la consola web de OSGi[importar datos del archivo pdf](./assets/onlineToOffline.core-1.0.0-SNAPSHOT.jar)
* Agregue la entrada _**DesarrollandoConUsuarioServicio.core:getresourceresolver=data**_ en el _**Servicio de asignador de usuarios de Apache Sling Service**_ consola de configuración de OSGi
