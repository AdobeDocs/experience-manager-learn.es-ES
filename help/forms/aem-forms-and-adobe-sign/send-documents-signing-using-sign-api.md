---
title: Uso de la API de Adobe Sign en AEM Forms
description: Enviar documentos para firmar mediante los métodos de ayuda de Adobe Sign
feature: Adaptive Forms
jira: KT-15474
topic: Development
role: Developer
level: Beginner
badgeIntegration: label="Integración" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: 3400b728-58ca-44c3-a882-e3170755f845
duration: 74
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '298'
ht-degree: 0%

---

# Uso de métodos de ayuda de Adobe Sign

AEM En determinados casos de uso, es posible que tenga el requisito de enviar un documento para firmas sin utilizar un flujo de trabajo de la. En estos casos, será muy conveniente utilizar los métodos de envoltorio expuestos por el paquete de muestra proporcionado en este artículo.

## Implementar el paquete OSGi de muestra

[Implementar el paquete OSGi](assets/AdobeSignHelperMethods.core-1.0.0-SNAPSHOT.jar) AEM a través de la consola web de OSGi de OSGi. AEM Especifique la clave de integración de API y el usuario de API mediante la configuración OSGi, como se muestra a continuación, a través del Administrador de configuración de la consola web OSGi de la consola web de OSGi.

 Tenga en cuenta que las `AdobeSignHelperMethods` El paquete OSGi no se reconoce como código de producto de Adobe Experience Manager AEM () y, como tal, no es compatible con la compatibilidad con Adobes.
![configuración de firma](assets/sign-configuration.png)


## Documentación de API

Las siguientes opciones están disponibles a través de la `AcrobatSignHelperMethods` Servicio OSGi proporcionado en el paquete OSGi.

### getTransientDocumentID

`String getTransientDocumentID(Document documentForSigning) throws IOException`


Documento que se utiliza para crear un acuerdo o un formulario web. El remitente carga primero el documento en Acrobat Sign. Esto se conoce como _efímero_ ya que solo está disponible para su uso durante 7 días después de la carga. Este método acepta `com.adobe.aemfd.docmanager.Document` y devuelve el ID de documento transitorio.

### getAgreementID

`String getAgreementId(String transientDocumentID, String email) throws ClientProtocolException, IOException`

Envíe el documento para su firma mediante el ID de documento transitorio para su firma al usuario identificado por el parámetro de correo electrónico.

### getWidgetID

`String getWidgetID(String transientDocumentID)`

Un widget es como una plantilla reutilizable que se puede presentar a los usuarios varias veces y firmar varias veces. Utilice este método para obtener el ID del widget mediante el ID de documento transitorio.

### getWidgetURL

`String getWidgetURL(String widgetId) throws ClientProtocolException, IOException`

Obtenga una URL de widget para un ID de widget específico. Esta URL de widget se puede presentar a los usuarios para firmar el documento.

## Usar la API

El `AcrobatSignHelperMethods` es un servicio OSGi, por lo que debe anotarse con la anotación @Reference en el código java.

```java
...
// Import the AcrobatSignHelperMethods from the provided bundle
import com.acrobatsign.core.AcrobatSignHelperMethods;
...

@Component(service = { Example.class })
public class ExampleImpl implements Example {

 // Gain a reference to the provided AcrobatSignHelperMethods OSGi service
 @Reference
 com.acrobatsign.core.AcrobatSignHelperMethods acrobatSignHelperMethods;

 function void example() { 
    ...
    // Use the AcrobatSignHelperMethods API methods in your code
    String transientDocumentId = acrobatSignHelperMethods.getTransientDocumentID(documentForSigning);

    String agreementId = acrobatSignHelperMethods.getAgreementId(transientDocumentID, "johndoe@example.com");
    ...
 }
}
```
