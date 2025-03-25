---
title: Certificar documentos en AEM Forms
description: Usar el servicio Docassurance para certificar documentos de PDF en AEM Forms
feature: Document Security
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 1471929f-d269-4adc-88ad-2ad3682305e1
last-substantial-update: 2019-07-07T00:00:00Z
duration: 75
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '362'
ht-degree: 0%

---

# Certificar documentos en AEM Forms

Un documento certificado proporciona a los destinatarios de formularios y documentos de PDF garantías adicionales de su autenticidad e integridad.

Para certificar un documento, puede utilizar Acrobat DC en el escritorio o AEM Forms Document Services como parte de un proceso automatizado en un servidor.

Este artículo le proporciona un paquete OSGI de muestra para certificar documentos PDF mediante AEM Forms Document Services. El código utilizado en el ejemplo es [disponible aquí](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

Para certificar documentos con AEM Forms, se deben seguir los siguientes pasos

## Agregando certificado al almacén de confianza {#adding-certificate-to-trust-store}

Siga los pasos que se mencionan a continuación para agregar el certificado al repositorio de claves en AEM

* [Inicializar almacén de confianza global](http://localhost:4502/libs/granite/security/content/truststore.html)
* [Buscar usuario de fd-service](http://localhost:4502/security/users.html)
* **Tendrá que desplazar la página de resultados para cargar todos los usuarios y encontrar al usuario de fd-service**
* Haga doble clic en el usuario fd-service para abrir la ventana de configuración del usuario
* Haga clic en &quot;Agregar clave privada del archivo del almacén de claves&quot;. Especifique el alias y la contraseña específicos del certificado
  ![add-certificate](assets/adding-certificate-keystore.PNG)
* Guarde los cambios

## Creando servicio OSGI

Puede escribir su propio paquete OSGi y utilizar AEM Forms Client SDK para implementar un servicio para certificar documentos de PDF. Los siguientes vínculos serían útiles para escribir su propio paquete OSGi

* [Creando su primer paquete OSGi](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/developing/aem-project-archetype.html?lang=es)
* [Usar API de servicio de documentos](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

O puede utilizar el paquete de muestra incluido como parte de estos recursos de tutorial.

>[!NOTE]
>
>El paquete de ejemplo utiliza un alias denominado &quot;ares&quot; para certificar los documentos. Por lo tanto, asegúrese de que su alias se llame &quot;ares&quot; al utilizar este paquete

## Prueba de la muestra en el sistema local

* Descargar e instalar [Paquete de servicios de documentos personalizados](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* Descargar e instalar [Desarrollo con paquete de usuario de servicio](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [Asegúrese de haber agregado la siguiente entrada en el servicio de asignador de usuarios del servicio Apache Sling](http://localhost:4502/system/console/configMgr)
  **DesarrollandoConUsuarioServicio.core:getformsresourceresolver=fd-service** como se muestra en la captura de pantalla siguiente
  ![Asignador de usuarios](assets/user-mapper-service.PNG)
* [Importar formulario adaptable de ejemplo](assets/certify-pdf-af.zip)
* [Importar e instalar el envío personalizado](assets/custom-submit-certify.zip)
* [Abrir el formulario adaptable](http://localhost:4502/content/dam/formsanddocuments/certifypdf/jcr:content?wcmmode=disabled)
* Cargar un documento de PDF que deba certificarse
  **opcional** - Especifique el campo de firma que desea utilizar para certificar el documento
* Haga clic en enviar.
* PDF certificado debe devolverse a usted.
