---
title: Certificación de documento en AEM Forms
description: Uso del servicio Docsurance para certificar documentos de PDF en AEM Forms
feature: Document Security
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 1471929f-d269-4adc-88ad-2ad3682305e1
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '400'
ht-degree: 1%

---

# Certificación de documento en AEM Forms

Un documento certificado proporciona a los destinatarios de documentos PDF y formularios garantías adicionales de su autenticidad e integridad.

Para certificar un documento, puede utilizar Acrobat DC en el escritorio o AEM Forms Document Services como parte de un proceso automatizado en un servidor.

Este artículo le proporciona un paquete OSGI de muestra para certificar documentos pdf mediante AEM Forms Document Services. El código utilizado en el ejemplo es [disponible aquí](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

Para certificar documentos mediante AEM Forms, se deben seguir los siguientes pasos

## Añadir certificado al almacén de confianza {#adding-certificate-to-trust-store}

Siga los pasos que se indican a continuación para agregar el certificado al almacén de claves en AEM

* [Inicializar el almacén de confianza global](http://localhost:4502/libs/granite/security/content/truststore.html)
* [Buscar fd-service](http://localhost:4502/security/users.html) usuario
* **Tendrá que desplazarse por la página de resultados para cargar todos los usuarios y encontrar el usuario del fd-service**
* Haga doble clic en el usuario de fd-service para abrir la ventana de configuración del usuario
* Haga clic en &quot;Agregar clave privada del archivo del almacén de claves&quot;. Especifique el alias y la contraseña específicos de su certificado
   ![add-certificate](assets/adding-certificate-keystore.PNG)
* Guarde los cambios

## Creación del servicio OSGI

Puede escribir su propio paquete OSGi y utilizar el SDK de cliente de AEM Forms para implementar un servicio de certificación de documentos de PDF. Los siguientes vínculos serían útiles para escribir su propio paquete OSGi

* [Creación de su primer paquete OSGi](https://helpx.adobe.com/experience-manager/using/maven_arch13.html)
* [Usar la API del servicio de documentos](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

O puede utilizar el paquete de muestra incluido como parte de estos recursos de tutorial.

>[!NOTE]
>
>El paquete de muestra utiliza un alias llamado &quot;ares&quot; para certificar los documentos. Por lo tanto, asegúrese de que su alias se llame &quot;ares&quot; al utilizar este paquete

## Prueba del ejemplo en el sistema local

* Descargar e instalar [Paquete de servicios de documentos personalizados](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* Descargar e instalar [Desarrollo con Service User Bundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [Asegúrese de haber añadido la siguiente entrada en el servicio de asignador de usuarios del servicio Apache Sling](http://localhost:4502/system/console/configMgr)

   **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service** como se muestra en la captura de pantalla siguiente
   ![User-Mapper](assets/user-mapper-service.PNG)
* [Importar formulario adaptable de ejemplo](assets/certify-pdf-af.zip)
* [Importar e instalar el envío personalizado](assets/custom-submit-certify.zip)
* [Abrir el formulario adaptable](http://localhost:4502/content/dam/formsanddocuments/certifypdf/jcr:content?wcmmode=disabled)
* Cargar documento de PDF que debe certificarse
   **opcional** - Especifique el campo de firma que desea utilizar para certificar el documento
* Haga clic en enviar.
* El PDF certificado debe devolverse a usted.
