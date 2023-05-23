---
title: Certificar documentos en AEM Forms
description: Uso del servicio Docassurance para certificar documentos de PDF en AEM Forms
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
ht-degree: 3%

---

# Certificar documentos en AEM Forms

Un documento certificado proporciona a los destinatarios de formularios y documentos del PDF garantías adicionales de su autenticidad e integridad.

Para certificar un documento, puede utilizar Acrobat DC en el escritorio o AEM Forms Document Services como parte de un proceso automatizado en un servidor.

Este artículo proporciona un paquete OSGI de ejemplo para certificar documentos PDF mediante AEM Forms Document Services. El código utilizado en el ejemplo es [disponible aquí](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

Para certificar documentos con AEM Forms, se deben seguir los siguientes pasos

## Agregando certificado al almacén de confianza {#adding-certificate-to-trust-store}

AEM Siga los pasos que se mencionan a continuación para agregar el certificado al repositorio de claves en el área de nombres de la

* [Inicializar almacén de confianza global](http://localhost:4502/libs/granite/security/content/truststore.html)
* [Buscar fd-service](http://localhost:4502/security/users.html) usuario
* **Tendrá que desplazarse por la página de resultados para cargar todos los usuarios y encontrar al usuario de fd-service**
* Haga doble clic en el usuario fd-service para abrir la ventana de configuración del usuario
* Haga clic en &quot;Agregar clave privada del archivo del almacén de claves&quot;. Especifique el alias y la contraseña específicos del certificado
   ![add-certificate](assets/adding-certificate-keystore.PNG)
* Guarde los cambios

## Creando servicio OSGI

Puede escribir su propio paquete OSGi y utilizar el AEM Forms Client SDK para implementar un servicio para certificar documentos de PDF. Los siguientes vínculos serían útiles para escribir su propio paquete OSGi

* [Creación de su primer paquete OSGi](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/developing/aem-project-archetype.html?lang=es)
* [Usar la API de Document Service](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

O puede utilizar el paquete de muestra incluido como parte de estos recursos de tutorial.

>[!NOTE]
>
>El paquete de ejemplo utiliza un alias denominado &quot;ares&quot; para certificar los documentos. Por lo tanto, asegúrese de que su alias se llame &quot;ares&quot; al utilizar este paquete

## Prueba de la muestra en el sistema local

* Descargar e instalar [Paquete de servicios de documentos personalizados](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* Descargar e instalar [Desarrollo con paquete de usuario de servicio](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [Asegúrese de haber agregado la siguiente entrada en el servicio Asignador de usuarios del servicio Apache Sling](http://localhost:4502/system/console/configMgr)

   **DesarrollarWithServiceUser.core:getformsresourceresolver=fd-service** como se muestra en la captura de pantalla siguiente
   ![User-Mapper](assets/user-mapper-service.PNG)
* [Importar formulario adaptable de ejemplo](assets/certify-pdf-af.zip)
* [Importar e instalar el envío personalizado](assets/custom-submit-certify.zip)
* [Abra el formulario adaptable](http://localhost:4502/content/dam/formsanddocuments/certifypdf/jcr:content?wcmmode=disabled)
* Cargar documento de PDF que debe certificarse
   **opcional** - Especifique el campo de firma que desea utilizar para certificar el documento
* Haga clic en enviar.
* Se le debe devolver un PDF certificado.
