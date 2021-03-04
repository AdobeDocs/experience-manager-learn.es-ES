---
title: Certificación de documento en AEM Forms
seo-title: Certificación de documento en AEM Forms
description: Uso del servicio Docsurance para certificar documentos PDF en AEM Forms
seo-description: Uso del servicio Docsurance para certificar documentos PDF en AEM Forms
uuid: ecb1f9b6-bbb3-43a3-a0e0-4c04411acc9f
feature: document-security
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '417'
ht-degree: 0%

---


# Certificación de documento en AEM Forms

Un documento certificado proporciona a los destinatarios de documentos PDF y formularios garantías adicionales de su autenticidad e integridad.

Para certificar un documento, puede utilizar Acrobat DC en el escritorio o AEM Forms Document Services como parte de un proceso automatizado en un servidor.

Este artículo le proporciona un paquete OSGI de muestra para certificar documentos pdf mediante AEM Forms Document Services. El código utilizado en el ejemplo está [disponible aquí](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

Para certificar documentos mediante AEM Forms, se deben seguir los siguientes pasos

## Añadir certificado al almacén de confianza {#adding-certificate-to-trust-store}

Siga los pasos que se indican a continuación para añadir el certificado al almacén de claves en AEM

* [Inicializar el almacén de confianza global](http://localhost:4502/libs/granite/security/content/truststore.html)
* [Buscar fd-](http://localhost:4502/security/users.html) serviceuser
* **Tendrá que desplazarse por la página de resultados para cargar todos los usuarios y encontrar el usuario del fd-service**
* Haga doble clic en el usuario de fd-service para abrir la ventana de configuración del usuario
* Haga clic en &quot;Agregar clave privada del archivo del almacén de claves&quot;. Especifique el alias y la contraseña específicos de su certificado
   ![add-certificate](assets/adding-certificate-keystore.PNG)
* Guarde los cambios

## Creación del servicio OSGI

Puede escribir su propio paquete OSGi y utilizar el SDK de cliente de AEM Forms para implementar un servicio de certificación de documentos PDF. Los siguientes vínculos serían útiles para escribir su propio paquete OSGi

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

   **DevelopingWithServiceUser.core:getformsresourceresolver=fd-** serviceas como se muestra en la captura de pantalla siguiente
   ![User-Mapper](assets/user-mapper-service.PNG)
* [Importar formulario adaptable de ejemplo](assets/certify-pdf-af.zip)
* [Importar e instalar el envío personalizado](assets/custom-submit-certify.zip)
* [Abrir el formulario adaptable](http://localhost:4502/content/dam/formsanddocuments/certifypdf/jcr:content?wcmmode=disabled)
* Cargar documento PDF que necesita ser certificado
   **opcional** : especifique el campo de firma que desea utilizar para certificar el documento
* Haga clic en enviar.
* El PDF certificado debe devolverse a usted.


